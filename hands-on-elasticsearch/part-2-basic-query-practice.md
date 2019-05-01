---
layout: sublevel
title: Hands-On Elasticsearch - Part 2
---

## Easy Query

*Kibana*:
  
    Mozilla

*Query*: 

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        { "multi_match": 
          { "query": "Mozilla" }
        }
      ]
    }
  }
}
</pre>




## Range Query

*Kibana*:
  
    Mozilla

*Query*: 

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        { "multi_match": 
          { "query": "Mozilla" }
        },
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
  }
}
</pre>

*Task*: Search for "error"

## Basic "And" Query

*Kibana*:
  
    error && osx

*Query*: 

<pre>
GET kibana_sample_data_logs/_search
{
  "query": {
    "bool": {
      "must": [
        { "multi_match": 
          { "query": "error" }
        },
        { "multi_match": 
          { "query": "osx" }
        },
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
  }
}
</pre>


*Task*: Search for 503 response and security tag (4 min)


## Screencast

<p>Screencast going over these exercises.</p>

<video src="https://s3-us-west-2.amazonaws.com/nddg-vids/elasticsearch/basic-query-practice.mp4" style="max-width: 60%;" controls></video>





