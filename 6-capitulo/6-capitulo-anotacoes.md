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
![Obtendo a contagem de tornados por ano](C:/Users/Mateus/Documents/MateusYamaguti/TUTORIAL-Introducao-a-linguagem-sql/assets/img/cap6-1.png)

![Obtendo a contagem de tornados por ano](TUTORIAL-Introducao-a-linguagem-sql/assets/img/cap6-1.png)

![Obtendo a contagem de tornados por ano](6-capitulo/cap6-1.png)

![Obtendo a contagem de tornados por ano](https://github.com/mateusyamaguti/TUTORIAL-Introducao-a-linguagem-sql/blob/main/assets/img/cap6-1.png)