# poshe_sales_dashboard
Realiznado uma atividade prática utilizando o método dashboard simulando a criação de um site da Poshe contendo as informações sobre as vendas dos carros.

# 🏎️ PORSCHE INTELLIGENCE // Telemetria Executiva de Vendas

Documentação técnica e analítica do painel executivo interativo (**`porsche_dashboard.html`**) e da base de dados auditada (**`Planilha base Porsche (Sanitizada).xlsx`**).

---

## 📋 1. Visão Geral do Projeto

O **Porsche Intelligence** é uma dashboard analítica desenvolvida em **HTML5, CSS3 (Tailwind CSS) e JavaScript (Chart.js)** com identidade visual inspirada na linha **Porsche Exclusive Manufaktur** (combinando tons de vermelho *Guards Red* `#D5001C`, preto carbono `#09090B` e detalhes em dourado escovado `#D4AF37`).

O sistema opera de forma **100% client-side** (sem necessidade de servidor ou banco de dados externo), permitindo filtragem multidimensional instantânea, consultas estratégicas automatizadas e auditoria completa das operações comerciais.

---

## 📊 2. Documentação da Planilha Base (`Planilha base Porsche (Sanitizada).xlsx`)

### 2.1 Resumo Executivo da Base Auditada
* **Volume Total de Registros:** `100` transações de venda (`sale_id` de `#6` a `#105`).
* **Faturamento Bruto Total:** `$12.827.800,50`
* **Ticket Médio Geral:** `$128.278,01`
* **Amplitude de Preços:** `$58.900,00` (mínimo) a `$286.500,00` (máximo).
* **Quilometragem (Odômetro) Média:** `11.422 km` (incluindo `11` veículos **Zero Km**).
* **Cobertura de Portfólio:** `40` modelos distintos distribuídos em `6` famílias Porsche (`911`, `Cayenne`, `Macan`, `Taycan`, `Panamera` e `718`).
* **Cobertura Geográfica:** `79` cidades em `35` estados norte-americanos.
* **Safras (Anos de Fabricação):** `2020` a `2026`.

---

### 2.2 Dicionário de Dados & Sanitização

A tabela abaixo descreve cada campo extraído da planilha sanitizada e os tratamentos aplicados para exibição em **Português do Brasil (PT-BR)** na dashboard:

| Coluna Original na Planilha | Tipo de Dado | Descrição Analítica | Tratamento & Tradução no Site (PT-BR) |
| :--- | :--- | :--- | :--- |
| **`sale_id`** | Inteiro | Identificador único da operação de venda (`6` a `105`). | Exibido como `#ID` na tabela e nos cards de recorde. |
| **`SaleDateSanitized`** | Data / Texto | Data da transação no formato `AAAA-MM-DD` ou marcador `INVALID`. | Convertido para o padrão brasileiro `DD/MM/AAAA`. Registros `INVALID` são exibidos como *"Data Não Informada"*. |
| **`customer_name`** | Texto | Nome completo do comprador. | Preservado originalmente para auditoria cadastral. |
| **`PorscheModelSanitized`** | Texto | Nome comercial padronizado do modelo Porsche (40 variantes). | Classificado automaticamente em 6 famílias (`911`, `Cayenne`, `Macan`, `Taycan`, `Panamera`, `718`). |
| **`ModelYearSanitized`** | Inteiro | Ano de fabricação / modelo do veículo (`2020` a `2026`). | Agrupado por safra com cálculo de volume, participação percentual e selo **PICO** no ano líder (`2024`). |
| **`SalesPriceSanitized`** | Decimal | Valor final pago na transação em dólares (`$`). | Formatado no padrão numérico brasileiro com separador de milhar e 2 casas decimais (`$128.278,01`). |
| **`VehicleMileageSanitized`** | Inteiro | Quilometragem registrada no odômetro no ato da venda. | Exibido com sufixo `km` e destaque especial em verde para veículos `0 km (Zero Km)`. |
| **`PayMethodSanitized`** | Texto | Modalidade financeira utilizada pelo cliente (9 categorias). | Traduzido integralmente para PT-BR (ex.: *Cartão de Crédito*, *Transferência Internacional (Wire)*, *Financiamento*, *À Vista (Dinheiro)*, *Leasing*, *Criptomoeda*). |
| **`CitySanitized`** | Texto | Cidade onde a venda foi registrada (`79` praças). | Indexada com busca instantânea, contagem de operações e receita acumulada por cidade. |
| **`StateSanitized`** | Texto | Sigla do estado (UF) correspondente à cidade (`35` estados). | Exibido junto à cidade (`Cidade, UF`) para análise regional. |
| **`salesperson`** | Texto | Nome do consultor de vendas responsável pela operação. | Mantido na estrutura de dados da aplicação para rastreabilidade. |
| **`DeliveryStatusSanitized`** | Texto | Situação logística atual do pedido (`10` status distintos). | Traduzido e categorizado por badges visuais: *Entregue*, *Em Trânsito*, *Pendente*, *Cancelado*, *Enviado*, *Aguardando Aprovação*, *Aguardando Entrega*, *Aguardando Retirada*, *Em Análise* e *Aguardando Revisão*. |

---

### 2.3 Principais Indicadores e Recordes da Base Completa

1. **Modelos Mais Vendidos (Empate Técnico no Topo — 4 unidades cada):**
   * *Panamera*, *Cayenne Coupe*, *Macan T*, *Taycan 4S*, *Cayenne E-Hybrid* e *Macan Electric*.
2. **Maior Valor Pago (Recorde Absoluto):**
   * **$286.500,00** — **Porsche 911 GT3 RS (2024)**, adquirido por *Nora Simmons* em *Arlington, TX* (Operação `#47`).
3. **Ano de Fabricação com Maior Volume de Vendas:**
   * **2024**, liderando com **29 veículos vendidos** (`29,0%` da frota) e **$4.573.350,00** em receita, seguido por **2023** (`19` veículos).
4. **Cidade com Maior Número de Vendas:**
   * **Atlanta, GA**, liderando isoladamente com **3 veículos vendidos** e **$434.800,00** de faturamento acumulado.

---

## 🖥️ 3. Arquitetura e Funcionalidades do Site (`porsche_dashboard.html`)

### 3.1 Console de Parâmetros de Telemetria & Filtragem
* **Monitor de Amostra Ativa:** Tacômetro proporcional que exibe em tempo real quantos veículos estão selecionados (`X de 100`), o percentual da frota, receita filtrada, variação do ticket médio em relação à média global (`+X%` ou `-X%`) e pílulas removíveis (`×`) para cada filtro ativo.
* **Filtro 01 — Modelo & Linha Porsche:** Abas rápidas por família (`Todos`, `911`, `Cayenne`, `Macan`, `Taycan`, `Panamera`, `718`) com ticket médio e contagem, além de seletor detalhado dos 40 modelos e card de telemetria específica (preço médio, faixa mínimo–máximo, odômetro médio e share).
* **Filtro 02 — Ano de Fabricação (Safra):** Botões interativos de `2020` a `2026` com barras proporcionais, contagem exata e destaque visual para a safra líder.
* **Filtro 03 — Faixa de Preço & Densidade:** Histograma visual de 10 faixas que destaca em vermelho o intervalo escolhido no controle deslizante duplo (*dual-range slider*), acompanhado de atalhos rápidos (*Entrada `< $90k`*, *Executivo `$90k–$150k`*, *Superesportivo `> $150k`*) e cálculo dinâmico de **Mediana Ativa**.
* **Filtro 04 — Cidade & Mercado Regional:** Campo de busca textual por cidade ou estado, menu seletor com faturamento por praça e botões de acesso rápido para as principais cidades.

### 3.2 Central de Inteligência Executiva (Sport Chrono Q&A)
Quatro botões interativos inspirados nos modos de condução Porsche que respondem dinamicamente às perguntas-chave do negócio (recalculando os resultados sempre que qualquer filtro for alterado):
* **Modo 01 • Volume:** *Qual o carro mais vendido?*
* **Modo 02 • Recorde:** *Maior valor pago?*
* **Modo 03 • Safra:** *Qual o ano que teve carro mais vendidos?*
* **Modo 04 • Praça:** *Qual a cidade que vendeu mais carros?*

### 3.3 KPIs, Visualizações Gráficas e Tabela Auditável
* **4 Cards de KPIs Executivos:** Receita Total, Veículos Vendidos, Ticket Médio e Quilometragem Média.
* **4 Gráficos Dinâmicos (Chart.js):**
  1. *Faturamento e Volume por Ano de Fabricação* (Barras + Linha de Receita).
  2. *Top 10 Modelos por Receita ($)* (Barras Horizontais).
  3. *Distribuição por Método de Pagamento* (Rosca / Doughnut).
  4. *Status Logístico de Entrega* (Barras Horizontais).
* **Registro Detalhado de Operações:** Tabela responsiva com ordenação em todas as 10 colunas, busca textual instantânea e paginação configurável (`10`, `15`, `25` ou `50` registros por página).

---

## 🚀 4. Como Utilizar

1. Salve o arquivo **`porsche_dashboard.html`** em qualquer diretório local ou servidor web estático.
2. Abra o arquivo em qualquer navegador moderno (*Google Chrome, Microsoft Edge, Firefox ou Safari*).
3. Como os 100 registros sanitizados já estão incorporados na estrutura da página e as bibliotecas (*Tailwind CSS* e *Chart.js*) são carregadas via CDN, **nenhuma instalação adicional é necessária**.
