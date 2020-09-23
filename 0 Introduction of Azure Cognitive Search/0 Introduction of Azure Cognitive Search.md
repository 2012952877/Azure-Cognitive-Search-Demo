# 1. 什么是Azure认知搜索?

Azure 认知搜索是内置有人工智能功能的仅云搜索服务，可以扩充所有类型的信息，从而轻松地大规模标识和浏览相关内容。它原来是 Azure 搜索，使用必应和 Office 已使用了十几年的同一集成式 Microsoft 自然语言堆栈，并且在视觉、语言和语音方面使用 AI 服务。将更多的时间放在创新上，减少维护复杂的云搜索解决方案所需花费的时间。它为开发人员提供 API 和工具，以便基于 Web、移动和企业应用程序中的专用异类内容添加丰富的搜索体验。

在自定义解决方案中，搜索服务位于两个主要工作负载之间：内容引入和查询。 使用代码或工具定义架构，并调用数据引入（索引）以将索引加载到 Azure 认知搜索中。 或者，可以添加认知技能，以便在编制索引期间应用 AI 流程。 这样可以创建用于搜索和知识挖掘方案的新信息与结构。存在索引后，应用程序代码会发出对搜索服务的查询请求并处理响应。功能通过简单的 [REST API](https://docs.microsoft.com/zh-cn/rest/api/searchservice/) 或 [.NET SDK](https://docs.microsoft.com/zh-cn/azure/search/search-howto-dotnet-sdk) 公开，消除了信息检索固有的复杂性。 除了 API，Azure 门户还通过原型制作和查询索引工具，提供管理和内容管理支持。 因为服务在云中运行，所以基础结构和可用性由 Microsoft 管理。

![Azure 认知搜索体系结构](https://docs.microsoft.com/zh-cn/azure/search/media/search-what-is-azure-search/azure-search-diagram.svg)

![image-20200916110827738](C:\Users\Deon\AppData\Roaming\Typora\typora-user-images\image-20200916110827738.png)



参考文献：https://docs.microsoft.com/zh-cn/azure/search/search-what-is-azure-search

# 2. Azure认知搜索的能力

| 核心搜索         | 功能                                                         |
| :--------------- | :----------------------------------------------------------- |
| 自由格式文本搜索 | [全文搜索](https://docs.microsoft.com/zh-cn/azure/search/search-lucene-query-architecture)是大多数基于搜索的应用的主要用例。 查询可以使用支持的语法进行陈述。  [简单查询语法](https://docs.microsoft.com/zh-cn/azure/search/query-simple-syntax)提供逻辑运算符、短语搜索运算符、后缀运算符和优先运算符。  [Lucene 查询语法](https://docs.microsoft.com/zh-cn/azure/search/query-lucene-syntax)包括简单语法中的所有操作，以及模糊搜索、邻近搜索、术语提升和正则表达式扩展。 |
| 相关性           | [**简单计分**](https://docs.microsoft.com/zh-cn/azure/search/index-add-scoring-profiles)是 Azure 认知搜索的主要优势。 计分配置文件用于在文档中自行将相关性建模为值的函数。 例如，你可能希望较新产品或打折产品显示在搜索结果的顶部位置。 也可以基于已跟踪和单独存储的客户搜索首选项将标记用于个性化计分，来生成计分配置文件。 |
| 地理搜索         | Azure 认知搜索可以处理、筛选和显示地理位置。 它可以让用户基于搜索结果与物理位置的临近程度浏览数据。 [观看此视频](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data)或[查看此示例](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)了解详细信息。 |
| 筛选器和分面导航 | 通过单个查询参数实现[**分面导航**](https://docs.microsoft.com/zh-cn/azure/search/search-faceted-navigation)。 Azure 认知搜索返回一个分面导航结构，可以将该结构用作类别列表背后的代码，用于自定向筛选（例如，按价格范围或品牌来筛选目录项）。  可以使用[**筛选器**](https://docs.microsoft.com/zh-cn/azure/search/query-odata-filter-orderby-syntax)将分面导航纳入到应用程序的 UI 中，改进查询表述，以及基于用户或开发人员指定的条件进行筛选。 可以使用 OData 语法创建筛选器。 |
| 用户体验功能     | 可以为搜索栏中预先键入的查询启用[自动完成](https://docs.microsoft.com/zh-cn/azure/search/search-autocomplete-tutorial)。  [**搜索建议**](https://docs.microsoft.com/zh-cn/rest/api/searchservice/suggesters)也基于搜索栏中的部分文本输入开始工作，但结果是索引中的实际文档而不是查询术语。  [**同义词**](https://docs.microsoft.com/zh-cn/azure/search/search-synonyms)功能无需用户提供替换术语，便可关联隐式扩展查询范围的等效术语。  [命中项突出显示](https://docs.microsoft.com/zh-cn/rest/api/searchservice/Search-Documents)向搜索结果中的匹配关键字应用文本格式设置。 可以选择哪些字段返回突出显示的片段。  [**排序**](https://docs.microsoft.com/zh-cn/rest/api/searchservice/Search-Documents)通过索引架构覆盖多个字段，可以使用一个搜索参数在查询时进行切换。  通过 Azure 认知搜索所提供的对搜索结果的优化控制，[**分页**](https://docs.microsoft.com/zh-cn/azure/search/search-pagination-page-layout)和限制搜索结果将变得更简单。 |

| AI 扩充                                    | 功能                                                         |
| :----------------------------------------- | :----------------------------------------------------------- |
| 在编制索引期间进行 AI 处理                 | 适用于图像和文本分析的 [**AI 扩充**](https://docs.microsoft.com/zh-cn/azure/search/cognitive-search-concept-intro)可以应用于索引管道，以从原始内容中提取文本信息。 [内置技术](https://docs.microsoft.com/zh-cn/azure/search/cognitive-search-predefined-skills)的一些示例包括：光学字符识别（使扫描的 JPEG 变得可搜索）、实体识别（标识组织、名称或位置）、关键短语识别。 也可[将自定义技术编码](https://docs.microsoft.com/zh-cn/azure/search/cognitive-search-create-custom-skill-example)，以便将其附加到管道。 也可[集成 Azure 机器学习创作技能](https://docs.microsoft.com/zh-cn/azure/search/cognitive-search-tutorial-aml-custom-skill)。 |
| 存储丰富的内容以供在非搜索场景中分析和使用 | [知识存储](https://docs.microsoft.com/zh-cn/azure/search/knowledge-store-concept-intro)是基于 AI 的索引扩展。 通过将 Azure 存储用作后端，可以保存在编制索引期间创建的扩充。 这些项目可用于帮助你设计更好的技能集，或创建不含无固定结构或不明确数据的形状和结构。 可以创建定目标到特定工作负荷或用户的这些结构的投影。 还可以直接分析已提取的数据，或将它加载到其他应用中。 |
| 缓存内容                                   | [**增量扩充（预览版）** ](https://docs.microsoft.com/zh-cn/azure/search/cognitive-search-incremental-indexing-conceptual)将处理限制为仅处理通过对管道进行特定编辑而更改的文档，并对未更改的管道部分使用缓存内容。 |

| 数据导入/编制索引  | 功能                                                         |
| :----------------- | :----------------------------------------------------------- |
| 数据源             | Azure 认知搜索索引接受来自任何源的数据，前提是以 JSON 数据结构提交这些数据。  [索引器](https://docs.microsoft.com/zh-cn/azure/search/search-indexer-overview)自动引入受支持的 Azure 数据源中的数据，并处理 JSON 序列化。 连接到 [Azure SQL 数据库](https://docs.microsoft.com/zh-cn/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers)、[Azure Cosmos DB](https://docs.microsoft.com/zh-cn/azure/search/search-howto-index-cosmosdb) 或 [Azure Blob 存储](https://docs.microsoft.com/zh-cn/azure/search/search-howto-indexing-azure-blob-storage)，以提取主要数据存储中的可搜索内容。 Azure Blob 索引器可以执行“文档破解”[从主要文件格式提取文本](https://docs.microsoft.com/zh-cn/azure/search/search-howto-indexing-azure-blob-storage)，包括 Microsoft Office、PDF 和 HTML 文档。 |
| 分层的嵌套数据结构 | 借助[复杂类型](https://docs.microsoft.com/zh-cn/azure/search/search-howto-complex-data-types)和集合，可以将几乎所有类型的 JSON 结构建模为 Azure 认知搜索索引。 可以通过集合、复杂类型和复杂类型集合，以本机方式表示一对多和多对多基数。 |
| 语言分析           | 分析器是在编制索引和搜索操作期间用于处理文本的组件。 有两种类型。  [自定义词汇分析器](https://docs.microsoft.com/zh-cn/azure/search/index-add-custom-analyzers)用于使用拼音匹配和正则表达式的复杂搜索查询。  Lucene 或 Microsoft 的[语言分析器](https://docs.microsoft.com/zh-cn/azure/search/index-add-language-analyzers)用于智能处理特定于语言的语言学，包括谓词时态、词性、不规则复数名词（例如“mouse”与“mice”）、词取消复合、词拆分（对于不带空格的语言）等。 |

| 平台级别                 | 功能                                                         |
| :----------------------- | :----------------------------------------------------------- |
| 用于原型制作和检查的工具 | 在门户中，可以使用[**导入数据向导**](https://docs.microsoft.com/zh-cn/azure/search/search-import-data-portal)来配置索引器、索引设计器以建立索引，并可以使用[**搜索浏览器**](https://docs.microsoft.com/zh-cn/azure/search/search-explorer)来测试查询并优化评分配置文件。 还可以打开任何索引来查看其架构。 |
| 监视和诊断               | [启用监视功能](https://docs.microsoft.com/zh-cn/azure/search/search-monitor-usage)可查看除门户中始终可见的一目了然指标外的其他指标。 门户页面中会捕获并报告关于每秒查询数、延迟和限制的指标，无需额外进行配置。 |
| 服务器端加密             | [Microsoft 托管的静态加密](https://docs.microsoft.com/zh-cn/azure/search/search-security-overview#encrypted-transmissions-and-storage)内置在内部存储层中，它是不可撤消的。 可以视需要使用[客户托管的加密密钥](https://docs.microsoft.com/zh-cn/azure/search/search-security-manage-encryption-keys)来补充默认加密。 你在 Azure Key Vault 中创建和管理的密钥用于加密 Azure 认知搜索中的索引和同义词映射。 |
| 基础结构                 | **高可用性平台**确保极其可靠的搜索服务体验。 正确缩放时，[Azure 认知搜索可提供 99.9% SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)。  作为一种**完全托管且可缩放的**端到端解决方案，Azure 认知搜索绝对不需要基础结构管理。 通过在两个维度进行缩放以便处理更多文档存储和/或更高的查询负载，可以根据需求来定制服务。 |