# Elasticsearch


```
jq -j . *.json > combined.json
```

```
jq -c . combined.json | sed 'i{"index":{}}' > bulk.json
```
```
curl -X DELETE -k -u elastic:airbnb "https://localhost:9200/sport/"
```
```
curl -s -X POST -k -u elastic:airbnb "https://localhost:9200/sport/_bulk" -H 'Content-Type: application/json' --data-binary @bulk.json
```




Example for a query:

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
