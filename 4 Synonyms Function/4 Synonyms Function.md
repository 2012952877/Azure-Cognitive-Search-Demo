### Synonyms Function

#### a. Concept

Synonyms in search engines associate equivalent terms that implicitly expand the scope of a query, without the user having to actually provide the term. For example, given the term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within the scope of the query.

In Azure Cognitive Search, synonym expansion is done at query time. You can add synonym maps to a service with no disruption to existing operations. You can add a **synonymMaps** property to a field definition without having to rebuild the index.

#### b. Create synonymMaps

There is no portal support for creating synonyms but you can use the REST API or .NET SDK. To get started with REST, we recommend [using Postman](https://docs.microsoft.com/en-us/azure/search/search-get-started-postman) and formulation of requests using this API: [Create Synonym Maps](https://docs.microsoft.com/en-us/rest/api/searchservice/create-synonym-map). For C# developers, you can get started with [Add Synonyms in Azure Cognitive Searching using C#](https://docs.microsoft.com/en-us/azure/search/search-synonyms-tutorial-sdk).

We will use Postman for this demo. If you have any question about Postman, please check this link: https://docs.microsoft.com/en-us/azure/search/search-get-started-postman

Here are some code for Postman:

```http
POST https://[servicename].search.windows.net/synonymmaps?api-version=2020-06-30
    api-key: [admin key]

{   
    "name" : "synonymmap1",  
    "format" : "solr",
    "synonyms" : "wechat, 微信, weixin, Wechat, WECHAT, TTTTTT => wechat"
}
```

"TTTTTT" will be used for check the synonymMaps is correctly build. “Format” only support "solr" now.

If you want to update this, please use:

```http
PUT https://[servicename].search.windows.net/synonymmaps/[synonymmap name]?api-version=2020-06-30  
  Content-Type: application/json  
  api-key: [admin key]
  
  {   
    anything you want update
    }
```

For Get and Delete:

```http
GET https://[servicename].search.windows.net/synonymmaps?api-version=2020-06-30
    api-key: [admin key]
```

```http
DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2020-06-30
    api-key: [admin key]
```

#### c. Map synonymMaps to our index

Then we are going to map the created synonymmap1 to our index.

You have two to finish this step:

1. use Postman
2. Manual modification

For option 1, please read this code and change a little for your own index:

```http
POST https://[servicename].search.windows.net/indexes?api-version=2020-06-30
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }
```

For option 2, please open your index definition and change a little. You can set it for title, key word and content for this demo.

![image-20200924113857618](https://i.loli.net/2020/09/24/YojRxOMUzq7mwQb.png)

Then it gonna be work. Now we search for "TTTTTT" will also get same results as "wechat" returned.

![image-20200924114042534](https://i.loli.net/2020/09/24/tL4wdZFCQz9yix1.png)