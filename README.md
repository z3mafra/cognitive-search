# Desafio: Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados
## Repositório: cognitive-search  

> 1. Criar um novo repositório no github com um nome a sua preferência  
> 2. Descrever o passo a passo para se configurar uma pesquisa, no arquivo readme.md  
>> 2.1. Criar recursos do Azure  
>> 2.2. Extrair dados de uma fonte  
>> 2.3. Enriquecer os dados com habilidades de IA: indexador no portal do Azure  
>> 2.4. Consulta ao índice de pesquisa  
>> 2.5. Revisão dos resultados salvos
>> 2.6. Insights e Possibilidades  
> 3. Fnalizar e Compartilhar o link do repositório  
>>  
--------
##  Explore um índice do Azure AI Search (UI)
Vamos imaginar que você trabalha para a *Fourth Coffee*, uma rede nacional de cafés. Você foi solicitado a ajudar a construir uma solução de mineração de conhecimento que facilite a busca de *insights* sobre as experiências dos clientes. Você decide criar um índice do **Azure AI Search** usando dados extraídos de avaliações de clientes..  

Neste laboratório você irá:  

>- Criar recursos do Azure  
>- Extrair dados de uma fonte de dados  
>- Enriquecer os dados com habilidades de IA  
>- Usar o indexador do Azure no portal do Azure  
>- Consultar seu índice de pesquisa  
>- Revisar os resultados salvos em um Repositório de conhecimento (*Knowledge Store*)  


--------
## 1. Criar um novo repositório no github com um nome a sua preferência  

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
> - Descrição: "Projeto Vision Studio com três processos de identificação: Detect faces; Read text; e, Analyze images".  
> - Visibilidade: "pública".  
> - Opção: LEIAME.  
> - Cliquei em "Criar repositório".  
> 

-------
## 2. Descrever o passo a passo para se configurar uma pesquisa, no arquivo readme.md    

--------
### 2.1. Criar recursos do Azure  
A solução que você criará para o *Fourth Coffee* requer os seguintes recursos em sua assinatura do Azure:

> - Um recurso do **Azure AI Search**, que gerenciará a indexação e a consulta.  
> - Um recurso de serviços do **Azure AI services**, que fornece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.  
> - Uma conta de armazenamento (**Storage account**) com contêineres de blobs, que armazenará documentos brutos e outras coleções de tabelas, objetos ou arquivos.  

>[!NOTE]
> Os recursos do Azure AI Search e dos serviços Azure AI devem estar no mesmo local!!  

--------
### 2.1.1. Criando um recurso "**Azure AI Search**"   

>    - Fazer login no [Azure portal](https://portal.azure.com/).
>    - Clicar no botão "**+ Create a resource**, pesquise o *Azure AI Search* e crie um recurso do **Azure AI Search** com as seguintes configurações:  
>      - Subscription: Azure subscription 1.  
>      - Resource group: LabCogSearch.  
>      - Service name: cognitionsearch.  
>      - Location: East US.  
>      - Pricing tier: Basic  
>    - Selecionar "**Review + create**", e depois de ver a resposta "**Validation Success**", selecionar "**Create**".  
>    - Após a conclusão da implantação, selecionar "**Go to resource**". Na página de visão geral do "**Azure AI Search**", você pode adicionar índices, importar dados e pesquisar índices criados.  


--------
### 2.1.2. Criando um recurso "**Azure AI services**"  

>    - Será necessário provisionar um recurso no "**Azure AI services**" na mesma localidade do recurso "**Azure AI Search**". Sua solução de pesquisa usará esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.  
>    - Regressar à página inicial do portal Azure. Clicar no botão "**+ Create a resource** e pesquise o *Azure AI Services*. Selecione create an Azure AI services plan. Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:  
>      - Subscription: Azure subscription 1.  
>      - Resource group: LabCogSearch.  
>      - Region: East US.  
>      - Name: CogLabSearch.  
>      - Pricing tier: Standard S0  
>      - Selecionar o box: "By checking this box I acknowledge that I have read and understood all the terms below"   
>    - Selecionar "**Review + create**". e depois de ver a resposta "**Validation Passed**", selecionar "**Create**".  
>    - Aguarde a conclusão da implantação e visualize os detalhes da implantação.  
  
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
### 2.2. Extrair dados de uma fonte  
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
### 2.3. Enriquecer os dados com habilidades de IA: indexador no portal do Azure  

After you have the documents in storage, you can use Azure AI Search to extract insights from the documents. The Azure portal provides an  _Import data wizard_. With this wizard, you can automatically create an index and indexer for supported data sources. You’ll use the wizard to create an index, and import your search documents from storage into the Azure AI Search index.

1.  In the Azure portal, browse to your Azure AI Search resource. On the  **Overview**  page, select  **Import data**.
    
    [![Screenshot that shows the import data wizard.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/azure-search-wizard-1.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/azure-search-wizard-1.png)
    
2.  On the  **Connect to your data**  page, in the  **Data Source**  list, select  **Azure Blob Storage**. Complete the data store details with the following values:
    -   **Data Source**: Azure Blob Storage
    -   **Data source name**: coffee-customer-data
    -   **Data to extract**: Content and metadata
    -   **Parsing mode**: Default
    -   **Connection string**: *Select  **Choose an existing connection**. Select your storage account, select the  **coffee-reviews**  container, and then click  **Select**.
    -   **Managed identity authentication**: None
    -   **Container name**:  _this setting is auto-populated after you choose an existing connection_.
    -   **Blob folder**:  _Leave this blank_.
    -   **Description**: Reviews for Fourth Coffee shops.
3.  Select  **Next: Add cognitive skills (Optional)**.
    
4.  In the  **Attach Cognitive Services**  section, select your Azure AI services resource.
    
>    5.  In the  **Add enrichments**  section:
>        -   Change the  **Skillset name**  to  **coffee-skillset**.
>        -   Select the checkbox  **Enable OCR and merge all text into merged_content field**.
    
> **Note**  It’s important to select  **Enable OCR**  to see all of the enriched field options. 
        
  -   Ensure that the  **Source data field**  is set to  **merged_content**.
  -   Change the  **Enrichment granularity level**  to  **Pages (5000 character chunks)**.
  -   Don’t select  _Enable incremental enrichment_
  -   Select the following enriched fields:
      
  | **Cognitive Skill**  | **Parameter**  |  **Field name** |
  |  ---------  |  ---------  |  ---------  |
  |   Extract location names    |              |   locations   |   
  |   Extract key phrases    |       |   keyphrases    |   
  |   Detect sentiment    |       |   sentiment    |   
  |   Generate tags from images    |       |   imageTags    |   
  |   Generate captions from images    |       |   imageCaption    |   


>    6.  Under  **Save enrichments to a knowledge store**, select:  
    
  -   Image projections
  -   Documents
  -   Pages
  -   Key phrases
  -   Entities
  -   Image details
  -   Image references  
--  

  > **Note**  If a warning asking for a  **Storage Account Connection String**  appears.
  >   
  >   [![Screenshot that shows the Storage account connection screen warning with 'Choose an existing connection' selected.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-enrichments-warning.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-enrichments-warning.png)  
>>  
>> a.  Select  **Choose an existing connection**. Choose the storage account you created earlier.  
>> b.  Click on  **+ Container**  to create a new container called  **knowledge-store**  with the privacy level set to  **Private**, and select  **Create**.  
>> c.  Select the  **knowledge-store**  container, and then click  **Select**  at the bottom of the screen.    
>   
>    
>    8.  Select  **Azure blob projections: Document**. A setting for  _Container name_  with the  _knowledge-store_  container auto-populated displays. Don’t change the container name.
>    
>    9.  Select  **Next: Customize target index**. Change the  **Index name**  to  **coffee-index**.
>        
>    10.  Ensure that the  **Key**  is set to  **metadata_storage_path**. Leave  **Suggester name**  blank and  **Search mode**  autopopulated.
>        
>    11.  Review the index fields’ default settings. Select  **filterable**  for all the fields that are already selected by default.
>    
>   [![Screenshot that shows the customize index pane with the index name entered and 'Filterable' selected for a default index field.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-customize-index.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-customize-index.png)
>    
>    11.  Select  **Next: Create an indexer**.
>    12.  Change the  **Indexer name**  to  **coffee-indexer**.
>    
>    13.  Leave the  **Schedule**  set to  **Once**.
>    
>    14.  Expand the  **Advanced options**. Ensure that the  **Base-64 Encode Keys**  option is selected, as encoding keys can make the index more efficient.
>    
>    15.  Select  **Submit**  to create the data source, skillset, index, and indexer. The indexer is run automatically and runs the indexing pipeline, which:
    -   Extracts the document metadata fields and content from the data source.
    -   Runs the skillset of cognitive skills to generate more enriched fields.
    -   Maps the extracted fields to the index.
>    16.  Return to your Azure AI Search resource page. On the left pane, under  **Search Management**, select  **Indexers**. Select the newly created  **coffee-indexer**. Wait a minute, and select  **&orarr; Refresh**  until the  **Status**  indicates success.
>        
>    17.  Select the indexer name to see more details.
>    
>    [![Screenshot that shows the coffee-indexer Indexer successfully created.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-search-indexer-success.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-search-indexer-success.png)
    




    
--------
### 2.4. Consulta ao índice de pesquisa 

Use the Search explorer to write and test queries. Search explorer is a tool built into the Azure portal that gives you an easy way to validate the quality of your search index. You can use Search explorer to write queries and review results in JSON.

> 1.  In your Search service’s  _Overview_  page, select  **Search explorer**  at the top of the screen.
>    
>    [![Screenshot of how to find Search explorer.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/5-exercise-screenshot-7.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/5-exercise-screenshot-7.png)

    
> 2.  Notice how the index selected is the  _coffee-index_  you created. Below the index selected, change the  _view_  to  **JSON view**.
>    
>    [![Screenshot of the Search explorer.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/search-explorer-query.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/search-explorer-query.png)
    


In the  **JSON query editor**  field, copy and paste:

CodeCopy

~~~json
{
    "search": "*",
    "count": true
}

~~~





> 1.  Select  **Search**. The search query returns all the documents in the search index, including a count of all the documents in the  **@odata.count**  field. The search index should return a JSON document containing your search results.
    
> 2.  Now let’s filter by location. In the  **JSON query editor**  field, copy and paste:
    
    CodeCopy
    
~~~json
    {
     "search": "locations:'Chicago'",
     "count": true
    }
    
~~~

 
> 3.  Select  **Search**. The query searches all the documents in the index and filters for reviews with a Chicago location. You should see  `3`  in the  `@odata.count`  field.
    
> 4.  Now let’s filter by sentiment. In the  **JSON query editor**  field, copy and paste:
    
    CodeCopy
    
 ~~~json
    {
     "search": "sentiment:'negative'",
     "count": true
    }
    
   ~~~


   
> 5.  Select  **Search**. The query searches all the documents in the index and filters for reviews with a negative sentiment. You should see  `1`  in the  `@odata.count`  field.
    
    > **Note**  See how the results are sorted by  `@search.score`. This is the score assigned by the search engine to show how closely the results match the given query.
    
> 6.  One of the problems we might want to solve for is why there might be certain reviews. Let’s take a look at the key phrases associated with the negative review. What do you think might be the cause of the review?








--------
### 2.5. Revisão dos resultados salvos  


Let’s see the power of the knowledge store in action. When you ran the  _Import data wizard_, you also created a knowledge store. Inside the knowledge store, you’ll find the enriched data extracted by AI skills persists in the form of projections and tables.

> 1.  In the Azure portal, navigate back to your Azure storage account.
    
> 2.  In the left-hand menu pane, select  **Containers**. Select the  **knowledge-store**  container.
    
>    [![Screenshot of the knowledge-store container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-0.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-0.png)
    
> 3.  Select any of the items, and then click the  **objectprojection.json**  file.
    
>    [![Screenshot of the objectprojection.json.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-1.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-1.png)
    
> 4.  Select  **Edit**  to see the JSON produced for one of the documents from your Azure data store.
    
>    [![Screenshot of how to find the edit button.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-2.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-2.png)
    
> 5.  Select the storage blob breadcrumb at the top left of the screen to return to the Storage account  _Containers_.
>     
>    [![Screenshot of the storage blob breadcrumb.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-4.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-4.png)
>        
> 6.  In the  _Containers_, select the container  _coffee-skillset-image-projection_. Select any of the items.   
    
>    [![Screenshot of the skillset container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-5.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-5.png)
    
> 7.  Select any of the  _.jpg_  files. Select  **Edit**  to see the image stored from the document. Notice how all the images from the documents are stored in this manner.
    
>    [![Screenshot of the saved image.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-3.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-3.png)
    
> 8.  Select the storage blob breadcrumb at the top left of the screen to return to the Storage account  _Containers_.
    
> 9.  Select  **Storage browser**  on the left-hand panel, and select  **Tables**. There’s a table for each entity in the index. Select the table  _coffeeSkillsetKeyPhrases_.
    
>    Look at the key phrases the knowledge store was able to capture from the content in the reviews. Many of the fields are keys, so you can link the tables like a relational database. The last field shows the key phrases that were extracted by the skillset.

--------
### 2.6. Insights e Possibilidades  
  
> O Processamento de Linguagem Natural (PNL) é fundamental para compreender e interagir com a linguagem escrita e falada, possibilitando a extração de significado semântico e a formulação de respostas em linguagem natural.  
  
> Agências de Viagens, entre outros serviçoes, podem usar o Azure Language Studio para analisar avaliações de hotéis, identificando sentimentos e entidades mencionadas, melhorando a experiência do cliente.  
  
> O Azure Language Studio é uma ferramenta poderosa oferecida pela Microsoft para análise de texto e compreensão de linguagem natural. Uma das aplicações mais significativas do Language Studio é a análise de sentimentos, onde ele pode ser utilizado para determinar se as avaliações de produtos, serviços ou qualquer outro tipo de conteúdo são predominantemente positivas ou negativas. Isso é essencial para empresas que desejam entender o feedback dos clientes e a reputação de sua marca. Ao empregar técnicas de processamento de linguagem natural avançadas, o Language Studio é capaz de identificar nuances no texto, capturando não apenas palavras-chave, mas também o contexto e o tom geral das avaliações.  
  
> Através do uso de algoritmos de aprendizado de máquina e modelos de linguagem pré-treinados, o Language Studio é capaz de analisar grandes volumes de avaliações de forma rápida e eficiente. Ele pode detectar palavras e frases que indicam sentimentos positivos ou negativos, levando em consideração aspectos como sarcasmo, ironia e ambiguidade. Isso permite uma avaliação mais precisa e detalhada do sentimento expresso no texto, proporcionando insights valiosos para as empresas.  
  
> Uma das vantagens do Azure Language Studio é sua capacidade de personalização. As empresas podem ajustar os modelos de análise de sentimento de acordo com suas necessidades específicas e o domínio de seu negócio. Isso permite uma análise mais precisa e relevante das avaliações, levando em consideração termos e expressões específicas da indústria ou do público-alvo. Além disso, o Language Studio oferece integração com outras ferramentas e serviços do Azure, permitindo uma implementação suave em diferentes sistemas e plataformas.  
  
> No entanto, é importante reconhecer que nenhuma ferramenta de análise de sentimento é perfeita. O Azure Language Studio pode enfrentar desafios ao lidar com textos complexos ou ambíguos, onde o contexto pode influenciar significativamente o sentimento expresso. Além disso, como qualquer tecnologia baseada em machine learning, o desempenho do Language Studio pode variar dependendo da qualidade dos dados de treinamento e das configurações específicas utilizadas. Portanto, é fundamental complementar a análise automatizada com revisão humana e outras formas de feedback para obter uma compreensão abrangente das avaliações.  
  
> Um outro aspecto importante, que também deve ser consdierado, é a integração dessas ferramentas com os ambientes de navegação na internet, tais como plataformas, sites e aplicativos.  



--------
--------
## 3. Fnalizar e Compartilhar o link do repositório  
Antes de finalizar o desafio, uma providência importate é limpar (Clean up) o Language Studio, deletando os recrusos para não gerar custos desnecessários.

### **Clean up**  
Como é recomendado na documentção, senão se pretende fazer mais exercícios, excluir todos os recursos que não precisa mais. Isso evita acumular custos desnecessários.  
‑
>    1. Abra o portal do Azure (<https://portal.azure.com/>) e selecione o grupo de recursos que contém o recurso que você criou.  
>    2. Selecione o recurso e selecione Excluir e depois Sim para confirmar. O recurso é então excluído.
>
>>  - **Observação:** Uma curiosidade que, nesta etapa, a plataforma informou que não houve consumo de recursos, restando ainda um crédito de R$ 988,18, na assinatura gratuita. Foi a mesma posição do último desafio no Azura, Vision Studio: Detect faces; Read text; e, Analyze images.  

Após completar o Desafio, fiz a atualização no repositório local com o comando git pull.  

>- No repositório local;  
>  Clica no botão direito do mouse; e
>- Opção: "Open Git Bash here"
> No terminal digita o comando Git Pull <endereço do repositório remoto>

~~~bash
$ git pull https://github.com/z3mafra/language-studio.git
~~~

### **Postar link do repositório**
Como é recomendado na documentção, senão se pretende fazer mais exercícios, excluir todos os recursos que não precisa mais. Isso evita acumular custos desnecessários.  
>    1. Abra o portal da DIO (<https://www.dio.me/>), com as credenciais e selecione o [Bootcamp Microsoft AZure AI Fundamentals](https://web.dio.me/track/microsoft-azure-ai-fundamentals).
>    2. Entra na Atividade Desafio de projeto: "Reconhecimento Facial e transformação de imagens em Dados no Azure ML"
>    3. Clica no botão "ENTREGAR PROJETO" 
>    4. Cola o link do projeto na caixa de texto "Repositório do Projeto"; Cola a descrição do repositório; Ler e marcar o Check box "Termos de uso", concordando com os termos o recurso; e,
>    5. Selecione Entregar. O Desafio é então entregue.
>

--------
--------
