**Online version:** https://jfk-demo.azurewebsites.net/#/

**GitHub:** https://github.com/microsoft/AzureSearch_JFK_Files

**Description:** The JFK files example leverages the built-in Cognitive Skills inside of Cognitive Search and combines it with custom skills using extensibility. The architecture below showcases how the new Cognitive Search capabilities of Azure enable you to easily create structure from almost any datasource.

![image-20200925131943238](https://i.loli.net/2020/09/25/RIKbvr4PcdOCnaw.png)

**Functionality:** This project includes the following capabilities for you to build your own version of the JFK files.

- We have provided a subset of the JFK PDF documents and images that have been uploaded to the cloud into Azure Blob Storage.
- An Azure Search service is used to index the content and power the UX experience. We use the new Cognitive Search capabilities to apply pre-built cognitive skills to the content, and we also use the extensibility mechanism to add custom skills using Azure Functions.
  - Uses the Cognitive Services Vision API to extract text information from the image via OCR, and image captioning,
  - Applies Named Entity Recognitition to extract named entities from the documents,
  - Annotates text using a custom CIA Cryptonyms skill,
  - Generates HOCR content based on results.
- A standalone website to search the index and explore the documents

<IMG  src="https://github.com/microsoft/AzureSearch_JFK_Files/raw/master/images/jfk-files-architecture.JPG"  alt="建筑"/>