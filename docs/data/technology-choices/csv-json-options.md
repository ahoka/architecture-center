---
title: 
description: 
author: zoinerTejada
ms:date: 01/17/2018
---

# Technology choices: Processing CSV and JSON files

## What are your options for processing CSV and JSON files?
In Azure, the following services will help you process CSV and JSON files:

- [Azure Data Factory](/azure/data-factory/)
- [Azure Logic Apps](/azure/logic-apps/)
- [Azure Functions](/azure/azure-functions/functions-overview)
- [App Service](/azure/app-service/)
- [Azure Data Lake Analytics](/azure/data-lake-analytics/)
- Azure HDInsight
    - [Spark SQL](/azure/hdinsight/spark/apache-spark-overview)
    - [HBase](/azure/hdinsight/hbase/apache-hbase-overview)
    - [Hive](/azure/hdinsight/hadoop/hdinsight-use-hive)
- [SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)
- [Azure Machine Learning Workbench](/azure/machine-learning/preview/quickstart-installation)
- [SQL SSIS](/sql/integration-services/sql-server-integration-services)

The topic of processing CSV and JSON files can be separated into the following categories:

- Moving and importing CSV and JSON data, otherwise known as orchestration
- Storing the data in its original format
- Transforming CSV and JSON data

First, let's start with orchestration, or moving and importing your CSV and JSON files. [Azure Data Factory](/azure/data-factory/copy-activity-overview) provides a great option to automate the process for either one-time or recurring operations. Use the copy activity to copy your on-premises or cloud-based files into a number of [supported data stores](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats).

[SQL Server Integration Services](/sql/integration-services/sql-server-integration-services) (SSIS) is a platform for data integration and workflow applications capable of a broad range of data migration tasks. SSIS can be used to extract and transform data from a wide variety of sources such as CSV and JSON files, and then load the data into one or more destinations. Consider using SSIS for these tasks as part of your ETL process when using SQL Server or SQL Data Warehouse as your data store, or using Azure Data Factory to orchestrate your data pipeline processing.

Azure SQL Database enables you to directly load files stored on Azure Blob Storage using the [BULK INSERT T-SQL command and OPENROWSET function](/sql/relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage). This method does not require additional provisioning or cost of using another tool, as it involves a series of T-SQL commands to load your data.

For the topic of storing CSV and JSON data in its original format, there are a few options that can be used that do not require any up-front processing. Both Azure Storage blob containers and Azure Data Lake Store are excellent options for storing massive amounts of unstructured and semi-structured data for consumption by a number of Azure services, as [outlined in the data ingest technology choices](../technology-choices/data-ingest.md#file-storage) article. Both SQL Server and Azure SQL Database provide native [JSON data transformation and storage options](/sql/relational-databases/json/json-data-sql-server) if you want to store JSON data in its original form. A NoSQL data store you can use for JSON data instead is [Azure Cosmos DB](/azure/cosmos-db/introduction). Read more about NoSQL database options in the [data ingest technology choices](../technology-choices/data-ingest.md#nosql-databases) article.

Once stored, there are a number of options for transforming and processing your files. Because many programming languages can easily work with CSV and JSON files from applications that run on a virtual machine or locally, we are going to focus on Azure services you can use, some of which require no programming.

Starting out with a visual design-driven option for working with CSV and JSON files, you could use [Azure Logic Apps](/azure/logic-apps/) to create an automated workflow that processes these files, and translates them to another format, such as XML, or sends the data to a connected service or mobile device. You use an [integration account](/azure/logic-apps/logic-apps-enterprise-integration-create-integration-account) and link it to your logic app to [work with your flat files](/azure/logic-apps/logic-apps-enterprise-integration-flatfile). What you get out of flat files decoding is an XML document and you may further convert that document to JSON using the [workflow definition language](/azure/logic-apps/logic-apps-workflow-definition-language) json() function. In the opposite direction, you may convert JSON data to XML content using the xml() function of the workflow definition language.

An option that requires coding, but still allows you not have to manage infrastructure due to its serverless nature, is to use [Azure Functions](/azure/azure-functions/). Azure Functions is an event driven, compute-on-demand service that helps you implement code triggered by events occurring in virtually any Azure or third-party service as well as on-premises systems. In one scenario, you may have a process that stores your CSV or JSON files in blob storage, then create an Azure Function that is triggered any time a file is added to do some processing of that file. Another option is to use a webhook trigger on an Azure Function to parse and process JSON data sent from an online service like GitHub or Salesforce. Since Azure Functions can be written in C#, F#, Node.js, or Python, it is very easy to write code that can work CSV and JSON files. There are also [many bindings](/azure/azure-functions/functions-reference#bindings) for triggering functions, automatically working with inputs, and outputting processed information.

Process CSV and JSON files through web and API apps with [App Service](/azure/app-service/) for a more traditional web-based approach. Create web apps that can accept CSV and JSON files that are uploaded, or stored in Azure Storage or Azure Data Lake Store for processing, using .NET, .NET Core, Java, Ruby, Node.js, PHP, or Python.

When you need a big data solution, use [Azure HDInsight](/azure/hdinsight/), a managed open source big data analytics service for the enterprise. With it, you can create and use optimized Hadoop, Spark, Hive, HBase, Storm, and Microsoft R Server clusters to process and analyze your files at scale. Another option in this space is [Azure Data Lake Analytics](/azure/data-lake-analytics/)that runs massively parallel data transformation and processing programs using a T-SQL/C# hybrid called U-SQL, R, Python, and .NET over petabytes of data. Its capabilities for working with CSV and JSON files is mostly in line with HDInsight for batch operations. If you are interested in real-time stream processing of your files, such as live log analytics, HDInsight offers options for doing so with Hadoop technologies like Spark, HBase, and Storm.

If you would like to use advanced data preparation tools for efficiently exploring, understanding, and fixing problems in your source CSV or JSON data, consider using [Data Preparation](/azure/machine-learning/preview/data-prep-getting-started) as part of the [Azure Machine Learning Workbench](/azure/machine-learning/preview/scenario-big-data) experience. You can work with files stored locally, or in Azure Blob Storage. Once you have prepared your data, you can export it to another datastore more suitable for querying, or use it do develop a machine learning model using different compute targets, like Ubuntu DSVM or an Azure HDInsight cluster, for experimentation.

Consider using [SQL Data Warehouse](/azure/sql-data-warehouse/) when you need to integrate your relational and non-relational data, such as data stored in CSV and JSON files, in a cloud-based, scale-out database capable of processing massive volumes of data. This is an ideal solution for those more comfortable with using SQL Server Transact-SQL (T-SQL) and related tools. PolyBase is what allows SQL Data Warehouse to access and combine both non-relational and relational data. It allows you to run queries on external data in Hadoop or Azure blob storage. PolyBase uses external tables to access data in Azure Blob storage. By simply using Transact-SQL (T-SQL) statements, you can import and export data back and forth between relational tables in SQL Server and non-relational data stored in Hadoop or Azure Blob Storage. You can also query the external data using a T-SQL query and join it with relational data. However, the most common use of PolyBase is to load data into the data warehouse as part of the extract transform load (ETL) process, rather than performing ad hoc queries against CSV and JSON files.

## How do you choose?
Each service brings with it a unique set of capabilities, giving you options in selecting the one that most closely meets your requirements. You need to consider your specific challenges, like scale. Do you need to process one at a time, in large batches, or lots of incoming files streaming in real time?

## Key Selection Criteria

Answer the following questions to help you narrow down your choices, then use the matrices below to select your options for your scenario:

- Do you need big data capabilities for processing your files? Usually this means multi-gigabytes to terabytes of data.
    - If yes, then narrow your options to those that are big data services.
- Does your solution require moving data stored in one location into another?
    - If so, also look at the orchestration options.
- Does your data arrive via REST endpoints or through uploads through a web application?
    - If yes, compare options provided in the compute category.
- Does your data need to be processed in real time? In other words, do you need to process streaming data?
    - If so, consider the options that provide stream processing.
- Do you need to be able to re-process large amounts of data?
    - If so, select one of the big data options.

## Capability matrix

Capabilities are separated into the following categories: big data, machine learning, compute, and orchestration. These categories help compare like-services, due to the breadth of options and scenarios for processing CSV and JSON files.

### Big data capabilities

When you need to work with very large data sets, or process streaming data in real time, compare the capabilities of these big data services:

| | Azure Data Lake Analytics | HDInsight |
| --- | --- | --- |
| Is Managed Service | Yes | Yes&mdash;but requires manual configuration and scaling |
| Batch processing | Yes | Yes |
| Stream (Real-time processing) | No | Yes&mdash;using Spark Streaming, Storm, and/or Kafka (using the Streams API)|
| Interactive queries | No | Yes&mdash;using [Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) or [HDInsight Interactive Query (Hive LLAP)](/azure/hdinsight/hdinsight-hadoop-use-interactive-hive) |
| Programmability | U-SQL (combination of C# and T-SQL) | Python, Scala, Java, SQL (via Spark SQL or HiveQL), C# (when using Storm) |
| Scalability | Can scale individual queries | Scale by adding additional worker nodes |
| Install additional software | No | Yes&mdash;[manually](/azure/hdinsight/hdinsight-apps-install-custom-applications) or through the [Marketplace](/azure/hdinsight/hdinsight-apps-install-applications) |
| Cost platform | Only pay for query processing (analytic units) | Pay by the hour for lifetime of the cluster |

### Compute capabilities

Services in this category utilize serverless and web-based solutions for processing your CSV and JSON files.

Azure Functions and SQL Data Warehouse are both useful for streaming/real-time processing scenarios. For example, Azure Functions [can be used directly with Azure Cosmos DB](/azure/cosmos-db/serverless-computing-database) to create a serverless, globally distributed solution for providing real-time customer experiences based on data and events, without needing to build a full-blown application. The natural choice between CSV and JSON files in this scenario is JSON. SQL Data Warehouse, on the other hand, can be used in an architecture that captures CSV or JSON files and documents in real time through Kafka on HDInsight or Event Hubs, processed in real time using Spark or Storm on HDInsight, or with Stream Analytics, then ingested into SQL Data Warehouse through micro-batching offered by Event Grid, which sends processed data using PolyBase.

| | Logic apps | Azure Functions | App services | SQL Data Warehouse |
| --- | --- | --- | --- | --- |
| Programmability | None (uses workflow definition language) | C#, F#, Node.js, Python | .NET, .NET Core, Java, Ruby, Node.js, PHP, Python | T-SQL |
| Development model | Visual designer | Web application, WebJobs for background tasks | Functions with triggers | Stored procedures, views, dynamic SQL, PolyBase, massively parallel processing (MPP) |
| Serverless | Yes | Yes | No | No |
| Stream (Real-time processing) | No | Yes | No | Yes |

### Orchestration

When it comes to orchestration, Azure Data Factory and SQL Server Integration Services both offer a number of data sources and destinations you can use to move and transform your CSV and JSON file data. When your data destination is an Azure service, such as Azure Storage or HDInsight, Azure Data Factory <!--no acronyms using Azure, except in very rare cases. -->is the natural choice.

| | Azure Data Factory | SQL Server Integration Services (SSIS) |
| --- | --- | --- |
| Managed | Yes | No |
| Cloud-based | Yes | No (local) |
| Prerequisite | Azure Subscription | SQL Server  |
| Management Tools | Azure Portal, PowerShell, CLI, .NET SDK | SSMS, PowerShell |
| Pricing | Pay per usage | Licensing/Pay for features |
| Copy Data | Yes | Yes |
| Custom Transformations (C#) | Yes | Yes |
| Azure Machine Learning scoring | Yes | Yes (with scripting) |
| HDInsight on-demand | Yes | No |
| Azure Batch | Yes | No |
| Pig and Hive | Yes | No |
| Execute SSIS Package | Yes | Yes |

## Where to go from here
Read next:
[Data Warehousing](../solutions/data-warehousing.md)

See also:

Related pipeline patterns
- Working with transactional data
    - [Online Transaction Processing (OLTP)](../solutions/online-transaction-processing.md)
    - [Online Analytical Processing (OLAP)](../solutions/online-analytical-processing.md)

Related technology choices
- Transactional data stores
    - [Online Analytical Processing (OLAP) data stores](../technology-choices/olap-data-stores.md)
    - [Data Warehouses](../technology-choices/data-warehouses.md)
