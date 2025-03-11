# Lab5
# Explorando um Índice de Pesquisa do Azure AI

## Introdução
Este guia fornece um passo a passo detalhado sobre como configurar e explorar um índice de pesquisa no Azure AI. O objetivo é criar uma solução de mineração de conhecimento para analisar e recuperar insights de avaliações de clientes da Fourth Coffee.

## Objetivos
- Criar recursos do Azure
- Extrair dados de uma fonte de dados
- Enriquecer os dados com habilidades de IA
- Usar o indexador do Azure no portal do Azure
- Consultar o índice de pesquisa
- Revisar os resultados salvos em um Knowledge Store

## 1. Criando Recursos do Azure
Para configurar o ambiente, serão necessários os seguintes recursos no Azure:

- **Azure AI Search**: Gerencia a indexação e a consulta de dados.
- **Azure AI Services**: Fornece habilidades de IA para enriquecer os dados.
- **Conta de Armazenamento do Azure**: Armazena documentos brutos e coleções de dados.

### 1.1 Criar um recurso de Pesquisa do Azure AI
1. Acesse o portal do Azure.
2. Clique em `+ Criar um recurso` e pesquise por `Azure AI Search`.
3. Configure os seguintes parâmetros:
   - Assinatura: (seu plano Azure)
   - Grupo de recursos: (escolha ou crie um novo grupo)
   - Nome do serviço: (nome exclusivo)
   - Localização: (por exemplo, "East US 2")
   - Nível de preço: `Básico`
4. Clique em `Revisar + Criar` e depois `Criar`.
5. Após a implantação, acesse o recurso.

### 1.2 Criar um recurso de AI Services
1. Volte à página inicial do Azure.
2. Clique em `+ Criar um recurso` e pesquise `Azure AI Services`.
3. Configure os seguintes parâmetros:
   - Grupo de recursos: (o mesmo do Azure AI Search)
   - Região: (o mesmo do Azure AI Search)
   - Nome: (nome exclusivo)
   - Nível de preço: `Standard S0`
4. Clique em `Revisar + Criar` e depois `Criar`.

### 1.3 Criar uma Conta de Armazenamento
1. Volte ao portal do Azure e clique em `+ Criar um recurso`.
2. Pesquise por `Conta de Armazenamento` e configure:
   - Grupo de recursos: (o mesmo utilizado anteriormente)
   - Nome da conta de armazenamento: (nome exclusivo)
   - Localização: (qualquer disponível)
   - Redundância: `LRS (Armazenamento redundante local)`
3. Clique em `Revisar + Criar` e depois `Criar`.

## 2. Carregar Documentos no Armazenamento do Azure
1. Acesse sua conta de armazenamento e clique em `Contêineres`.
2. Crie um novo contêiner chamado `coffee-reviews`.
3. Defina `Nível de acesso público` como `Container`.
4. Baixe os dados de avaliações de [este link](https://aka.ms/mslearn-coffee-reviews) e extraia os arquivos.
5. No contêiner `coffee-reviews`, clique em `Upload` e envie os arquivos extraídos.

## 3. Criar um Índice de Pesquisa
1. No portal do Azure, acesse o recurso `Azure AI Search`.
2. Clique em `Importar Dados`.
3. Escolha `Azure Blob Storage` como fonte de dados e configure:
   - Nome da fonte de dados: `coffee-customer-data`
   - Dados a extrair: `Conteúdo e metadados`
   - String de conexão: `Escolha a conexão existente` e selecione `coffee-reviews`.
4. Clique em `Avançar: Adicionar habilidades cognitivas`.

### 3.1 Adicionar Habilidades Cognitivas
1. Selecione seu recurso de AI Services.
2. Nomeie o Skillset como `coffee-skillset`.
3. Ative `Habilitar OCR` e defina o campo de dados de origem para `merged_content`.
4. Selecione habilidades adicionais como:
   - Extração de nomes de localização
   - Extração de frases-chave
   - Detecção de sentimento
   - Geração de tags e legendas de imagem

### 3.2 Criar um Armazenamento de Conhecimento
1. Escolha `Escolher uma conexão existente` e selecione sua conta de armazenamento.
2. Crie um contêiner chamado `knowledge-store`.
3. Ative `Azure Blob Projections` e selecione `Document`.

### 3.3 Criar um Indexador
1. Nomeie o indexador como `coffee-indexer`.
2. Defina `Chave` como `metadata_storage_path`.
3. Ative `Base-64 Encode Keys`.
4. Clique em `Enviar` para iniciar o processamento dos dados.

## 4. Consultar o Índice
1. No recurso `Azure AI Search`, acesse `Explorador de Pesquisa`.
2. Mude a visualização para `JSON View`.
3. Execute a seguinte consulta para retornar todos os documentos:
   ```json
   {
       "search": "*",
       "count": true
   }
   ```
4. Para filtrar por localização:
   ```json
   {
       "search": "locations:'Chicago'",
       "count": true
   }
   ```
5. Para filtrar por sentimento negativo:
   ```json
   {
       "search": "sentiment:'negative'",
       "count": true
   }
   ```

## 5. Revisar o Repositório de Conhecimento
1. Acesse a conta de armazenamento no portal do Azure.
2. Navegue até `Containers` > `knowledge-store`.
3. Abra um arquivo `objectprojection.json` para ver os dados enriquecidos.
4. No contêiner `coffee-skillset-image-projection`, abra um arquivo `.jpg` para visualizar imagens armazenadas.
5. No painel esquerdo, selecione `Storage Browser` > `Tables`.
6. Acesse a tabela `coffeeSkillsetKeyPhrases` para revisar frases-chave extraídas.

## Conclusão
Seguindo esse processo, criamos um índice de pesquisa poderoso com enriquecimento de IA no Azure AI Search. Esse sistema permite consultar dados de avaliações de clientes de maneira eficiente e explorar insights valiosos para a Fourth Coffee.

