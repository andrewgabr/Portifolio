# 📊 Portfólio de Projetos - Andrew Gabr

Este repositório reúne projetos de Análise de Dados, com dashboards, consultas SQL e aplicações com Python + Machine Learning.

---

## 1 - Dashboards

### 1.1 Dashboard de Vendas
![GIF_DASHBOARD_VENDAS](coloque_o_caminho_do_gif_aqui)

🔗 [Acesse o repositório do dashboard de vendas](https://github.com/andrewgabr/link_projeto_vendas)

---

### 1.2 Dashboard de Campanha de Marketing
![GIF_DASHBOARD_MARKETING](coloque_o_caminho_do_gif_aqui)

🔗 [Acesse o repositório do dashboard de marketing](https://github.com/andrewgabr/link_projeto_marketing)

---

### 1.3 Dashboard de Leads
![GIF_DASHBOARD_LEADS](coloque_o_caminho_do_gif_aqui)

🔗 [Acesse o repositório do dashboard de leads](https://github.com/andrewgabr/link_projeto_leads)

---

## 2 - Análise com SQL

Análise de dados de um e-commerce, utilizando CTEs e funções de data para gerar insights de desempenho de vendas, visitas, conversão e muito mais.

```sql
-- Métricas Gerais do Funil (Receita, Ticket Médio, Visitas e Conversão)
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
```

