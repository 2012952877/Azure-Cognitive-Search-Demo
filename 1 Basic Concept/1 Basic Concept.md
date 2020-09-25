### 1 Basic concept

Azure Cognitive Search ([formerly known as "Azure Search"](https://docs.microsoft.com/en-au/azure/search/whats-new#new-service-name)) is a cloud search service that gives developers APIs and tools for building a rich search experience over private, heterogeneous content in web, mobile, and enterprise applications.

When you create a Cognitive Search service, you get a search engine that performs indexing and query execution, persistent storage of indexes that you create and manage, and a query language for composing simple to complex queries. Optionally, a search service integrates with other Azure services in the form of *indexers* that automate data ingestion/retrieval from Azure data sources, and *skillsets* that incorporate consumable AI from Cognitive Services, such as image and text analysis, or custom AI that you create in Azure Machine Learning or wrap inside Azure Functions.

![Azure Cognitive Search architecture](https://docs.microsoft.com/en-au/azure/search/media/search-what-is-azure-search/azure-search-diagram.svg)

Architecturally, a search service sits in between the external data stores that contain your un-indexed data, and a client app that sends query requests to a search index and handles the response. An index schema determines the structure of searchable content.

The two primary workloads of a search service are *indexing* and *querying*.

- Indexing brings text into to your search service and makes it searchable. Internally, inbound text is processed into tokens and stored in inverted indexes for fast scans. During indexing, you have the option of adding *cognitive skills*, either predefined ones from Microsoft or custom skills that you create. The subsequent analysis and transformations can result in new information and structures that did not previously exist, providing high utility for many search and knowledge mining scenarios.
- Once an index is populated with searchable data, your client app sends query requests to a search service and handles responses. All query execution is over a search index that you create, own, and store in your service. In your client app, the search experience is defined using APIs from Azure Cognitive Search, and can include relevance tuning, autocomplete, synonym matching, fuzzy matching, pattern matching, filter, and sort.

Functionality is exposed through a simple [REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/) or [.NET SDK](https://docs.microsoft.com/en-au/azure/search/search-howto-dotnet-sdk) that masks the inherent complexity of information retrieval. You can also use the Azure portal for service administration and content management, with tools for prototyping and querying your indexes and skillsets. Because the service runs in the cloud, infrastructure and availability are managed by Microsoft.

**For more details:** https://docs.microsoft.com/en-au/azure/search/search-what-is-azure-search