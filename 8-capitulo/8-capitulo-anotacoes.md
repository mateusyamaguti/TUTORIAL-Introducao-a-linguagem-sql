# Capítulo 8 - JOIN

JOIN é a funcionalidade que distingue o SQL das outras tecnologias de dados, ela permite combinas dados (linhas) entre tabelas a partir de uma condição relacionada.

### Associando tabelas.

Por exemplo, na fig 8.2 de Nield (2016) podemos observar que a tabela **CUSTOMER_ORDER** tem relacionamento direto com a tabela **CUSTOMER**. Ou seja, percebe-se que cada consumidor tem um **ID** presente em CUSTOMER_ORDER e serve para buscar os dados do consumidor em CUSTOMER. Veja que na lógica do mundo real, um consumidor pode ter vários pedidos, o que se traduz no banco de dados. Logo, CUSTOMER é pai de CUSTOMER_ORDER, que por sua vez chamamos esse relacionamento de um-para-muitos, pois um consumidor pode ter muitos pedidos.

### INNER JOIN

Quando queremos consultar varias colunas que possuem relacionamento, por exemplo, os produtos que o consumidor XYZ comprou naquele mês, terias que fazer várias consultas individuais. Com os operadores de JOIN isso não será necessário, pois o operador **INNER JOIN** nos permite mesclar duas tabelas.

Para isso, precisamos definir campos em comum entras elas, neste caso CUSTOMER_ID que é a chave estrangeira. Exemplo:
```sql
SELECT 
	order_id, 
    CUSTOMER.CUSTOMER_ID, 
    order_id, 
    ship_date, 
    name, 
    street_address, 
    state, 
    zip, 
	product_id,
    order_qty
FROM CUSTOMER INNER JOIN CUSTOMER_ORDER
ON CUSTOMER.CUSTOMER_ID = CUSTOMER_ORDER.CUSTOMER_ID
```

Resultado da consulta.

![CUSTOMER mesclado a CUSTOMER_ORDER](https://github.com/mateusyamaguti/TUTORIAL-Introducao-a-linguagem-sql/blob/main/assets/img/cap8-1.png)

Perceba que na parte do SELECT foi selecionado todos os campos que deseja-se consultar, e como o campo CUSTOMER_ID faz parte das duas tabela deve-se explicitar de qual tabela ele será consultado (não importa a tabela). A partir da instrução FROM podemos observar que a mescla entre as duas tabela pelo atributo em comum CUSTOMER_ID.<br>

### LEFT JOIN
Na última busca, não apareceu alguns clientes cadastrados, pois eles não tinham realizado nenhum tipo de compra. Mas existem momentos que é desejável traze-los na consulta, para isso utilizamos o **LEFT JOIN**<br>
Comparado com a consulta anterior, basta trocar INNER por LEFT. Além disso, o importante é concentra-se na alteração, onde significa que todos os registros a esquerda devem ser incluidos mesmo que não haja registro.
```sql
SELECT 
	order_id, 
    CUSTOMER.CUSTOMER_ID, 
    order_id, 
    ship_date, 
    name, 
    street_address, 
    state, 
    zip, 
	product_id,
    order_qty
FROM CUSTOMER LEFT JOIN CUSTOMER_ORDER
ON CUSTOMER.CUSTOMER_ID = CUSTOMER_ORDER.CUSTOMER_ID
```

Em alguns momentos é interassante encontrar os clientes que não fizeram compra dentro de uma consulta dessa. Para isso, pode-se utilizar a instrução **WHERE** para procura-los (WHERE order_id IS NULL).
```sql
SELECT 
	order_id, 
    CUSTOMER.CUSTOMER_ID, 
    order_id, 
    ship_date, 
    name, 
    street_address, 
    state, 
    zip, 
	product_id,
    order_qty
FROM CUSTOMER LEFT JOIN CUSTOMER_ORDER
ON CUSTOMER.CUSTOMER_ID = CUSTOMER_ORDER.CUSTOMER_ID
WHERE order_id IS NULL
```

### Associando várias tabelas
Veja que a tabela COSTUMER e PRODUCT fornecem dados para a tabela CUSTOMER_ORDER pelos atributos CUSTOMER_ID e PRODUCT_ID. A partir disso, podemos criar um relacionamento que abrange os dados do cliente e do produto.
```sql
SELECT 
    order_id,
    CUSTOMER.CUSTOMER_ID,
    name as CUSTOMER_NAME,
    street_address,
    city,
    state,
    zip,
    order_date,
    product_id,
    description,
    order_qty

    FROM CUSTOMER

    INNER JOIN CUSTOMER_ORDER
    ON CUSTOMER_ORDER.CUSTOMER_ID = CUSTOMER.CUSTOMER_ID

    INNER JOIN PRODUCT
    ON CUSTOMER_ORDER.PRODUCT_ID = PRODUCT.PRODUCT_ID
```
No SQLite essa consulta estava dando conflito por conta de ambiguida de campos, então apelidei alguns campos para poder excluir a ambiguidade.

```sql
SELECT
    co.order_id,
    c.customer_id,
    c.name AS customer_name,
    c.street_address,
    c.city,
    c.state,
    c.zip,
    co.order_date,
    p.product_id,
    p.description,
    co.order_qty
FROM CUSTOMER AS c
INNER JOIN CUSTOMER_ORDER AS co
    ON co.customer_id = c.customer_id
INNER JOIN PRODUCT AS p
    ON co.product_id = p.product_id;
```
Com isso, agora podemos calcular a receita obtida com cada pedido mesmo que os dados venha de colunas separadas, exemplo: `co.order_qty * p.price AS REVENUE`.
```sql
SELECT
    co.order_id,
    c.customer_id,
    c.name AS customer_name,
    c.street_address,
    c.city,
    c.state,
    c.zip,
    co.order_date,
    p.product_id,
    p.description,
    co.order_qty,
    co.order_qty * p.price AS REVENUE
    
FROM CUSTOMER AS c
INNER JOIN CUSTOMER_ORDER AS co
    ON co.customer_id = c.customer_id
INNER JOIN PRODUCT AS p
    ON co.product_id = p.product_id;
```

### Agrupando JOINs
A partir do exemplo anterior podemos calcular a receita total de cada cliente.
```sql
SELECT
    c.customer_id,
    c.name AS customer_name,
    sum(co.order_qty * p.price) AS TOTAL_REVENUE
    
FROM CUSTOMER AS c

INNER JOIN CUSTOMER_ORDER AS co
    ON co.customer_id = c.customer_id
INNER JOIN PRODUCT AS p
    ON co.product_id = p.product_id

GROUP BY 1, 2
```