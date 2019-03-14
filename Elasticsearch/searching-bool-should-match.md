```console
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "name": "pregnant"
          }
        },
        {
          "match_phrase": {
            "detail.content": "milk powder"
          }
        }
      ]
    }
  },
  "sort": [
    { "_ts" : "desc" }
  ]
}
```