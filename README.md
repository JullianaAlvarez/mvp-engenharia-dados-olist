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

As tabelas da camada Bronze foram documentadas utilizando o **Unity Catalog do Databricks**.

Para cada tabela foram cadastradas informações de contexto e origem dos dados. Para seus respectivos campos foram documentados o significado, o tipo de dado e o domínio observado, incluindo, conforme aplicável, intervalos de valores, categorias possíveis, possibilidade de valores nulos e características dos identificadores.

A origem dos dados também foi registrada nas descrições das tabelas, permitindo identificar o arquivo do dataset Olist responsável por cada objeto da camada Bronze.

O catálogo foi construído a partir do perfilamento dos dados realizado no Databricks, preservando na camada Bronze a estrutura recebida da fonte. Dessa forma, eventuais tratamentos, padronizações e regras de negócio são realizados apenas nas etapas posteriores do pipeline.

#### Evidência do catálogo

A imagem abaixo apresenta um exemplo da documentação da tabela `bronze.orders` no Unity Catalog, contendo a descrição da tabela, os tipos de dados e os comentários cadastrados para seus campos.

![Catálogo das tabelas bronze no Unity Catalog](images)

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
| `dm_tempo` | Dimensão | Uma linha por data (`id_data`) | Análises temporais dos pedidos e vendas |

A tabela `ft_vendas` mantém também os identificadores `id_vendedor` e `id_cliente_unico`, permitindo segmentações por vendedor e consumidor mesmo sem a criação de dimensões específicas para essas entidades no escopo atual do MVP.

A dimensão `dm_tempo` é utilizada como referência para a data de compra, permitindo análises por dia, mês, trimestre e ano. As demais datas relacionadas ao ciclo do pedido permanecem como atributos da `ft_pedidos`.

Devido à existência de múltiplos eventos de negócio com granularidades distintas, o modelo possui múltiplas tabelas fato compartilhando dimensões analíticas. Dessa forma, sua organização pode ser caracterizada como uma **constelação de fatos baseada em princípios de modelagem Star Schema**.
