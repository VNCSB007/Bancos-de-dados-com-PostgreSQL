# DML e Truncate

Revisão da aula passada; DML ou CRUD (Create, Read, Update e Delete); Truncate 



revisão: PK | FK | tipos de dados | DDL | DML 

IDEMPOTÊNCIA

propriedade que algumas ações/operações possuem possibilitando-as de serem executadas diversas vezes sem alterar o resultado após a aplicação inicial (comando IF EXISTS)

**Melhores práticas em DDL:**

importante as colunas das tabelas serem relevantes e terem um atributo direto a um objetivo em comum.

- Criar/acrescentar colunas que são "atributos básicos" do objeto: Tabela CLIENTE: coluna TELEFONE / coluna AGENCIA_BANCARIA (Será que todo mundo realmente tem telefone? Será que o campo pode ficar nulo? Caso sim, coloque-o em um outra tabela)
- cuidado com regras (vulgo _constraints_)
- Cuidado com FKs excessivas (pensa em criar outras tabelas, afinal elas são _constraints_)
- Cuidado com tamanho exagerado de colunas (coluna CEP VARCHAR(9999))

## DML - CRUD

### SELECT

é uma consulta que você vai fazer no banco de dados

SELECT [campos desejados] FROM [tabela] [condições]

***EXEMPLO:***

- SELECT numero FROM banco WHERE ativo IS TRUE;

- SELECT nome, numero FROM cliente WHERE email LIKE '%gmail.com';

- SELECT numero FROM agencia WHERE banco_numero IN (SELECT nome, numero FROM cliente WHERE email LIKE '%gmail.com') **NÃO RECOMENDÁVEL, POIS CONSOME MUITO CPU, BUFFER, ENTER OUTROS**

  #### CONDIÇÃO (WHERE/AND/OR)

  WHERE (coluna/condição):

  - =
  - " > "/ " >= "
  - < / <=
  - <> / !=
  - LIKE
  - ILIKE
  - IN

Primeira condição sempre WHERE

Demais condições, AND ou OR



Recomendação: usar LEFT JOIN 

Cuidado com SELECT * (a diferença entre * e um SELECT id pode ser de 100% -> de 6.1Kb para 12Kb)

### INSERT

INSERT (campos da tabela,) VALUES (valores,);

INSERT (campos da tabela,) SELECT (valores,);

***EXEMPLO***

INSERT INTO agencia (banco_numero, numero, nome) SELECT 341, 1, 'Centro da cidade' WHERE NOT EXISTS (SELECT banco_numero, numero, nome FROM agencia WHERE banco_numero = 341 AND numero = 1 AND nome = 'Centro da cidade')

_uma forma melhor de escrever esse bagulho é com o comando ON CONFLICT:_

INSERT INTO agencia (banco_numero, numero, nome) VALUES (341, 1, 'Centro da cidade') ON CONFLICT (banco_numero, numero) DO NOTHING

### UPDATE

UPDATE (tabela) SET campo1 = novo_valor WHERE (condição)

Jamais faça update sem a condição WHERE, muito perigoso pois todos os valores sem exceção serão 'atualizados'

### DELETE 

DELETE FROM (tabela) SET campo1 = novo_valor WHERE (condição)

Sem o WHERE você deleta a tabela inteira

## TRUNCATE 

TRUNCATE [tabela] [only] name [''*''] [, ...] [RESTART IDENTITY | CONTINUE IDENTITY] [CASCADE | RESTRICT]

ele esvazia a sua tabela 

o restart ou continue identity: o padrão é o último; TRUNCATE TABLE banco CONTINUE IDENTITY -> o próximo registro numa tabela de 10 tuplas será o id 11, se for 20 o proximo derá o 21;; no restart identity, ele volta o id da tabela pra 1 pra colocar novos registros (não recomendado)

cascade ou restrict: ultimo é o padrão; respeita se tem FK enão apaga. O cascade apaga as referencias em outras tabelas



 ***praticando:***

1. create table if not exists teste(
   	id serial primary key,
   	nome varchar (50) not null,
   	created_at timestamp not null default current_timestamp
   );

2. DROP TABLE IF EXISTS teste;

3. create table if not exists teste(
   	primary key (cpf),
   	cpf varchar (11) not null,
   	nome varchar (50) not null,
   	created_at timestamp not null default current_timestamp
   );
4. select cpf from teste;
   insert into teste (cpf, nome, created_at) values ('56748939201', 'jose da silva pereira', '2019-07-09 12:00:00');
   insert into teste (cpf, nome, created_at) values ('56748939201', 'jose da silva pereira', '2019-07-09 12:00:00') ON CONFLICT (cpf) DO NOTHING;

5. UPDATE teste SET nome = 'PEdro alvares' WHERE cpf = '56748939201';
   Select * from teste

6. TRUNCATE teste;
   DROP TABLE teste;
