# 📊 Portfólio de Projetos - Análise de Dados

Bem-vindo ao meu portfólio! Aqui você encontrará projetos nas áreas de **Business Intelligence**, **SQL** e **Data Science com Python**, desenvolvidos para resolver problemas de negócio com base em dados.

---

## 1️⃣ Dashboards Interativos

### 1.1 📈 Dashboard de Vendas

Análise de vendas com indicadores de desempenho como faturamento, lucro, ticket médio e evolução mensal.

<!-- GIF do projeto -->
![Dashboard Vendas](assets/dashboard-vendas.gif)

🔗 [Acessar repositório](https://github.com/seu-usuario/dashboard-vendas)

### 1.2 📢 Dashboard de Campanha de Marketing

Visualização da performance de campanhas de marketing com foco em taxa de conversão, ROI e canais de aquisição.

<!-- GIF do projeto -->
![Dashboard Marketing](assets/dashboard-marketing.gif)

🔗 [Acessar repositório](https://github.com/seu-usuario/dashboard-marketing)

### 1.3 👥 Dashboard de Leads

Acompanhamento de geração e qualificação de leads, com funil de conversão e análise por canal.

<!-- GIF do projeto -->
![Dashboard Leads](assets/dashboard-leads.gif)

🔗 [Acessar repositório](https://github.com/seu-usuario/dashboard-leads)

---

## 2️⃣ Análise com SQL

Esta análise foi desenvolvida com o objetivo de identificar oportunidades de otimização em um funil de vendas online, utilizando consultas SQL para extrair KPIs mensais e rankings de performance de marcas, lojas, canais e estados.

### 📌 Métricas Gerais do Funil (Receita, Ticket Médio, Visitas e Conversão)
## Querys utilizadas
```sql
WITH leads AS (
  SELECT 
    date_trunc('month', visit_page_date)::date AS data1,
    COUNT(*) AS visitas
  FROM sales.funnel
  GROUP BY data1
)

SELECT 
  date_trunc('month', fun.paid_date)::date AS data,
  SUM((prod.price * (1 + fun.discount))) AS receita,
  AVG((prod.price * (1 + fun.discount))) AS ticket_medio,
  leads.visitas,
  (COUNT(fun.paid_date)::numeric / leads.visitas::numeric) * 100 AS conversao
FROM sales.funnel AS fun
LEFT JOIN sales.products AS prod ON prod.product_id = fun.product_id
LEFT JOIN leads ON date_trunc('month', fun.paid_date)::date = leads.data1
WHERE fun.paid_date IS NOT NULL
GROUP BY data, leads.visitas;

-- Top 5 Marcas do Mês (2021-08-01)
SELECT 
  date_trunc('month', fun.paid_date)::date AS data,
  prod.brand,
  COUNT(*) AS quantidade
FROM sales.funnel AS fun
INNER JOIN sales.products AS prod ON prod.product_id = fun.product_id
WHERE date_trunc('month', fun.paid_date)::date = '2021-08-01'
GROUP BY data, prod.brand
ORDER BY quantidade DESC
LIMIT 5;

-- Top 5 Lojas do Mês (2021-08-01)
SELECT 
  date_trunc('month', fun.paid_date)::date AS data,
  store_name,
  COUNT(fun.paid_date) AS qntdd
FROM sales.funnel AS fun
INNER JOIN sales.stores AS store ON store.store_id = fun.store_id
WHERE fun.paid_date IS NOT NULL 
  AND date_trunc('month', fun.paid_date)::date = '2021-08-01'
GROUP BY store.store_name, data
ORDER BY qntdd DESC
LIMIT 5;

-- Visitas por Dia da Semana (2021-08-01)
SELECT 
  EXTRACT('dow' FROM visit_page_date) AS dia_semana,
  COUNT(*) AS visitas
FROM sales.funnel
WHERE date_trunc('month', visit_page_date)::date = '2021-08-01'
GROUP BY dia_semana;

-- Estados com Mais Vendas (2021-08-01)
SELECT 
  cum.state,
  date_trunc('month', fun.paid_date)::date AS data,
  COUNT(*) AS qntdd
FROM sales.funnel AS fun
INNER JOIN sales.customers AS cum ON fun.customer_id = cum.customer_id
WHERE fun.paid_date IS NOT NULL 
  AND date_trunc('month', fun.paid_date)::date = '2021-08-01'
GROUP BY cum.state, data
ORDER BY qntdd DESC;

