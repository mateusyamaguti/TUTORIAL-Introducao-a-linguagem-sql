# Capítulo 4 - SELECT

SELECT é uma das instruções mais usadas e tem a função de consultar dados, porém ela pode ser combinados com outras intruções, tornando-a muito poderosa e útil.<br>

Instrução para consultar todos as colunas da tabelas COSTUMER. O * significa todas as colunas.
```sql
SELECT * FROM CUSTOMER;
```
Podemos também acessar colunas especificas.

```sql
SELECT CUSTOMER_ID, NAME FROM CUSTOMER;
```
Intrução utilizada para consultar as tabelas presentes no banco de dados
```sql
SELECT name FROM sqlite_master WHERE type='table';
```
Intrução utilizada para consultar o nome das colunas das tabelas.
```sql
PRAGMA table_info(CUSTOMER);
```

### Expressões em instruções SELECT
É possível utilizar o SELECT para efetuar cálculos e incluí-los em uma coluna adicional da consulta.<br>
Para isso consulte a tabela **PRODUCT** `SELECT * FROM PRODUCT`. E suponha que você queira gerar uma coluna adicionar que acrescenta 7% de taxa ao preço, TAXED_PRICE.

```sql
SELECT PRODUCT_ID, DESCRIPITION, PRICE, PRICE 1.07 AS TAXED_PRICE FROM PRODUCT;
```
A instrução **AS** em TAXED_PRICE indica que o nome da coluna incluida na consulta. Lembre-se que essa coluna não foi adicionada ao banco de dados, ela apenas foi includia na consuta.<br>
Perceba que o resultado da consulta obteve vários números quebrados, pode ser utilizado a função **round()** para arredondas as casa decimais.
```sql
SELECT PRUDUCT_ID, DESCRIPITION, PRICE, round(PRICE * 1.07, 2) AS TAXED_PRICE FROM PRODUCT;
```

### Concatenação de texto
Para concatenar textos que estão em colunas diferentes podemos utilizar o operador pipe duplo ||
```sql
SELECT NAME, CITY || ', ' || STATE AS LOCATION FROM CUSTOMER;
```
Neste caso uniu-se a cidade ao estado para gerar uma coluna adicional na consulta chamada **location**. Outro exemplo:

```sql
SELECT name, STREET_ADRESS || ' ' || CITY || ', ' || STATE || ' ' || ZIP as SHIP_ADRESS FROM CUSTOMER;
```

