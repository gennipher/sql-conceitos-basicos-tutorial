# Praticando

Nesse tutorial vamos utilizar PostgreSQL + Docker

## Criando Schemas

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuXvdt7VJafd6b00nLq%2Fimage.png?alt=media&token=6705d6fd-2c13-4997-93d4-407be0d35e22)

Você pode fazer de duas maneiras, uma é clicando com o botão direito sobre **Schema > New Schema**; ou clicar com o botão direito sobre **PostgreSQL - postgres > SQL Editor**. Vamos com a segunda opção.
Vai abrir uma janela igual da imagem abaixo:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuXwUNicG0BwLHvu2i8%2Fimage.png?alt=media&token=c6e8d94a-483d-4b18-b289-d04674cc558f)


Nesta janela é onde vamos escrever nossos scripts.

```sql
CREATE SCHEMA lista_telefonica;
```

Selecione tudo, dê **Ctrl + Enter**, clique com o botão direito sobre **PostgreSQL - postgres > Refresh**. 
Note que seu Schema foi criado.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuXxfgcyqvWa5gXI_cS%2Fimage.png?alt=media&token=af2febc0-7d4c-40f4-9b95-75b9c9f25712)

## Criando Tabelas

```sql
CREATE SEQUENCE sq_lista_telefonica START WITH 1;
CREATE TABLE lista_telefonica(
    id           INT8             NOT NULL,
    nome         VARCHAR(20)      NOT NULL,
    sobrenome    VARCHAR(20)      NOT NULL,
    ddd          CHAR(2)          NOT NULL,
    telefone     CHAR(9)          NOT NULL,
    CONSTRAINT pk_lista_telefonica PRIMARY KEY (ID),
    CONSTRAINT uk_lista_telefonica  UNIQUE (ddd, telefone)
);
CREATE UNIQUE INDEX ix_lista_telefonica ON lista_telefonica (ddd, telefone);
```

Primeiro vamos entender o script acima.

```CREATE SEQUENCE sq_lista_telefonica START WITH 1;``` -> Vai fazer com que o id da sua tabela seja auto incrementado a partir do 1. Mas há uma observação sobre o *SEQUENCE*: rodando o script direto no banco pelo *dbeaver*, o *sequence* não é usado, mas na aplicação onde tem explicitamente essa ligação, funciona. Para funcionar tanto na aplicação quanto direto no banco, teríamos que usar ```default nextval('nome_da_sequencia')``` quando a tabela fosse criada ;

```CREATE TABLE lista_telefonica``` -> cria uma tabela;

```( ... )``` -> Tudo o que estiver entre parêntesis serão dados de sua tabela;

```id / nome / sobrenome / ddd / telefone``` -> são suas colunas;

```INT8``` -> tipo inteiro com 8 bytes;

```VARCHAR(20)``` -> comprimento variável com limite e o limite fica entre ( );

```CHAR(2)``` -> comprimento fixo, completado com brancos;

```NOT NULL``` -> ao usar not null, permitimos que valores sejam adicionados posteriormente;

```CONSTRAINT pk_lista_telefonica PRIMARY KEY (ID)``` -> A restrição PRIMARY KEY (Chave Primária) indica que uma coluna ou grupo de colunas identifica de forma única cada registro em uma tabela;

```CONSTRAINT uk_lista_telefonica UNIQUE (ddd, telefone)``` -> A restrição UNIQUE assegura que os dados sejam únicos.

Puts, coloquei o tipo errado, e agora? Vamos supor que coloquei o tipo do id errado, ao invés de **INT8**, quero **INT2**:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuY1ING_UJFA9OauGIC%2Fimage.png?alt=media&token=052ee126-f668-426f-895b-5d258b7d38db)

```sql
ALTER TABLE lista_telefonica.lista_telefonica ALTER COLUMN id TYPE int2
	USING id::int2;
```

> **lista_telefonica.lista_telefonica**
Porque está duplicado? 
O primeiro **lista_telefonica** é o nome do Schema e o segundo é o nome da tabela.

Ou você pode alterar clicando duas vezes sobre a coluna id **int8** e em Data type, selecionar **int2**:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuY1jTOlBW7sagiQBai%2Fimage.png?alt=media&token=463a51b5-8c25-467a-b3f1-77c1fc12e89d)


Vamos ver as mudanças:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuY25o5RNj2626qXe36%2Fimage.png?alt=media&token=bd165a37-97f7-4b80-9a73-ee5940133f95)

## Inserção de Registros

```sql
INSERT
	INTO
	lista_telefonica.lista_telefonica (id,
	nome,
	sobrenome,
	ddd,
	telefone)
VALUES (1,
'Jennifer',
'Omena',
'21',
'977778888');
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuY6Q_Wilo4rpslxZsu%2Fimage.png?alt=media&token=0556bf67-f5fe-4e5e-a5c7-862df7fed044)

```sql
INSERT
	INTO
	lista_telefonica.lista_telefonica (	id,
	nome,
	sobrenome,
	ddd,
	telefone)
VALUES (2,
'Leonardo',
'Omena',
'21',
'988887777');
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuYCHjS9duqvVns7YKh%2Fimage.png?alt=media&token=0cb638b0-50c8-435e-a5a1-a31149a0a870)

## Exclusão de Registros

Digamos que adicionei um registro erroneamente e quero excluir do meu banco. 

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuYD1mdJC5BYtRnXQD2%2Fimage.png?alt=media&token=612f2e36-62f2-4751-8ff5-c0f10411f3ba)


Vamos deletar o quinto registro:

```sql
DELETE
FROM
	lista_telefonica.lista_telefonica
WHERE
	ID = 5;
``` 

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuYENwb8lHd3yIPOhNd%2Fimage.png?alt=media&token=beee37a9-b2e5-4c07-8912-e81d7eec553d)

## Select Básico

O Select serve para buscar os dados de uma tabela do banco de dados, retornando dados em forma de tabela. 
Digamos que quero saber o nome de todo mundo que está salvo na minha lista telefônica:

```sql
SELECT
	nome,
	sobrenome
FROM
	lista_telefonica.lista_telefonica;
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuYFRJk0X44dsdkkWln%2Fimage.png?alt=media&token=fc4a8ff0-bdc4-4af2-b23d-9b9e85569890)

## Atualização de Registro

Vamos fazer de conta que colocamos o sobrenome do Luciano e da Raquel errado, e precisamos atualizar esses dados. Quero mudar de Ferraz para Mascarenhas.

```sql
UPDATE
	lista_telefonica.lista_telefonica
SET
	sobrenome = 'Mascarenhas'
WHERE
	sobrenome = 'Ferraz'
```

Traduzindo o script acima fica: *Atualize no schema lista_telefonica na tabela lista_telefonica para sobrenome Mascarenhas onde sobrenome está Ferraz*.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuZ-q79QYXePLl05Mb8%2Fimage.png?alt=media&token=b78254c5-fd77-4488-96c1-ae8b8041e674)

## Condicionais

Antes de mais nada, vou inserir mais dois registros na nossa lista telefônica. Observe:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuZ3-E18xLuPMne0fcA%2Fimage.png?alt=media&token=4c0e2107-a08a-47bb-b4a2-d33ff0ebf4a0)


Mas descobrimos que o ddd de *Jessica* e *Giselle* foi salvo como 22, mas é ddd 21. Vamos corrigir usando  condicional, **where**:

```sql
UPDATE
	lista_telefonica.lista_telefonica
SET
	ddd = '21'
WHERE
	ddd = '22'
```

## Ordenação

Vamos pegar todas as colunas e registros e ordenar pelos nomes por ordem alfabética decrescente:

```sql
select
	*
from
	lista_telefonica.lista_telefonica
order by
	nome desc;
```
![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuhnjS_xqaljCXbyMyj%2F-Luhq-rJjlTV8CbY_YJ0%2Fimage.png?alt=media&token=d1edafc4-4f97-46e1-af18-ef1d1206f548)


## Agrupamentos

Antes de mais nada, vamos inserir mais alguns registros na nossa lista telefônica:

```sql
INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (7,'Ana','Souza', '91', '922221111');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (8,'Amanda','Silva', '81', '922223333');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (9,'Amadel','Ferreira', '71', '944443333');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (10,'Jorge','Cabral', '91', '977773333');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (11,'Xandi','Lira', '81', '988882222');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (12,'Leandro','Mariani', '91', '900007777');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (13,'Fábio','Lopes', '51', '966661111');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (14,'Manuel','Freitas', '71', '955559999');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (15,'Sonia','Carvalho', '71', '988880000');

INSERT INTO lista_telefonica.lista_telefonica (id, nome,sobrenome,ddd,telefone) VALUES (16,'Marta','Estevez', '81', '911110000');
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuhqtkPIbUh55FlZXpw%2F-Lui5U_p7eROtDjUaYqy%2Fimage.png?alt=media&token=c246d1ea-13e1-4e32-8910-b6ce02cc0c76)


Agora quero agrupar todo mundo que tenha o mesmo sobrenome, ordernar por ddd e quero saber quantas pessoas têm o mesmo sobrenome. Para isso, vamos precisar de um contador.

```sql
SELECT
	sobrenome,
	ddd,
	count(id) "total agrupado"
FROM
	lista_telefonica.lista_telefonica lt
GROUP BY
	ddd,
	sobrenome
ORDER BY  
	ddd
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuhqtkPIbUh55FlZXpw%2F-Lui40mjIvOmWK4lW3np%2Fimage.png?alt=media&token=dbf4d38f-336d-4d0b-88ce-cbcd0bc199f3)