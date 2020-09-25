# Azure Cognitive Search Demo

![image-20200924101620844](https://i.loli.net/2020/09/24/YrZRMT8tjlNP3dn.png)

## About this demo

Welcome to the Azure Cognitive Search Demo! This demo contains a basic web front end to create a view of your azure cognitive search results.  Use this Demo application to jump-start your understanding and development efforts with your own data to better connect with such a distinguished technology. We've infused best practices throughout the documentation to help guide you step by step. With Cognitive Search, you can easily index your data with Cosmos database for search and filter, and other enrichment skills including Synonym customization, type ahead and scoring search.

Once you finished, you will have a web app ready to search for data like this:

![image-20200924102747313](https://i.loli.net/2020/09/24/QeEBJURnwtou4KD.png)

## Pre-requisites

### 0 Visual Studio IDE

Please follow this tutorial to set up your IDE in local machine.

https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2019

### 1 Azure portal account

https://docs.microsoft.com/en-us/azure/azure-portal/

### 2 Postman

Please follow this tutorial to install Postman.

https://www.postman.com/downloads/

- **Optional**
  - https://docs.microsoft.com/en-us/azure/search/search-get-started-postman

## Process overview

### 0 Basic concept

Azure Cognitive Search ([formerly known as "Azure Search"](https://docs.microsoft.com/en-au/azure/search/whats-new#new-service-name)) is a cloud search service that gives developers APIs and tools for building a rich search experience over private, heterogeneous content in web, mobile, and enterprise applications.

When you create a Cognitive Search service, you get a search engine that performs indexing and query execution, persistent storage of indexes that you create and manage, and a query language for composing simple to complex queries. Optionally, a search service integrates with other Azure services in the form of *indexers* that automate data ingestion/retrieval from Azure data sources, and *skillsets* that incorporate consumable AI from Cognitive Services, such as image and text analysis, or custom AI that you create in Azure Machine Learning or wrap inside Azure Functions.

![Azure Cognitive Search architecture](https://docs.microsoft.com/en-au/azure/search/media/search-what-is-azure-search/azure-search-diagram.svg)

Architecturally, a search service sits in between the external data stores that contain your un-indexed data, and a client app that sends query requests to a search index and handles the response. An index schema determines the structure of searchable content.

The two primary workloads of a search service are *indexing* and *querying*.

- Indexing brings text into to your search service and makes it searchable. Internally, inbound text is processed into tokens and stored in inverted indexes for fast scans. During indexing, you have the option of adding *cognitive skills*, either predefined ones from Microsoft or custom skills that you create. The subsequent analysis and transformations can result in new information and structures that did not previously exist, providing high utility for many search and knowledge mining scenarios.
- Once an index is populated with searchable data, your client app sends query requests to a search service and handles responses. All query execution is over a search index that you create, own, and store in your service. In your client app, the search experience is defined using APIs from Azure Cognitive Search, and can include relevance tuning, autocomplete, synonym matching, fuzzy matching, pattern matching, filter, and sort.

Functionality is exposed through a simple [REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/) or [.NET SDK](https://docs.microsoft.com/en-au/azure/search/search-howto-dotnet-sdk) that masks the inherent complexity of information retrieval. You can also use the Azure portal for service administration and content management, with tools for prototyping and querying your indexes and skillsets. Because the service runs in the cloud, infrastructure and availability are managed by Microsoft.

**For more details:** https://docs.microsoft.com/en-au/azure/search/search-what-is-azure-search

### 1 Deploy resource

#### a. Deploy search service

First, you need to register and log in to your azure portal.

![image-20200924103739014](https://i.loli.net/2020/09/24/9ns7S18kzpVKo53.png)

Click "Create a resource", then choose azure cognitive search and fill in some information, such as search service url and pricing tier.

![image-20200924103903695](https://i.loli.net/2020/09/24/ThAaJmtgi3IUMYD.png)

![image-20200924104151289](https://i.loli.net/2020/09/24/VtoAQEKBhIDj5pJ.png)

#### b. Deploy database

As an example, we use Cosmos database to store our sample data.

Similarly, find and create a Cosmos database in the portal.

![image-20200924104435486](https://i.loli.net/2020/09/24/RLJwSqg57TciG6H.png)

You also need to fill in some basic information, please make sure use Core(SQL) API and put your database in the same location as your search service.

![image-20200924104541280](https://i.loli.net/2020/09/24/7lP3i1cETFHJ52j.png)

### 2 Create index

#### a. Import data to your Cosmos database

First, create a container for your database.

![image-20200924105016307](https://i.loli.net/2020/09/24/tPRUd4c8X1lYghT.png)

You can insert large amounts of data by directly using code. Or if you only have small amount of dataset, use the data explorer shown in the figure to manually insert the data will be more convenience.

![image-20200924105151409](https://i.loli.net/2020/09/24/tsE2DWp1q6FnKxZ.png)

Here are some data for you.

```json
{
    "id": "1",
    "title" : "企业微信平台注册流程步骤",
    "keyword": ["企业微信","wechat","企微","nsb企业微信","企业微信注册","注册企业微信","企业微信平台注册"],
    "kbnumber": "KBA00040516",
    "content": "微信关注企业微信号（简化版）。Follow the Enterprise Official Account through WeChat (simplified version)1.扫描二维码关注企业微信号Scan the QR code and follow the Enterprise Official Account2.点击下图图文进行身份验证并成功关注诺基亚贝尔人Click the picture for authentication and follow the 诺基亚贝尔人 successfully3.输入邮箱进行身份验证Enter the Nokia E-mail address for authentication4.输入邮箱验证码点击验证Enter the verification code for authentication5.关注成功Followed Successfully"
}

{
    "id": "2",
    "title" : "IT资产归还流程",
    "keyword": ["离职归还","资产归还","PC归还"],
    "kbnumber": "KBA00017103",
    "content": "电脑归还给IT的说明。离职员工必须将Office PC交还给当地IT。没有常驻IT的分公司,请联系上海地区IT,工程师会联系EMS前去公司上门取件回收PC。（如果离职时间有限也可先行移交给分公司行政同事，由其帮忙代为操作）。此资源会帮助解决该部门新员工的需求。非正式员工PC，员工本人离职的须将该非正式员工PC交还给当地IT（没有常驻IT的分公司，请联系上海地区IT，工程师会联系EMS前去公司上门取件回收PC。如果离职时间有限也可先行移交给分公司行政同事，由其帮忙代为操作）。如非正式员工本人未离职但是其直接主管已离职，须请接任的新主管去IT Service Portal中“IT 服务 → IT 服务申请 → 非正式员工帐号/PC提交非正式员工PC转移将该PC转移至新主管名下。研发PC不用退还IT。"
}

{
    "id": "3",
    "title" : "IT资产归还流程修订版",
    "keyword": ["备份","指南","注意"],
    "kbnumber": "KBA00036716",
    "content": "问题：PC更新、PC重装使用备份空间进行数据备份时需要注意哪些事项。解决方案：IT为需要重装PC操作系统或更换PC的用户提供临时网络数据备份空间。备份空间最大400G，从申请提交之日起，请在10个工作日以内尽快将数据迁移至本地PC。此备份空间仅限本人账号使用，无法共享。申请网址：IT Service Portal→  IT 服务 → PC、软件和打印 → 重装数据备份空间。备份空间注意事项： 1. 备份前请先确认需要备份的数据，您只需备份自己的个人数据，备份结束后，请仔细检查数据，以免有遗漏；2. 请在备份数据前关闭相关应用程序（比如备份邮件时，请先把outlook关掉）；3. 请不要同时使用有线和无线网络进行数据上传，以避免网络信号冲突影响上传速度；4. 由于拷贝数据时间会很长，建议用户在使用外接电源的情况下拷贝数据，以避免数据丢失；5. 空间大小400G，数据保存期限为10天，10天后数据将被自动删除，请用户及时关注空间数据存储期限及时下载数据，避免数据丢失；6. 请保管好备份路径的链接，以免下载数据时找不到路径；"
}

{
    "id": "4",
    "title" : "企业Wechat平台注册流程步骤",
    "keyword": ["企业微信","wechat","企微","nsb企业微信","企业微信注册","注册企业微信","企业微信平台注册"],
    "kbnumber": "KBA00040516",
    "content": "微信关注企业微信号（简化版）。Follow the Enterprise Official Account through WeChat (simplified version)1.扫描二维码关注企业微信号Scan the QR code and follow the Enterprise Official Account2.点击下图图文进行身份验证并成功关注诺基亚贝尔人Click the picture for authentication and follow the 诺基亚贝尔人 successfully3.输入邮箱进行身份验证Enter the Nokia E-mail address for authentication4.输入邮箱验证码点击验证Enter the verification code for authentication5.关注成功Followed Successfully"
}
```

#### b. Create index

When you complete the previous step, there should be four json-formatted data in your database. 

Now you can enter the search service created earlier and click "import data".

![image-20200924105654084](https://i.loli.net/2020/09/24/pwHSdOZq1yelxvj.png)



Select your database.

![image-20200924105900756](https://i.loli.net/2020/09/25/JzDFkfTO2cQd6GA.png)



Add cognitive skills is optional, **we skill this for now**. 

(Basically, cognitive skills (AI enrichment) is an extension of [indexers](https://docs.microsoft.com/en-us/azure/search/search-indexer-overview) that can be used to extract text from images, blobs, and other unstructured data sources. Enrichment and extraction make your content more searchable in indexer output objects, either a [search index](https://docs.microsoft.com/en-us/azure/search/search-what-is-an-index) or a [knowledge store](https://docs.microsoft.com/en-us/azure/search/knowledge-store-concept-intro). For more information about cognitive skills, please check: https://docs.microsoft.com/en-us/azure/search/cognitive-search-concept-intro)

![image-20200924110043605](https://i.loli.net/2020/09/24/FWlLGI9bUY271D6.png)

Next is the specific setting part of the index. It is recommended to fill in and record the following parts:

- index name
- Key: the unique document identifier. It's always a string, and it is required.
- Suggester name: use for type ahead, we will cover more details for this later, you can set it for "sg1" now.
- Search mode: only support "analyzingInfixMatching" for now.

![image-20200924110537946](https://i.loli.net/2020/09/24/H4doDaLY2s15wpO.png)

Data operability:

- Retrievable: it shows up in search results list. You can mark individual fields as off limits for search results by clearing this checkbox, for example for fields used only in filter expressions.
- Filterable, Sortable, Facetable: determine whether fields are used in a filter, sort, or faceted navigation structure.
- Searchable: a field is included in full text search. Strings are searchable. Numeric fields and Boolean fields are often marked as not searchable.
- Storage requirements do not vary as a result of your selection. For example, if you set the Retrievable attribute on multiple fields, storage requirements do not go up.

By default, the wizard scans the data source for unique identifiers as the basis for the key field. Strings are attributed as Retrievable and Searchable. Integers are attributed as Retrievable, Filterable, Sortable, and Facetable.

When you complete the above steps, your index should look like this.

![image-20200924111645202](https://i.loli.net/2020/09/24/6KpHwZehrCOA2o8.png)

After the index is created, you need to wait a while for the data to be successfully loaded into the search service. After done, this search service can get some search results. You can view all return json data here.

![image-20200924111911312](https://i.loli.net/2020/09/24/XEVIQ9oOj6rk7mB.png)

**For more details:** https://docs.microsoft.com/en-us/azure/search/search-get-started-portal

### 3 Synonyms function

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

### 4 Scoring profile (Weighted search)

#### a. Concept

*Scoring* computes a search score for each item in a rank ordered result set. Every item in a search result set is assigned a search score, then ranked highest to lowest.

Azure Cognitive Search uses default scoring to compute an initial score, but you can customize the calculation through a *scoring profile*. Scoring profiles give you greater control over the ranking of items in search results. For example, you might want to boost items based on their revenue potential, promote newer items, or perhaps boost items that have been in inventory too long.

**For more details:** https://docs.microsoft.com/en-us/azure/search/index-add-scoring-profiles

#### b. Create your scoring for index

In azure portal, select "Scoring profiles" 

![image-20200924114622933](https://i.loli.net/2020/09/24/8RciLKgtOmBqau6.png)

Then set weight number for each fields.

![image-20200924114940700](https://i.loli.net/2020/09/24/lWcF9AN8uGboIfk.png)

The final step is check your index definition, make sure these information are listed.

![image-20200924115136090](https://i.loli.net/2020/09/25/pM76tAa8mnVIdlW.png)

### 5 Type ahead (Suggester)

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

## Web application setting

After setting up your azure search service, a usable interface will be provided to you. Now all you need to do is to open the demo and fill in the interface information. Please open the demo with visual studio and find the appsetting.json file.

![image-20200924143611351](https://i.loli.net/2020/09/25/wujYtI285Np97eq.png)

### 0 Required fields

```js
 // Required fields
  "SearchServiceName": "",
  "SearchApiKey": "",
  "SearchIndexName": "",
  "SearchIndexerName": "",
  "StorageAccountName": "",
  "StorageAccountKey": "",
  "StorageContainerAddress": "https://{storage-account-name}.blob.core.windows.net/{container-name}",
  "KeyField": "metadata_storage_path",
  "IsPathBase64Encoded": true,
```

1. **SearchServiceName** - The name of your Azure Cognitive Search service
2. **SearchApiKey** - The API Key for your Azure Cognitive Search service
3. **SearchIndexName** - The name of your Azure Cognitive Search index
4. **SearchIndexerName** - The name of your Azure Cognitive Search indexer
5. **StorageAccountName** - The name of your Azure Blob Storage Account
6. **StorageAccountKey** - The key for your Azure Blob Storage Account
7. **StorageContainerAddress** - The URL to the storage container where your documents are stored. This should be in the following format: *https://*storageaccountname*.blob.core.windows.net/*containername**
8. **KeyField** - They key field for your search index. This should be set to the field specified as a key document Id in the index. By default this is *metadata_storage_path*.
9. **IsPathBase64Encoded** - By default, metadata_storage_path is the key, and it gets base64 encoded so this is set to true by default. If your key is not encoded, set this to false.

### 1 Optional fields

While some fields are optional, we recommend not removing them from *appsettings.json* to avoid any possible errors.

```json
  // Optional instrumentation key
  "InstrumentationKey": "",

  // Optional container addresses if using more than one indexer:
  "StorageContainerAddress2": "https://{storage-account-name}.blob.core.windows.net/{container-name}",
  "StorageContainerAddress3": "https://{storage-account-name}.blob.core.windows.net/{container-name}",

  // Optional key to an Azure Maps account if you would like to display the geoLocation field in a map
  "AzureMapsSubscriptionKey": "",

  // Set to the name of a facetable field you would like to represent as a graph.
  // You may also set to a comma separated list of the facet names if you would like more than one facet type on the graph.
  "GraphFacet": "keyPhrases, locations",

  // Additional Customizations
  "Customizable": "true",
  "OrganizationName": "Microsoft",
  "OrganizationLogo": "~/images/logo.png",
  "OrganizationWebSiteUrl": "https://www.microsoft.com"

```

1. **InstrumentationKey** - Optional instumentation key for Application Insights. The instrumentation key connects the web app to Application Inisghts in order to populate the Power BI reports.
2. **StorageContainerAddress2** & **StorageContainerAddress3** - Optional container addresses if using more than one indexer
3. **AzureMapsSubscriptionKey** - You have the option to provide an Azure Maps account if you would like to display a geographic point in a map in the document details. The code expects a field called *geolocation* of type Edm.GeographyPoint. If your wish to change this behavior (for instance if you would like to use a different field), you can modify details.js.

![geolocation](https://i.loli.net/2020/09/25/rRcIw6B1ESDts3f.png)

1. **GraphFacet** - The GraphFacet is used for generating the relationship graph. This can now be edited in the UI.
2. **Customizable** - Determines if user is allowed to *customize* the web app. Customizations include uploading documents and changing the colors/logo of the web app. **OrganizationName**,  **OrganizationLogo**, and **OrganizationWebSiteUrl** are additional fields that also allow you to do light customization.

### 2 UI folder

Much of the UI is rendered dynamically by javascript. Some important files to know when making changes to the UI are:

1. **wwroot/js/results.js** - contains the code used to render search results on the UI
2. **wwroot/js/details.js** - contains the code for rending the detail view once a result is selected
3. **Search/DocumentSearchClient.cs** - contains the code for talking with Azure Cognitive Search's APIs. Setting breakpoints in this file is a great way to debug.