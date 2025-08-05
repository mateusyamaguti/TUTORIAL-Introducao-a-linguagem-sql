# Capítulo 6 - GROP BY  e ORDER  BY

Agregação de dados é a criação de um tipo de total a partir de outros registro. Estes podem ser de soma, máximo, mínimo, contagem e média.

### Agrupando registros

Contar número de registro em uma tabela `COUNT`

```sql
SELECT COUNT (*) AS record_count FROM station_data
```

`COUNT` significa juntar registros, podemos unir essa instrução com outras operações. Contar todos os registros em que ocorreu tornado.
```sql
SELECT COUNT (*) AS record_count FROM station_data
    WHERE tornado = 1;
```

Além disso também podemos ordernar essa consulta por ano com a expressão `GRUPO BY year`. Ela retornará o número de tornado por ano.
```sql
SELECT year, COUNT (*) AS record_count FROM station_data
    WHERE tornado = 1
    GROUP BY year;
```

![Obtendo a contagem de tornados por ano](https://github.com/mateusyamaguti/TUTORIAL-Introducao-a-linguagem-sql/blob/main/assets/img/cap6-1.png)
 
Também podemos fatiar essa consulta por ano (year) e mês (month), lembrando que os meses vão de 1 (janeiro) a 12 (dezembro)
```sql 
SELECT year, month, COUNT (*) AS record_count From STATION_DATA
	WHERE tornado = 1
    GROUP BY year, month;
```

Caso a instrução **SELECT** for muito longa, você pode usar a posição ordinais do SELECT para realizar o agrupamento. Exemplo:
```sql
SELECT year, month, COUNT (*) AS record_count From STATION_DATA
	WHERE tornado = 1
    GROUP BY 1, 2;
```

### Ordenando Registros

O operador `ORDER BY` é utilizado para organizar a consulta. Geralmente ele é usado no final da expressão WHERE e GROUP BY, sendo assim, para classificarmos por ano e depois por mês, podemos incluir o seguinte trecho.
```sql
SELECT year, month, COUNT (*) AS record_count From STATION_DATA
	WHERE tornado = 1
    GROUP BY year, month
    ORDER BY year, month;
```

Para classificarmos na ordem do mais recente registro, podemos utilizar o operador DESC
```sql
SELECT year, month, COUNT (*) AS record_count From STATION_DATA
	WHERE tornado = 1
    GROUP BY year, month
    ORDER BY year DESC, month;
```

### Funções de agregação

Além da função COUNT(*) também temos, SUM(), MIN(), MAX(), e AVG().<br>
Porém ainda existem outros pontos da função COUNT que podemos explorar, por exemplo: Se especificarmos uma coluna e não um *, o count retornará a quantidade de valores não nulos dessa coluna.
```sql
SELECT COUNT(snow_depth) AS recorded_snow_depth from STATION_DATA;
```
<p style="color:orange">Atenção: Funções de agregação não incluem valores nulos.</p>

Se você quiser calcular todas as temperaturas médias anuais a partir de 2000 por mês, exemplo:

```sql
SELECT year, month, AVG(temperature) AS avg_temp FROM station_data
    WHERE year >= 2000
    GROUP BY month;
```
Podemos também arredondar o valor da temperatura para com `round(AVG(temperature), 2)`

Além disso, podemos calcular a soma de profundidade da neve (snow_depth) desde 2000 por ano
```sql
SELECT year, SUM(snow_depth) AS total_snow FROM station_data
    WHERE year >= 2000
    GROUP BY year;
```

Podemos usar diversas operações de agregação, como por exemplo: calcular o total de neve, o total de precipitação e a precipitação máxima a partir do ano 2000

```sql
SELECT 
    YEAR, 
    round(SUM(snow_depth), 1) AS total_depth, 
    round(SUM(precipitation), 1) AS total_precipitaion, 
    round(MAX(precipitation), 1) as max_precipitation FROM station_data

        WHERE year >= 2000
        GROUP BY year;
```

Podemos fazer consultas unitárias, levando em conta por exemplo a precipitação total por ano que houve tornado
```sql
SELECT YEAR, round(SUM(precipitation), 1) AS total_precipitaion FROM station_data
	WHERE tornado = 1
    GROUP BY year;
```

### Instrução HAVING

Se quisermos fazer alguma consulta a partir de operações realizadas por operadores de agregação, temos que utilizar a instrução `HAVING` pois a operação **WHERE** neste caso busca apenas os dados no banco de dados, enquanto **HAVING** consegue puxar os dados processados.<br>
Exemplo: Calcular consultar os valores de total_precipitation maior que 30

```sql
SELECT year, round((SUM(precipitation)), 1) AS total_precipitation FROM station_data
    GROUP BY year
    HAVING total_precipitation > 30;
```
Perceba que nesse caso a instrução **HAVING** vem depois do **GROUP BY**

### Obtendo registros distintos

Para evitar duplicas podemos usar o operador `DISTINCT`
```sql
SELECT DISTINCT(station_number) from STATION_DATA
```

Rode esse código para perceber a quantidade de duplicadas que existem nessa tabela.
```sql
SELECT count(DISTINCT (station_number))from STATION_DATA
```