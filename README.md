# Meu Portfólio de Projetos 

---

## 1 - Dashboards

### 1.1 Dashboard de Vendas (Power BI)
![GIF_DASHBOARD_VENDAS](https://github.com/andrewgabr/Supermercado_Dashboard/raw/master/assets/gif.gif?raw=true)

🔗 [Acesse o repositório do dashboard de vendas](https://github.com/andrewgabr/Supermercado_Dashboard)

### 1.2 Dashboard de Campanha de Marketing (Power BI + SQL)
![GIF_DASHBOARD_MARKETING](https://raw.githubusercontent.com/andrewgabr/Campanha_Marketing-Dashboard/refs/heads/master/imgs/Screenshot%202025-03-30%20215249.png)

🔗 [Acesse o repositório do dashboard de marketing](https://github.com/andrewgabr/Campanha_Marketing-Dashboard)

### 1.3 Dashboard de Leads (Excel)
![GIF_DASHBOARD_LEADS](https://raw.githubusercontent.com/andrewgabr/Leads_excel_dashboard/refs/heads/master/imgs/query.png)

🔗 [Acesse o repositório do dashboard de leads](https://github.com/andrewgabr/Leads_excel_dashboard?tab=readme-ov-file)

---

## 2 - Análise com SQL

Análise de dados de um e-commerce com foco em conversão, performance de vendas, marcas, lojas e localidade.

### Receita, Ticket Médio, Visitas e Conversão
Query ->
```sql
-- Métricas Gerais do Funil
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
```
🔗Resultado ->

![GIF_DASHBOARD_VENDAS](https://github.com/andrewgabr/querys_ecomerce/blob/master/imgs/1.png?raw=true)

### Top 5 Marcas do Mês
Query ->
```sql
-- Marcas com maior volume de vendas
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
```
🔗Resultado ->
![GIF_DASHBOARD_VENDAS](https://github.com/andrewgabr/querys_ecomerce/blob/master/imgs/2.png?raw=true)

### Top 5 Lojas do Mês
Query ->
```sql
-- Lojas com mais vendas no mês
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
```
🔗Resultado ->
![GIF_DASHBOARD_VENDAS](https://github.com/andrewgabr/querys_ecomerce/blob/master/imgs/3.png?raw=true)

### Visitas por Dia da Semana
Query ->
```sql
-- Visitas ao site por dia da semana
SELECT
  EXTRACT('dow' FROM visit_page_date) AS dia_semana,
  COUNT(*) AS visitas
FROM sales.funnel
WHERE date_trunc('month', visit_page_date)::date = '2021-08-01'
GROUP BY dia_semana;
```
🔗Resultado -> 

![GIF_DASHBOARD_VENDAS](https://github.com/andrewgabr/querys_ecomerce/blob/master/imgs/4.png?raw=true)


### Estados com Mais Vendas
Query ->
```sql
-- Regiões com maior volume de vendas
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
🔗Resultado ->
![GIF_DASHBOARD_VENDAS](https://github.com/andrewgabr/querys_ecomerce/blob/master/imgs/5.png?raw=true)

## 3 - Python + Machine Learning

### 2.1 Análise Preditiva de Empréstimos
![GIF_LOAN](https://raw.githubusercontent.com/andrewgabr/aprovacao-emprestimo/refs/heads/master/imgs/Anima%C3%A7%C3%A3o.gif)

🔗 [Acesse o repositório do projeto de empréstimo](https://github.com/andrewgabr/aprovacao-emprestimo)

### 2.2 Análise Climática com Python
![GIF_LOAN](https://blog4.mfrural.com.br/wp-content/uploads/2020/02/clima-x-tempo-1024x660.jpg)

🔗 [Acesse o repositório do projeto de clima](https://github.com/andrewgabr/Analise_Climatica_Szeged-Regressao)




