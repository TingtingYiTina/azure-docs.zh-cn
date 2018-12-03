---
title: 在 Azure Stack 中轮换机密 | Microsoft Docs
description: 了解如何在 Azure Stack 中轮换机密。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 8d84801aacfc60bb11aac4c9046a433378a59b79
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314467"
---
# <a name="rotate-secrets-in-azure-stack"></a>在 Azure Stack 中轮换机密

这些说明仅适用于 Azure Stack 集成系统 1803 和更高版本。请勿在低于 1802 的 Azure Stack 版本上尝试使用机密轮换

Azure Stack 使用各种机密来维持 Azure Stack 基础结构资源与服务之间的安全通信。

- **内部机密**  
由 Azure Stack 基础结构使用的所有证书、密码、安全字符串和密钥，无需 Azure Stack 操作员的介入。 

- **外部机密**  
对外服务的基础结构服务证书，由 Azure Stack 操作员提供。 包括以下服务的证书： 
    - 管理员门户 
    - 公共门户 
    - 管理员 Azure 资源管理器 
    - 全局 Azure 资源管理器 
    - 管理员 Keyvault 
    - KeyVault 
    - ACS（包括 Blob、表和队列存储） 
    - ADFS<sup>*</sup>
    - Graph<sup>*</sup>

   <sup>*</sup> 仅当环境的标识提供者是 Active Directory 联合身份验证服务 (AD FS) 时才适用。

> [!NOTE]
> 其他所有安全密钥和字符串（包括 BMC 和交换密码以及用户和管理员帐户密码）仍然由管理员手动更新。 

为保持 Azure Stack 基础结构的完整性，操作员需要能够定期轮换其基础结构的机密，轮换频率应与其组织的安全要求一致。

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>使用新证书颁发机构颁发的外部证书轮换机密

在以下上下文中，Azure Stack 支持使用新证书颁发机构 (CA) 颁发的外部证书进行机密轮换：

|已安装的证书 CA|要轮换到的 CA|支持|支持的 Azure Stack 版本|
|-----|-----|-----|-----|
|从自签名|到企业|不支持||
|从自签名|到自签名|不支持||
|从自签名|到公共<sup>*</sup>|支持|1803 和更高版本|
|从企业|到企业|只要客户使用与在部署时使用的相同的企业 CA，就可以支持|1803 和更高版本|
|从企业|到自签名|不支持||
|从企业|到公共<sup>*</sup>|支持|1803 和更高版本|
|从公共<sup>*</sup>|到企业|不支持|1803 和更高版本|
|从公共<sup>*</sup>|到自签名|不支持||
|从公共<sup>*</sup>|到公共<sup>*</sup>|支持|1803 和更高版本|

<sup>*</sup> 这里的公共证书颁发机构属于 Windows 受信任根计划。 可以找到完整列表 [Microsoft 受信任根证书计划：参与者（截至 2017 年 6 月 27 日）](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca)。

## <a name="alert-remediation"></a>警报修正

机密过期后的 30 天内，管理员门户中会生成以下警报： 

- 挂起的服务帐户密码过期 
- 挂起的内部证书过期 
- 挂起的外部证书过期 

使用以下说明运行机密轮换将会修正这些警报。

## <a name="pre-steps-for-secret-rotation"></a>机密轮换前的步骤

   > [!IMPORTANT]  
   > 请确保未在环境中成功执行机密轮换。 如果已执行机密轮换，请在执行机密轮换之前将 Azure Stack 更新为版本 1807 或更高版本。 

1.  在 Azure Stack 机密轮换期间，操作员可能会注意到警报打开并自动关闭。  此行为是预期行为，可以忽略警报。  操作员可以通过运行 Test-AzureStack 来验证这些警报的有效性。  对于使用 SCOM 监视 Azure Stack 系统的操作人员来说，将系统置于维护模式将阻止这些警报到达其 ITSM 系统，但如果 Azure Stack 系统无法访问，则将继续发出警报。 
2. 在执行任何维护操作之前通知用户。 将普通的维护时间段尽量安排在非营业时间。 维护操作可能会同时影响用户工作负荷和门户操作。
    > [!note]  
    > 后续步骤仅适用于轮换 Azure Stack 外部机密。

3. 运行**[Test-azurestack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** 并确认所有测试输出都是在轮换机密之前正常运行。
4. 准备新的替换外部证书集。 新集与 [Azure Stack PKI 证书要求](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs)中所述的证书规范匹配。
5.  将用于轮换的证书备份存储在安全的备份位置。 如果运行轮换时发生失败，请使用备份副本替换文件共享中的证书，然后重新运行轮换。 请记得将备份副本保存在安全的备份位置。
6.  创建可从 ERCS VM 访问的文件共享。 该文件共享必须可供 **CloudAdmin** 标识读取和写入。
7.  在可以访问该文件共享的计算机上打开 PowerShell ISE 控制台。 导航到该文件共享。 
8.  运行 **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** 创建外部证书所需的目录。

## <a name="rotating-external-and-internal-secrets"></a>轮换外部和内部机密

若要轮换外部和内部机密：

1. 在前期步骤中新建的 **/Certificates** 目录内，根据 [Azure Stack PKI 证书要求](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates)的“必需证书”部分中所述的格式，将新的替换外部证书集放入目录结构。
2. 使用 **CloudAdmin** 帐户创建具有[特权终结点](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint)的 PowerShell 会话，并将会话存储为变量。 在下一步骤中要使用此变量作为参数。

    > [!IMPORTANT]  
    > 请勿输入会话，而是将会话存储为变量。
    
3. 运行 **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**。 将特权终结点 PowerShell 会话变量作为 **Session** 参数传递。 
4. 结合以下参数运行 **Start-SecretRotation**：
    - **PfxFilesPath**  
    将网络路径指定为前面创建的 Certificates 目录。  
    - **PathAccessCredential**  
    用于访问共享的凭据的 PSCredential 对象。 
    - **CertificatePassword**  
    创建的所有 pfx 证书文件使用的密码安全字符串。
4. 等待机密完成轮换。  
机密轮换成功完成后，控制台会显示“总体操作状态: 成功”。 
    > [!note]  
    > 如果机密轮换失败，请按照错误消息中的说明操作，并使用 **-Rerun** 参数重新运行 start-secretrotation。 如果遇到反复的机密轮换失败，请联系技术支持。 
5. 成功完成机密轮换后，请从前期步骤创建的共享中删除证书，并将其存储在安全的备份位置。 

## <a name="walkthrough-of-secret-rotation"></a>机密轮换的演练

以下 PowerShell 示例演示轮换机密时需要运行的 cmdlet 和参数。

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -AsPlainText -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```
## <a name="rotating-only-internal-secrets"></a>仅轮换内部机密

如果只轮换 Azure Stack 的内部机密：

1. 创建具有[特权终结点](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint)的 PowerShell 会话。
2. 在特权终结点会话中，不结合任何参数运行 **Start-SecretRotation**。
3. 等待机密完成轮换。  
机密轮换成功完成后，控制台会显示“总体操作状态: 成功”。 
    > [!note]  
    > 如果机密轮换失败，请按照错误消息中的说明操作，并使用 **-Rerun** 参数重新运行 start-secretrotation。 如果遇到反复的机密轮换失败，请联系技术支持。 

## <a name="start-secretrotation-reference"></a>Start-SecretRotation 参考

轮换 Azure Stack 系统的机密。 只针对 Azure Stack 特权终结点执行。

### <a name="syntax"></a>语法

路径（默认值）

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>说明

Start-SecretRotation cmdlet 轮换 Azure Stack 系统的基础结构机密。 默认情况下，它会轮换向内部基础结构网络公开的所有机密；在使用用户输入的情况下，它还会轮换所有外部网络基础结构终结点的证书。 轮换外部网络基础结构终结点时，应通过 Invoke-Command 脚本块，并结合以会话参数形式传入的 Azure Stack 环境特权终结点会话，来执行 Start-SecretRotation。
 
### <a name="parameters"></a>parameters

| 参数 | 类型 | 需要 | 位置 | 默认 | 说明 |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | String  | False  | 名为  | 无  | 包含所有外部网络终结点证书的 **\Certificates** 目录的文件共享路径。 仅当轮换外部机密或所有机密时才需要。 结尾目录必须是 **\Certificates**。 |
| CertificatePassword | SecureString | False  | 名为  | 无  | -PfXFilesPath 中提供的所有证书的密码。 如果在轮换内部和外部密码时提供了 PfxFilesPath，则是必需的值。 |
| PathAccessCredential | PSCredential | False  | 名为  | 无  | 包含所有外部网络终结点证书的 **\Certificates** 目录的文件共享的 PowerShell 凭据。 仅当轮换外部机密或所有机密时才需要。  |
| 重新运行 | SwitchParameter | False  | 名为  | 无  | 每当尝试失败后重新尝试机密轮换时，都必须使用重新运行。 |

### <a name="examples"></a>示例
 
**仅轮换内部基础结构机密**

```powershell  
PS C:\> Start-SecretRotation  
```

此命令轮换向 Azure Stack 内部网络公开的所有基础结构机密。 Start-SecretRotation 轮换堆栈生成的所有机密，但由于未提供证书，因此不会轮换外部终结点证书。  

**轮换内部和外部基础结构机密**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

此命令轮换向 Azure Stack 内部网络公开的所有基础结构机密，以及用于 Azure Stack 外部网络基础结构终结点的 TLS 证书。 Start-SecretRotation 轮换堆栈生成的所有机密，由于提供了证书，因此也会轮换外部终结点证书。  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>更新基板管理控制器 (BMC) 密码

基板管理控制器 (BMC) 监视服务器的物理状态。 有关更新 BMC 密码的规范和说明会根据原始设备制造商 (OEM) 硬件供应商而有所不同。 应定期更新 Azure Stack 组件的密码。

1. 按照 OEM 说明在 Azure Stack 的物理服务器上更新 BMC。 环境中每个 BMC 的密码必须相同。
2. 在 Azure Stack 会话中打开特权终结点。 有关说明，请参阅[使用 Azure Stack 中的特权终结点](azure-stack-privileged-endpoint.md)。
3. 在 PowerShell 提示符更改为 **[IP 地址或 ERCS VM 名称]: PS>** 或更改为 **[azs-ercs01]: PS>**（取决于环境）后，通过运行 `invoke-command` 来运行 `Set-BmcPassword`。 将特权终结点会话变量作为参数传递。 例如：

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    也可以将静态 PowerShell 版本与密码搭配使用，如以下代码行所示：
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>后续步骤

[详细了解 Azure Stack 安全性](azure-stack-security-foundations.md)
