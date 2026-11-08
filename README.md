# Porsche · Painel de Vendas

Dashboard interativa em HTML (arquivo único) sobre 100 vendas de veículos Porsche, feita a partir de uma planilha sanitizada.

**Dashboard publicada:** [https://github.com/diogocosta1117-design/dashbord_porche_sales.git]

> Os trechos marcados com `[PREENCHER]` dependem de você. Complete ou apague antes de entregar.

---

## 1. Perguntas de negócio escolhidas

| # | Pergunta | Como é respondida |
|---|---|---|
| 1 | Quantos veículos foram vendidos por ano? | Gráfico de vendas por ano da venda |
| 2 | Qual a forma de pagamento mais usada e qual a menos usada? | Ranking de pagamentos, com selos "Mais usada" e "Menos usada" |
| 3 | Quais modelos mais vendem, e quais lideram em cada cidade? | Ranking dos 10 modelos e tabela de cidades com os modelos líderes |
| 4 | Qual ano-modelo teve maior saída? | Gráfico por ano de fabricação |

**Por que estas perguntas:** [ ajudam a planejar estoque e metas (vendas por ano e ano-modelo), a definir condições comerciais (formas de pagamento) e a decidir onde concentrar a oferta (modelos e cidades).]

**Indicadores de topo:** veículos vendidos, receita total, ticket médio, pagamento líder e cidades atendidas.

**Filtros:** modelo Porsche, ano-modelo, cidade e método de pagamento. Todos os indicadores, gráficos, a tabela e os textos de insight respondem aos filtros.

---

## 2. Tratamento da base antes da IA

A planilha traz cada campo em duas versões: o dado cru e o sanitizado. O tratamento seguiu estes passos:

1. **Só as colunas sanitizadas** (`...Sanitized`) alimentam a dashboard. As originais ficaram apenas para conferência.
2. **Datas** normalizadas em `YYYY-MM-DD`. Datas impossíveis (ex.: 30/02/2024, 31/04/2024) viraram `INVALID`: são **24 de 100** registros.
3. **Preço e ano** convertidos para número. Preço em USD com 2 casas; ano-modelo com 4 dígitos (2020 a 2026).
4. **Textos padronizados:** modelos, formas de pagamento, cidades (título), estados (sigla de 2 letras) e status de entrega.
5. **Dados pessoais fora:** as colunas `customer_name` e `salesperson` **não** foram embutidas no HTML, porque a base vai dentro do arquivo publicado.
6. Nenhum valor foi inventado ou preenchido: o que não pôde ser normalizado ficou como `INVALID`.

**Ressalvas que a dashboard mostra ou que valem para a leitura**
- As 24 vendas com data inválida **não entram** no gráfico por ano (76 vendas), mas contam nos demais indicadores.
- 18 das 76 datas válidas são **posteriores a 07/11/2026** (a mais distante é de out/2027). A planilha não explica se são vendas agendadas ou erro de digitação.
- Não há um único modelo mais vendido: seis modelos empatam com 4 vendas.
- A receita total inclui 7 vendas com status `Cancelled`.
- Quase todas as cidades têm só 1 venda, então "popular por cidade" é indicativo, não tendência.

---

## 3. Ferramentas e prompt

**Ferramentas usadas:** [ No meu caso: um assistente de IA (Claude) com o recurso de artefatos, que é o equivalente ao Canvas, para gerar e refinar o HTML. Também tentei montar um agente com duas habilidades (tratamento de dados e desenvolvimento de dashboard), primeiro no Dify e depois no Gemini.]

**Prompt que gerou a dashboard (versão inicial):**

```
Utilizando o recurso de Canvas, renderize uma dashboard em HTML.
Filtros: Modelo da Porsche, Modelyear, city, pay method.
Perguntas de negócio e KPIs: quais os principais modelos de carros
vendidos por cidade, qual o ano de modelo de carro que mais saiu
em um período, insights de carros populares de vendas com base nos
dados em cada cidade.
UI/UX: baseie-se no site oficial da Porsche Brasil, com ar elegante
e refinado.
```

**O que mudou até a versão final**
1. **Troca de perguntas:** pedi para mudar as perguntas para vendas por ano, forma de pagamento mais e menos usada, e modelos populares por cidade.
2. **Cores:** a primeira versão era sóbria demais, então pedi para colocar cores na página. Entraram uma paleta com acento vermelho, cartões de KPI coloridos e uma cor por gráfico, com versão para tema escuro.
3. **Conferência dos números** contra a planilha (seção 4).

---

## 4. Validação dos números

Conferi os indicadores contra a planilha, fora da IA:

| Indicador | Valor |
|---|---|
| Veículos vendidos | 100 |
| Receita total | US$ 12.827.800,50 |
| Ticket médio | US$ 128.278,01 |
| Forma de pagamento mais usada | Wire Transfer (26) |
| Menos usadas (empate) | Debit Card e ACH Payment (4 cada) |
| Ano-modelo com mais vendas | 2024 (29) |
| Vendas por ano da venda | 2024: 11 · 2025: 19 · 2026: 31 · 2027: 15 |

**Aprendizado:** numa tentativa com um agente no Gemini, o painel saiu bonito mas com números diferentes dos da planilha (por exemplo, 95 vendas em vez de 100 e Wire Transfer com 25 em vez de 26), e eu só percebi ao conferir. Por isso a validação contra a base é parte do processo, e a versão publicada aqui é a que passou nessa conferência.

---

---

.

## 7. Estrutura do repositório

```
porsche-dashboard/
├── index.html   # dashboard (HTML, CSS e JavaScript em um só arquivo, com os dados embutidos)
└── README.md    # este arquivo
```

