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
  
>    - Return to the home page of the Azure portal, and then Selecionar the + Create a resource button.  
>    - Search for storage account, and create a Storage account resource with the following settings:  
>      - Subscription: Azure subscription 1.  
>      - Resource group: LabCogSearch.  
>      - Storage account name: cogsrchstg.  
>      - Location: Choose any available location.  
>      - Performance: Standard  
>      - Redundancy: Locally redundant storage (LRS)  
>    - Clicar "**Review**" e então clicar "**Create**". Aguarde a conclusão da implantação e vá para o recurso implantado.  
>    - No Azure Storage account you created, no left-hand menu pane, Selecionar Configuration (under Settings).  
>    - Change the setting for Allow Blob anonymous access to Enabled and then Selecionar Save.  
  
--------
### 2.2. Extrair dados de uma fonte  
> ## Upload Documents to Azure Storage

> 1.  No left-hand menu pane, Selecionar **Containers**.

[![Screenshot that shows the storage blob overview page.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-1.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-1.png)    

> 2.  Selecionar **+ Container**. A pane on your right-hand side opens.
    
> 3.  Enter the following settings, and click **Create**:
    -  **Name**: coffee-reviews
    -  **Public access level**: Container (anonymous read access for containers and blobs)
    -  **Advanced**:  _no changes_.
> 4.  In a new browser tab, download the  [zipped coffee reviews](https://aka.ms/mslearn-coffee-reviews)  from  `https://aka.ms/mslearn-coffee-reviews`, and extract the files to the  _reviews_  folder.
    
> 5.  No Azure portal, Selecionar your  _coffee-reviews_  container. No container, Selecionar **Upload**.    
    
 [![Screenshot that shows the storage container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-3.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/storage-blob-3.png)    
    
> 6.  No **Upload blob** pane, Selecionar **Select a file**.
    
> 7.  No Explorer window, Selecionar **all** the files No  _reviews_  folder, Selecionar **Open**, and then Selecionar **Upload**.
        
[![Screenshot that shows the files uploaded to the Azure container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-container-upload-files.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-container-upload-files.png)    
    
> 8.  After the upload is complete, you can close the **Upload blob** pane. Your documents are now in your  _coffee-reviews_  storage container.

--------
### 2.3. Enriquecer os dados com habilidades de IA: indexador no portal do Azure  

After you have the documents in storage, you can use Azure AI Search to extract insights from the documents. The Azure portal provides an  _Import data wizard_. With this wizard, you can automatically create an index and indexer for supported data sources. You’ll use the wizard to create an index, and import your search documents from storage into the Azure AI Search index.

> 1.  No Azure portal, browse to your Azure AI Search resource. On the **Overview** page, Selecionar **Import data**.
    
   [![Screenshot that shows the import data wizard.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/azure-search-wizard-1.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/azure-search-wizard-1.png)
    
> 2.  On the **Connect to your data** page, no **Data Source** list, Selecionar **Azure Blob Storage**. Complete the data store details with the following values:
    -  **Data Source**: Azure Blob Storage
    -  **Data source name**: coffee-customer-data
    -  **Data to extract**: Content and metadata
    -  **Parsing mode**: Default
    -  **Connection string**: Selecionar **Choose an existing connection**. Selecionar your storage account, Selecionar the **coffee-reviews** container, and then click **Select**.
    -  **Managed identity authentication**: None
    -  **Container name**:  _this setting is auto-populated after you choose an existing connection_.
    -  **Blob folder**:  _Leave this blank_.
    -  **Description**: Reviews for Fourth Coffee shops.
> 3.  Selecionar **Next: Add cognitive skills (Optional)**.
    
> 4.  No **Attach Cognitive Services** section, Selecionar your Azure AI services resource.
    
> 5.  No **Add enrichments** section:
>        -   Change the **Skillset name** to **coffee-skillset**.
>        -   Selecionar the checkbox **Enable OCR and merge all text into merged_content field**.
    
> **Note** It’s important to Selecionar **Enable OCR** to see all of the enriched field options. 
        
  -   Ensure that the **Source data field** is set to **merged_content**.
  -   Change the **Enrichment granularity level** to **Pages (5000 character chunks)**.
  -   Don’t Selecionar  _Enable incremental enrichment_
  -   Selecionar the following enriched fields:
      
  | **Cognitive Skill** | **Parameter** | **Field name** |
  |  ---------  |  ---------  |  ---------  |
  |   Extract location names    |              |   locations   |   
  |   Extract key phrases    |       |   keyphrases    |   
  |   Detect sentiment    |       |   sentiment    |   
  |   Generate tags from images    |       |   imageTags    |   
  |   Generate captions from images    |       |   imageCaption    |   


> 6.  Under **Save enrichments to a knowledge store**, Selecionar:  
    
  -   Image projections
  -   Documents
  -   Pages
  -   Key phrases
  -   Entities
  -   Image details
  -   Image references  
--  

  > **Note** Se um alerta aparecer, perguntando por **Storage Account Connection String**.
  >   
  >   [![Screenshot that shows the Storage account connection screen warning with 'Choose an existing connection' selected.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-enrichments-warning.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-enrichments-warning.png)  
>>  
>> a.  Selecionar **Choose an existing connection**. EScolha a conta de armazenamento (*storage account*) criada antes.  
>> b.  Clicar em **+ Container** para criar um novo conteiner chamado **knowledge-store** com o nível de privacidade definido para **Private**, e Selecionar **Create**.  
>> c.  Selecionar o conteiner **knowledge-store**, e então clicar **Select** na parte inferior da tela.    
>   
>    
>    8.  Selecionar **Azure blob projections: Document**. Uma configuração para _Container name_ com as exibições preenchidas automaticamente do contêiner _knowledge-store_. Não altere o nome do contêiner.
>    
>    9.  Selecionar **Next: Customize target index**. Mudar o **Index name** para **coffee-index**.
>        
>    10.  Certifique-se de que **Key** esteja definida como **metadata_storage_path**. Deixe **Sugerir nome** em branco e **Modo de pesquisa** preenchido automaticamente.
>        
>    11.  Revise as configurações padrão dos campos de índice. Selecionar **filterable** para todos os campos que já estão selecionados por padrão.
>  
>   [![Screenshot that shows the customize index pane with the index name entered and 'Filterable' selected for a default index field.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-customize-index.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-azure-cognitive-search-customize-index.png)
>    
>    11.  Selecionar **Next: Create an indexer**.  
>    12.  Mudar o **Indexer name** para **coffee-indexer**.  
>    
>    13.  Deixar o **Schedule** definido como **Once**.  
>    
>    14.  Expanda **Advanced options**. Certifique-se de que a opção **Base-64 Encode Keys** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.  
>    
>    15.  Selecionar **Submit** para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
>    - Extrai os campos de metadados do documento e o conteúdo da fonte de dados.  
>    - Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.  
>    - Mapeia os campos extraídos para o índice.  


>[!NOTE]
> Azure AI Search notification  
> Importação configurada com sucesso, clique aqui para monitorar o progresso do indexador..  
> Quando o indexador terminar a execução, clique aqui para começar a pesquisar.  

>    17.  Retorne à página de recursos do **Azure AI Search**. No painel esquerdo, em **Search Management**, Selecionar **Indexers**. Selecionar o recém-criado **coffee-indexer**. Espere um minuto e selecionar **Refresh** até o **Status** indicar "**success**".
>        
>    18.  Selecionar o nome do "*indexer*" para ver mais detalhes.
>    
>    [![Screenshot that shows the coffee-indexer Indexer successfully created.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-search-indexer-success.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/6a-search-indexer-success.png)
    




    
--------
### 2.4. Consulta ao índice de pesquisa 

Use o Search Explorer para escrever e testar consultas. O explorador de pesquisa é uma ferramenta incorporada no portal do Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Search Explorer para escrever consultas e revisar resultados em JSON.

> 1.  Na página _Overview_ do seu serviço de pesquisa, selecione **Search Explorer** na parte superior da tela.
>    
>    [![Screenshot of how to find Search explorer.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/5-exercise-screenshot-7.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/5-exercise-screenshot-7.png)

    
> 2.  Observe como o índice selecionado é o _coffee-index_ que você criou. Abaixo do índice selecionado, altere _view_ para **JSON View**.
>    
>    [![Screenshot of the Search explorer.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/search-explorer-query.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/search-explorer-query.png)
    


No campo **JSON query editor** field, copiar e colar:

~~~json
{
    "search": "*",
    "count": true
}
~~~


> 1.  Selecionar **Search**. TA consulta de pesquisa retorna todos os documentos sem índice de pesquisa, incluindo uma contagem de todos os documentos no campo **@odata.count**. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.
    
> 2.  Agora filtrar por localização. No campo **JSON query editor**, copiar and colar:
    
   
~~~json
    {
     "search": "locations:'Chicago'",
     "count": true
    } 
~~~

 
> 3.  Selecionar **Search**. A consulta pesquisa todos os documentos sem índice e filtra revisões com localização em Chicago. Você deveria ver `3` no campo `@odata.count`.
    
> 4.  Agora, filtrar por sentimento. No campo **JSON query editor**, copiar e colar:
  
~~~json
    {
     "search": "sentiment:'negative'",
     "count": true
    }
~~~

   
> 5.  Selecionar **Search**. A consulta pesquisa todos os documentos sem índice e filtra revisões com sentimento negativo. Você deveria ver `1` no campo `@odata.count`.
    
>[!NOTE]
> Veja como os resultados são classificados por `@search.score`. Esta é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão próximos os resultados correspondem à consulta fornecida.
    
> 6. Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa. O que você acha que pode ser a causa da revisão?


--------
### 2.5. Revisão dos resultados salvos  


Vamos ver o poder do armazenamento de conhecimento em ação. Ao executar o *Import data wizard* (Assistente para importação de dados), também foi criou um armazenamento de conhecimento. Dentro do armazenamento de conhecimento, você encontrará os dados enriquecidos extraídos pelas habilidades de IA que persistem sem forma de projeções e tabelas.

> 1.  No Azure portal, navegue de volta para sua conta de armazenamento do Azure (Azure storage account).
    
> 2.  No painel de menu esquerdo, selecionar **Containers**. Selecionar o conteiner **knowledge-store**.
    
>    [![Screenshot of the knowledge-store container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-0.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-0.png)
    
> 3.  Selecionar qualquer um dos itens e clique no botão do arquivo **objectprojection.json**.
    
>    [![Screenshot of the objectprojection.json.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-1.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-1.png)
    
> 4.  Selecionar **Edit** para ver o JSON produzido para um dos documentos dos dados armazenados no Azure.
    
>    [![Screenshot of how to find the edit button.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-2.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-2.png)
    
> 5.  Selecionar a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar à conta de armazenamento *Containers*.
>     
>    [![Screenshot of the storage blob breadcrumb.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-4.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-4.png)
>        
> 6.  No *Containers*, Selecionar o container *coffee-skillset-image-projection*. Selecionar qualquer um dos itens.   
    
>    [![Screenshot of the skillset container.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-5.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-5.png)
    
> 7.  Selecionar qualquer um dos *.jpg*  files. Selecionar **Edit** para ver a imagem armazenada no documento. Todas as imagens dos documentos são armazenadas desta maneira.
    
>    [![Screenshot of the saved image.](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-3.png)](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/create-cognitive-search-solution/knowledge-store-blob-3.png)
    
> 8.  Selecionar a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar _Containers_ da conta de armazenamento (Storage account _Containers_).
    
> 9.  Selecionar **Storage browser** no painel esquerdo e selecionar **Tables**. Existe uma tabela para cada entidade sem índice. Selecionar a tabela _coffeeSkillsetKeyPhrases_.
    
>    Observe as frases-chave que o armazenamento de conhecimento conseguiu capturar do conteúdo sem revisões. Muitos dos campos são chaves, portanto você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.

--------
### 2.6. Insights, Possibilidades e Exemplos de Aplicações   

#### 🗒️ Insights e Possibilidades

1. **Melhor organização de dados**: O Azure Cognitive Search permite estruturar informações não estruturadas para facilitar a pesquisa e análise.
2. **Indexação eficiente**: Com a capacidade de criar índices de pesquisa, é possível indexar grandes volumes de dados para uma recuperação rápida e eficiente.
3. **Análise semântica avançada**: A funcionalidade semântica do Azure Cognitive Search permite encontrar insights significativos em grandes conjuntos de dados, utilizando recursos avançados de IA.
4. **Automatização de indexação**: Os indexadores do Azure AI Search podem ser configurados para atualizar os dados automaticamente em intervalos programados, garantindo que as informações estejam sempre atualizadas.
5. **Integração com outras fontes de dados**: É possível indexar dados de terceiros para pesquisa utilizando o Azure Cognitive Search, proporcionando uma visão abrangente das informações disponíveis.


#### 🗒️ Exemplos de Aplicações do Azure Cognitive Search

1. **Aplicativo de E-Commerce**: Implementar pesquisa avançada para produtos, permitindo aos usuários encontrar itens com base em critérios específicos, como marca, categoria ou preço.
  
2. **Portal de Documentos**: Criar um portal de documentos onde os usuários podem pesquisar e recuperar informações relevantes de grandes volumes de documentos, como manuais, relatórios e artigos técnicos.
  
3. **Aplicativo de Recrutamento**: Desenvolver um aplicativo de recrutamento que permita aos recrutadores pesquisar rapidamente por currículos com base em habilidades, experiência e localização .
  
4. **Plataforma de Notícias**: Construir uma plataforma de notícias onde os usuários possam pesquisar e acessar informações relevantes de diversas fontes de notícias, utilizando recursos semânticos para encontrar insights significativos.


--------
--------
## 3. Fnalizar e Compartilhar o link do repositório  
Antes de finalizar o desafio, é importate limpar (Clean up) os recrusos na sua assinatura do Azure, deletando para não gerar custos desnecessários.

### **Clean up** 
Neste laboratório a documentção não recomenda excluir todos os recursos que não serão mais utilizados. Porém, é importante fazer isso para evita acumular custos desnecessários. Foram criados três grupos de recusos: Azure AI Search, Azure AI services e Storage account.
‑
#### Deletando o Azure AI Search
>    1. Abri o portal do Azure (<https://portal.azure.com/>) e selecionei o grupo de recursos _LabCogSearch_.  
>    2. Clicar no recurso e selecionar _Excluir_ e depois _Sim_ para confirmar. O recurso foi excluído.
>
#### Deletando o Azure AI services
>    1. De volta ao portal do Azure (<https://portal.azure.com/>), selecionei o grupo de recursos _CogLabSearch_.  
>    2. Clicar no recurso e selecionar _Excluir_ e depois _Sim_ para confirmar. O recurso foi excluído.
>
-
#### Deletando o Storage account
>    1. Voltando ao portal do Azure (<https://portal.azure.com/>), selecionei o grupo de recursos _cogsrchstg_.  
>    2. Selecionei o recurso e depois _Excluir_ e _Sim_ para confirmar. O recurso foi excluído.
>
-
>>  - **Observação:** Com todos os três recursos excluídos, a plataforma informou que houve consumo de recursos na ordem de R$ 0,55, restando ainda um crédito de R$ 987,63, na assinatura gratuita.
>
> 
Após completar o Desafio, fiz a atualização no repositório local com o comando git pull.  

>- No repositório local;  
>  Clica no botão direito do mouse; e
>- Opção: "Open Git Bash here"
> No terminal digita o comando Git Pull <endereço do repositório remoto>

~~~bash
$ git pull https://github.com/z3mafra/cognitive-search.git
~~~

### **Postar link do repositório**  

Como é solicitado no enunciado do desafio, o link do GitHub deve ser compartilhado na plataforma da DIO. Para isso, segui os seguintes passos:
>
>     1. Abra o portal da DIO (<https://www.dio.me/>), com as credenciais e selecione o [Bootcamp Microsoft AZure AI Fundamentals](https://web.dio.me/track/microsoft-azure-ai-fundamentals).
>    2. Entra na Atividade Desafio de projeto: "Reconhecimento Facial e transformação de imagens em Dados no Azure ML"
>    3. Clica no botão "ENTREGAR PROJETO" 
>    4. Cola o link do projeto na caixa de texto "Repositório do Projeto"; Cola a descrição do repositório; Ler e marcar o Check box "Termos de uso", concordando com os termos o recurso; e,
>    5. Selecione Entregar. O Desafio é então entregue.
>

--------
--------
