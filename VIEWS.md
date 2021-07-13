# VIEWS

### Definição

Views são a forma como uma pessoa verá uma base de dados. São 'camadas' para tabelas. São 'alias' para uma ou mais queries. Se a view for feita para uma tabela única, temos **INSERT, UPDATE E DELETE**, além do SELECT. Caso a VIEW ocorra em cima de uma tabela com JOIN, apenas **SELECT** é possível.

***Como Funciona:***

CREATE [OR REPLACE] [TEMP | TEMPORARY] [RECURSIVE] VIEW name [ ( column_name [, ...] ) ] [WITH ( view_option_name [= view_option_value] [, ... ] ) ] AS query [ WITH [ CASCADED | LOCAL] CHECK OPTION ]

OR REPLACE = **CUIDADO** ao fazer caso já tenha dado RUN na view de mesmo nome. Isso substituirá a view que você havia criado e perderá todo o script errado, ouvai dar erro dizendoq que você não pode dar replace em algumas colunas ou em algumas views

TEMP = view temporária, dentro da sessão logada naquele momento; mesmo vale se outra pessoa estiver logada: ela perderá o acesso imediatamente após você sair (mais uma vez, se você se desconectar, a VIEW não estará disponível = CREATE OR REPLACE TEMP VIEW vw_bancos AS(SELECT numero, nome, ativo FROM banco);)

RECURSIVE = Código robusto; um SELECT dentro da view até se esgotar uma determinada opção 

### Criação de VIEW atento à idempotência

idempotência é a condição em que alguma fórmula/condição possa ser repetida sem alterar o valor inicial (como um loop que não necessariamente imprimirá infinitas vezes a mesma coisa, mas que sempre retornará o mesmo resultado)

CREATE **OR REPLACE** VIEW vw_bancos AS (SELECT numero, nome, ativo FROM banco);

SELECT numero, nome, ativo FROM vw_bancos;

CREATE **OR REPLACE** VIEW vw_bancos (banco_numero, banco_nome, banco_ativo) AS (SELECT numero, nome, ativo FROM banco);

SELECT banco_numero, banco_nome, banco_ativo FROM vw_bancos;



Nós não definimos Data_type para views pq eles assumem o tipo de dados da consulta

### INSERT, UPDATE, DELETE

**DISCLAIMER: FUNCIONAM APENAS PARA VIEWS COM APENAS _1_ TABELA**

INSERT INTO vw_bancos (numero, nome, ativo) VALUES (100, 'Banco CEM', TRUE);

UPDATE vw_bancos SET nome = 'Banco 100' WHERE numero = 100;

DELETE FROM vw_bancos WHERE numero = 100;

### RECURSIVE

CREATE OR REPLACE RECURSIVE VIEW (nome_da_view) (campos_da_view) AS (SELECT base UNION ALL SELECT campos FROM tabela_base JOIN (nome_da_view));

É obrigatório o uso da UNION ALL; UNION agrupa resultados iguais, já o UNION ALL não  unifica.

Criei uma tabela, por exemplo, com um PK id e uma FK gerente com referencia para ela mesma. 