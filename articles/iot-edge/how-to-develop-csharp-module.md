---
title: 调试 Azure IoT Edge 的 C# 模块 | Microsoft Docs
description: 使用 Visual Studio Code 开发、生成和调试 Azure IoT Edge 的 C# 模块
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 275ee95261b168b0da7f0a4638679fe38fc0581b
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443877"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>使用 Visual Studio Code 开发和调试 Azure IoT Edge 的 C# 模块

可以将业务逻辑转变为用于 Azure IoT Edge 的模块。 本文展示了如何使用 Visual Studio Code (VS Code) 作为主要工具来开发和调试 C# 模块。

## <a name="prerequisites"></a>先决条件

可以使用运行 Windows、macOS 或 Linux 的计算机或虚拟机作为开发计算机。 IoT Edge 设备可以是另一台物理设备。

有两种方法可用来在 VS Code 中调试 C# 模块。 一种方法是在模块容器中附加一个进程，另一种方法是在调试模式下启动模块代码。 如果你不熟悉 Visual Studio Code 的调试功能，请阅读有关[Debugging](https://code.visualstudio.com/Docs/editor/debugging)（调试）的信息。

请首先安装 Visual Studio Code，然后添加以下必要的扩展：
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge 扩展](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker 扩展](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

要创建一个模块，需要生成项目文件夹的 .NET，生成模块映像的 Docker 以及用来保存模块映像的容器注册表：
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)。
* 开发计算机上的 [Docker Community Edition](https://docs.docker.com/install/)。 
* [Azure 容器注册表](https://docs.microsoft.com/azure/container-registry/)或 [Docker 中心](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * 对于原型和测试用途，可以使用本地 Docker 注册表，而不使用云注册表。 

若要设置本地开发环境以调试、运行和测试 IoT Edge 解决方案，需要具有 [Azure IoT EdgeHub 开发工具](https://pypi.org/project/iotedgehubdev/)。 安装 [Python (2.7/3.6) 和 Pip](https://www.python.org/)。 然后，通过在终端中运行以下命令来安装 iotedgehubdev。

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

若要在设备上测试模块，需要一个活动的 IoT 中心，其中至少创建了一个 IoT Edge 设备 ID。 如果正在开发计算机上运行 IoT Edge 守护程序，则可能需要在转到下一步之前停止 EdgeHub 和 EdgeAgent。 

## <a name="create-a-new-solution-with-c-module"></a>通过 C# 模块创建新的解决方案

执行以下步骤来使用 Visual Studio Code 和 Azure IoT Edge 扩展创建基于 .NET Core 2.1 的 IoT Edge 模块。 首先创建一个解决方案，然后生成该解决方案中的第一个模块。 每个解决方案可以包含多个模块。 

1. 在 Visual Studio Code 中，选择“视图” > “集成终端”。
2. 选择“视图” > “命令面板”。 
3. 在“命令面板”中，输入并运行“Azure IoT Edge: New IoT Edge Solution”命令。

   ![运行新的 IoT Edge 解决方案](./media/how-to-develop-csharp-module/new-solution.png)

4. 浏览到要创建新解决方案的文件夹。 选择“选择文件夹”。 
5. 输入解决方案的名称。 
6. 选择“C# 模块”作为解决方案中第一个模块的模板。
7. 输入模块的名称。 选择容器注册表中唯一的名称。 
8. 提供模块的映像存储库的名称。 VS Code 使用 localhost:5000 自动填充模块名。 将其替换为你自己的注册表信息。 如果使用本地 Docker 注册表进行测试，则可以使用 localhost。 如果使用 Azure 容器注册表，那么请从注册表的设置中使用登录服务器。 登录服务器如下所示：\<registry name\>.azurecr.io。 仅替换字符串的 localhost 部分，不要删除模块名。 最终的字符串看起来类似于 \<注册表名称\>.azurecr.io/\<modulename\>。

   ![提供 Docker 映像存储库](./media/how-to-develop-csharp-module/repository.png)

VS Code 采用你提供的信息，创建一个 IoT Edge 解决方案，然后在新窗口中加载它。

   ![查看 IoT Edge 解决方案](./media/how-to-develop-csharp-module/view-solution.png)

该解决方案中有四个项： 

* 一个 .vscode 文件夹，包含调试配置。
* 一个 modules 文件夹，包含每个模块的子文件夹。 现在，只有一个模块。 但是可以在命令面板中使用“Azure IoT Edge: Add IoT Edge Module”命令添加更多模块。 
* 一个 .env 文件，列出环境变量。 如果 Azure 容器注册表是注册表，则其中将包含 Azure 容器注册表用户名和密码。 

   >[!NOTE]
   >仅当为模块提供了映像存储库时，才会创建环境文件。 如果接受 localhost 默认值在本地进行测试和调试，则不需要声明环境变量。 

* 一个 deployment.template.json 文件，列出新模块以及模拟可用于测试的数据的示例 tempSensor 模块。 有关部署清单如何工作的详细信息，请参阅[了解如何使用部署清单部署模块和建立路由](module-composition.md)。 
* **deployment.debug.template.json** 文件包含具有适当容器选项的模块映像的调试版本。


## <a name="develop-your-module"></a>开发模块

解决方案附带的默认 C# 模块代码位于“模块”> ** [你的模块名称] ** >“Program.cs”。 设置模块和 deployment.template.json 文件，以便可以生成解决方案，将其推送到容器注册表，然后部署到设备以开始测试而无需触及任何代码。 该模块构建为只需从源（在此示例中，为模拟数据的 tempSensor 模块）获取输入并通过管道将其传送到 IoT Hub。 

当你准备使用自己的代码自定义 C# 模板时，请使用 [Azure IoT Hub SDK](../iot-hub/iot-hub-devguide-sdks.md) 生成模块，以满足 IoT 解决方案的关键需求（例如安全性、设备管理和可靠性）。 

VS Code 中的 C# 支持针对跨平台 .NET Core 开发进行了优化。 详细了解[如何使用 VS Code 中的 C#](https://code.visualstudio.com/docs/languages/csharp)。

## <a name="launch-and-debug-module-code-without-container"></a>在没有容器的情况下，启动和调试模块代码

IoT Edge C# 模块是一个 .Net Core 应用程序。 它依赖于 Azure IoT C# 设备 SDK。 因为 IoT C# 模块要求启动和运行环境设置，因此，请在默认的模块代码中使用环境设置和输入名称初始化 **ModuleClient**。 还需要将消息发送或路由到输入通道。 默认的 C# 模块仅包含一个名为 input1 的输入通道。

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>为 IoT Edge 解决方案设置 IoT Edge 模拟器

在开发计算机中，可以启动 IoT Edge 模拟器（而不是安装 IoT Edge 安全守护程序）以运行 IoT Edge 解决方案。 

1. 在左侧的设备资源管理器中，右键单击 IoT Edge 设备 ID，选择“设置 IoT Edge 模拟器”以使用设备连接字符串启动模拟器。

2. 可以看到 IoT Edge 模拟器已在集成终端中成功设置。

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>为单个模块应用设置 IoT Edge 模拟器
在开发计算机中，可以启动 IoT Edge 模拟器（而不是安装 IoT Edge 安全守护程序）以运行 IoT Edge 解决方案。 

1. 在左侧的设备资源管理器中，右键单击 IoT Edge 设备 ID，选择“设置 IoT Edge 模拟器”以使用设备连接字符串启动模拟器。

2. 可以看到 IoT Edge 模拟器已在集成终端中成功设置。

3. 在 VS Code 命令面板中，键入并选择“Azure IoT Edge: 启动单个模块的 IoT Edge 中心模拟器”。 此外，还需要为单个模块应用程序指定输入名称，键入 input1 并按 Enter。 该命令将触发 iotedgehubdev CLI 并启动 IoT Edge 模拟器并测试实用程序模块容器。 如果模拟器已成功以单模块模式启动，则可以在集成终端中看到下面的输出。 还可以看到 `curl` 命令以帮助发送消息。 稍后将使用它。

   ![为单个模块应用设置 IoT Edge 模拟器](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   可以移动到 Docker Explorer 并查看模块运行状态。

   ![模拟器模块状态](media/how-to-develop-csharp-module/simulator-status.png)

   edgeHubDev 容器是本地 IoT Edge 模拟器的核心。 它可以在没有 IoT Edge 安全守护程序的情况下在开发计算机上运行，并为本机模块应用或模块容器提供环境设置。 input 容器公开 restAPI 以帮助将消息桥接到模块上的目标输入通道。

4. 在 VS Code 命令面板中，键入并选择“Azure IoT Edge：将模块凭据设置为用户设置”，以在用户设置中将模块环境设置设为 `azure-iot-edge.EdgeHubConnectionString` 和 `azure-iot-edge.EdgeModuleCACertificateFile`。 可以在 .vscode  >  launch.json 和[ VS Code 用户设置](https://code.visualstudio.com/docs/getstarted/settings)中找到这些环境设置。

### <a name="build-module-app-and-debug-in-launch-mode"></a>生成模块应用并在启动模式下调试

1. 在集成终端中，将目录更改为 CSharpModule 文件夹，运行以下命令以生成 .Net Core 应用程序。

    ```cmd
    dotnet build
    ```

2. 导航到 `program.cs`。 在此文件中添加一个断点。

3. 导航到 VS Code 调试视图：查看 > 调试。 从下拉列表中选择调试配置“ModuleName 本地调试(.NET Core)”。 

  ![在 VS Code 中进入调试模式](media/how-to-develop-csharp-module/debug-view.png)

4. 单击“开始调试”或按 F5。 将启动调试会话。

   > [!NOTE]
   > 如果 .Net Core `TargetFramework` 与 `launch.json` 中的程序路径不一致， 则你需要手动更新 `launch.json` 中的程序路径以反映 .csproj 文件中的 `TargetFramework`， 以便 VS Code 可以成功启动此程序。

5. 在 VS Code 集成终端中，运行以下命令向模块发送 Hello World 消息。 这是在前面的步骤中成功设置 IoT Edge 模拟器时所显示的命令。

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > 如果使用的是 Windows，请确保 VS Code 集成终端的 shell 为 Git Bash 或 WSL Bash。 无法在 PowerShell 中或命令提示符中运行 `curl` 命令。 
   
   > [!TIP]
   > 还可以使用 [PostMan](https://www.getpostman.com/) 或其他 API 工具（而不是 `curl`）来发送消息。

6. 在 VS Code 调试视图中，将在左侧面板中看到变量。 

    ![监视变量](media/how-to-develop-csharp-module/single-module-variables.png)

7. 若要停止调试会话，请单击“停止”按钮或按 Shift + F5。 在 VS Code 命令面板中，键入并选择“Azure IoT Edge: 停止 IoT Edge 模拟器”以停止并清理模拟器。

## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>生成用于调试的模块容器并在附加模式下进行调试

默认解决方案包含两个模块，一个是模拟的温度传感器模块，另一个是 C# 管道模块。 模拟的温度传感器不断向 C# 管道模块发送消息，然后将消息通过管道传送到 IoT 中心。 在创建的模块文件夹中，有适用于不同容器类型的多个 Docker 文件。 使用以扩展名 .debug 结尾的任何文件来生成用于测试的模块。 默认情况下，**deployment.debug.template.json** 包含映像的调试版本。 目前，C# 模块仅支持在 Linux amd64 容器的附加模式下进行调试。 可以在 VS Code 状态栏中切换 Azure IoT Edge 默认平台。

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>为 IoT Edge 解决方案设置 IoT Edge 模拟器

在开发计算机中，可以启动 IoT Edge 模拟器（而不是安装 IoT Edge 安全守护程序）以运行 IoT Edge 解决方案。 

1. 在左侧的设备资源管理器中，右键单击 IoT Edge 设备 ID，选择“设置 IoT Edge 模拟器”以使用设备连接字符串启动模拟器。

2. 可以看到 IoT Edge 模拟器已在集成终端中成功设置。

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>生成和运行用于调试的容器并在附加模式下进行调试

1. 导航到 `program.cs`。 在此文件中添加一个断点。

2. 在 VS Code 文件资源管理器中，为解决方案选择 `deployment.debug.template.json` 文件，在上下文菜单中，单击“在模拟器中生成并运行 IoT Edge 解决方案”。 可以在同一窗口中监视所有模块容器日志。 还可以导航到 Docker Explorer 以监视容器状态。

   ![监视变量](media/how-to-develop-csharp-module/view-log.png)

3. 导航到 VS Code 调试视图。 选择你的模块的调试配置文件。 调试选项名称应类似于“ModuleName 远程调试(.NET Core)”

   ![选择“配置”](media/how-to-develop-csharp-module/debug-config.png)

4. 选择“开始调试”或选择 F5。 选择要附加到的进程。

5. 在 VS Code 调试视图中，将在左侧面板中看到变量。

6. 若要停止调试会话，请单击“停止”按钮或按 Shift + F5。 在 VS Code 命令面板中，键入并选择“Azure IoT Edge：停止 IoT Edge 模拟器”。

    > [!NOTE]
    > 此示例展示了如何调试容器上的 .NET Core IoT Edge 模块。 它基于 `Dockerfile.debug` 的调试版本，在生成容器映像时这将在容器映像中包括 Visual Studio .NET Core 命令行调试器 (VSDBG)。 对于生产就绪的 IoT Edge 模块，建议在调试完 C# 模块后在不使用 VSDBG 的情况下直接使用 Dockerfile。


## <a name="next-steps"></a>后续步骤

生成模块后，了解如何[从 Visual Studio Code 部署 Azure IoT Edge 模块](how-to-deploy-modules-vscode.md)。

若要开发用于 IoT Edge 设备的模块，请参阅[了解并使用 Azure IoT 中心 SDK](../iot-hub/iot-hub-devguide-sdks.md)。
