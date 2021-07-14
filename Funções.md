# Funções

### Definição

Conjunto de códigos que são **executados em uma transação** com a finalidade de facilitar a programação e obter o reaproveitamento/reutilização de códigos

4 tipos:

- Query Language Functions (funções escritas em SQL)
- Procedural language functions (funções em PL/pgSQL ou PL/py)
- internal functions - que já vem com o postgre
- C-language functions (em C ou C++)

Lembre-se que também é possível alterar o código fonte 

O foco dessa aula é em **USER DEFINED FUNCTIONS**, logo, funções que podem ser criadas por usuários

### Linguagens 

Por exemplo, utilizar a PL/R é ótima quando se estiver trabalhando com vetores num banco de dados, já que essa é a especialidade do R Programming (velocidade de processamento e economia no uso de recursos, entre outros benefícios)

- SQL (Focaremos aqui)
- PL/PGSQL (Focaremos aqui)
- PL/PY
- PL/R (https://github.com/postgres-plr/plr)
- PL/RUBY
- PL/Java
- PL/Lua
- PL/PHP
- ...

Sobre as linguagens em : https://www.postgresql.org/docs/11/external-pl.html

### returns

tipo de retorno (data type)

- integer
- char/varchar
- boolean
- row
- table
- json

### Segurança

Security

- INVOKER (permite que a função seja executada com as permissões do usuário que está executando a função. Se ele não tiver acesso ao INSERT e essa função tive-lo, haverá uma mensagem de erro)

- DEFINER (o usuário que esta executando a função execute-a com as permissões de quem a criou, logo, no caso, ele poderia acessar o INSERT da função mesmo estando impedido fora dela; **CUIDADO**)

  ### COMPORTAMENTO

  - IMMUTABLE: não pode alterar o banco de dados. Funções que garantem o mesmo resultado para os mesmos argumentos/parâmetros da função. Evitar a utilização de SELECTs, pois tabelas podem sofrer alterações
  - STABLE: Não pode alterar o db. Funções que garantem o mesmo resultado para os mesmos argumentos/parâmetros da função. Trabalha melhor com tipos de current_timestamp e outros tipos variáveis. Podem conter SELECTs
  - VOLATILLE: Comportamento padrão. Aceita todos os cenários 

o IMMUTABLE  e o STABLE são parecidos. pense o fun_somar com dois parâmetros e você inserir 2, 2. no IMMUTABLE sempre garante o imutável, sem update na tabela, já a stable permite select e timestamps. o valatille permite tuto.

### Segurança e Boas Práticas

- CALLED ON NULL INPUT: padrão; mesmo que haja argumentos NULL, será executado.

- RETURNS NULL ON NULL INPUT: se houver um parâmetro NULL, a função retornará NULL

- SECURITY INVOKER: (permite que a função seja executada com as permissões do usuário que está executando a função. Se ele não tiver acesso ao INSERT e essa função tive-lo, haverá uma mensagem de erro. Pense que essa função é como uma fechadura: apenas quem tiver a chave correta poderá abrir essa porta)

- SECURITY DEFINER: (o usuário que esta executando a função execute-a com as permissões de quem a criou, logo, no caso, ele poderia acessar o INSERT da função mesmo estando impedido fora dela; **CUIDADO**, pense numa porta sem fechadura)

- SELECT COALESCE: retorna o primeiro objeto não nulo, logo, numa lista de (null, null, 'digital', 'daniel'), retornará digital. No caso da função de Soma de dois inteiros, caso se utilize As $$ SELECT COALESCE ($1, 0) + COALESCE ($2, 0), o termo nulo se considera 0 e assim é possível executar a função trnaquilamente

  para se testar o SECURITY INVOKER, faça CREATE USER restrito e peça para ele utilizar a função

### Recursos

- COST: Custo/row em unidades de CPU
- ROWS: número estimado de linhas que serão analisadas pelo planner

## SQL FUNCTIONS

CREATE OR REPLACE FUNCTION fc_somar (INTEGER, INTEGER) 

RETURNS INTEGER 

LANGUAGE SQL

AS $$

​			SELECT $1 + $2;

$$;

### OU

CREATE OR REPLACE FUNCTION fc_somar (num1 INTEGER, num2 INTEGER)

RETURNS INTEGER

LANGUAGE SQL

AS $$

​			SELECT num1 + num2;

$$;



