# Capítulo 9 - Design de banco de dado

O design de um banco de dados requer a análise de diversos fatores, como por exemplo as entidades presentes no banco, as quais emergem da necessidade da situação em que está sendo averiguada.<br>
Outros fatores que devem ser levado em consideração é os relacionamento dos dado e a segurança do banco.<br>

**Atenção**: para esse capítulo será utilizado o sqlite portable

### A conferência SurgeTech
Criar um banco de dados para gerenciar os participantes, as empresas, as apresentações, as salas e o comparecimento nas apresentações. Como esse banco de dados deve ser planejado?<br>
Primeiramente, análise as entidades e suas estruturas (atributos/campos): participantes, empresas, apresentações, salas e comparecimento.

### Chave primaria e externa

A chave primeira corresponde ao um ID específico de cada entidade. A chave externa ou estrangeira corresponde a um equivalente da chave primaria em outra tabela (entidada) a fim de gerar um relacionamento.

### O esquema

Um esquema de banco de dados é um diagrama que representa as tabelas, colunas (campos) e seus relacionamentos.

### Criando um novo banco de dados
- Nome do banco de dados: surge_tech_conference.db

Para criar tabelas, podemos utilizar a instrução `CREATE TABLE` ou utilizar as ferramentas gráficas.
```sql
CREATE TABLE <nome da tabela>(
    <Colunas> <tipo>
)
```

Ao criar uma tabele é muito importante configurar sua chave principal. No SQLite portable podemos fazer a criação dessa forma:
```sql
CREATE TABLE "COMPANY" (
	"COMPANY_ID"	INTEGER,
	"NAME"	TEXT NOT NULL,
	"DESCRIPITION"	TEXT,
	"PRIMARY_CONTACT_ATTENDEE_ID"	INTEGER NOT NULL,
	PRIMARY KEY("COMPANY_ID" AUTOINCREMENT)
);
```
O **AUTOINCREMENT** em **COMPANY_ID**, significa que a cada novo registro é criado um ID sequencial iniciando em 1.<br>
Também seria possível criar essa mesma tabela com alguns detalhes diferentes, como por exemplo:
```sql
CREATE TABLE "COMPANY" (
	"COMPANY_ID"	INTEGER    PRIMARY KEY AUTOINCREMENT,
	"NAME"	VARCHAR(30) NOT NULL,
	"DESCRIPITION"	VARCHAR(60),
	"PRIMARY_CONTACT_ATTENDEE_ID"	INTEGER NOT NULL,
);
```
Em que VARCHAR significa variáveis de caracteres e entre parenteses a limitação de quantidade caracteres<br>

Vamos agora criar a tabela/entidade ROOM
```sql
CREATE TABLE ROOM(
	ROOM_ID INTEGER PRIMARY KEY AUTOINCREMENT,
	FLOOR_NUMBER INTEGER NOT NULL,
	SEAT_CAPACITY INTEGER NOT NULL
)
```

Vamos agora criar a tabela/entidade PRESENTATION
```sql
CREATE TABLE PRESENTATION(
	PRESENTATION_ID INTEGER PRIMARY KEY AUTOINCREMENT,
	BOOK_COMPANY_ID INTEGER NOT NULL,
	BOOK_ROOM_ID INTEGER NOT NULL,
	START_TIME time,
	END_TIME time
);
```
Vamos agora criar a tabela/entidade ATTENDEE
```sql
CREATE TABLE ATTENDEE(
	ATTENDEE INTEGER PRIMARY KEY AUTOINCREMENT,
	FIRST_NAME VARCHAR (30) NOT NULL,
	LAST_NAME VARCHAR (30) NOT NULL,
	PHONE INTEGER,
	EMAIL VARCHAR(30),
	VIP BOOLEAN DEFAULT
);
```

Vamos agora criar a tabela/entidade PRESENTATION_ATTENDANCE
```sql
CREATE TABLE PRESENTATION_ATTENDANCE(
	TICKET_ID INTEGER PRIMARY KEY AUTOINCREMENT,
	PRESENTATION_ID INTEGER,
	ATTENDEE_ID INTEGER
);
```

### Definindo as chaves externas
É importante saliente que não podemos deixar registros orfãos, ou seja, registro com chave extrangeira na tabela filho obrigatoriamente precisam existir na tabela pai. Exemplo da incrementação da chave estrangeira de BOOK_COMPANY_ID ao pai COMPANY

![Booked_company_id chave externa de company_id](https://github.com/mateusyamaguti/TUTORIAL-Introducao-a-linguagem-sql/blob/main/assets/img/cap9-1.png)

```sql
CREATE TABLE "APRESENTATION" (
	"PRESENTATION_ID"	INTEGER,
	"BOOKED_COMPANY_ID"	INTEGER NOT NULL,
	"BOOKED_ROOM_ID"	INTEGER NOT NULL,
	"START_TIME"	TIME,
	"END_TIME"	TIME,
	PRIMARY KEY("PRESENTATION_ID" AUTOINCREMENT),
	FOREIGN KEY("BOOKED_COMPANY_ID") REFERENCES ""
);
```

Em formato no MySQL
```sql
ALTER TABLE PRESENTATION
ADD CONSTRAINT fk_presentation_booked_company
FOREIGN KEY (BOOKED_COMPANY_ID)
REFERENCES COMPANY (COMPANY_ID);
```

### Criando views
É usual em banco de dados, que uma uma consulta SELECT seja feita diversas vezes. Para facilitar isso, criamos uma VIEW que é basicamente a executação de uma consulta SELECT já pronta. A view também pode ser consultadas com o SELECT. Exemplo de select para fazer uma view posteriomente.

```sql
SELECT
    COMPANY.NAME AS BOOKED_COMPANY,
    ROOM.ROOM_ID AS ROOM_NUMBER,
    ROOM.FLOOR_NUMBER AS FLOOR,
    ROOM.SEAT_CAPACITY AS SEATS,
    START_TIME,
    END_TIME

    FROM PRESENTATION

    INNER JOIN COMPANY
    ON PRESENTATION.BOOKED_COMPANY_ID = COMPANY.COMPANY_ID

    INNER JOIN ROOM
    ON PRESENTATION.BOOKED_ROOM_ID = ROOM.ROOM_ID
```

A partir disso você pode criar um view clicando em ** Create a view** ou **Crie uma vista**.

![Criando VIEW](https://github.com/mateusyamaguti/TUTORIAL-Introducao-a-linguagem-sql/blob/main/assets/img/cap9-1.png)

![Resultado da VIEW](https://github.com/mateusyamaguti/TUTORIAL-Introducao-a-linguagem-sql/blob/main/assets/img/cap9-1.png)
