**Opinião:**
  É um ótimo serviço de IA, as possibilidades para o mercado são enorme. Reduz problemas como queima de arquivo por perdas naturais, se for implementado com tecnologia blockchain, pode expandir as possibilidades.

**Crie um recurso do Azure AI Search**

1- Entre no portal do Azure .
2-Clique no botão + Criar um recurso , pesquise Azure AI Search e crie um recurso Azure AI Search com as seguintes configurações:

  *Grupo de recursos:* Selecione ou crie um grupo de recursos com um nome exclusivo .
  *Nome do serviço:* um nome exclusivo .
  *Localização:* Escolha qualquer região disponível .
  *Nível de preços:* Básico
  Selecione Review + create e depois de ver a resposta Validation Success , selecione Create .

  Após a conclusão da implantação, selecione Ir para o recurso . Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.

**Crie um recurso de serviços de IA do Azure**

  Você precisará provisionar um recurso de serviços de IA do Azure que esteja no mesmo local que seu recurso do Azure AI Search. Sua solução de pesquisa usará esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

  Retorne à página inicial do portal do Azure. Clique no botão ＋Criar um recurso e pesquise os serviços de IA do Azure . Selecione criar um plano de serviços de IA do Azure . Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:

  *Assinatura:* sua assinatura do Azure .
  *Grupo de recursos:* O mesmo grupo de recursos que seu recurso do Azure AI Search .
  *Região:* o mesmo local do recurso do Azure AI Search .
  *Nome:* Um nome exclusivo .
  *Nível de preços:* Padrão S0
  *Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo:* Selecionado

  Selecione Revisar + criar . Depois de ver a resposta Validation Passed , selecione Create .

  Aguarde a conclusão da implantação e visualize os detalhes da implantação.
Crie uma conta de armazenamento


**Crie um Armazenamento**

  Retorne à página inicial do portal do Azure e selecione o botão + Criar um recurso .

  Procure conta de armazenamento e crie um recurso de conta de armazenamento com as seguintes configurações:
  Assinatura : sua assinatura do Azure .
  Grupo de recursos : O mesmo grupo de recursos que os recursos do Azure AI Search e dos serviços Azure AI .
  Nome da conta de armazenamento : um nome exclusivo .
  Localização : Escolha qualquer localização disponível .
  Padrão de desempenho
  Redundância : armazenamento localmente redundante (LRS)
  Clique em Revisar e em Criar . Aguarde a conclusão da implantação e vá para o recurso implantado.

  Na conta de Armazenamento do Azure que você criou, no painel de menu esquerdo, selecione Configuração (em Configurações ).
  Altere a configuração de Permitir acesso anônimo de Blob para Habilitado e selecione Salvar .

**Carregar documentos para o armazenamento do Azure**

  Crie um Container no azure
  Importe os dados: 
  No portal do Azure, navegue até o recurso Azure AI Search. Na página Visão geral , selecione Importar dados .
  Na página Conectar-se aos seus dados , na lista Fonte de Dados , selecione Azure Blob Storage . Preencha os detalhes do armazenamento de dados com os seguintes valores:
  *Fonte de dados:* Armazenamento de Blobs do Azure
  *Nome da fonte de dados : ******
  *Dados a extrair : Conteúdo e metadados
  *Modo de análise:* Padrão
  *Cadeia de conexão:* *Selecione Escolha uma conexão existente . Selecione sua conta de armazenamento, selecione o contêiner de avaliações de café e clique em Selecionar .
  *Autenticação de identidade gerenciada:* Nenhuma 
  *Nome do contêiner:* esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente .
  *Pasta Blob:* deixe em branco .
  *Descrição:* Avaliações sobre *******.

  Selecione Próximo: Adicionar habilidades cognitivas (opcional) .

  Na secção Anexar Serviços Cognitivos , selecione o seu recurso de serviços Azure AI.

  *Na seção Adicionar enriquecimentos:*

Altere o nome da qualificação para coffee-skillse .

  Selecione os seguintes campos enriquecidos:

Habilidade Cognitiva	Parâmetro	Nome do campo
Extraia nomes de locais	 	Localizações
Extraia frases-chave	 	frases chave
Detectar sentimento	 	sentimento
Gerar tags de imagens	 	imagemTags
Gere legendas de imagens	 	legenda da imagem

  *Em Salvar enriquecimentos em um armazenamento de conhecimento , selecione:*

Projeções de imagem
Documentos
Páginas
Frases chave
Entidades
Detalhes da imagem
Referências de imagem

Marque a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content .

Certifique-se de que o campo Dados de origem esteja configurado como merged_content .
Altere o nível de granularidade de enriquecimento para Páginas (blocos de 5.000 caracteres) .
Não selecione Habilitar enriquecimento incremental

Selecione projeções de blob do Azure: Documento . Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento . Não altere o nome do contêiner.
*Selecione Próximo:* Personalizar índice de destino . Altere o nome do índice para coffee-index .
Certifique-se de que a chave esteja configurada como metadata_storage_path . Deixe o nome do sugeridor em branco e o modo de pesquisa preenchido automaticamente.
Revise as configurações padrão dos campos de índice. Selecione filtrável para todos os campos que já estão selecionados por padrão.

*Selecione Próximo:*Criar um indexador .

Altere o nome do indexador para coffee-indexer .

Deixe a programação definida como Once .

Expanda as opções avançadas . Certifique-se de que a opção Base-64 Encode Keys esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

Selecione Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
Mapeia os campos extraídos para o índice.
Volte à página de recursos do Azure AI Search. No painel esquerdo, em Gerenciamento de pesquisa , selecione Indexadores . Selecione o indexador de café recém-criado . Espere um minuto e selecione ↻ Atualize até que o Status indique sucesso.
