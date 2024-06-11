# Build Chat App with Your Data using Azure OpenAI

This repo contains steps for the technical workshop aiming to create generative AI based web app that integrates with your own data source utilizing Azure OpenAI, Azure AI Search and Azure Cosmos DB. 

## Prerequisites
- An existing **Azure OpenAI** resource and deployment of following models: **gpt-4o** and **text-embedding-ada-002**
- To use Azure OpenAI on your data following data source options will be examined:
  - **Azure Blob Storage**: to host uploaded pdf or text documents
  - **Azure AI Search**: to create indexes from source documents
  - **Azure Cosmos DB**: to serve products data in structured format to chat application
- **Azure App Service**: to host chat based custom application (Python / ASP.NET)
- **Azure Cosmos DB**: to host conversation history and summary of conversations
- [Optional] VS Code, Python, Node JS, and Azure CLI to modify / run the app locally

## Step 1: Basic Chat

Go to [oai.azure.com](https://oai.azure.com) and  navigate to chat under playground. Test basic chat with gpt-4o and play with prompt and parameters to customize responses.

> ![Azure Open AI Studio](/images/image.png)

**Optional steps if you don't have existing deployment:**

If your Azure OpenAI Studio looks like below, you need to create a new deployment. You can skip this step if not necessary.

> ![deployment](/images/image2.png)

Make selections like below and choose your own deployment name (idealy same with model name).

> ![deployment model](/images/image3.png)

If you are able to chat with gpt-4o succesfully, try to upload an image and asking question to image. You may test this with any sample.jpg in this repository or any image.

> ![chat playground](/images/image4.png)


## Step 2: Basic Chat with your own data

In chat interface in playground, open Add Your Own Data tab and click "+ Add Data Source" button. 

> ![add data button](/images/image5.png)

> ![alt text](/images/imageupload.png)

Select Subscription, Azure Blob Storage to store .pdf files, and Azure AI Search service to index files. Give an Index name, check add vector search box and then select available text-embedding-ada-002 model from dropdown.

> ![add data resource](/images/image6.png)


Click next and add documents you want your chatbot to use to answer questions. You can use a sample in this repository or use your own PDF file. 

*Hint: Try to limit file selection to single subject, without mixing different topics to increase output quality.*

> ![upload document](/images/image7.png)
<img scr="/images/image7.png" width="200">


Click Next and select search type. (Hybrid is recommended for most scenarios). Click next, and then wait until indexing is finished. After completing indexing you should see below:

![completed index](/images/image8.png)


Now, chatbot should answer questions regarding data source with citations like below:

![indexes](/images/image9.png)


## Step 3: Create a customized index using Azure AI Search

In step 2, we created our index in Azure OpenAI Studio directly but actualy index is created in Azure AI Search resource at the back stage. In this step, we will create a custom index using Azure AI search interface with more granular control, more data source options and flexibility.

As a first step, go to [portal.azure.com](https://portal.azure.com) and search for "Azure AI Search" resources. Use existing or create new resource.

> ![AI Search Page](/images/image10.png)

If you can't see any Azure AI Search resource, please create new resource in a resource group. 


> ![AI Search Resource](/images/image11.png)

Now, open your AI search resource, and select "Import Data" option. You can choose Sample Data option, and then select hotels-sample (Cosmos DB Sample Database including a table with Hotel list, and attributes). If you want you can also select your own data source such as **Cosmos DB**, **SQL Database**, or **Blob Storage**.

After connecting your data, if you want you can add new cognitive skills. **This is an optional step*

Then you can customize target index and override default selection and then determine the your index and indexer name. If you don't want any customization just keep defaults and continue.

After completion, you should see your indexer in the Indexers list: 

> ![indexer](/images/image12.png)


Then we go back to [oai.azure.com](https://oai.azure.com). Here we will use and test the index we created. Click "Add your data" and if you have existing data index connection remove it and create new connection. Select the AI Search resource and choose your newly created index.

> ![AI Search Resource](/images/image13.png)


Then choose search type and click Save & Close.

**Additional Information: In above step, there is no hybrid search option since we are connecting to an index without Vector search capability. If vector search capability is needed, then we sould use other options in Azure AI Search, including vectorization in Cosmos DB itself. You can visit https://github.com/Azure/Vector-Search-AI-Assistant-MongoDBvCore to learn more about this scenario. *

> ![AI search Complete](/images/image14.png)


Let's test it. For example, when we ask that "I am going to New York for 3 days, my budget is $300 per night and I need something that sleeps 2 people. I would like to be near the major tourist attractions", it should respond like below:

> ![Text Chat](/images/image15.png)


## [Optional] Step 4: Create Another Index from a table in Cosmos DB

**Create Cosmos DB resource**
    - if you already exist please pass the other
    - click Azure Cosmos DB for NoSQL
    ![Cosmos DB](/images/image16.png)
 
## Step 5: Create Custom Index from Cosmos DB Product Table
 
 
## Step 6:  Test indexes in Studio and Prompt Improvements
 
 
## Step 7:  Deploy Assistant as Web App with Chat History

After testing your index in Studio, click deploy app button on top right and select deploy custom app option.

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/270c53a7-aaf5-41cb-8dd6-24edd0d14e29)

Then make selections like below and click deploy.

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/2ea0ad86-fb62-418f-bc01-2a653f7cbc2f)

After this, it will take 5-10 minutes until your app is published, with relevant resources in the resource group. 

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/7f0c6cc4-f3f0-4d5e-b0d3-6167fd55805d)

It will automaticaly create one **Cosmos DB* resouce to store conversation history and one web app to host backend and frontend application.

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/90427da5-5e76-4b63-802f-1555dc91b799)

Now you can open web app from Azure OpenAI Studio > Launch App button at top right or finding releted Azure Web App in Azure portal and Browse option.

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/d6a97b69-9bf1-4e0c-a65a-34bf9bb21943)

App should look like below and you can test functionality with some questions from your source data.

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/3d0bc7f9-8a03-4e22-a979-4947d171ce6e)


## Step 8: Edit Environment Variables, System Prompt and Customize App

In previous step, we deployed sample application with one-click deployment but we may need to customize the app interface and functionality. First we will customize deployed app using Environment variables, then we will deploy this app from Visual Studio code using sample reposityory and make desired customizations freely.

**Editing Environment Variables**

Navigate to app service from Azure Portal and open Environment Variables of the app.

![image](https://github.com/mustafaasiroglu/azure-openai-workshop/assets/38222743/df639bb0-8405-4071-92ee-3a5fc348cf3d)

You can edit System Prompt or make UI changes using following variables. Then save and test app to see changes. List of all available variables are listed at !(https://github.com/microsoft/sample-app-aoai-chatGPT)

- `UI_TITLE`
- `UI_LOGO`
- `UI_CHAT_TITLE`
- `UI_CHAT_LOGO`
- `UI_CHAT_DESCRIPTION`
- `UI_FAVICON`
- `UI_SHOW_SHARE_BUTTON`


**Deploying App from Strach using VS Code**

Clone the repository at !(https://github.com/microsoft/sample-app-aoai-chatGPT/tree/main) and follow detailed steps in repository.

## CONGRAGULATIONS!!

This is end of all steps required for this workshop. Learn more and discover additonal scenarios using below resources.

- https://github.com/microsoft/sample-app-aoai-chatGPT
- https://github.com/Azure-Samples/azure-openai-gpt-4-vision-pdf-extraction-sample
- https://github.com/Azure-Samples/chat-with-your-data-solution-accelerator
- https://github.com/Azure/Vector-Search-AI-Assistant-MongoDBvCore
-
-
-
-


## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
