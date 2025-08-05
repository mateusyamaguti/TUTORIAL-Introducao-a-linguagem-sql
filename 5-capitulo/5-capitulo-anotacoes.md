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

De forma semelhante podemos realizar operações com a instrução **WHERE** em texto, porém com algumas mudanças sutis. Exemplo, filtrar report_code que tenha o código 513A63.
```sql
SELECT * FROM station_data
    WHERE report_code = '513A63';
```

Ou podemos filtrar vários código
```sql
SELECT * FROM station_data
    WHERE report_code IN ('513A63', '1F8A7B', 'EF616A');
```

Podemos utilizar a função `length()` para contar o número de caracteres de um valor específico.
```sql
SELECT * FROM station_data
    WHERE length(report_code) <> 6;
```
Ou seja, o resultado não retornará nenhum registro, pois não há códigos em report_code que não seja de 6 caracteres.<br>

Quando desejamos buscar códigos que possuem letras em locais específicos em cadeias de caracteres utilizamos a expressão **LIKE** onde **%** significa qualquer número de caracteres e **_** um único caracter. Exemplo: Todos os registros que iniciam com a letra A.
```sql
SELECT * FROM station_data
    WHERE report_code LIKE 'A%';
```
Se quisessemos encontrar todos que terminam com A, poderiamos inveter para '%A'.<br>

Se quisessemos encontrar todos os códigos que iniciam com B e o terceiro código é C, precisamos usar o _.
```sql
SELECT * FROM station_data
    WHERE report_code LIKE 'B_C%';
```

### Usando WHERE com booleano

Booleanos são valores de tipo verdadeiro (1) ou falso (0). 
```sql
SELECT * FROM station_data
    WHERE tornado = true AND hail = true;
```
O mesmo vale para
```sql
SELECT * FROM station_data
    WHERE tornado = 1 AND hail = 1;
```
Se quisessemos somente valores verdadeiros, nem precisariamos usar a expressão = 1.

### Manipulando NULL

Espaços em branco geralmente são preenchidos por **NULL**, esses valores não podem ser determinados por igualdade **=**. Portanto é necessário usar as instruções `IS NULL` ou `IS NOT NULL`. Logo para determinar os registro de **snow_depth**, pode executar essa consultar:
```sql
SELECT * FROM station_data
    WHERE snow_depth IS NULL;
```
<p style="color:orange">Atenção: é díficil descrever e lidar com os valores nulos, portanto é importante sempre documenta-los. Não confundir valores nulos com textos vazios</p>

Por conta dos valores nulos serem difeceis de lidar, precisamos ter cautela ao manipular colunas que os tem. Exemplo se quisessemos consultar todos os valores do campo **precipitation** menor ou igual que 0.5 usariamos
```sql
SELECT * FROM station_data
    WHERE precipitation <= 0.5;
```
Porém se os valores nulos por algum motivo representasse 0, como poderiamos inclui-los na consulta? Logo é nencessário usar OR na consulta
```sql
SELECT * FROM  station_data
    WHERE precipitation <= 0.5 OR precipitation IN NULL;
```

Uma outra maneira mais elegante de consultar os valores nulos junto com outras condições é usar a função `coalesce(campo_possivelmente_nulo,valor_que_substituirá_NULL)`. Exemplo:
```sql
SELECT * FROM statio_data
    WHERE coalesce(precipitation, 0) <= 0.5;
```

Também podemos usar a função `coalesce()` para gerar outro campo de consulta e deixar a apresentação mais limpa
```sql
SELECT report_code, coalesce(precipitation, 0) AS rainfall FROM station_data;
```

### Agrupando condições

Para agruparmos condições devemos fazer uso da semantica e da lógica matemática a partir do uso dos parenteses, assim como dos operadores **AND** e **OR**. Como por exemplo, se quisermos consultar os registros que houve chuva e temperatura menor ou igual a 32 graus F, ou, que a profundida da neve seja menor que 0.
```sql
SELECT * FROM station_data
    WHERE (rain = 1 AND temperature <= 32)
    OR snow_depth <0;
```
