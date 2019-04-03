---
layout: sublevel
title: Hands-On Elasticsearch
---

## First Queries

First of all, [start  here](https://search-nddg-elasticsearch-fun-tofdyfevowxw3uvg6jyaml5brq.us-west-2.es.amazonaws.com/_plugin/kibana/app/kibana#/discover?_g=(refreshInterval:(pause:!t,value:0),time:(from:'2015-05-18T05:00:00.000Z',mode:absolute,to:'2015-05-21T04:59:59.999Z'))&_a=(columns:!(_source),index:cc172f40-5427-11e9-95d6-8d2558177170,interval:auto,query:(language:lucene,query:'*'),sort:!('@timestamp',desc))).

### Full text search

*Kibana*:
  
    error

*Query*: 

<pre>
GET logstash-*/_search
{
  "query": {
    "bool": {
      "must": [
        { "multi_match": 
          { "query": "error" }
        }
      ]
    }
  }
}
</pre>

*Task*: Try to search for "media". Then search for "medi". What does this tell us?

### Specify a field

*Kibana*:

    @tags: error

*Query*: 

<pre>
GET logstash-*/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": 
          { "@tags": "error" }
        }
      ]
    }
  }
}
</pre>


### And-ing Things

*Kibana*:

    @tags: error && machine.os: ios

*Query*:

<pre>
GET logstash-*/_search
{
  "query": {
    "bool": {
      "must": [
        { "match":{ "@tags": "error" } },
        { "match": { "machine.os": "ios" } }
      ]
    }
  }
}
</pre>

### NOT

*Kibana*:

    @tags: error && -machine.os: ios

*Query*:
 
<pre>
GET logstash-*/_search
{
  "query": {
    "bool": {
      "must": [
        { "match":{ "@tags": "error" } }
      ],
      "must_not": [
        { "match": { "machine.os": "ios" } }
      ]
    }
  }
}
</pre>


### Cheating

If you get stuck and can't figure out how to convert the Lucene syntax used in Kibana to the json syntax of the REST api, you can cheat like this:

*Kibana*:

    @tags: error && (machine.os: ios || machine.os: "osx")

*Query*:

<pre>
GET logstash-*/_search
{
  "query": {
    "bool": {
        "must": [
            {
                "query_string" : {"query":"@tags: error && (machine.os: ios || machine.os: \"osx\")"}
            }
        ]
    }
  }
}
</pre>





