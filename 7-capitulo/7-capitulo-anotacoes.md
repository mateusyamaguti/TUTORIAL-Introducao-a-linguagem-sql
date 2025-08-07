# Capítulo 7 - CASE

O operador **CASE** permite trocar o valor de uma coluna por outro.

### A instrução CASE

O operador **CASE** permite mapear uma ou mais condições para um valor correspondente a cada condição. Ela deve iniciar com CASE e terminar com END. Entre CASE e END, deve-se especificar a condição **WHEN**(condição) e **THEN**(valor). Por exemplo, poderiamos fazer uma classificação da velocidade do vento (wind_speed) sendo HIGH velocidade maior que 40, MODERATE maior que 30 e menor que 40 e LOW menores que 30.
```sql
SELECT report_code, year, month, day, wind_speed,
    CASE
        WHEN wind_speed >= 40 THEN 'HIGH'
        WHEN wind_speed >= 30 AND wind_speed < 40 THEN 'MODERATE'
        ELSE 'LOW'
    END as wind_severity
FROM station_data
```
Não precisamos utilizar uma condição composto no segundo WHEN, pois entende-se que tudo que sobrar na primeira condição WHEN que não for menor que 40 automaticamente receberá a equitiqueta de 'MODERATE' desde que ela seja maior ou igual a 30

### Agrupando instruções

Podemos agrupar a instrução CASE para obter uma contagem de registro de acordo com year e wind_severity, além de ordernal por posição ordinal

```sql
SELECT year, 
    CASE
        WHEN wind_speed >= 40 THEN 'HIGH'
        WHEN wind_speed >= 30 THEN 'MODERATE'
        ELSE 'LOW'
    END as wind_severity,

    COUNT(*) as record_count
    FROM station_data
    ORDER BY 1, 2
```

### O truque da instrução CASE "Zero/Null"

Com a instrução CASE pode realizar funções de agrupamento que não seria possível fazer com a consulta WHERE, a não ser que fosse feita mais de uma consulta. Por exemplo, queremos agregar com mês e ano **tornado_precipitation** e **non_tornado_precipitation**

```sql
SELECT year, month,
    SUM(CASE WHEN tornado = 1 THEN precipitation ELSE 0 END) as tornado_precipitation,
    SUM(CASE WHEN tornado = 0 THEN precipitation ELSE 0 END) as non_tornado_precipitation

FROM station_data
GROUP BY year, month;
```

A instrução **CASE** tem grande valor e é muito importante, pois pode realizar muitas operações complexas e retornar o valor 0 caso não for atendida.<br>

Poderiamos calcular o máximo e o mínimo dessa operação, agrupando apenas por ano, substituindo 0 por NULL para que o processo não leve em conta os dias que não teve medição

```sql
SELECT year,
    MAX(CASE WHEN tornado = 1 THEN precipitation ELSE NULL END) as max_tornado_precipitation,
    MIN(CASE WHEN tornado = 0 THEN precipitation ELSE NULL END) as min_non_tornado_precipitation

FROM station_data
GROUP BY year;
```

Em **CASE** podemos utilizar consultas com operadores AND, OR e NOT. Por exemplo, queremos calcular as temperaturas médias por mês chuva/granizo estavam ou não rresentes após 2000

```sql
SELECT month,
    AVG(CASE WHEN rain or hail THEN temperature ELSE NULL END)
        as avg_precipitation_temp,
    AVG(CASE WHEN NOT (rain or hail) THEN temperature ELSE NULL END)
        as non_avg_precipitation_temp
FROM station_data
WHERE year >= 2000
GROUP BY month
```