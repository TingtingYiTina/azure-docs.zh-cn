---
title: 使用 Azure 数据工厂向/从 Azure SQL 数据库托管实例复制数据 | Microsoft Docs
description: 了解如何使用 Azure 数据工厂将数据移入和移出 Azure SQL 数据库托管实例。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: jingwang
ms.openlocfilehash: 561e672436c38cd0b3e637b794662483fc630676
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706715"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-using-azure-data-factory"></a>使用 Azure 数据工厂向/从 Azure SQL 数据库托管实例复制数据

本文概述了如何使用 Azure 数据工厂中的复制活动从/向 Azure SQL 数据库托管实例复制数据。 它是基于概述复制活动总体的[复制活动概述](copy-activity-overview.md)一文。

## <a name="supported-capabilities"></a>支持的功能

可将数据从 Azure SQL 数据库托管实例复制到任一支持的接收器数据存储，或从任一支持的源数据存储复制到托管实例。 有关复制活动支持作为源/接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

具体而言，此 Azure SQL 数据库托管实例连接器支持：

- 使用 SQL 或 Windows 身份验证复制数据。
- 作为源，使用 SQL 查询或存储过程检索数据。
- 作为接收器，在复制期间将数据追加到目标表，或调用带有自定义逻辑的存储过程。

## <a name="prerequisites"></a>先决条件

若要使用 VNET 中的“从 Azure SQL 数据库托管实例复制数据”，需要在可以访问该数据库的同一 VNET 中设置自承载集成运行时。 有关详细信息，请参阅[自承载集成运行时](create-self-hosted-integration-runtime.md)一文。

## <a name="getting-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 Azure SQL 数据库托管实例连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

Azure SQL 数据库托管实例链接的服务支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为：SqlServer | 是 |
| connectionString |指定使用 SQL 身份验证或 Windows 身份验证连接到托管实例时所需的 connectionString 信息。 请参阅以下示例。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 |是 |
| userName |如果使用的是 Windows 身份验证，请指定用户名。 示例：域名\\用户名。 |否 |
| password |指定为 userName 指定的用户帐户的密码。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 |否 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 在与托管实例相同的 VNET 中预配自承载集成运行时。 |是 |

>[!TIP]
>如果遇到错误（错误代码为“UserErrorFailedToConnectToSqlServer”，且消息如“数据库的会话限制为 XXX 且已达到。”），请将 `Pooling=false` 添加到连接字符串中，然后重试。

**示例 1：使用 SQL 身份验证**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**示例 2：使用 Windows 身份验证**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
             "userName": "<domain\\username>",
             "password": {
                "type": "SecureString",
                "value": "<password>"
             }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
     }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅数据集一文。 本部分提供 Azure SQL 数据库托管实例数据集支持的属性列表。

要从/向 Azure SQL 数据库托管实例复制数据，请将数据集的 type 属性设置为“SqlServerTable”。 支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为：SqlServerTable | 是 |
| tableName |链接服务引用的数据库实例中的表名称或视图名称。 | 对于源为“No”，对于接收器为“Yes” |

**示例**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 Azure SQL 数据库托管实例源和和接收器支持的属性列表。

### <a name="azure-sql-database-managed-instance-as-source"></a>Azure SQL 数据库托管实例作为源

若要从 Azure SQL 数据库托管实例复制数据，请将复制活动中的源类型设置为“SqlSource”。 复制活动源部分支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动源的 type 属性必须设置为：SqlSource | 是 |
| sqlReaderQuery |使用自定义 SQL 查询读取数据。 示例：`select * from MyTable`。 |否 |
| sqlReaderStoredProcedureName |从源表读取数据的存储过程的名称。 最后一个 SQL 语句必须是存储过程中的 SELECT 语句。 |否 |
| storedProcedureParameters |存储过程的参数。<br/>允许的值为：名称/值对。 参数的名称和大小写必须与存储过程参数的名称和大小写匹配。 |否 |

**需要注意的要点**

- 如果为 SqlSource 指定 sqlReaderQuery，则复制活动针对托管实例源运行此查询以获取数据。 此外，也可以通过指定 sqlReaderStoredProcedureName 和 storedProcedureParameters 来指定存储过程（如果存储过程使用参数）。
- 如果不指定“sqlReaderQuery”或“sqlReaderStoredProcedureName”属性，则使用在数据集 JSON 的“结构”部分定义的列，构建针对托管实例运行的查询 (`select column1, column2 from mytable`)。 如果数据集定义不具备该“结构”，则从表中选择所有列。

**示例：使用 SQL 查询**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**示例：使用存储过程**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**存储过程定义**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-managed-instance-as-sink"></a>Azure SQL 数据库托管实例作为接收器

若要向 Azure SQL 数据库托管实例复制数据，请将复制活动中的接收器类型设置为“SqlSink”。 复制活动接收器部分中支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动接收器的 type 属性必须设置为：SqlSink | 是 |
| writeBatchSize |缓冲区大小达到 writeBatchSize 时会数据插入 SQL 表。<br/>允许的值为：整数（行数）。 |否（默认值：10000） |
| writeBatchTimeout |超时之前等待批插入操作完成时的等待时间。<br/>允许的值为：timespan。 示例：“00:30:00”（30 分钟）。 |否 |
| preCopyScript |将数据写入到托管实例之前，指定复制活动要执行的 SQL 查询。 每次运行复制仅调用该查询一次。 此属性可用于清理预先加载的数据。 |否 |
| sqlWriterStoredProcedureName |存储过程的名称，该存储过程定义如何将源数据应用到目标表，例如使用你自己的业务逻辑执行更新插入或转换。 <br/><br/>请注意，此存储过程将由每个批处理调用。 如果要执行仅运行一次且与源数据无关的操作（例如删除/截断），请使用 `preCopyScript` 属性。 |否 |
| storedProcedureParameters |存储过程的参数。<br/>允许的值为：名称/值对。 参数的名称和大小写必须与存储过程参数的名称和大小写匹配。 |否 |
| sqlWriterTableType |指定要在存储过程中使用的表类型名称。 通过复制活动，使移动数据在具备此表类型的临时表中可用。 然后，存储过程代码可合并复制数据和现有数据。 |否 |

> [!TIP]
> 将数据复制到 Azure SQL 数据库托管实例时，复制活动默认将数据追加到接收器表后面。 要执行 UPSERT 或其他业务逻辑，请在 SqlSink 中使用存储过程。 参阅[调用 SQL 接收器的存储过程](#invoking-stored-procedure-for-sql-sink)，了解更多详细信息。

**示例 1：追加数据**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**示例 2：在 upsert 的复制过程中调用存储过程**

参阅[调用 SQL 接收器的存储过程](#invoking-stored-procedure-for-sql-sink)，了解更多详细信息。

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>目标数据库中的标识列

本部分提供一个示例，该示例演示将数据从没有标识列的源表复制到具有标识列的目标表。

**源表**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**目标表**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

请注意，目标表包含标识列。

**源数据集 JSON 定义**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**目标数据集 JSON 定义**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

请注意，源表和目标表具有不同架构（目标表具有一个额外标识列）。 在本方案中，需在不包含标识列的目标数据集定义中指定 structure 属性。

## <a name="invoking-stored-procedure-for-sql-sink"></a> 调用 SQL 接收器的存储过程

将数据复制到 Azure SQL 数据库托管实例时，可以配置某个用户指定的存储过程并配合其他参数进行调用。

内置的复制机制无法使用时，可使用存储过程。 在最后一次将源数据插入目标表前需要完成 upsert（插入+更新）或额外处理（合并列、查找其他值、插入到多个表等）时，通常使用此过程。

以下示例演示如何使用存储过程，在托管实例中的表内执行 upsert。 假设输入数据和接收器“Marketing”表中各具有三列：ProfileID、状态和类别。 基于“ProfileID”列执行 upsert，仅应用于特定类别。

**输出数据集**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

如下所示，在复制活动中定义 SqlSink 节。

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

在数据库中，使用与 SqlWriterStoredProcedureName 相同的名称定义存储过程。 它处理来自指定源的输入数据，并合并到输出表中。 存储过程中的表类型的参数名称应与数据集中定义的“tableName”相同。

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

在数据库中，使用与 SqlWriterTableType 相同的名称定义表类型。 请注意，表类型的架构应与输入数据返回的架构相同。

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

存储过程功能利用[表值参数](https://msdn.microsoft.com/library/bb675163.aspx)。

>[!NOTE]
>如果要通过调用存储过程写入 Money/Smallmoney 数据类型，值可能会舍入。 在 TVP 中将相应的数据类型指定为十进制而不是 Money/Smallmoney 来缓解。 

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Azure SQL 数据库托管实例的数据类型映射

从/向 Azure SQL 数据库托管实例复制数据时，以下映射用于从托管实例数据类型映射到 Azure 数据工厂临时数据类型。 若要了解复制活动如何将源架构和数据类型映射到接收器，请参阅[架构和数据类型映射](copy-activity-schema-and-type-mapping.md)。

| Azure SQL 数据库托管实例数据类型 | 数据工厂临时数据类型 |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |布尔 |
| char |String, Char[] |
| 日期 |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| 小数 |小数 |
| FILESTREAM 特性 (varbinary(max)) |Byte[] |
| Float |Double |
| 图像 |Byte[] |
| int |Int32 |
| money |小数 |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |小数 |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |小数 |
| sql_variant |对象 * |
| 文本 |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md##supported-data-stores-and-formats)。
