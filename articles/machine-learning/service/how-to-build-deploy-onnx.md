---
title: ONNX 和 Azure 机器学习 | 创建和部署模型
description: 了解 ONNX 以及如何使用 Azure 机器学习来创建和部署 ONNX 模型
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 09/24/2018
ms.openlocfilehash: 2e5c0e479d5564a48048b9fa9c67ad8870122601
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706052"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX 和 Azure 机器学习：创建和部署可互操作 AI 模型

[开放式神经网络交换](https://onnx.ai) (ONNX) 格式是用于表示机器学习模型的开放标准。 ONNX 由[合作伙伴社区](https://onnx.ai/supported-tools)提供支持，包括 Microsoft，他们创建兼容的框架和工具。 Microsoft 致力于开放式和可互操作 AI，以便数据科学家和开发人员能够：

+ 使用自己选择的框架来创建模型并定型
+ 使用最低程度的集成工作跨平台部署模型

Microsoft 跨其产品（包括 Azure 和 Windows）支持 ONNX 以帮助实现这些目标。  

## <a name="why-choose-onnx"></a>为什么选择 ONNX？
通过 ONNX 获得的互操作性可以更快地将好的想法投入生产。 使用 ONNX，数据科学家可以选择其首选的作业框架。 同样，开发人员可以花费更少的时间让模型为生产做好准备，并跨云和边缘部署。  

可以从许多框架创建 ONNX 模型，这些框架包括 PyTorch、Chainer、Microsoft Cognitive Toolkit (CNTK)、MXNet 和 ML.Net、TensorFlow、Keras、SciKit-Learn 等等。

此外，还有用于可视化和加速 ONNX 模型的工具生态系统。 许多预先定型的 ONNX 模型也可用于常见方案。

可以使用 Azure 机器学习和 ONNX 运行时将 [ONNX 模型部署](#deploy)到云中。 还可以使用 [Windows 机器学习](https://docs.microsoft.com/windows/ai/)将它们部署到 Windows 10 设备。 甚至可以使用 ONNX 社区中提供的转换器将它们部署到其他平台。 

[ ![显示定型、转换器和部署的 ONNX 流程图](media/concept-onnx/onnx.png) ] (./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>获取 ONNX 模型

可通过以下几种方式获取 ONNX 模型：
+ 从 [ONNX 模型 Zoo](https://github.com/onnx/models) 获取预先定型的 ONNX 模型（请参阅本文底部的示例）
+ 从 [Azure 自定义影像服务](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/)生成自定义 ONNX 模型 
+ 将现有模型从另一种格式转换为 ONNX（请参阅本文底部的示例） 
+ 在 Azure 机器学习服务中将新的 ONNX 模型定型（请参阅本文底部的示例）

## <a name="saveconvert-your-models-to-onnx"></a>将模型保存/转换为 ONNX

可以将现有模型转换为 ONNX，也可以在定型结束时将其保存为 ONNX。

|用于模型的框架|转换示例或工具|
|-----|-------|
|PyTorch|[Jupyter 笔记本](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)|
|Microsoft&nbsp;Cognitive&nbsp;Toolkit&nbsp;(CNTK)|[Jupyter 笔记本](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)|
|TensorFlow|[tensorflow-onnx 转换器](https://github.com/onnx/tensorflow-onnx)|
|Chainer|[Jupyter 笔记本](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)|
|MXNet|[Jupyter 笔记本](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)|
|Keras、ScitKit-Learn、CoreML<br/>XGBoost 和 libSVM|[WinMLTools](https://docs.microsoft.com/windows/ai/convert-model-winmltools)|

可以在 [ONNX 教程站点](https://github.com/onnx/tutorials)中找到支持的框架和转换器的最新列表。

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>在 Azure 中部署 ONNX 模型

使用 Azure 机器学习服务，可以部署、管理和监视 ONNX 模型。 使用标准[部署工作流](concept-model-management-and-deployment.md)和 ONNX 运行时，可以创建在云中托管的 REST 终结点。 请参阅本文末尾的完整示例 Jupyter 笔记本，亲自试用。 

### <a name="install-and-configure-the-onnx-runtime"></a>安装和配置 ONNX 运行时

ONNX 运行时是 ONNX 模型的高性能推理引擎。 它附带 Python API，并提供 CPU 和 GPU 的硬件加速。 目前支持 ONNX 1.2 模型，并在 Ubuntu 16.04 Linux 上运行。 [CPU](https://pypi.org/project/onnxruntime) 包和 [GPU](https://pypi.org/project/onnxruntime-gpu) 包在 [PyPi.org](https://pypi.org) 上都可找到。

若要安装 ONNX 运行时，请使用：
```python
pip install onnxruntime
```

若要在 Python 脚本中调用 ONNX 运行时，请使用：
```python
import onnxruntime

session = onnxruntime.InferenceSession("path to model")
```

模型附带的文档通常会指示有关使用模型的输入和输出。 还可使用可视化工具（如 [Netron](https://github.com/lutzroeder/Netron)）查看模型。 ONNX 运行时还可以查询模型元数据、输入和输出：
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

若要推理模型，请使用 `run`，并传入要返回的输出列表（如果需要所有输出，则保留为空）和输入值的映射。 结果是输出列表。
```python
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

有关完整的 API 参考，请参阅 [ONNX 运行时参考文档](https://aka.ms/onnxruntime-python)。

### <a name="example-deployment-steps"></a>示例部署步骤

下面是一个部署 ONNX 模型的示例：

1. 初始化 Azure 机器学习服务工作区。 如果尚不具有，请了解如何在[本快速入门](quickstart-get-started.md)中创建工作区。

   ```python
   from azureml.core import Workspace

   ws = Workspace.from_config()
   print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
   ```

2. 使用 Azure 机器学习注册模型。

   ```python
   from azureml.core.model import Model

   model = Model.register(model_path = "model.onnx",
                          model_name = "MyONNXmodel",
                          tags = ["onnx"],
                          description = "test",
                          workspace = ws)
   ```

3. 使用模型和任何依赖项创建映像。

   ```python
   from azureml.core.image import ContainerImage
   
   image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                     runtime = "python",
                                                     conda_file = "myenv.yml",
                                                     description = "test",
                                                     tags = ["onnx"]
                                                    )

   image = ContainerImage.create(name = "myonnxmodelimage",
                                 # this is the model object
                                 models = [model],
                                 image_config = image_config,
                                 workspace = ws)

   image.wait_for_creation(show_output = True)
   ```

   文件 `score.py` 包含评分逻辑，需要包括在映像中。 此文件用于运行映像中的模型。 有关如何创建评分脚本的说明，请参阅本[教程](tutorial-deploy-models-with-aml.md#create-scoring-script)。 ONNX 模型的示例文件如下所示：

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global model_path
       model_path = Model.get_model_path(model_name = 'MyONNXmodel')

   def run(raw_data):
       try:
           data = json.loads(raw_data)['data']
           data = np.array(data)
        
           sess = onnxruntime.InferenceSession(model_path)
           result = sess.run(["outY"], {"inX": data})
        
           return json.dumps({"result": result.tolist()})
       except Exception as e:
           result = str(e)
           return json.dumps({"error": result})
   ```

   文件 `myenv.yml` 描述映像所需的依赖项。 有关如何创建环境文件（例如以下示例文件）的说明，请参阅本[教程](tutorial-deploy-models-with-aml.md#create-environment-file)：

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies()
   myenv.add_pip_package("numpy")
   myenv.add_pip_package("azureml-core")
   myenv.add_pip_package("onnxruntime")

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. 使用 Azure 机器学习将 ONNX 模型部署到：
   + Azure 容器实例 (ACI)：[了解如何...](how-to-deploy-to-aci.md)

   + Azure Kubernetes 服务 (AKS)：[了解如何...](how-to-deploy-to-aks.md)


## <a name="examples"></a>示例
 
以下笔记本演示如何使用 Azure 机器学习创建和部署 ONNX 模型： 
+ [onnx/onnx-modelzoo-aml-deploy-resnet50.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-modelzoo-aml-deploy-resnet50.ipynb)
+ [onnx/onnx-convert-aml-deploy-tinyyolo.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-convert-aml-deploy-tinyyolo.ipynb)
+ [onnx/onnx-train-pytorch-aml-deploy-mnist.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-train-pytorch-aml-deploy-mnist.ipynb)

以下笔记本演示如何使用 Azure 机器学习部署现有 ONNX 模型： 
+ [onnx/onnx-inference-mnist-deploy.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-mnist-deploy.ipynb) 
+ [onnx/onnx-inference-facial-expression-recognition-deploy.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-facial-expression-recognition-deploy.ipynb)
 
获取以下笔记本：
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>更多信息

详细了解 ONNX 或为项目做出贡献：
+ [ONNX 项目网站](https://onnx.ai)

+ [GitHub 上的 ONNX 代码](https://github.com/onnx/onnx)
