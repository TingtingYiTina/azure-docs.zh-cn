---
title: Azure IoT Edge Node.js 教程 | Microsoft Docs
description: 本教程介绍如何使用 Node.js 代码创建 IoT Edge 模块并将其部署到边缘设备
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 11/25/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 12ba0ba4addd882d82007df34b79d5f13f6b1ec6
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309566"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-to-your-simulated-device"></a>教程：开发 Node.js IoT Edge 模块并将其部署到模拟设备

可以使用 IoT Edge 模块部署代码，以直接将业务逻辑实现到 IoT Edge 设备。 本教程详细介绍如何创建并部署用于筛选传感器数据的 IoT Edge 模块。 将使用在快速入门中创建的模拟 IoT Edge 设备。 本教程介绍如何执行下列操作：    

> [!div class="checklist"]
> * 使用 Visual Studio Code 创建 IoT Edge Node.js 模块
> * 使用 Visual Studio Code 和 Docker 创建 Docker 映像并将其发布到注册表 
> * 将模块部署到 IoT Edge 设备
> * 查看生成的数据


在本教程中创建的 IoT Edge 模块可以筛选由设备生成的温度数据。 它只在温度高于指定阈值的情况下，向上游发送消息。 在边缘进行的此类分析适用于减少传递到云中和存储在云中的数据量。 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

Azure IoT Edge 设备：

* 可以按照适用于 [Linux](quickstart-linux.md) 或 [Windows 设备](quickstart.md)的快速入门中的步骤，将开发计算机或虚拟机用作 Edge 设备。

云资源：

* Azure 中的免费或标准层 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。 

开发资源：

* [Visual Studio Code](https://code.visualstudio.com/)。 
* 适用于 Visual Studio Code 的 [Azure IoT Edge 扩展](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)。 
* [Docker CE](https://docs.docker.com/engine/installation/)。 
* [Node.js 和 npm](https://nodejs.org)。 npm 包是随 Node.js 分发的，也就是说，下载 Node.js 时，npm 会自动安装在计算机上。

## <a name="create-a-container-registry"></a>创建容器注册表

本教程将使用适用于 Visual Studio Code 的 Azure IoT Edge 扩展来生成模块并从文件创建**容器映像**。 然后将该映像推送到用于存储和管理映像的**注册表**。 最后，从注册表部署在 IoT Edge 设备上运行的映像。  

可以使用任意兼容 Docker 的注册表来保存容器映像。 两个常见 Docker 注册表服务分别是 [Azure 容器注册表](https://docs.microsoft.com/azure/container-registry/)和 [Docker 中心](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)。 本教程使用 Azure 容器注册表。 

如果还没有容器注册表，请执行以下步骤，以便在 Azure 中创建一个新的：

1. 在 [Azure 门户](https://portal.azure.com)中，选择“创建资源” > “容器” > “容器注册表”。

2. 提供以下值，以便创建容器注册表：

   | 字段 | 值 | 
   | ----- | ----- |
   | 注册表名称 | 提供唯一名称。 |
   | 订阅 | 从下拉列表中选择“订阅”。 |
   | 资源组 | 建议对在 IoT Edge 快速入门和教程中创建的所有测试资源使用同一资源组。 例如，**IoTEdgeResources**。 |
   | 位置 | 选择靠近你的位置。 |
   | 管理员用户 | 设置为“启用”。 |
   | SKU | 选择“基本”。 | **终端**

5. 选择“创建”。

6. 创建容器注册表后，请浏览到其中，然后选择“访问密钥”。 

7. 复制“登录服务器”、“用户名”和“密码”的值。 本教程后面会用到这些值来访问容器注册表。 

## <a name="create-an-iot-edge-module-project"></a>创建 IoT Edge 模块项目
以下步骤介绍如何使用 Visual Studio Code 和 Azure IoT Edge 扩展来创建 IoT Edge Node.js 模块。

### <a name="create-a-new-solution"></a>创建新的解决方案

使用 **npm** 创建一个 Node.js 解决方案模板，以便在其上生成项目。 

1. 在 Visual Studio Code 中选择“视图” > “集成终端”，打开 VS Code 集成终端。

2. 在集成终端输入以下命令，以便安装 **yeoman** 以及适用于 Node.js Azure IoT Edge 模块的生成器： 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. 选择“视图” > “命令面板”，打开 VS Code 命令面板。 

3. 在命令面板中，键入并运行“Azure: 登录”命令，然后按说明登录 Azure 帐户。 如果已登录，则可跳过此步骤。

4. 在命令面板中，键入并运行“Azure IoT Edge: 新建 IoT Edge 解决方案”命令。 按命令面板中的提示创建解决方案。

   | 字段 | 值 |
   | ----- | ----- |
   | 选择文件夹 | 在适用于 VS Code 的开发计算机上选择用于创建解决方案文件的位置。 |
   | 提供解决方案名称 | 输入解决方案的描述性名称，或者接受默认的 **EdgeSolution**。 |
   | 选择模块模板 | 选择“Node.js 模块”。 |
   | 提供模块名称 | 将模块命名为 **NodeModule**。 |
   | 为模块提供 Docker 映像存储库 | 映像存储库包含容器注册表的名称和容器映像的名称。 容器映像是在上一步预先填充的。 将 **localhost:5000** 替换为 Azure 容器注册表中的登录服务器值。 可以在 Azure 门户的容器注册表的“概览”页中检索登录服务器。 最终的字符串看起来类似于 \<注册表名称\>.azurecr.io/nodemodule。 |
 
   ![提供 Docker 映像存储库](./media/tutorial-node-module/repository.png)

VS Code 窗口将加载你的 IoT Edge 解决方案空间。 解决方案工作区包含五个顶级组件。 **modules** 文件夹包含模块的 Node.js 代码以及用于将模块构建为容器映像的 Dockerfile。 **\.env** 文件存储容器注册表凭据。 **deployment.template.json** 文件包含 IoT Edge 运行时用于在设备上部署模块的信息。 **deployment.debug.template.json** 文件包含模块的调试版本。 你不会在本教程中编辑 **\.vscode** 文件夹或 **\.gitignore** 文件。 

如果在创建解决方案时未指定容器注册表，但接受了默认的 localhost:5000 值，则不会有 \.env 文件。 

<!--
   ![Node.js solution workspace](./media/tutorial-node-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>添加注册表凭据

环境文件存储容器存储库的凭据，并将其与 IoT Edge 运行时共享。 此运行时需要这些凭据才能将专用映像拉取到 IoT Edge 设备中。 

1. 在 VS Code 资源管理器中，打开 **.env** 文件。 
2. 使用从 Azure 容器注册表复制的 **username** 和 **password** 值更新相关字段。 
3. 保存此文件。 

### <a name="update-the-module-with-custom-code"></a>使用自定义代码更新模块

每个模板都附带示例代码，该代码从 **tempSensor** 模块提取模拟传感器数据并将其路由到 IoT 中心。 在此部分，请添加代码，以便让 NodeModule 分析消息，然后再发送它们。 

1. 在 VS Code 资源管理器中，打开 **modules** > **NodeModule** > **app.js**。

5. 在所需的节点模块下面添加温度阈值变量。 温度阈值设置一个值，若要向 IoT 中心发送数据，测量的温度必须超出该值。

    ```javascript
    var temperatureThreshold = 25;
    ```

6. 将整个 `PipeMessage` 函数替换为 `FilterMessage` 函数。
    
    ```javascript
    // This function filters out messages that report temperatures below the temperature threshold.
    // It also adds the MessageType property to the message with the value set to Alert.
    function filterMessage(client, inputName, msg) {
        client.complete(msg, printResultFor('Receiving message'));
        if (inputName === 'input1') {
            var message = msg.getBytes().toString('utf8');
            var messageBody = JSON.parse(message);
            if (messageBody && messageBody.machine && messageBody.machine.temperature && messageBody.machine.temperature > temperatureThreshold) {
                console.log(`Machine temperature ${messageBody.machine.temperature} exceeds threshold ${temperatureThreshold}`);
                var outputMsg = new Message(message);
                outputMsg.properties.add('MessageType', 'Alert');
                client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
            }
        }
    }

    ```

7. 将函数名称 `pipeMessage` 替换为 `client.on()` 函数中的 `filterMessage`。

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

8. 将以下代码片段复制到 `client.open()` 函数回调中，具体说来是在 `else` 语句中的 `client.on()` 后面。 更新所需属性时，会调用此函数。

    ```javascript
    client.getTwin(function (err, twin) {
        if (err) {
            console.error('Error getting twin: ' + err.message);
        } else {
            twin.on('properties.desired', function(delta) {
                if (delta.TemperatureThreshold) {
                    temperatureThreshold = delta.TemperatureThreshold;
                }
            });
        }
    });
    ```

9. 保存此文件。

10. 在 VS Code 资源管理器的 IoT Edge 解决方案工作区中打开 **deployment.template.json** 文件。 

   此文件告知 `$edgeAgent` 部署两个模块：**tempSensor**，用于模拟设备数据，以及 **NodeModule**。 在 VS Code 状态栏中将 IoT Edge 的默认平台设置为 **amd64**，这意味着将 **NodeModule** 设置为映像的 Linux amd64 版本。 在状态栏中将默认平台从 **amd64** 更改为 **arm32v7** 或 **windows-amd64**（如果这就是 IoT Edge 设备的体系结构）。 若要详细了解部署清单，请参阅[了解如何使用、配置和重用 IoT Edge 模块](module-composition.md)。 

   此文件也包含注册表凭据。 在模板文件中，用户名和密码会使用占位符填充。 生成部署清单时，这些字段会被更新为添加到 **.env** 的值。 

12. 将 NodeModule 模块孪生添加到部署清单。 在 `moduleContent` 节底部 `$edgeHub` 模块孪生后插入以下 JSON 内容： 

   ```json
       "NodeModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![将模块孪生添加到部署模板](./media/tutorial-node-module/module-twin.png)

13. 保存此文件。


## <a name="build-your-iot-edge-solution"></a>生成 IoT Edge 解决方案

在上一部分，你已经创建了一个 IoT Edge 解决方案并将代码添加到了 NodeModule，该函数会筛选出其中报告的计算机温度低于可接受阈值的消息。 现在需将解决方案生成为容器映像并将其推送到容器注册表。 

1. 在 Visual Studio Code 集成终端输入以下命令，登录到 Docker，以便将模块映像推送到 ACR： 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   使用用户名、密码以及在第一部分从 Azure 容器注册表复制的登录服务器。 或者再次在 Azure 门户中从注册表的“访问密钥”部分检索它们。

2. 在 VS Code 资源管理器中右键单击“deployment.template.json”文件，然后选择“生成并推送 IoT Edge 解决方案”。 

要求 Visual Studio Code 生成解决方案时，它首先获取部署模板中的信息，然后在新的 **config** 文件夹中生成 `deployment.json` 文件。 然后，它在集成终端运行两个命令，即 `docker build` 和 `docker push`。 这两个命令会生成代码，将 Node.js 代码容器化，然后将其推送到在初始化解决方案时指定的容器注册表。 

可以使用在 VS Code 集成终端中运行的 `docker build` 命令查看具有标记的完整容器映像地址。 映像地址根据 `module.json` 文件中的信息生成，其格式为 **\<存储库\>:\<版本\>-\<平台\>**。 在本教程中，它应该类似于 **registryname.azurecr.io/nodemodule:0.0.1-amd64**。

## <a name="deploy-and-run-the-solution"></a>部署并运行解决方案

在用于设置 IoT Edge 设备的快速入门文章中，已使用 Azure 门户部署了一个模块。 还可以使用用于 Visual Studio Code 的 Azure IoT Toolkit 扩展来部署模块。 你已经为方案准备了部署清单，即 **deployment.json** 文件。 现在需要做的就是选择一个设备来接收部署。

1. 在 VS Code 命令面板中，运行“Azure IoT 中心: 选择 IoT 中心”。 

2. 选择包含要配置的 IoT Edge 设备的订阅和 IoT 中心。 

3. 在 VS Code 资源管理器中，展开“Azure IoT 中心设备”部分。 

4. 右键单击 IoT Edge 设备的名称，然后选择“为单个设备创建部署”。 

   ![为单个设备创建部署](./media/tutorial-node-module/create-deployment.png)

5. 选择 **config** 文件夹中的 **deployment.json** 文件，然后单击“选择 Edge 部署清单”。 不要使用 deployment.template.json 文件。 

6. 单击“刷新”按钮。 此时会看到新的 **NodeModule** 在运行，此外还有 **TempSensor** 模块以及 **$edgeAgent** 和 **$edgeHub** 在运行。 


## <a name="view-generated-data"></a>查看生成的数据

将部署清单应用到 IoT Edge 设备以后，设备上的 IoT Edge 运行时就会收集新的部署信息并开始在其上执行操作。 在设备上运行的未包括在部署清单中的任何模块都会停止。 设备中缺失的任何模块都会启动。 

可以通过 Visual Studio Code 资源管理器的“Azure IoT 中心设备”部分查看 IoT Edge 设备的状态。 展开设备的详细信息，可以看到已部署的正在运行的模块的列表。 

在 IoT Edge 设备本身上，可以使用 `iotedge list` 命令查看部署模块的状态。 应该看到四个模块：两个 IoT Edge 运行时模块、tempSensor 以及在本教程中创建的自定义模块。 启动所有模块可能需要数分钟，因此如果一开始没有看到全部模块，请重新运行命令。 

若要查看由任何模块生成的消息，请使用 `iotedge logs <module name>` 命令。 

可以使用 Visual Studio Code 在消息到达 IoT 中心时查看它们。 

1. 若要监视到达 IoT 中心的数据，请单击“...”，然后选择“开始监视 D2C 消息”。
2. 若要监视特定设备的 D2C 消息，请右键单击列表中的设备，然后选择“开始监视 D2C 消息”。
3. 若要停止监视数据，请在命令面板中运行“Azure IoT 中心: 停止监视 D2C 消息”命令。 
4. 若要查看或更新模块孪生，请右键单击列表中的模块，然后选择“编辑模块孪生”。 若要更新模块孪生，请保存孪生 JSON 文件，然后右键单击编辑器区域并选择“更新模块孪生”。
5. 若要查看 Docker 日志，可以安装用于 VS Code 的 [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)，然后通过 Docker 资源管理器在本地找到正在运行的模块。 在上下文菜单中单击“显示日志”，在集成终端中进行查看。 

## <a name="clean-up-resources"></a>清理资源 

如果打算继续学习下一篇建议的文章，可以保留已创建的资源和配置，以便重复使用。 还可以继续使用相同的 IoT Edge 设备作为测试设备。 

否则，可以删除本文中创建的本地配置和 Azure 资源，以避免收费。 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>后续步骤

在本教程中，创建了 IoT Edge 模块，其中包含用于筛选 IoT Edge 设备生成的原始数据的代码。 可以继续查看以下任一教程，了解 Azure IoT Edge 有助于将数据转化为边缘业务洞察力的其他方式。

> [!div class="nextstepaction"]
> [将 Azure 函数部署为模块](tutorial-deploy-function.md)
> [将 Azure 流分析部署为模块](tutorial-deploy-stream-analytics.md)

