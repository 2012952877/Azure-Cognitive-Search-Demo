### Create Index

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