### Aggregations

Below are the aggregation examples for [SampleData](project-wiki/SampleData.json)

###### Get Average of All Transactions

`````
GET /transaction-analytics/transaction/_search
{
  "aggregations":{
    "avg_amount":{"avg":{"field":"amount"}}
  }
}
`````


###### Sum of Amounts for date range

````
Sum of all the amount for a date range:

GET /transaction-analytics/transaction/_search
{
  "aggs": {
    "date_ranges":{
   "range":{
     "field":"createdDate",
     "ranges": [
      { "from": "2018-01-01T02:52:24.000"},
       {"to": "2019-05-23T02:59:18.423"}
     ]
   },
   "aggs":{
     "agg_partners":{
       "sum":{"field":"amount"}
     }
   }
  }
}
  }
````

###### Get Records grouped by partner name

```
GET /transaction-analytics/transaction/_search
{
  "aggs": {
    "date_ranges":{
   "range":{
     "field":"createdDate",
     "ranges": [
      { "from": "2018-01-01T02:52:24.000"},
       {"to": "2019-05-23T02:59:18.423"}
     ]
   },
   "aggs":{
     "aggs_partners":{
       "terms":{"field":"partnerKey.keyword"}
     }
   }
  }
}
  
}
````
###### Get cumulative revenue of all partners

````
{
  "aggs": {
    "date_ranges":{
   "range":{
     "field":"createdDate",
     "ranges": [
      { "from": "2018-01-01T02:52:24.000"},
       {"to": "2019-05-23T02:59:18.423"}
     ]
   },
   "aggs":{
     "aggs_partners":{
       "terms":{"field":"partnerKey.keyword"},
       
       "aggs":{
         "aggs_revenue":{
           "sum":{"field": "amount"}
         }
       }
     }
   }
  }
}
  }

````

###### Get cumulative revenue of all partners in descending order:

```
GET /transaction-analytics/transaction/_search
{
  "aggs": {
    "date_ranges":{
   "range":{
     "field":"createdDate",
     "ranges": [
      { "from": "2018-01-01T02:52:24.000"},
       {"to": "2019-05-23T02:59:18.423"}
     ]
   },
   "aggs":{
     "aggs_partners":{
       "terms":{
         "field":"partnerName.keyword",
         "size": 10, 
         "order":{"aggs_revenue":"desc"}
       },
       
       "aggs":{
         "aggs_revenue":{
           "sum":{"field": "amount"}
         }
       }
     }”
   }
  }
}
  }
```
