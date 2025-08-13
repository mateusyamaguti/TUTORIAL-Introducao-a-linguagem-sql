# Geriando dados

A partir da criação bem feita do modelo de banco de dados, sempre que precisarmos inserir os excluir dados, precisaremos atuar apenas em uma tabela. A instrução INSERT é muito mais simples de se fazer que a instrução SELECT, além disso, muitas outras linguagens como o Python utilizam da manipulação do SQL para fazer esses tipos de gereciamento

### INSERT

A instrução **INSERT** insere registro no banco de dados. Inicialmente insira dados do nome e sobre nome do cliente (attendde) no banco de dados.
```sql
INSERT INTO ATTENDEE (FIRST_NAME, LAST_NAME)
	VALUES('Thomas', 'Nield')
```

Execute `SELECT * FROM ATTENDEE` para verificar se os registros foram inseridos correntamente.<br>
Veja que houve o auto incremento na coluna ATTENDEE_ID, assim como o padrão 0 do VIP foi fornecido.<br>

### Inserções multíplas

Caso seja necessário realizar mais de uma inserção, podemos executar o código abaixo, ou fazer o uso de scripts de linguagens de programação para poder automatizar esse processo.
```sql
INSERT INTO ATTENDEE 
    (FIRST_NAME, LAST_NAME, PHONE, EMAIL, VIP)
    VALUES
        ('Jon', 'Skeeter',4802185842,'john.skeeter@rex.net', 1),
        ('Sam','Scala',2156783401,'sam.scala@gmail.com', 0),
        ('Brittany','Fisher',5932857296,'brittany.fisher@outlook.com', 0)
```

Atenção: Também é possível inserir registros usando os resultados de uma consulta SELECT. Isso será útil se você precisar migrar dados de uma tabela para outra. Apenas certifique-se de que os campos de SELECT estejam alinhados com os de INSERT, que estejam na mesma ordem e que tenham os mesmos tipos de dados:

```sql
INSERT INTO ATTENDEE (FIRST_NAME, LAST_NAME, PHONE, EMAIL)
SELECT FIRST_NAME, LAST_NAME, PHONE, EMAIL
FROM SOME_OTHER_TABLE
```

### Testando chaves externas

Vamos testar uma chave estrangeira a partir da inserção de um valor na chave primaria PRIMARY_CONTACT_ID da tabela COMPANY
```sql
INSERT INTO COMPANY(name, DESCRIPITION, PRIMARY_CONTACT_ATTENDEE_ID)
VALUES ('RexApp Solutions', 'A mobile app delivery service', 5)
```
Essa instrução deve retornar um erro: **FOREIGN KEY**. Isso é algo bom, pois quer dizer que a restrição referente a chave estrangeira está funcionando. Para que o registro não fique orfão, troque o valor da chave estrangeira por 3, pois o registro 3 foi criando em ATTENDEE.

### DELETE

A instrução DELETE pode excluir todo o conteúdo de uma tabele `DELETE FROM ATTENDEE`, ou essa exclusão pode ser condicionada a partir da instrução **WHERE**.
```sql
DELETE FROM ATTENDEE
    WHERE PHONE IS NULL
    AND EMAIL IS NULL
```

### TRUNCATE TABLE

De maneira semelhante ao DELETE, em plataformas como o MySQL temos o `TRUNCATE TABLE ATTEMDEE`. Essa opção geralmente permite a otimização de elementos como autoincrementos e chaves estrangeira, para que o banco mantenha seu funcionamento.

### UPDATE

A instrução UPDATE modifica registros existentes. Por exemplo
```sql
UPDATE ATTENDEE SET EMAIL = UPPER(EMAIL) -- Deixar todo campo de e-mail com letra maiúscula --

UPDATE ATTENDEE SET FIRST_NAME = UPPER(FIRST_NAME),
LAST_NAME = UPPER(LAST_NAME) -- Alterar nome e sobrenome para letra maiúscula

UPDATE ATTENDEE SET VIP = 1
WHERE ATTENDEE_ID IN (3,4) -- Alterar o valor de vip em ATTENDEE onde o valor do ATTENDEE_ID for 3 e 4
```

### DROP TABLE
A instrução DROP TABLE é perigosa pois pode excluir um tabel de forma irrecuperável: `DROP TABLE MY_UNWANTED_TABLE`