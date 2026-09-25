# MVP de Engenharia de Dados

## Pipeline de Dados na Nuvem — Brazilian E-Commerce Olist

Este projeto foi desenvolvido como MVP da disciplina de Engenharia de Dados, com o objetivo de construir um pipeline de dados em ambiente de nuvem.

## 1. Contexto de Negócio e Perguntas

### 1.1 Dataset

**Brazilian E-Commerce Public Dataset by Olist**

O conjunto de dados utilizado neste projeto é uma base pública brasileira de e-commerce disponibilizada pela Olist. A base contém informações sobre aproximadamente 100 mil pedidos realizados entre 2016 e 2018 por meio da Olist Store em diferentes marketplaces brasileiros.

Os dados abrangem diferentes etapas e entidades do processo de compra, incluindo informações sobre pedidos, itens, produtos, clientes, vendedores, pagamentos, avaliações e localização geográfica. Essa estrutura permite analisar o processo de venda sob diferentes perspectivas e relacionar aspectos comerciais, geográficos, logísticos e de experiência do cliente.

**Fonte:** Brazilian E-Commerce Public Dataset by Olist — Kaggle

**Licença dos dados:** CC BY-NC-SA 4.0 (Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International). A licença permite o compartilhamento e a adaptação do material, desde que seja atribuída a devida autoria à fonte original, que sua utilização não tenha finalidade comercial e que eventuais adaptações sejam distribuídas sob a mesma licença.
Neste projeto, os dados são utilizados exclusivamente para fins acadêmicos, mantendo a referência à fonte original.

### 1.2 Problema de Negócio

O presente projeto busca compreender a evolução e as características das vendas realizadas por vendedores integrados ao ecossistema Olist em diferentes marketplaces brasileiros, identificando padrões relacionados ao desempenho comercial, às categorias de produtos, à distribuição geográfica e ao comportamento de compra dos clientes, além de explorar aspectos relacionados à operação logística e à experiência do consumidor.

### 1.3 Pergunta Central

**Como os aspectos comerciais, geográficos e operacionais caracterizam os pedidos realizados no ecossistema Olist e de que forma o desempenho logístico se relaciona com a experiência dos clientes?**

Para responder ao problema central, a análise será estruturada em cinco dimensões:

1. **Comercial:** Como o volume de pedidos e o valor vendido evoluíram ao longo do período analisado?

2. **Produtos:** Quais categorias de produtos apresentam maior participação no valor vendido e no volume de vendas, e como se diferenciam em relação ao ticket médio?

3. **Clientes:** Como os clientes e pedidos estão distribuídos geograficamente e qual é o comportamento de recompra?

4. **Pagamentos:** Quais são as principais formas de pagamento utilizadas nos pedidos e como os clientes utilizam parcelamento e múltiplos meios de pagamento?

5. **Experiência:** Pedidos entregues após a data estimada apresentam avaliações diferentes daqueles entregues dentro ou antes do prazo?

### 1.4 Estrutura dos Dados Brutos

O dataset da Olist é composto por diferentes arquivos relacionados entre si por identificadores de pedidos, clientes, produtos e vendedores. Para o desenvolvimento deste MVP, serão utilizadas as tabelas descritas abaixo.

#### `olist_orders_dataset`

Tabela central de pedidos, utilizada para identificar cada compra e acompanhar as principais etapas do processo de entrega.

**Principais campos:**
- `order_id`: identificador único do pedido.
- `customer_id`: identificador do cliente associado ao pedido.
- `order_status`: status do pedido.
- `order_purchase_timestamp`: data e hora da compra.
- `order_approved_at`: data e hora da aprovação do pagamento.
- `order_delivered_carrier_date`: data de entrega do pedido ao parceiro logístico.
- `order_delivered_customer_date`: data efetiva de entrega ao cliente.
- `order_estimated_delivery_date`: data estimada de entrega.

---

#### `olist_order_items_dataset`

Contém os itens pertencentes a cada pedido e estabelece a ligação entre pedidos, produtos e vendedores.

**Principais campos:**
- `order_id`: identificador do pedido.
- `order_item_id`: número sequencial do item dentro do pedido.
- `product_id`: identificador do produto.
- `seller_id`: identificador do vendedor.
- `shipping_limit_date`: data limite para envio do produto pelo vendedor.
- `price`: preço do item.
- `freight_value`: valor do frete associado ao item.

---

#### `olist_products_dataset`

Contém as características dos produtos comercializados.

**Principais campos:**
- `product_id`: identificador único do produto.
- `product_category_name`: categoria do produto.
- `product_name_lenght`: quantidade de caracteres do nome do produto.
- `product_description_lenght`: quantidade de caracteres da descrição.
- `product_photos_qty`: quantidade de fotos cadastradas.
- `product_weight_g`: peso do produto em gramas.
- `product_length_cm`: comprimento do produto.
- `product_height_cm`: altura do produto.
- `product_width_cm`: largura do produto.

---

#### `olist_customers_dataset`

Contém informações dos clientes e sua localização. A base possui dois identificadores de cliente: `customer_id`, associado a cada pedido, e `customer_unique_id`, que permite identificar compras realizadas pelo mesmo cliente em pedidos diferentes.

**Principais campos:**
- `customer_id`: identificador do cliente associado ao pedido.
- `customer_unique_id`: identificador único do cliente.
- `customer_zip_code_prefix`: prefixo do CEP do cliente.
- `customer_city`: cidade do cliente.
- `customer_state`: estado do cliente.

---

#### `olist_sellers_dataset`

Contém informações sobre os vendedores responsáveis pelos produtos comercializados nos pedidos.

**Principais campos:**
- `seller_id`: identificador único do vendedor.
- `seller_zip_code_prefix`: prefixo do CEP do vendedor.
- `seller_city`: cidade do vendedor.
- `seller_state`: estado do vendedor.

---

#### `olist_order_reviews_dataset`

Contém as avaliações realizadas pelos clientes após os pedidos.

**Principais campos:**
- `review_id`: identificador da avaliação.
- `order_id`: identificador do pedido.
- `review_score`: nota de satisfação atribuída pelo cliente, de 1 a 5.
- `review_comment_title`: título do comentário.
- `review_comment_message`: comentário da avaliação.
- `review_creation_date`: data de criação da pesquisa de satisfação.
- `review_answer_timestamp`: data e hora da resposta à pesquisa.

---

#### olist_order_payments_dataset

Contém informações sobre os pagamentos associados aos pedidos. Um mesmo pedido pode apresentar mais de um registro de pagamento.

*Principais campos:*
- order_id: identificador do pedido.
- payment_sequential: sequência do pagamento dentro do pedido.
- payment_type: método de pagamento utilizado.
- payment_installments: número de parcelas.
- payment_value: valor da transação.


### 1.5 Relacionamento entre os Dados

A tabela `olist_orders_dataset` funciona como o principal ponto de conexão do conjunto de dados.

Os principais relacionamentos utilizados no projeto são:

- Pedidos → Clientes: `customer_id`
- Pedidos → Itens: `order_id`
- Pedidos → Avaliações: `order_id`
- Pedidos -> Pagamentos: `order_id`
- Itens → Produtos: `product_id`
- Itens → Vendedores: `seller_id`
- Produtos → Tradução de Categorias: `product_category_name`


## 2. Carga dos Dados

Os dados utilizados neste projeto são disponibilizados publicamente no Kaggle. Para tornar o processo de obtenção dos dados reproduzível e evitar a necessidade de download e upload manual dos arquivos, foi utilizada a biblioteca `kagglehub` para realizar a transferência do dataset diretamente para o ambiente Databricks.

### 2.1 Importação do dataset

Inicialmente, a biblioteca `kagglehub` foi instalada no ambiente Databricks:

```python
%pip install kagglehub
```
Em seguida, o dataset foi obtido diretamente do Kaggle:
```python
import kagglehub

path = kagglehub.dataset_download(
    "olistbr/brazilian-ecommerce"
)

print("Path to dataset files:", path)
```
O comando realiza o download da versão disponível do dataset e retorna o diretório em que os arquivos foram disponibilizados no ambiente Databricks.

### 2.2 Validação dos arquivos recebidos
Após a importação, foi realizada uma verificação dos arquivos disponibilizados:
```python
import os
arquivos = os.listdir(path)
for arquivos in arquivos:
  print(arquivos)
```
Foram identificados nove arquivos no dataset original. Considerando as perguntas de negócio definidas para esse MVP, serão utilizados os arquivos relacionados a pedidos, itens, produtos, clientes, vendedores, pagamentos, avaliações e tradução das categorias de produtos.

## 3. Modelagem e Catálogo de Dados

Antes da definição e implementação do modelo analítico, os arquivos selecionados foram persistidos no Databricks em uma camada **Bronze**, preservando a estrutura e a granularidade dos dados de origem.

A análise inicial dessas tabelas teve como objetivo compreender a granularidade dos dados, seus relacionamentos, domínios de valores e características relevantes para a posterior definição do modelo analítico.

### 3.1 Camada Bronze

Foram materializadas oito tabelas na camada Bronze:

| Tabela | Origem | Granularidade |
|---|---|---|
| `bronze.orders` | `olist_orders_dataset.csv` | Uma linha por pedido (`order_id`) |
| `bronze.order_items` | `olist_order_items_dataset.csv` | Uma linha por item do pedido (`order_id` + `order_item_id`) |
| `bronze.products` | `olist_products_dataset.csv` | Uma linha por produto (`product_id`) |
| `bronze.customers` | `olist_customers_dataset.csv` | Uma linha por `customer_id` |
| `bronze.sellers` | `olist_sellers_dataset.csv` | Uma linha por vendedor (`seller_id`) |
| `bronze.order_reviews` | `olist_order_reviews_dataset.csv` | Uma linha por avaliação associada ao pedido (`order_id` + `review_id`) |
| `bronze.order_payments` | `olist_order_payments_dataset.csv` | Uma linha por registro de pagamento (`order_id` + `payment_sequential`) |
| `bronze.category_translation` | `product_category_name_translation.csv` | Uma linha por categoria de produto em português |

A granularidade das tabelas foi validada por meio de consultas exploratórias antes da definição do modelo analítico. Essa análise evidenciou, por exemplo, que um pedido pode possuir múltiplos itens, registros de pagamento e avaliações.

Também foi identificada uma particularidade importante na base de clientes: o campo `customer_id` identifica o registro de cliente associado ao pedido, enquanto `customer_unique_id` permite reconhecer um mesmo consumidor em diferentes pedidos. Essa distinção será utilizada posteriormente na análise de recompra.

### 3.2 Catálogo de Dados

As tabelas das camadas Bronze, Silver e Gold foram documentadas utilizando o **Unity Catalog do Databricks**.

Na camada Bronze, para cada tabela foram cadastradas informações de contexto e origem dos dados. Para seus respectivos campos foram documentados o significado, o tipo de dado e o domínio observado, incluindo, conforme aplicável, intervalos de valores, categorias possíveis, possibilidade de valores nulos e características dos identificadores.

A origem dos dados também foi registrada nas descrições das tabelas Bronze, permitindo identificar o arquivo do dataset Olist responsável por cada objeto. O catálogo foi construído a partir do perfilamento realizado no Databricks, preservando nessa camada a estrutura recebida da fonte.

Na camada Silver, foram documentadas as tabelas resultantes dos processos de qualidade e preparação dos dados, registrando o contexto de cada objeto e as principais alterações realizadas em relação à camada Bronze, como adequações de tipos, padronizações e enriquecimentos. Essa documentação permite acompanhar a evolução dos dados entre a fonte original e sua posterior utilização no modelo analítico.

Na camada Gold, a catalogação foi realizada após a construção do modelo analítico descrito na seção 3.3. Foram documentados o contexto e a granularidade de cada tabela fato e dimensão, além do significado, tipo e domínio dos campos utilizados para análise.

Para os campos derivados ou agregados, a documentação também registra as principais regras utilizadas em sua construção. Entre elas estão a geração dos atributos da dimensão de tempo, a consolidação dos valores dos itens por pedido e o cálculo da quantidade e da nota média das avaliações.

A linhagem dos dados foi registrada e validada por meio do **Unity Catalog**, permitindo acompanhar as dependências entre as tabelas ao longo das camadas Bronze, Silver e Gold e identificar as principais fontes utilizadas na construção dos objetos analíticos.

| Tabela Gold | Principais origens |
|---|---|
| `dm_tempo` | `silver.orders` |
| `dm_produto` | `silver.products` |
| `ft_pagamentos` | `silver.order_payments` |
| `ft_vendas` | `silver.order_items`, `silver.orders` e `silver.customers` |
| `ft_pedidos` | `silver.orders`, `silver.customers`, `silver.order_items` e `silver.order_reviews` |

As descrições detalhadas dos campos que compõem cada tabela Gold são apresentadas na seção **3.3.1 Estrutura das tabelas do modelo analítico**.

#### Evidências do catálogo

O link abaixo apresenta screenshots da documentação do schema `bronze` e `silver` no Unity Catalog, contendo a descrição da tabela, os tipos de dados e os comentários cadastrados para seus campos.

[Catálogo das tabelas Bronze no Unity Catalog](images/catalogo_bronze)

[Catálogo das tabelas Silver no Unity Catalog](images/catalogo_silver)

A documentação da camada Gold segue o mesmo padrão, incluindo o contexto das tabelas, granularidade e descrição dos campos do modelo analítico.

[Catálogo das tabelas Gold no Unity Catalog](images/catalogo_gold)


### 3.3 Modelo Analítico

Para a camada analítica foi adotada uma **modelagem dimensional baseada em Star Schema**, separando os eventos de negócio em tabelas fato e os atributos utilizados para contextualização das análises em tabelas dimensão.

A definição do modelo foi realizada após a exploração das tabelas da camada Bronze, considerando principalmente a granularidade das fontes e as perguntas de negócio definidas para o projeto.

Enquanto a camada Bronze preserva a nomenclatura original dos dados de origem, as tabelas do modelo analítico adotam uma nomenclatura padronizada em português, buscando maior clareza e consistência na utilização dos dados.

Foram identificados três eventos com granularidades distintas:

- **Pedido:** uma linha por `id_pedido`;
- **Venda:** uma linha por item de pedido, identificada por `id_pedido` + `id_item_pedido`;
- **Pagamento:** uma linha por registro de pagamento, identificada por `id_pedido` + `sequencial_pagamento`.

A separação dessas informações em diferentes tabelas fato evita a multiplicação indevida de registros em relacionamentos entre tabelas com cardinalidade 1:N, como ocorre com itens e pagamentos de um mesmo pedido.

O modelo analítico é composto pelas seguintes tabelas:

| Tabela | Tipo | Granularidade | Finalidade |
|---|---|---|---|
| `ft_pedidos` | Fato | Uma linha por pedido (`id_pedido`) | Análises de pedidos, clientes e experiência |
| `ft_vendas` | Fato | Uma linha por item (`id_pedido` + `id_item_pedido`) | Análises comerciais e de produtos |
| `ft_pagamentos` | Fato | Uma linha por registro de pagamento (`id_pedido` + `sequencial_pagamento`) | Análises de meios de pagamento e parcelamento |
| `dm_produto` | Dimensão | Uma linha por produto (`id_produto`) | Características e categorias dos produtos |
| `dm_tempo` | Dimensão | Uma linha por data (`data`) | Análises temporais dos pedidos e vendas |

A tabela `ft_vendas` mantém também os identificadores `id_vendedor` e `id_cliente_unico`, permitindo segmentações por vendedor e consumidor mesmo sem a criação de dimensões específicas para essas entidades no escopo atual do MVP.

Devido à existência de múltiplos eventos de negócio com granularidades distintas, o modelo possui múltiplas tabelas fato compartilhando dimensões analíticas. Dessa forma, sua organização pode ser caracterizada como uma **constelação de fatos baseada em princípios de modelagem Star Schema**.

#### 3.3.1 Estrutura das tabelas do modelo analítico

A estrutura das tabelas foi definida considerando a granularidade de cada evento de negócio e as informações necessárias para responder às perguntas propostas no MVP.

##### `ft_pedidos`

Possui granularidade de **uma linha por pedido** e concentra informações relacionadas ao cliente, status do pedido, ciclo de entrega, valores consolidados e avaliação.

| Campo | Descrição |
|---|---|
| `id_pedido` | Identificador único do pedido |
| `id_cliente` | Identificador do cliente associado ao pedido |
| `id_cliente_unico` | Identificador utilizado para reconhecer o mesmo consumidor em diferentes pedidos |
| `cidade_cliente` | Cidade associada ao cliente no pedido |
| `uf_cliente` | UF associada ao cliente no pedido |
| `status_pedido` | Status do pedido |
| `data_compra` | Identificador da data de compra para relacionamento com `dm_tempo` |
| `data_hora_compra` | Data e hora de realização da compra |
| `data_hora_aprovacao` | Data e hora de aprovação do pedido |
| `data_hora_envio_transportadora` | Data e hora de envio do pedido à transportadora |
| `data_hora_entrega_cliente` | Data e hora de entrega ao cliente |
| `data_estimada_entrega` | Data estimada para entrega do pedido |
| `qtd_itens` | Quantidade de itens presentes no pedido |
| `valor_produtos` | Soma do valor dos itens do pedido |
| `valor_frete` | Soma do valor de frete dos itens do pedido |
| `valor_total_pedido` | Soma do valor dos produtos e do frete |
| `qtd_avaliacoes` | Quantidade de avaliações associadas ao pedido |
| `nota_media_avaliacao` | Média das notas das avaliações associadas ao pedido |

##### `ft_vendas`

Possui granularidade de **uma linha por item de pedido**, identificada pela combinação `id_pedido` + `id_item_pedido`.

| Campo | Descrição |
|---|---|
| `id_pedido` | Identificador do pedido |
| `id_item_pedido` | Número sequencial do item dentro do pedido |
| `id_produto` | Identificador do produto |
| `id_vendedor` | Identificador do vendedor responsável pelo item |
| `id_cliente_unico` | Identificador do consumidor associado ao pedido |
| `data_compra` | Data de realização da compra utilizada para relacionamento com `dm_tempo` |
| `valor_item` | Valor de venda do item |
| `valor_frete_item` | Valor de frete associado ao item |

##### `ft_pagamentos`

Possui granularidade de **uma linha por registro de pagamento**, identificada pela combinação `id_pedido` + `sequencial_pagamento`.

| Campo | Descrição |
|---|---|
| `id_pedido` | Identificador do pedido |
| `sequencial_pagamento` | Sequência do registro de pagamento dentro do pedido |
| `tipo_pagamento` | Meio de pagamento utilizado |
| `qtd_parcelas` | Quantidade de parcelas associadas ao pagamento |
| `valor_pagamento` | Valor registrado para o pagamento |

##### `dm_produto`

Possui granularidade de **uma linha por produto** e reúne os atributos utilizados para caracterização e segmentação dos produtos.

| Campo | Descrição |
|---|---|
| `id_produto` | Identificador único do produto |
| `categoria_produto` | Categoria original do produto em português |
| `categoria_produto_ingles` | Tradução da categoria do produto para inglês |
| `tamanho_nome_produto` | Quantidade de caracteres do nome do produto |
| `tamanho_descricao_produto` | Quantidade de caracteres da descrição do produto |
| `qtd_fotos_produto` | Quantidade de fotos cadastradas para o produto |
| `peso_produto_g` | Peso do produto em gramas |
| `comprimento_produto_cm` | Comprimento do produto em centímetros |
| `altura_produto_cm` | Altura do produto em centímetros |
| `largura_produto_cm` | Largura do produto em centímetros |

##### `dm_tempo`

Possui granularidade de **uma linha por data** e permite a realização de análises temporais em diferentes níveis de agregação.

| Campo | Descrição |
|---|---|
| `data` | Data de referência da dimensão |
| `dia` | Dia do mês |
| `mes` | Número do mês |
| `nome_mes` | Nome do mês |
| `trimestre` | Trimestre do ano |
| `ano` | Ano |

A dimensão `dm_tempo` é utilizada como referência para a data de compra, permitindo análises por dia, mês, trimestre e ano. As demais datas relacionadas ao ciclo do pedido permanecem como atributos da ft_pedidos.

#### 3.3.2 Diagrama do Modelo Analítico

O diagrama abaixo apresenta a estrutura do modelo analítico proposto, suas tabelas fato e dimensão e os relacionamentos utilizados entre elas.

![Modelo dimensional](images/modelo_dimensional.png)

Os relacionamentos definidos no modelo são:

| Tabela de origem | Campo | Cardinalidade | Tabela relacionada | Campo |
|---|---|---|---|---|
| `dm_tempo` | `data` | 1:N | `ft_pedidos` | `data_compra` |
| `dm_tempo` | `data` | 1:N | `ft_vendas` | `data_compra` |
| `dm_produto` | `id_produto` | 1:N | `ft_vendas` | `id_produto` |
| `ft_pedidos` | `id_pedido` | 1:N | `ft_pagamentos` | `id_pedido` |

A relação entre `ft_pedidos` e `ft_pagamentos` representa o fato de que um pedido pode possuir um ou mais registros de pagamento.

Embora `ft_pedidos` e `ft_vendas` compartilhem o identificador `id_pedido`, não foi definido um relacionamento direto entre essas tabelas no modelo. As duas tabelas representam eventos com granularidades distintas e foram mantidas de forma independente, preservando seus respectivos objetivos analíticos.

Da mesma forma, `ft_vendas` e `ft_pagamentos` não são relacionadas diretamente, evitando relacionamentos entre tabelas com múltiplos registros por pedido que poderiam resultar em multiplicação de linhas e duplicação de métricas durante as análises.

## 4. Pipeline de Dados

O pipeline de dados foi estruturado seguindo a arquitetura medalhão, organizando o processamento em três camadas: **Bronze**, **Silver** e **Gold**.

Para facilitar a organização, rastreabilidade e manutenção do processo, as etapas foram separadas em notebooks de acordo com suas responsabilidades:

| Notebook | Etapa | Finalidade |
|---|---|---|
| `01_ingestao_exploracao_bronze` | Bronze | Obtenção dos dados, ingestão das fontes, profiling, catalogação e análises exploratórias |
| `02_silver_qualidade` | Silver | Aplicação das regras de qualidade, tipagem, padronização e persistência dos dados tratados |
| `03_gold_modelagem` | Gold | Integração das fontes tratadas e construção das tabelas fato e dimensão do modelo analítico |

A camada **Bronze** preserva os dados provenientes das fontes com sua estrutura e granularidade originais. A camada **Silver** aplica as regras de qualidade e preparação necessárias para o consumo analítico. Por fim, a camada **Gold** integra os dados tratados e materializa o modelo dimensional definido para responder às perguntas de negócio do projeto.

Os notebooks utilizados na implementação do pipeline serão disponibilizados no repositório do projeto:

- [`01_ingestao_exploracao_bronze`](INSERIR_LINK_GITHUB)
- [`02_silver_qualidade`](INSERIR_LINK_GITHUB)
- [`03_gold_modelagem`](INSERIR_LINK_GITHUB)

### 4.1 Persistência das Camadas

As tabelas resultantes de cada etapa do pipeline foram persistidas no Databricks utilizando o formato Delta, organizadas nos schemas `bronze`, `silver` e `gold`.

Na camada Silver foram materializadas as tabelas:

- `silver.orders`
- `silver.order_items`
- `silver.products`
- `silver.customers`
- `silver.order_reviews`
- `silver.order_payments`

Nem todas as tabelas da camada Bronze originaram uma tabela independente na Silver. A tabela `category_translation` foi incorporada ao tratamento de `products`, enquanto os atributos da tabela `sellers` não foram necessários para o modelo analítico definido neste MVP. O identificador do vendedor foi preservado diretamente em `order_items`.

#### Evidência da camada Silver

![Tabelas da camada Silver](images/silver_tables.png)


Após os processos de qualidade e preparação realizados na camada Silver, os dados foram transformados e integrados na camada Gold para construção do modelo analítico.

A implementação da camada Gold foi realizada no notebook `03_gold_modelagem`, responsável pela materialização das tabelas fato e dimensão definidas durante a etapa de modelagem.

Foram construídas cinco tabelas analíticas:

- `dm_tempo`: dimensão utilizada para análises temporais;
- `dm_produto`: dimensão contendo os atributos descritivos e físicos dos produtos;
- `ft_pagamentos`: fato na granularidade de um registro por pedido e sequência de pagamento;
- `ft_vendas`: fato na granularidade de um registro por item do pedido;
- `ft_pedidos`: fato na granularidade de um registro por pedido.

As tabelas foram persistidas no schema `workspace.gold` em formato Delta e validadas após sua criação quanto à granularidade, unicidade das chaves, preservação dos registros e consistência dos relacionamentos.

#### Evidência da camada Gold

![Tabelas da camada Gold](images/gold_tables.png)

## 5. Qualidade e Transformação dos Dados

A análise de qualidade foi realizada a partir do profiling e das explorações da camada Bronze. Os tratamentos foram aplicados na camada Silver, buscando adequar tipos de dados, tratar inconsistências e padronizar informações sem alterar valores da fonte quando não existiam evidências suficientes para uma correção segura.

Como princípio geral, valores ausentes ou atípicos não foram automaticamente substituídos. Quando não foi possível determinar o valor correto a partir dos dados disponíveis, a informação original foi preservada e a ocorrência documentada.

### 5.1 Tratamentos realizados na camada Silver

| Fonte | Problema ou característica identificada | Tratamento adotado |
|---|---|---|
| `orders` | Campos temporais armazenados como `STRING` | Conversão para `TIMESTAMP` |
| `orders` | Datas de aprovação, envio e entrega ausentes, inclusive poucos casos em pedidos entregues | Valores nulos preservados por não existir informação suficiente para reconstrução das datas |
| `order_items` | `shipping_limit_date` armazenada como `STRING` | Conversão para `TIMESTAMP` |
| `order_items` | Quatro itens com limite de envio em 2020, aproximadamente três anos após a compra | Valores preservados e documentados como atípicos, sem imputação |
| `order_items` | `price` e `freight_value` armazenados como `DOUBLE` | Conversão para `DECIMAL(10,2)` |
| `products` | 610 produtos sem categoria e atributos descritivos | Categoria padronizada como `sem_categoria`; atributos quantitativos ausentes permaneceram nulos |
| `products` | Duas categorias, totalizando 13 produtos, sem tradução | Utilização do nome original da categoria como alternativa à tradução |
| `products` | Grafia `lenght` presente nos nomes de campos da fonte | Padronização para `length` |
| `products` | Campos de tamanho do nome, descrição e quantidade de fotos armazenados como `DOUBLE` | Conversão para `INT` após validação dos valores |
| `customers` | Não foram identificadas inconsistências que demandassem tratamento | Dados preservados sem alteração de conteúdo |
| `order_reviews` | Datas armazenadas como `STRING` | Conversão para `TIMESTAMP` |
| `order_reviews` | Títulos e mensagens opcionais com valores nulos | Nulos preservados por representarem ausência legítima de comentário |
| `order_reviews` | Possibilidade de múltiplas avaliações por pedido | Registros preservados na granularidade original; consolidação realizada posteriormente na Gold |
| `order_payments` | `payment_value` armazenado como `DOUBLE` | Conversão para `DECIMAL(10,2)` |
| `order_payments` | Sequência e quantidade de parcelas armazenadas como `BIGINT` | Conversão para `INT` |
| `order_payments` | 2 registros com zero parcelas, 3 com tipo `not_defined` e 9 com valor zero | Valores preservados por não existir evidência suficiente para determinar valores alternativos |

### 5.2 Transformações realizadas na camada Gold

A camada Gold foi construída a partir das tabelas tratadas na camada Silver, com o objetivo de integrar as diferentes fontes e adequar os dados às granularidades definidas no modelo analítico.

Diferentemente da camada Silver, concentrada principalmente em qualidade, tipagem e padronização, nesta etapa foram realizadas transformações de integração, agregação e derivação de atributos e métricas.

| Tabela Gold | Principais transformações |
|---|---|
| `dm_tempo` | Construção de um calendário entre a menor e a maior data de compra observada nos pedidos. Derivação dos atributos dia, mês, nome do mês, trimestre e ano. |
| `dm_produto` | Seleção dos atributos de produtos previamente tratados na Silver e adequação dos nomes dos campos para a nomenclatura em português adotada no modelo analítico. |
| `ft_pagamentos` | Seleção e renomeação dos campos tratados de pagamentos, preservando a granularidade de pedido + sequência de pagamento. |
| `ft_vendas` | Integração de itens, pedidos e clientes para construção da granularidade de venda por item. Inclusão do identificador único do cliente e da data da compra, além da adequação dos nomes dos campos para o modelo analítico. |
| `ft_pedidos` | Integração de pedidos e clientes com agregações prévias de itens e avaliações. Foram calculadas quantidade de itens, valores de produtos, frete e total do pedido, além da quantidade de avaliações e nota média por pedido. |

Para a construção de `ft_pedidos`, as tabelas de itens e avaliações foram agregadas separadamente antes dos relacionamentos com os pedidos. Essa estratégia evita a multiplicação de registros que poderia ocorrer em um relacionamento direto entre fontes com múltiplas ocorrências para o mesmo pedido.

Pedidos sem itens foram preservados no modelo, recebendo valor `0` para quantidade de itens e métricas monetárias derivadas. Da mesma forma, pedidos sem avaliações receberam `0` em `qtd_avaliacoes`, enquanto `nota_media_avaliacao` permaneceu nula, evitando a atribuição artificial de uma nota inexistente.

Como validação adicional, os valores de produtos e frete registrados em `ft_vendas` foram agregados e comparados com os valores consolidados em `ft_pedidos`. A comparação apresentou ausência de divergências tanto nos valores totais quanto na validação realizada pedido a pedido.

## 6. Análise de Dados

A etapa final do projeto consiste na análise dos dados disponibilizados na camada Gold, com o objetivo de responder às perguntas de negócio definidas no início do MVP.

As análises foram realizadas por meio de consultas SQL no Databricks e visualizações construídas a partir dos resultados obtidos. Para cada pergunta, buscou-se não apenas apresentar as métricas calculadas, mas também interpretar seu significado dentro do contexto do negócio.

Nas análises temporais, foi identificado que os períodos nas extremidades da base apresentam cobertura parcial. Os registros de 2016 estão concentrados principalmente a partir de outubro, enquanto setembro e outubro de 2018 possuem volumes muito inferiores aos meses anteriores. Por esse motivo, quando necessária a avaliação de tendências ao longo do tempo, as interpretações foram concentradas no período entre **janeiro de 2017 e agosto de 2018**.

### 6.1 Evolução do volume e valor dos pedidos

**Pergunta de negócio:** Como evoluíram o volume de pedidos e o valor associado aos pedidos ao longo do período analisado?

Para esta análise, foram considerados os pedidos realizados independentemente de seu status final, uma vez que o objetivo é observar o comportamento da demanda registrada na plataforma.

O **volume de pedidos** corresponde à quantidade de pedidos realizados em cada mês. O **valor dos pedidos** corresponde à soma do valor dos produtos associados aos pedidos, sem considerar o valor do frete.

A distribuição dos pedidos por status também foi analisada como visão complementar, permitindo avaliar se a evolução da demanda foi acompanhada por mudanças na participação de pedidos cancelados.

#### Evolução mensal do volume de pedidos

![Evolução mensal do volume de pedidos](images/dataviz/evolucao_pedidos.png)

#### Evolução mensal do valor dos pedidos

![Evolução mensal do valor dos pedidos](images/dataviz/evolucao_valor_pedidos.png)

#### Evolução da taxa de cancelamento

![Evolução mensal da taxa de cancelamento](images/dataviz/taxa_cancelamento.png)

#### Principais insights

A análise evidencia crescimento da demanda ao longo de 2017, com o volume mensal passando de 800 pedidos em janeiro para 4.631 em outubro. Em novembro ocorre um salto expressivo para 7.544 pedidos, maior volume observado no período, acompanhado por valor superior a R$ 1 milhão em produtos associados aos pedidos.

A análise diária de novembro mostra uma forte concentração de pedidos em **24/11/2017**, data da Black Friday daquele ano. Nesse dia foram registrados **1.176 pedidos e aproximadamente R$ 152,7 mil em produtos**, os maiores valores diários do mês. O comportamento observado é consistente com um possível efeito da Black Friday sobre a demanda, embora a base não permita identificar diretamente se os pedidos foram originados pela campanha.

Após o pico de novembro e a redução observada em dezembro, o volume retorna a um patamar elevado em 2018. Entre janeiro e agosto, foram registrados aproximadamente 6,2 mil a 7,3 mil pedidos por mês, indicando maior estabilidade em relação ao crescimento observado durante grande parte de 2017.

A taxa de cancelamento permaneceu baixa durante o período comparável, mas apresentou oscilações pontuais relevantes. Destacam-se março de 2017 (1,23%) e fevereiro de 2018 (1,09%), ambos seguidos por reduções expressivas nos meses seguintes. Agosto de 2018 apresentou a maior taxa do período comparável, de **1,29%**.

Apesar desses episódios, não foi observada uma trajetória contínua de crescimento da taxa de cancelamento. Os resultados indicam, portanto, expansão da demanda durante 2017 e manutenção de um patamar elevado em 2018, com evidência de sazonalidade relevante no período da Black Friday e taxas de cancelamento geralmente baixas, embora com picos específicos que podem justificar investigações adicionais.
