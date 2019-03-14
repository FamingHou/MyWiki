```console
{
  "query": {
    "multi_match": {
      "query": "milk powder",
      "fields": [
      	"name", 
      	"detail.content"
      ],
      "type": "phrase"
    }
  },
  "sort": [
    { "_ts" : "desc" }
  ]
}
```