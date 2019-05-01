---
layout: sublevel
title: Hands-On Elasticsearch - Part 2 - Aggregates
---

## Basic Terms Aggregates

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range" : {
            "timestamp" : {
                "gte": "2019-04-28T00:00:00",
                "lt": "2019-05-01T00:00:00",
                "time_zone": "-05:00"
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "response_code": {
      "terms": {
        "field": "response.keyword"
      }
    }
  },
  "size": 0
}
</pre>

*Task*: Aggregate file extension. Create a visualization to see the data and a query to fetch the raw data.

<video src="https://s3-us-west-2.amazonaws.com/nddg-vids/elasticsearch/first-aggregation.mp4" style="max-width: 60%;" controls></video>




## Time-Based Aggregates

This one mimics the histogram in the Discover tab

<pre>

GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range" : {
              "timestamp" : {
                  "gte": "2019-04-28T00:00:00",
                  "lt": "2019-05-01T00:00:00",
                  "time_zone": "-05:00"
              }
          }
        }
      ]
    }
  },
  "aggs": {
    "log_counts": {
      "date_histogram": {
        "field": "timestamp",
        "interval": "hour"
      }
      
    }
  },
  "size": 0
}
</pre>

<video src="https://s3-us-west-2.amazonaws.com/nddg-vids/elasticsearch/time-based-aggregate.mp4" style="max-width: 60%;" controls></video>


## Nested Time-Based Aggregations

What if you wanted requests over time broken out not only by hour but also by extension?

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range" : {
              "timestamp" : {
                  "gte": "2019-04-28T00:00:00",
                  "lt": "2019-05-01T00:00:00",
                  "time_zone": "-05:00"
              }
          }
        }
      ]
    }
  },
  "aggs": {
    "log_counts": {
      "date_histogram": {
        "field": "timestamp",
        "interval": "hour"
      },
      "aggs": {
        "by_extension": {
          "terms": {
            "field": "response.keyword"
          }
        }
      }
    }
  },
  "size": 0
}
</pre>

*Task*: Instead of aggregating on response code, aggregate on extension. Create a visualization to see the data and a query to fetch the raw data.

<video src="https://s3-us-west-2.amazonaws.com/nddg-vids/elasticsearch/nested-aggregations.mp4" style="max-width: 60%;" controls></video>

## Nested Average Aggregation

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range" : {
              "timestamp" : {
                  "gte": "2019-04-28T00:00:00",
                  "lt": "2019-05-01T00:00:00",
                  "time_zone": "-05:00"
              }
          }
        }
      ]
    }
  },
  "aggs": {
    "log_counts": {
      "date_histogram": {
        "field": "timestamp",
        "interval": "hour"
      },
      "aggs": {
        "avg_bytes": {
          "avg": {
            "field": "bytes"
          }
        }
      }
    }
  },
  "size": 0
}
</pre>

<video src="https://s3-us-west-2.amazonaws.com/nddg-vids/elasticsearch/nested-average-aggregation.mp4" style="max-width: 60%;" controls></video>

## Range Aggregation

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range" : {
            "timestamp" : {
                "gte": "2019-04-28T00:00:00",
                "lt": "2019-05-01T00:00:00",
                "time_zone": "-05:00"
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "response_code": {
      "range": {
        "field": "bytes",
        "ranges": [
          { "to": 5000 },
          { "from": 5000, "to": 10000 },
          { "from": 10000, "to": 15000 },
          { "from": 15000, "to": 20000 },
          { "from": 20000 }
        ]
      }
    }
  },
  "size": 0
}
</pre>

<video src="https://s3-us-west-2.amazonaws.com/nddg-vids/elasticsearch/range-aggregation.mp4" style="max-width: 60%;" controls></video>