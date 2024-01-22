# Elasticsearch

During the course of Information Retrieval, I made a project which contains the using of Elasticsearch.

<b>Elasticsearch</b> is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data for lightning fast search, fine tuned relevancy, and powerful analytics that scale with ease.


In this Readme Data I will demonstrate how to create some tools. I will analyse a text dataset, which contains 101 BBC-sports reports on athletics topics during the years 2004-2006.
For example, we can run a query looking for a phrase in this text.

### Converting the files into JSON

I used a program to convert every text file to a JSON, with 4 main fields: ID, Title, labels and text [i.e. content].

Using <b>Named Entity Recognition (NER)</b> we can label named “real-world” objects, like persons, companies or locations. The tool I used is called spaCy. <b>spaCy</b> is a free, open-source library for advanced Natural Language Processing (NLP) in Python. It can be used to build information extraction or natural language understanding systems, or to pre-process text for deep learning.

We will use Linux commands and therefore it is necessary to obtain a Linux environment (e.g. Cygwin).


For running a small scale of queries, I used Kibana dev tools. The <b>Dev tools</b> of <b>Kibana</b> provide a powerful way to interact with the ElasticStack. As it includes Console that supports developers to write Elasticsearch commands in one tab and view those commands in the different tab. Together with Console, a Grok debugger and a search profiler in this solution allow you to configure the app to meet your needs.

### Install Elasticsearch with Docker

Elasticsearch is provided through container format: 
<b>Images</b> are available for running Elasticsearch as <b>Docker containers</b>. They may be downloaded from the <b>Elastic Docker Registry</b>.

Docker Images für Elasticsearch & Kibana download:

```
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.11.0
```
```
docker pull docker.elastic.co/kibana/kibana:8.11.0
```

```
docker run --name es01 --net elastic -p 9200:9200 /
-it -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms2g -Xmx2g" /
docker.elastic.co/elasticsearch/elasticsearch:8.11.0
```

Here we should take a note of password and the Enrollment token.

Test Elasticsearch:

Copy a certificate file http_ca.crt . from container to the local folder
```
curl --cacert http_ca.crt -u elastic:$ELASTIC_PASSWORD https://localhost:9200
```
```
docker run --name kib01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.11.0 
```

It is possible to log in the kibana and change the password.









```
docker network create elastic
```

After we converted all the files with spaCy labels, we will join all 101 JSON files into JSON.
In order to apply operations on JSON files I installed <b>./jq</b>. jq is a very high-level lexically scoped functional programming language in which every JSON value is a constant.
```
jq -j . *.json > combined.json
```

After combining into JSON file, we must create a bulk file.
```
jq -c . combined.json | sed 'i{"index":{}}' > bulk.json
```


(necessary to delete index)
```
curl -X DELETE -k -u elastic:airbnb "https://localhost:9200/sport/"
```

We then have to name the index [`sport`]. Using port 9200, it is important to note that we must use the secure protocol – HTTPS and not HTTP.
```
curl -s -X POST -k -u elastic:airbnb "https://localhost:9200/sport/_bulk" -H 'Content-Type: application/json' --data-binary @bulk.json
```




Now we can a run a query:

```
curl -X POST -k -u elastic:airbnb "https://localhost:9200/sport/_search?pretty" -H "Content-Type: application/json" -d'
{
        "size": 200, "_source" : ["id","title"],
        "query":{
                "query_string" : {
                        "fields" : ["text"],
                        "query" : "+drugs"
                }
        },
        "sort": [{"id": "asc"}]
}
'
```

Query's results:
```
{
  "took" : 8,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 28,
      "relation" : "eq"
    },
    "max_score" : 2.3512743,
    "hits" : [
      {
        "_index" : "sport",
        "_id" : "0CQxkowBFxO5aAdXATBa",
        "_score" : 2.3512743,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "079",
          "title" : "Wada will appeal against ruling"
        }
      },
      {
        "_index" : "sport",
        "_id" : "myQxkowBFxO5aAdXATBZ",
        "_score" : 2.3450394,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "026",
          "title" : "Collins appeals against drugs ban"
        }
      },
      {
        "_index" : "sport",
        "_id" : "qCQxkowBFxO5aAdXATBZ",
        "_score" : 2.3450394,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "039",
          "title" : "Jones medals 'must go if guilty'"
        }
      },
      {
        "_index" : "sport",
        "_id" : "pCQxkowBFxO5aAdXATBZ",
        "_score" : 2.2861907,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "035",
          "title" : "Collins banned in landmark case"
        }
      },
      {
        "_index" : "sport",
        "_id" : "siQxkowBFxO5aAdXATBZ",
        "_score" : 2.0930693,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "049",
          "title" : "Jones files Conte lawsuit"
        }
      },
      {
        "_index" : "sport",
        "_id" : "2SQxkowBFxO5aAdXATBa",
        "_score" : 2.0930693,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "088",
          "title" : "Jones files lawsuit against Conte"
        }
      },
      {
        "_index" : "sport",
        "_id" : "ryQxkowBFxO5aAdXATBZ",
        "_score" : 2.0610254,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "046",
          "title" : "Radcliffe eyes hard line on drugs"
        }
      },
      {
        "_index" : "sport",
        "_id" : "0SQxkowBFxO5aAdXATBa",
        "_score" : 1.9946592,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "080",
          "title" : "Greek sprinters 'won't run again'"
        }
      },
      {
        "_index" : "sport",
        "_id" : "kiQxkowBFxO5aAdXATBZ",
        "_score" : 1.9324338,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "017",
          "title" : "Call for Kenteris to be cleared"
        }
      },
      {
        "_index" : "sport",
        "_id" : "syQxkowBFxO5aAdXATBZ",
        "_score" : 1.9324338,
        "_ignored" : [
          "text.keyword"
        ],
        "_source" : {
          "id" : "050",
          "title" : "IAAF awaits Greek pair's response"
        }
      }
    ]
  }
}
```
