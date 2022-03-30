---
title: Elasticsearch7.15指南
date: 2021-11-30 16:41:17
tags: [elk]
categories: [elk]
---

## Elasticsearch7.15指南

### 什么是Elasticsearch?

你知道，为了搜索（和分析）

Elasticsearch 是位于 Elastic Stack 核心的分布式搜索和分析引擎。Logstash 和 Beats 有助于收集、聚合和丰富您的数据并将其存储在 Elasticsearch 中。Kibana
使您能够以交互方式探索、可视化和共享对数据的洞察，并管理和监控堆栈。Elasticsearch 是索引、搜索和分析魔法发生的地方。

Elasticsearch 为所有类型的数据提供近乎实时的搜索和分析。无论您拥有结构化或非结构化文本、数值数据还是地理空间数据，Elasticsearch
都可以以支持快速搜索的方式高效地存储和索引它。您可以超越简单的数据检索和聚合信息来发现数据中的趋势和模式。随着您的数据和查询量的增长，Elasticsearch 的分布式特性使您的部署能够随之无缝增长。

虽然并非所有问题都是搜索问题，但 Elasticsearch 提供了在各种用例中处理数据的速度和灵活性：

* 向应用程序或网站添加搜索框
* 存储和分析日志、指标和安全事件数据
* 使用机器学习自动实时建模数据的行为
* 使用 Elasticsearch 作为存储引擎自动化业务工作流
* 使用 Elasticsearch 作为地理信息系统 (GIS) 管理、集成和分析空间信息
* 使用 Elasticsearch 作为生物信息学研究工具存储和处理遗传数据

我们不断对人们使用搜索的新颖方式感到惊讶。但是，无论您的用例是否与其中之一类似，或者您正在使用 Elasticsearch 来解决新问题，您在 Elasticsearch 中处理数据、文档和索引的方式都是相同的。

### 数据输入：文档和索引

Elasticsearch 是一个分布式文档存储。Elasticsearch 不是将信息存储为列状数据的行，而是存储已序列化为 JSON 文档的复杂数据结构。当集群中有多个 Elasticsearch
节点时，存储的文档分布在整个集群中，并且可以从任何节点立即访问。

存储文档后，它会被编入索引，并且可以近乎实时地在1 秒内完全搜索。Elasticsearch 使用一种称为倒排索引的数据结构，它支持非常快速的全文搜索。倒排索引列出出现在任何文档中的每个唯一单词，并标识每个单词出现的所有文档。

可以将索引视为文档的优化集合，每个文档都是字段的集合，这些字段是包含您的数据的键值对。默认情况下，Elasticsearch
索引每个字段中的所有数据，每个索引字段都有一个专用的、优化的数据结构。例如，文本字段存储在倒排索引中，数值和地理字段存储在 BKD 树中。使用每个字段的数据结构来组合和返回搜索结果的能力使 Elasticsearch 如此之快。

Elasticsearch 还具有无模式的能力，这意味着可以在不明确指定如何处理文档中可能出现的每个不同字段的情况下为文档编制索引。启用动态映射后，Elasticsearch
会自动检测并将新字段添加到索引中。这种默认行为使索引和探索数据变得容易——只需开始索引文档，Elasticsearch 就会检测并将布尔值、浮点和整数值、日期和字符串映射到适当的 Elasticsearch 数据类型。

但是，归根结底，您比 Elasticsearch 更了解您的数据以及您希望如何使用它。您可以定义规则来控制动态映射并显式定义映射以完全控制字段的存储和索引方式。

定义您自己的映射使您能够：

* 区分全文字符串字段和精确值字符串字段
* 执行特定于语言的文本分析
* 优化部分匹配的字段
* 使用自定义日期格式
* 使用无法自动检测到的 数据类型geo_point和geo_shape
  为了不同的目的以不同的方式索引相同的字段通常很有用。例如，您可能希望将字符串字段索引为用于全文搜索的文本字段和用于排序或聚合数据的关键字字段。或者，您可以选择使用多个语言分析器来处理包含用户输入的字符串字段的内容。

在索引期间应用于全文字段的分析链也在搜索时使用。当您查询全文字段时，在索引中查找术语之前，查询文本会经过相同的分析。

### 信息输出：搜索和分析

虽然您可以将 Elasticsearch 用作文档存储并检索文档及其元数据，但真正的强大之处在于能够轻松访问构建在 Apache Lucene 搜索引擎库上的全套搜索功能。

Elasticsearch 提供了一个简单、一致的 REST API 来管理您的集群以及索引和搜索您的数据。出于测试目的，您可以直接从命令行或通过 Kibana 中的开发人员控制台轻松提交请求。在您的应用程序中，您可以将
Elasticsearch 客户端 用于您选择的语言：Java、JavaScript、Go、.NET、PHP、Perl、Python 或 Ruby。

搜索您的数据

Elasticsearch REST API 支持结构化查询、全文查询和将两者结合的复杂查询。结构化查询类似于您可以在 SQL
中构造的查询类型。例如，您可以搜索索引中的gender和age字段并按字段employee对匹配项进行排序hire_date。全文查询查找与查询字符串匹配的所有文档，并按相关性排序返回它们——它们与您的搜索词的匹配程度。

除了搜索单个术语外，您还可以执行短语搜索、相似性搜索和前缀搜索，并获得自动完成建议。

有要搜索的地理空间数据或其他数字数据吗？Elasticsearch 在支持高性能地理和数值查询的优化数据结构中索引非文本数据。

您可以使用 Elasticsearch 的综合 JSON 样式查询语言 ( Query DSL )访问所有这些搜索功能。您还可以构建SQL 样式的查询以在 Elasticsearch 内部搜索和聚合数据，JDBC 和 ODBC
驱动程序使广泛的第三方应用程序能够通过 SQL 与 Elasticsearch 交互。

分析您的数据编辑 Elasticsearch 聚合使您能够构建复杂的数据摘要并深入了解关键指标、模式和趋势。聚合不仅可以找到众所周知的“大海捞针”，还可以让您回答以下问题：

* 大海捞针有多少针？
* 针的平均长度是多少？
* 制造商细分的针的中位数长度是多少？
* 在过去的六个月中，每一个月都向大海捞针添加了多少针？

您还可以使用聚合来回答更微妙的问题，例如：

* 您最受欢迎的针头制造商是哪些？
* 是否有异常或异常的针团？

因为聚合利用了用于搜索的相同数据结构，所以它们也非常快。这使您能够实时分析和可视化数据。您的报告和仪表板会随着数据的变化而更新，以便您可以根据最新信息采取行动。

更重要的是，聚合与搜索请求一起运行。您可以在单个请求中对相同数据同时搜索文档、过滤结果和执行分析。并且因为聚合是在特定搜索的上下文中计算的，所以您不仅会显示所有 70 号针的计数，还显示了与用户搜索条件匹配的 70
号针的计数——例如，所有尺寸 70不粘绣花针。

但是等等，还有更多编辑

想要自动化时间序列数据的分析吗？您可以使用 机器学习功能创建数据中正常行为的准确基线并识别异常模式。通过机器学习，您可以检测：

* 与值、计数或频率的时间偏差相关的异常
* 统计稀有度
* 某个群体成员的异常行为

最好的部分是？您无需指定算法、模型或其他与数据科学相关的配置即可执行此操作。

### 可扩展性和弹性：集群、节点和分片

Elasticsearch 旨在始终可用并根据您的需求进行扩展。它通过自然分布来做到这一点。您可以将服务器（节点）添加到集群以增加容量，Elasticsearch
会自动在所有可用节点之间分配您的数据和查询负载。无需大修您的应用程序，Elasticsearch 知道如何平衡多节点集群以提供可扩展性和高可用性。节点越多越好。

这是如何运作的？在幕后，Elasticsearch 索引实际上只是一个或多个物理分片的逻辑分组，其中每个分片实际上是一个独立的索引。通过将索引中的文档分布在多个分片中，并将这些分片分布在多个节点上，Elasticsearch
可以确保冗余，这既可以防止硬件故障，又可以在将节点添加到集群时增加查询容量。随着集群的增长（或缩小），Elasticsearch 会自动迁移分片以重新平衡集群。

有两种类型的分片：主分片和副本。索引中的每个文档都属于一个主分片。副本分片是主分片的副本。副本提供数据的冗余副本，以防止硬件故障并增加处理读取请求（如搜索或检索文档）的容量。

索引中的主分片数量在创建索引时是固定的，但副本分片的数量可以随时更改，而不会中断索引或查询操作。

这取决于……

关于分片大小和为索引配置的主分片数量，有许多性能考虑和权衡。分片越多，维护这些索引的开销就越大。分片大小越大，当 Elasticsearch 需要重新平衡集群时，移动分片所需的时间就越长。

查询大量小分片会使每个分片的处理速度更快，但更多查询意味着更多开销，因此查询较少数量的较大分片可能会更快。简而言之……视情况而定。

作为起点：

* 旨在将平均分片大小保持在几 GB 到几十 GB 之间。对于具有基于时间的数据的用例，通常会看到 20GB 到 40GB 范围内的分片。
* 避免大量碎片问题。一个节点可以容纳的分片数量与可用的堆空间成正比。作为一般规则，每 GB 堆空间的分片数应小于 20。

为您的用例确定最佳配置的最佳方法是 使用您自己的数据和查询进行测试。

发生灾害时

集群节点之间需要良好、可靠的连接。为了提供更好的连接，您通常将节点放在同一数据中心或附近的数据中心。但是，为了保持高可用性，您还需要避免任何单点故障。在一个位置发生重大中断的情况下，另一个位置的服务器需要能够接管。答案？跨集群复制 (
CCR)。

CCR 提供了一种将索引从主集群自动同步到可用作热备份的辅助远程集群的方法。如果主集群出现故障，辅助集群可以接管。您还可以使用 CCR 创建辅助集群，以便为您的用户提供地理位置邻近的读取请求。

跨集群复制是主动-被动的。主集群上的索引是活动的领导者索引，处理所有写请求。复制到辅助集群的索引是只读的跟随者。

护理和喂养

与任何企业系统一样，您需要工具来保护、管理和监控您的 Elasticsearch 集群。集成到 Elasticsearch 中的安全、监控和管理功能使您能够将Kibana 用作管理集群的控制中心。类似的特征数据汇总和指标生命周期管理
可帮助您明智随着时间的推移管理您的数据。

### 7.15 中的新功能

以下是 Elasticsearch 7.15 中新增和改进的亮点！

有关此版本的详细信息，请参阅发行说明和 迁移指南。

其他版本： 7.14 | 7.13 | 7.12 | 7.11 | 7.10 | 7.9 | 7.8 | 7.7 | 7.6 | 7.5 | 7.4 | 7.3 | 7.2 | 7.1 | 7.0

索引磁盘使用API编辑

有一个新的 API 支持分析索引的每个字段的磁盘使用情况，包括整个索引本身。API 通过迭代字段的内容并跟踪读取的字节数来估计字段的磁盘使用情况。请参阅分析索引磁盘使用情况 API。

搜索矢量切片 API编辑

有一个新端点可用于从存储在 Elasticsearch 中的地理空间数据生成矢量切片。此功能对于想要在地图上呈现存储在 Elasticsearch 中的地理空间信息的任何应用程序都很有用。请参阅搜索矢量切片 API。

复合运行时字段编辑

运行时字段支持 grok 和 dissect 模式，但之前仅针对单个字段发出值。您现在可以使用composite运行时字段从单个字段发出多个值。请参阅定义复合运行时字段。

### 快速开始

本指南帮助初学者学习如何：

* 在测试环境中安装和运行 Elasticsearch
* 将数据添加到 Elasticsearch
* 搜索和排序数据
* 在搜索期间从非结构化内容中提取字段

#### 运行 Elasticsearch编辑

设置 Elasticsearch 的最简单方法是在 Elastic Cloud 上使用 Elasticsearch Service 创建托管部署。如果您更喜欢管理自己的测试环境，可以使用 Docker 安装和运行
Elasticsearch。

> > 弹性搜索服务
> 1. 获得免费试用。
> 2. 登录Elastic Cloud。
> 3. 单击创建部署。
> 4. 选择一个解决方案并为您的部署命名。
> 5. 单击创建部署并下载elastic用户的密码。

> > docker 安装
>
> **安装并运行 Elasticsearch**
>
> 1.安装并启动Docker 桌面。
>
> 2.运行：
> ```
> docker network create elastic
> docker pull docker.elastic.co/elasticsearch/elasticsearch:7.15.2
> docker run --name es01-test --net elastic -p 127.0.0.1:9200:9200 -p 127.0.0.1:9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.15.2
> ```
>
>
> **安装并运行 Kibana**
>
>要使用直观的 UI 分析、可视化和管理 Elasticsearch 数据，请安装 Kibana。
>
> 1.在新的终端会话中，运行：
>```
>docker pull docker.elastic.co/kibana/kibana:7.15.2
>docker run --name kib01-test --net elastic -p 127.0.0.1:5601:5601 -e "ELASTICSEARCH_HOSTS=http://es01-test:9200" docker.elastic.co/kibana/kibana:7.15.2

>```
> 
> 2.要访问 Kibana，请转到http://localhost:5601

#### 向 Elasticsearch 发送请求编辑

您使用 REST API 向 Elasticsearch 发送数据和其他请求。这使您可以使用任何发送 HTTP 请求的客户端（例如curl ）与 Elasticsearch 进行交互 。您还可以使用 Kibana 的控制台向
Elasticsearch 发送请求。

> > 弹性搜索服务
> 使用卷曲
>
> 1.要使用 curl 或其他客户端与 Elasticsearch 通信，您需要集群的端点。进入Elasticsearch页面，点击Copy endpoint。
> 2.要提交示例 API 请求，请在新的终端会话中运行以下 curl 命令。替换<password>为elastic用户的密码。替换<elasticsearch_endpoint>为您的端点。
>```
> curl - u elastic :<密码> < elasticsearch_endpoint >/
>```
>
> 使用 Kibana
>
> 1.转到Kibana页面并单击Launch。
>
> 2.打开 Kibana 的主菜单并转到Dev Tools > Console。
>
> 在控制台中运行以下示例 API 请求：

Add dataedit

You add data to Elasticsearch as JSON objects called documents. Elasticsearch stores these documents in searchable indices.

For time series data, such as logs and metrics, you typically add documents to a data stream made up of multiple auto-generated backing indices.

A data stream requires an index template that matches its name. Elasticsearch uses this template to configure the stream’s backing indices. Documents sent to a data stream must have a @timestamp field.

Add a single documentedit

Submit the following indexing request to add a single log entry to the logs-my_app-default data stream. Since logs-my_app-default doesn’t exist, the request automatically creates it using the built-in logs-*-* index template.

````
curl -X POST "localhost:9200/logs-my_app-default/_doc?pretty" -H 'Content-Type: application/json' -d'
{
  "@timestamp": "2099-05-06T16:21:15.000Z",
  "event": {
    "original": "192.0.2.42 - - [06/May/2099:16:21:15 +0000] \"GET /images/bg.jpg HTTP/1.0\" 200 24736"
  }
}
'
````
The response includes metadata that Elasticsearch generates for the document:

* The backing _index that contains the document. Elasticsearch automatically generates the names of backing indices.
* A unique _id for the document within the index.
````
{
  "_index": ".ds-logs-my_app-default-2099-05-06-000001",
  "_type": "_doc",
  "_id": "gl5MJXMBMk1dGnErnBW8",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}

````
Add multiple documentsedit

Use the _bulk endpoint to add multiple documents in one request. Bulk data must be newline-delimited JSON (NDJSON). Each line must end in a newline character (\n), including the last line.

````
curl -X PUT "localhost:9200/logs-my_app-default/_bulk?pretty" -H 'Content-Type: application/json' -d'
{ "create": { } }
{ "@timestamp": "2099-05-07T16:24:32.000Z", "event": { "original": "192.0.2.242 - - [07/May/2020:16:24:32 -0500] \"GET /images/hm_nbg.jpg HTTP/1.0\" 304 0" } }
{ "create": { } }
{ "@timestamp": "2099-05-08T16:25:42.000Z", "event": { "original": "192.0.2.255 - - [08/May/2099:16:25:42 +0000] \"GET /favicon.ico HTTP/1.0\" 200 3638" } }
'

````

#### Search dataedit
Indexed documents are available for search in near real-time. The following search matches all log entries in logs-my_app-default and sorts them by @timestamp in descending order.
````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": { }
  },
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'

````
By default, the hits section of the response includes up to the first 10 documents that match the search. The _source of each hit contains the original JSON object submitted during indexing.
````
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 3,
      "relation": "eq"
    },
    "max_score": null,
    "hits": [
      {
        "_index": ".ds-logs-my_app-default-2099-05-06-000001",
        "_type": "_doc",
        "_id": "PdjWongB9KPnaVm2IyaL",
        "_score": null,
        "_source": {
          "@timestamp": "2099-05-08T16:25:42.000Z",
          "event": {
            "original": "192.0.2.255 - - [08/May/2099:16:25:42 +0000] \"GET /favicon.ico HTTP/1.0\" 200 3638"
          }
        },
        "sort": [
          4081940742000
        ]
      },
      ...
    ]
  }
}

````
##### Get specific fields

Parsing the entire _source is unwieldy for large documents. To exclude it from the response, set the _source parameter to false. Instead, use the fields parameter to retrieve the fields you want.
````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": { }
  },
  "fields": [
    "@timestamp"
  ],
  "_source": false,
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'
````
The response contains each hit’s fields values as a flat array.
````
{
  ...
  "hits": {
    ...
    "hits": [
      {
        "_index": ".ds-logs-my_app-default-2099-05-06-000001",
        "_type": "_doc",
        "_id": "PdjWongB9KPnaVm2IyaL",
        "_score": null,
        "fields": {
          "@timestamp": [
            "2099-05-08T16:25:42.000Z"
          ]
        },
        "sort": [
          4081940742000
        ]
      },
      ...
    ]
  }
}

````
##### Search a date range
To search across a specific time or IP range, use a range query.
````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "range": {
      "@timestamp": {
        "gte": "2099-05-05",
        "lt": "2099-05-08"
      }
    }
  },
  "fields": [
    "@timestamp"
  ],
  "_source": false,
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'

````
You can use date math to define relative time ranges. The following query searches for data from the past day, which won’t match any log entries in logs-my_app-default.
````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "range": {
      "@timestamp": {
        "gte": "now-1d/d",
        "lt": "now/d"
      }
    }
  },
  "fields": [
    "@timestamp"
  ],
  "_source": false,
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'

````
##### Extract fields from unstructured contentedit
You can extract runtime fields from unstructured content, such as log messages, during a search.

Use the following search to extract the source.ip runtime field from event.original. To include it in the response, add source.ip to the fields parameter.
````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "runtime_mappings": {
    "source.ip": {
      "type": "ip",
      "script": "String sourceip=grok(\u0027%{IPORHOST:sourceip} .*\u0027).extract(doc[ \"event.original\" ].value)?.sourceip;\nif (sourceip != null) emit(sourceip);"
    }
  },
  "query": {
    "range": {
      "@timestamp": {
        "gte": "2099-05-05",
        "lt": "2099-05-08"
      }
    }
  },
  "fields": [
    "@timestamp",
    "source.ip"
  ],
  "_source": false,
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'

````
##### Combine queriesedit
You can use the bool query to combine multiple queries. The following search combines two range queries: one on @timestamp and one on the source.ip runtime field.

````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "runtime_mappings": {
    "source.ip": {
      "type": "ip",
      "script": "String sourceip=grok(\u0027%{IPORHOST:sourceip} .*\u0027).extract(doc[ \"event.original\" ].value)?.sourceip;\nif (sourceip != null) emit(sourceip);"
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "@timestamp": {
              "gte": "2099-05-05",
              "lt": "2099-05-08"
            }
          }
        },
        {
          "range": {
            "source.ip": {
              "gte": "192.0.2.0",
              "lte": "192.0.2.240"
            }
          }
        }
      ]
    }
  },
  "fields": [
    "@timestamp",
    "source.ip"
  ],
  "_source": false,
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'

````
##### Aggregate dataedit
Use aggregations to summarize data as metrics, statistics, or other analytics.

The following search uses an aggregation to calculate the average_response_size using the http.response.body.bytes runtime field. The aggregation only runs on documents that match the query.

````
curl -X GET "localhost:9200/logs-my_app-default/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "runtime_mappings": {
    "http.response.body.bytes": {
      "type": "long",
      "script": "String bytes=grok(\u0027%{COMMONAPACHELOG}\u0027).extract(doc[ \"event.original\" ].value)?.bytes;\nif (bytes != null) emit(Integer.parseInt(bytes));"
    }
  },
  "aggs": {
    "average_response_size":{
      "avg": {
        "field": "http.response.body.bytes"
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "@timestamp": {
              "gte": "2099-05-05",
              "lt": "2099-05-08"
            }
          }
        }
      ]
    }
  },
  "fields": [
    "@timestamp",
    "http.response.body.bytes"
  ],
  "_source": false,
  "sort": [
    {
      "@timestamp": "desc"
    }
  ]
}
'

````
The response’s aggregations object contains aggregation results.
````
{
  ...
  "aggregations" : {
    "average_response_size" : {
      "value" : 12368.0
    }
  }
}

````
Explore more search optionsedit
To keep exploring, index more data to your data stream and check out Common search options.

#### Clean upedit
When you’re done, delete your test data stream and its backing indices.
````
curl -X DELETE "localhost:9200/_data_stream/logs-my_app-default?pretty"

````
You can also delete your test deployment.

>> Elasticsearch Service
>
> Click Delete deployment from the deployment overview page and follow the prompts.

>> Self-managed
>
> To stop your Elasticsearch and Kibana Docker containers, run:
>```
> docker stop es01-test
> docker stop kib01-test
>```
>
> To remove the containers and their network, run:
>```
>docker network rm elastic
> docker rm es01-test
> docker rm kib01-test
>```

**What’s next?edit**

* Get the most out of your time series data by setting up data tiers and ILM. See Use Elasticsearch for time series data.
* Use Fleet and Elastic Agent to collect logs and metrics directly from your data sources and send them to Elasticsearch. See the Fleet quick start guide.
* Use Kibana to explore, visualize, and manage your Elasticsearch data. See the Kibana quick start guide.


### Set up Elasticsearchedit
This section includes information on how to setup Elasticsearch and get it running, including:

* Downloading
* Installing
* Starting
* Configuring

#### Supported platforms

The matrix of officially supported operating systems and JVMs is available here: Support Matrix. Elasticsearch is tested on the listed platforms, but it is possible that it will work on other platforms too.

#### Java (JVM) Version
Elasticsearch is built using Java, and includes a bundled version of OpenJDK from the JDK maintainers (GPLv2+CE) within each distribution. The bundled JVM is the recommended JVM and is located within the jdk directory of the Elasticsearch home directory.

To use your own version of Java, set the ES_JAVA_HOME environment variable. If you must use a version of Java that is different from the bundled JVM, we recommend using a supported LTS version of Java. Elasticsearch will refuse to start if a known-bad version of Java is used. The bundled JVM directory may be removed when using your own JVM.

#### Use dedicated hosts
In production, we recommend you run Elasticsearch on a dedicated host or as a primary service. Several Elasticsearch features, such as automatic JVM heap sizing, assume it’s the only resource-intensive application on the host or container. For example, you might run Metricbeat alongside Elasticsearch for cluster statistics, but a resource-heavy Logstash deployment should be on its own host.

### Installing Elasticsearch

#### Hosted Elasticsearch
You can run Elasticsearch on your own hardware or use our hosted Elasticsearch Service that is available on AWS, GCP, and Azure. Try the Elasticsearch Service for free.

#### Installing Elasticsearch Yourself
Elasticsearch is provided in the following package formats:

|系统|描述|
|:---|:---|
|Linux and MacOS tar.gz archives|The tar.gz archives are available for installation on any Linux distribution and MacOS.|
|Windows .zip archive|The zip archive is suitable for installation on Windows.|
|deb|The deb package is suitable for Debian, Ubuntu, and other Debian-based systems. Debian packages may be downloaded from the Elasticsearch website or from our Debian repository.|
|rpm|The rpm package is suitable for installation on Red Hat, Centos, SLES, OpenSuSE and other RPM-based systems. RPMs may be downloaded from the Elasticsearch website or from our RPM repository.|
|msi|[beta] This functionality is in beta and is subject to change. The design and code is less mature than official GA features and is being provided as-is with no warranties. Beta features are not subject to the support SLA of official GA features.The msi package is suitable for installation on Windows 64-bit systems with at least .NET 4.5 framework installed, and is the easiest choice for getting started with Elasticsearch on Windows. MSIs may be downloaded from the Elasticsearch website.|
|docker|Images are available for running Elasticsearch as Docker containers. They may be downloaded from the Elastic Docker Registry.<br>[Install Elasticsearch with Docker](https://www.elastic.co/guide/en/elasticsearch/reference/7.15/docker.html)|
|brew|Formulae are available from the Elastic Homebrew tap for installing Elasticsearch on macOS with the Homebrew package manager.|


#### Configuration Management Tools
We also provide the following configuration management tools to help with large deployments:

|tool|url|
|:---|:---|
|Puppet|[puppet-elasticsearch](https://github.com/voxpupuli/puppet-elasticsearch)|
|Chef|[cookbook-elasticsearch](https://github.com/elastic/cookbook-elasticsearch)|
|Ansible|[ansible-elasticsearch](https://github.com/elastic/ansible-elasticsearch)|

### Install Elasticsearch from archive on Linux or MacOS

### Configuring Elasticsearchedit

Elasticsearch ships with good defaults and requires very little configuration. Most settings can be changed on a running cluster using the Cluster update settings API.

The configuration files should contain settings which are node-specific (such as node.name and paths), or settings which a node requires in order to be able to join a cluster, such as cluster.name and network.host.


#### Config files locationedit

Elasticsearch has three configuration files:

* elasticsearch.yml for configuring Elasticsearch
* jvm.options for configuring Elasticsearch JVM settings
* log4j2.properties for configuring Elasticsearch logging

These files are located in the config directory, whose default location depends on whether or not the installation is from an archive distribution (tar.gz or zip) or a package distribution (Debian or RPM packages).

For the archive distributions, the config directory location defaults to $ES_HOME/config. The location of the config directory can be changed via the ES_PATH_CONF environment variable as follows:
````
ES_PATH_CONF=/path/to/my/config ./bin/elasticsearch
````
Alternatively, you can export the ES_PATH_CONF environment variable via the command line or via your shell profile.

For the package distributions, the config directory location defaults to /etc/elasticsearch. The location of the config directory can also be changed via the ES_PATH_CONF environment variable, but note that setting this in your shell is not sufficient. Instead, this variable is sourced from /etc/default/elasticsearch (for the Debian package) and /etc/sysconfig/elasticsearch (for the RPM package). You will need to edit the ES_PATH_CONF=/etc/elasticsearch entry in one of these files accordingly to change the config directory location.
