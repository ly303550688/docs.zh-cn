---
title: 从文件和其他源加载数据
description: 了解如何使用 API 将数据加载到 ML.NET 中进行处理和训练。 数据存储在文件、数据库、JSON、XML 或内存集合中。
ms.date: 11/07/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to, title-hack-0625
ms.openlocfilehash: 83aaae2d2e75b3076841750bf5d505390a538bc0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "74344757"
---
# <a name="load-data-from-files-and-other-sources"></a>从文件和其他源加载数据

了解如何使用 API 将数据加载到 ML.NET 中进行处理和训练。 最初存储在文件或其他数据源（例如数据库、JSON、XML 或内存中集合）中的数据。

如果使用模型生成器，请参阅[将训练数据加载到模型生成器](load-data-model-builder.md)。

## <a name="create-the-data-model"></a>创建数据模型

ML.NET 允许通过类定义数据模型。 例如，给定以下输入数据：

```text
Size (Sq. ft.), HistoricalPrice1 ($), HistoricalPrice2 ($), HistoricalPrice3 ($), Current Price ($)
700, 100000, 3000000, 250000, 500000
1000, 600000, 400000, 650000, 700000
```

创建一个表示以下代码片段的数据模型：

```csharp
public class HousingData
{
    [LoadColumn(0)]
    public float Size { get; set; }

    [LoadColumn(1, 3)]
    [VectorType(3)]
    public float[] HistoricalPrices { get; set; }

    [LoadColumn(4)]
    [ColumnName("Label")]
    public float CurrentPrice { get; set; }
}
```

### <a name="annotating-the-data-model-with-column-attributes"></a>使用列特性注释数据模型

特性为 ML.NET 提供有关数据模型和数据源的详细信息。

[`LoadColumn`](xref:Microsoft.ML.Data.LoadColumnAttribute) 特性指定属性的列索引。

> [!IMPORTANT]
> 只有从文件加载数据时才需要 [`LoadColumn`](xref:Microsoft.ML.Data.LoadColumnAttribute)。

将列加载为：

- 单个列，例如 `Size` 类中的 `CurrentPrices` 和 `HousingData`。
- 以向量的形式一次加载多个列，例如 `HousingData` 类中的 `HistoricalPrices`。

如果有一个向量属性，请在数据模型中向该属性应用 [`VectorType`](xref:Microsoft.ML.Data.VectorTypeAttribute) 特性。 请务必注意，向量中的所有元素必须为相同的类型。 保持列与列之间的分隔状态可以提高特征工程的易用性和灵活性，但是对于非常多的列，在单个列上操作会对训练速度产生影响。

ML.NET 通过列名称进行操作。 如果要将某个列的名称更改为该属性名称以外的其他名称，请使用 [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) 特性。 创建内存中对象时，仍然使用该属性名称创建对象。 但是，对于数据处理和生成机器学习模型，ML.NET 使用 [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) 特性中提供的值覆盖并引用该属性。

## <a name="load-data-from-a-single-file"></a>从单个文件加载数据

若要从文件加载数据，请使用 [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile*) 方法以及要加载的数据的数据模型。 由于 `separatorChar` 参数默认为制表符分隔，因此请根据需要为数据文件更改该参数。 如果文件有标头，请将 `hasHeader` 参数设置为 `true`，以忽略文件中的第一行并开始从第二行加载数据。

```csharp
//Create MLContext
MLContext mlContext = new MLContext();

//Load Data
IDataView data = mlContext.Data.LoadFromTextFile<HousingData>("my-data-file.csv", separatorChar: ',', hasHeader: true);
```

## <a name="load-data-from-multiple-files"></a>从多个文件加载数据

如果数据存储在多个文件中，只要数据架构相同，ML.NET 就允许从同一目录或多个目录中的多个文件加载数据。

### <a name="load-from-files-in-a-single-directory"></a>从单个目录中的文件加载

当所有数据文件位于同一目录中时，请在 [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile*) 方法中使用通配符。

```csharp
//Create MLContext
MLContext mlContext = new MLContext();

//Load Data File
IDataView data = mlContext.Data.LoadFromTextFile<HousingData>("Data/*", separatorChar: ',', hasHeader: true);
```

### <a name="load-from-files-in-multiple-directories"></a>从多个目录中的文件加载

若要从多个目录加载数据，请使用 [`CreateTextLoader`](xref:Microsoft.ML.TextLoaderSaverCatalog.CreateTextLoader*) 方法创建 [`TextLoader`](xref:Microsoft.ML.Data.TextLoader)。 然后，使用 [`TextLoader.Load`](xref:Microsoft.ML.DataLoaderExtensions.Load*) 方法并指定单个文件路径（不能使用通配符）。

```csharp
//Create MLContext
MLContext mlContext = new MLContext();

// Create TextLoader
TextLoader textLoader = mlContext.Data.CreateTextLoader<HousingData>(separatorChar: ',', hasHeader: true);

// Load Data
IDataView data = textLoader.Load("DataFolder/SubFolder1/1.txt", "DataFolder/SubFolder2/1.txt");
```

## <a name="load-data-from-a-relational-database"></a>从关系数据库加载数据

ML.NET 支持从各种关系数据库中加载数据（包括 SQL Server、Azure SQL 数据库、Oracle、SQLite、PostgreSQL、Progress、IBM DB2 等），这些关系数据库由 [`System.Data`](xref:System.Data) 提供支持。

> [!NOTE]
> 若要使用 `DatabaseLoader`，请参考 [System.Data.SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) NuGet 包。

指定具有名为 `House` 的表和以下架构的数据库：

```SQL
CREATE TABLE [House] (
    [HouseId] INT NOT NULL IDENTITY,
    [Size] INT NOT NULL,
    [NumBed] INT NOT NULL,
    [Price] REAL NOT NULL
    CONSTRAINT [PK_House] PRIMARY KEY ([HouseId])
);
```

数据可以通过 `HouseData` 等类进行建模。

```csharp
public class HouseData
{
    public float Size { get; set; }

    public float NumBed { get; set; }

    public float Price { get; set; }
}
```

然后，在应用程序中创建 `DatabaseLoader`。

```csharp
MLContext mlContext = new MLContext();

DatabaseLoader loader = mlContext.Data.CreateDatabaseLoader<HouseData>();
```

定义连接字符串以及要在数据库上执行的 SQL 命令，并创建 `DatabaseSource` 实例。 此示例使用具有文件路径的 LocalDB SQL Server 数据库。 但是，DatabaseLoader 支持本地和云中数据库的任何其他有效连接字符串。

```csharp
string connectionString = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=<YOUR-DB-FILEPATH>;Database=<YOUR-DB-NAME>;Integrated Security=True;Connect Timeout=30";

string sqlCommand = "SELECT Size, CAST(NumBed as REAL) as NumBed, Price FROM House";

DatabaseSource dbSource = new DatabaseSource(SqlClientFactory.Instance, connectionString, sqlCommand);
```

类型不是 [`Real`](xref:System.Data.SqlDbType) 的数值数据必须转换为 [`Real`](xref:System.Data.SqlDbType)。 [`Real`](xref:System.Data.SqlDbType) 类型表示为单精度浮动点值或 [`Single`](xref:System.Single)，这是 ML.NET 算法应采用的输入类型。 在此示例中，`NumBed` 列是数据库中的一个整数。 使用 `CAST` 内置函数，将其转换为 [`Real`](xref:System.Data.SqlDbType)。 由于 `Price` 属性的类型已经是 [`Real`](xref:System.Data.SqlDbType)，则按原样加载。

使用 `Load` 方法将数据加载到 [`IDataView`](xref:Microsoft.ML.IDataView)。

```csharp
IDataView data = loader.Load(dbSource);
```

## <a name="load-data-from-other-sources"></a>从其他源加载数据

除了加载存储在文件中的数据外，ML.NET 还支持从各种源加载数据，这些源包括但不限于：

- 内存中集合
- JSON/XML

请注意，在使用流式处理源时，ML.NET 预计输入采用内存中集合的形式。 因此，在使用 JSON/XML 等源时，请确保将数据格式化为内存中集合。

给定以下内存中集合：

```csharp
HousingData[] inMemoryCollection = new HousingData[]
{
    new HousingData
    {
        Size =700f,
        HistoricalPrices = new float[]
        {
            100000f, 3000000f, 250000f
        },
        CurrentPrice = 500000f
    },
    new HousingData
    {
        Size =1000f,
        HistoricalPrices = new float[]
        {
            600000f, 400000f, 650000f
        },
        CurrentPrice=700000f
    }
};
```

使用 [`IDataView`](xref:Microsoft.ML.IDataView) 方法将内存中集合加载到 [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable*) 中：

> [!IMPORTANT]
> [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable*) 假定其所加载的 [`IEnumerable`](xref:System.Collections.IEnumerable) 是线程安全的。

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

//Load Data
IDataView data = mlContext.Data.LoadFromEnumerable<HousingData>(inMemoryCollection);
```

## <a name="next-steps"></a>后续步骤

- 若要清除或以其他方式处理数据，请参阅[准备建模的数据](prepare-data-ml-net.md)。
- 准备好建模后，请参阅[训练和评估模型](train-machine-learning-model-ml-net.md)。
