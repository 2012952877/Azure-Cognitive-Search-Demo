### Type Ahead (Suggester)

#### a. Concept

A suggester is an internal data structure that supports search-as-you-type behaviours by storing prefixes for matching on partial queries. As with tokenized terms, prefixes are stored in inverted indexes, one for each field specified in a suggester fields collection.

![image-20200924115532061](https://i.loli.net/2020/09/25/HThvtWRjzx34kqw.png)

#### b. Create suggester

Do you remember when you create your index in the earlier step. You may already have a suggester called "sg1".

![image-20200924110537946](https://i.loli.net/2020/09/25/4bZFkaJHjYi3Xw5.png)

Then you have two way to map this suggester for your index:

1. Use Postman
2. Manual modification

For option 1, please check this code and make a little change for your index defination.

```json
{
  "name": "hotels-sample-index",
  "fields": [
    . . .
        {
            "name": "HotelName",
            "type": "Edm.String",
            "facetable": false,
            "filterable": false,
            "key": false,
            "retrievable": true,
            "searchable": true,
            "sortable": false,
            "analyzer": "en.microsoft",
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "synonymMaps": [],
            "fields": []
        },
  ],
  "suggesters": [
    {
      "name": "sg",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["HotelName"]
    }
  ],
  "scoringProfiles": [
    . . .
  ]
}
```

For option 2, please open your Index Definition and change the following part:

![image-20200924120224080](https://i.loli.net/2020/09/25/qUdWcluMBh9DZCx.png)

I set type ahead function for "title" field, you can also change it to other field.

Now you may notice this gonna be work.