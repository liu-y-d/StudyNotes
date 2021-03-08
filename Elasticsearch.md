[开源搜索：Elasticsearch、ELK Stack 和 Kibana 的开发者 | Elastic](https://www.elastic.co/cn/)

# Elasticsearch简介

全文搜索属于最常见的需求，开源的Elasticsearch是目前全文搜索引擎的首选。它可以快速地存储、搜索和分析海量数据。维基百科、Stack Overflow、Github都采用它。

Elastic的底层是开源库Lucene。但是，你没法直接用Lucene，必须自己写代码去调用它的接口。Elastic是Lucene的封装，提供了REST API的操作接口，开箱即用。

基本概念

# 基本概念

1. Index（索引）

当做动词，相当于MySQL中的insert

当做名次，相当于MySQL中的Database

1. Type（类型）

在Index（索引）中，可以定义一个或多个类型。类似于MySQL中的Table表。每一种类型的数据放在一起。

1. Document（文档）

保存在某个索引（Index）下，某种类型（Type）的一个数据（Document），文档是JSON格式的，Document就像是MySQL中的某个Table的内容。

1. 倒排索引

Elasticsearch 使用的是一种名为_倒排索引_的数据结构，这一结构的设计可以允许十分快速地进行全文本搜索。倒排索引会列出在所有文档中出现的每个特有词汇，并且可以找到包含每个词汇的全部文档。

在索引过程中，Elasticsearch 会存储文档并构建倒排索引，这样用户便可以近实时地对文档数据进行搜索。索引过程是在索引 API 中启动的，通过此 API 您既可向特定索引中添加 JSON 文档，也可更改特定索引中的 JSON 文档。

# Docker安装

1. 下载镜像文件

```PowerShell
docker pull elasticsearch:7.4.2
// 可视化检索工具
docker pull kibana:7.4.2 
```

1. 创建实例

1. Elasticsearch

```Bash
mkdir -p /mydata/elasticsearch/config
mkdir -p /mydata/elasticsearch/data
echo "http.host: 0.0.0.0">>/mydata/elasticsearch/config/elasticsearch.yml 


docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m -Xmx128m" \
-v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /mydata/elasticsearch/data:/usr/share/elasticsearch/data \
-v /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.4.2 
 


```

特别注意：

-e ES_JAVA_OPTS="Xms64m -Xmx128m" 测试环境下，设置ES的初始内存和最大内存，否则导致过大启动不了ES

修改data、config、plugins目录的权限

```Bash
chmod -R 777 /mydata/elasticsearch/ 
```

出现下图则启动成功

![](https://secure-static.wolai.com/static/wDUEmzgSfJVD3ewa8oHSkW/image.png)

1. Kibana

```Bash
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://192.168.56.10:9200 -p 5601:5601 \
-d kibana:7.4.2 
```

启动成功

![](https://secure-static.wolai.com/static/8eucSoW5RLJ5v9NaH8Em26/image.png)

# Elasticsearch笔记