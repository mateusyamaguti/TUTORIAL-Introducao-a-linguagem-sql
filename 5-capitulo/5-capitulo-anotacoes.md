# Capítulo 5 - WHERE

WHERE é uma das instruções usada para filtrar dados.<br>

### Filtrando registro

Consulte a tabale para verificar os dados
```sql
SELECT * FROM ESTATION_DATA;
```

### Usando WHERE com números

Suponhamos que queremos registros apenas do ano de 2010, ou seja, registro da coluna (campo) **year** igual a 2010.
```sql
SELECT * FROM station_data
    WHERE year = 2010;
```

Inversamente podemos usar o operados `!=` ou `<>` para obeter tudo, exceto 2010.
```sql
SELECT * FROM station_data
    WHERE year <> 2010;
```
Podemos utilizar instrução *BETWEEN* para intervalos inclusivos.
```sql
SELECT * FROM station_data
    WHERE year BETWEEN 2005 and 2010;
```

### Instruções AND, OR e IN

Podemos realizar a mesma operação de **BETWEEN** utilizando os operadores matemáticos relacionais `>=` e `<=`
```sql
SELECT * FROM station_data
    WHERE year >= 2005 and year <= 2010;
```

Podemos utilizar a instrução **OR** para filtrar critérios que ao menos um deve ser atendido.
```sql
SELECT * FROM station_data
    WHERE month = 3
    OR month = 6
    OR month = 9
    OR month = 12;
```

Para verbalizar menos podemos utilizar a instrução **IN**. Se caso quiser todos os dados excetos esse meses, podemos utilizar a expressão **NOT IN**
```sql
SELECT * FROM station_data
    WHERE month IN (3, 6, 9, 12);
```

Se quisermos filtrar apenas valores divisíveis por 3, podemos utilizar o operador `%`
```sql
SELECT * FROM  station_data
    WHERE month % 3 = 0;
```

### Usando WHERE com texto

