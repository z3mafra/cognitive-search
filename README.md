# cognitive-search  
cognitive-search  

> 1. Crie um novo repositório no github com um nome a sua preferência  
> 2. Crie um arquivo readme.md descrevendo o passo a passo para se configurar uma pesquisa, assim como seus insights, possibilidades de ferramentas que se beneficiam com esse tipo de ferramenta e aprendizados adquiridos durante o processo.  
>> 2.1. Criar recursos do Azure  
> 2.2. Extrair dados de uma fonte de dados  
> 2.3. Enriqueça os dados com habilidades de IA  
> 2.4. Use o indexador no portal do Azure  
> 2.5. Consulte seu índice de pesquisa  
> 2.6. Revise os resultados salvos em uma Loja de conhecimento  

> 3. Compartilhe conosco o link desse repositório através do botão 'entregar projeto'  

--------
## 1. Crie um novo repositório no github com um nome a sua preferência  

Na maquina local foi crido o diretório 'cognitive-search'  

~~~bash  
$ mkdir cognitive-search  
~~~  

Depois, iniciei o diretório como um repositório no Git.

~~~bash  
$ git init  
~~~  

No GitHub foi criado o repositório cognitive-search

> Comandos/configurações:  
> 
> - "Novo repositório".  
> - Nome para o repositório: "cognitive-search".  
> - Descrição: "Projeto Vision Studio com três processos de identificação: Detect faces; Read text; e, Analyze images.".  
> - Visibilidade: "pública".  
> - Opção: LEIAME.  
> - Cliquei em "Criar repositório".  
> 

-------
## 2. Crie um arquivo readme.md descrevendo o passo a passo para se configurar uma pesquisa, assim como seus insights, possibilidades de ferramentas que se beneficiam com esse tipo de ferramenta e aprendizados adquiridos durante o processo.  

--------
### 2.1. Criar recursos do Azure  
The solution you’ll create for Fourth Coffee requires the following resources in your Azure subscription:

> - An Azure AI Search resource, which will manage indexing and querying.  
> - An Azure AI services resource, which provides AI services for skills that your search solution can use to enrich the data in the data source with AI-generated insights.  
> - A Storage account with blob containers, which will store raw documents and other collections of tables, objects, or files.  

>[!NOTE]
> Your Azure AI Search and Azure AI services resources must be in the same location!  

--------
### 2.1.1. Create an Azure AI Search resource  

>    - Sign into the Azure portal.
>    - Click the + Create a resource button, search for Azure AI Search, and create a Azure AI Search resource with the following settings:  
>      - Subscription: Azure subscription 1.  
>      - Resource group: LabVision.  
>      - Service name: A unique name.  
>      - Location: Choose any available region.  
>      - Pricing tier: Basic  
>    - Select Review + create, and after you see the response Validation Success, select Create.  
>    - After deployment completes, select Go to resource. On the Azure AI Search overview page, you can add indexes, import data, and search created indexes.  


--------
### 2.1.2. Create an Azure AI services resource  

>    - You’ll need to provision an Azure AI services resource that’s in the same location as your Azure AI Search resource. Your search solution will use this resource to enrich the data in the datastore with AI-generated insights.  
>    - Return to the home page of the Azure portal. Click the ＋Create a resource button and search for Azure AI services. Select create an Azure AI services plan. You will be taken to a page to create an Azure AI services resource. Configure it with the following settings:  
>      - Subscription: Your Azure subscription.  
>      - Resource group: The same resource group as your Azure AI Search resource.  
>      - Region: The same location as your Azure AI Search resource.  
>      - Name: A unique name.  
>      - Pricing tier: Standard S0  
>      - By checking this box I acknowledge that I have read and understood all the terms below: Selected  
>    - Select Review + create. After you see the response Validation Passed, select Create.  
>    - Wait for deployment to complete, then view the deployment details.  
  
--------
### 2.1.3. Create a storage account  
  
>    - Return to the home page of the Azure portal, and then select the + Create a resource button.  
>    - Search for storage account, and create a Storage account resource with the following settings:  
>      - Subscription: Your Azure subscription.  
>      - Resource group: The same resource group as your Azure AI Search and Azure AI services resources.  
>      - Storage account name: A unique name.  
>      - Location: Choose any available location.  
>      - Performance: Standard  
>      - Redundancy: Locally redundant storage (LRS)  
>    - Click Review and then click Create. Wait for deployment to complete, and then go to the deployed resource.  
>    - In the Azure Storage account you created, in the left-hand menu pane, select Configuration (under Settings).  
>    - Change the setting for Allow Blob anonymous access to Enabled and then select Save.  
  
--------
### 2.2. Extrair dados de uma fonte de dados  
> ## Upload Documents to Azure Storage

1.  In the left-hand menu pane, select  **Containers**.

[![Screenshot that shows the storage blob overview page.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-1.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-1.png)    

2.  Select  **+ Container**. A pane on your right-hand side opens.
    
3.  Enter the following settings, and click  **Create**:
    -   **Name**: coffee-reviews
    -   **Public access level**: Container (anonymous read access for containers and blobs)
    -   **Advanced**:  _no changes_.
4.  In a new browser tab, download the  [zipped coffee reviews](https://aka.ms/mslearn-coffee-reviews)  from  `https://aka.ms/mslearn-coffee-reviews`, and extract the files to the  _reviews_  folder.
    
5.  In the Azure portal, select your  _coffee-reviews_  container. In the container, select  **Upload**.    
    
 [![Screenshot that shows the storage container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-3.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-3.png)    
    
6.  In the  **Upload blob**  pane, select  **Select a file**.
    
7.  In the Explorer window, select  **all**  the files in the  _reviews_  folder, select  **Open**, and then select  **Upload**.
        
[![Screenshot that shows the files uploaded to the Azure container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-container-upload-files.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-container-upload-files.png)    
    
8.  After the upload is complete, you can close the  **Upload blob**  pane. Your documents are now in your  _coffee-reviews_  storage container.

--------
### 2.3. Enriqueça os dados com habilidades de IA  

--------
### 2.4. Use o indexador no portal do Azure  

--------
### 2.5. Consulte seu índice de pesquisa  

--------
### 2.6. Revise os resultados salvos em uma Loja de conhecimento  

--------
## 3. Compartilhe conosco o link desse repositório através do botão 'entregar projeto'  

--------
--------
