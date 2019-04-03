---
layout: sublevel
title: Hands-On Elasticsearch
---

## Bank Account Data

### Range Query

*Kibana*:
  
    balance: [30000 TO *]

*Query*: 

<pre>
GET bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "range": 
          { "balance": { "gte": "30000" } }
        }
      ]
    }
  }
}
</pre>

*Task*: Find all bank accounts that have a balance of less than $10,000.

*Task*: Find all bank accounts that have a balance of less than $10,000 and are in Texas.

*Task*: Find all bank accounts that have a balance of less than $10,000, are in Texas, and the age of bank account holder is between 20 and 30.


