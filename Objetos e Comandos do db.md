# Objetos e comandos do banco de dados

database/schemas/objetos

tabelas/colunas/tipos de dados

DML e DDL

### Database, Schemas e Objetos 

significado de database, schema e Objetos (ver aulas anteriores)

Relembrando que objetos são tabelas, views, funções, types, sequences, entre outros, pertencentes a um schema

**Melhores Práticas: **Sempre que for criar um objeto no banco, valide com CREATE SCHEMA IF NOT EXISTS schema_name [ AUTHORIZATION role_specification] ;ou; DROP SCHEMA IF EXISTS [nome];

uma tabela é composta por colunas e tuplas

**Primary Key**: Para se evitar o conflito de dados repetidos, é valioso o PK, já que eles garantem a integridade de um dado único. DICAS SOBRE PK: sem ocorrências repetidas; o atributo da chave primária não pode ser vazio no valor; os atributos identificadores deve ser o mínimo necessário para que se identifique cada instância de uma entidade; a informação não pode ser volátil

**Foreign Key**: sempre referencia uma pk de uma outra schema

uma boa prática do mercado, quando uma pessoa faz várias compras no mesmo nome (gerando um numero de série que repete várias vezes ao longo do banco de dados), é criar um ID que conta de 1 até o número de total de vendas/compras daquele período selecionado.

### Tipos de Dados

- Tipos Numéricos

Tipo monetário

- Tipo carcater

Tipo Dado Binário

- Tipo Hora/Dia | Tipo Booleano 

Tipo Enumerado

Geométrico

Endereço Network

Bit String

Pesquisa de Texto

UUID 

XML

JSON

Array

Composite

Range

Domínio

Identificado de Objeto

pg_lsn

Pseudo Tipo

**Melhores Práticas:** sempre precisa pensar na questão do consumo de recursos da máquina (sempre considere o que de fato é preciso fazer, qual os limites das variáveis e quais as possibilidades reais)

### DML e DDL

**DML** (Data Manipulation Language)

INSERT, UPDATE, DELETE, SELECT*

*SELECT -  alguns consideram como DML; outros como DQL, que é Linguagem de Consulta de Dados

**DDL** (Data Definition Language)

CREATE, ALTER, DROP



***Exemplo de criação de uma tabela***

CREATE TABLE [IF NOT EXISTS] [nome da tabela] (

​	[nome do campo] [tipo] [regras] [opções],	

​	[nome do campo] [tipo] [regras] [opções],	

​	[nome do campo] [tipo] [regras] [opções],	

);



**Melhores Práticas:** crie tabelas separadas, não junte todas as coisas... entre fazer uma tabela super cheia de informações adas mais diversas sobre o assunto a ser estudo e verificado, crie várias tabelas separadas em que elas se comunicam com Foreign Keys

Arquivo completo DDL:

https://www.github.com/drobcosta/digital_innovation_one

***Praticando:***

1. CREATE TABLE IF NOT EXISTS banco(
   	numero INTEGER NOT NULL,
   	nome VARCHAR(50) NOT NULL,
   	ativo BOOLEAN NOT NULL DEFAULT TRUE, 
   	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
   	PRIMARY KEY (numero)
   )
2. CREATE TABLE IF NOT EXISTS agencia(
   	banco_numero INTEGER NOT NULL,
   	numero INTEGER NOT NULL, 
   	nome VARCHAR (80) NOT NULL, 
   	ativo BOOLEAN NOT NULL DEFAULT TRUE, 
   	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
   	PRIMARY KEY (banco_numero, numero),
   	FOREIGN KEY (banco_numero) REFERENCES banco (numero)
   )
3. CREATE TABLE cliente(
   	numero BIGSERIAL PRIMARY KEY,
   	nome VARCHAR(120) NOT NULL, 
   	email VARCHAR(150) NOT NULL,
   	ativo BOOLEAN NOT NULL DEFAULT TRUE, 
   	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
   )
4. CREATE TABLE conta_corrente(
   	banco_numero INTEGER NOT NULL,
   	agencia_numero INTEGER NOT NULL,
   	numero BIGINT NOT NULL,
   	digito SMALLINT NOT NULL,
   	cliente_numero BIGINT NOT NULL,
   	ativo BOOLEAN NOT NULL DEFAULT TRUE, 
   	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
   	PRIMARY KEY (banco_numero, agencia_numero, numero, digito, cliente_numero),
   	FOREIGN KEY (banco_numero, agencia_numero) REFERENCES agencia (banco_numero, numero),
   	FOREIGN KEY (cliente_numero) REFERENCES cliente (numero)
   )
5. CREATE TABLE tipo_de_transacao(
   	id SMALLSERIAL PRIMARY KEY,
   	nome VARCHAR(50) NOT NULL,
   	ativo BOOLEAN NOT NULL DEFAULT TRUE, 
   	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
   )
6. CREATE TABLE cliente_transacoes(
   	id BIGSERIAL PRIMARY KEY,
   	banco_numero INTEGER NOT NULL,
   	agencia_numero INTEGER NOT NULL,
   	conta_corrente_numero BIGINT NOT NULL,
   	conta_corrente_digito SMALLINT NOT NULL,
   	cliente_numero BIGINT NOT NULL,
   	tipo_transacao_id SMALLINT NOT NULL,
   	valor NUMERIC(15,2) NOT NULL,
   	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
   	FOREIGN KEY (banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_digito, cliente_numero) REFERENCES conta_corrente (Banco_numero, Agencia_numero, numero, digito, cliente_numero)
   )
7. SELECT table_name, column_name, data_type FROM information_schema.columns WHERE table_name = 'agencia'