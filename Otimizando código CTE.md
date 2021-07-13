# Otimizando código CTE

### CTE - Commom Table Expression

forma auxiliar de organizar statements (blocos de códigos formados, por exemplo, por SELECTs, INSERTs, UPDATEs ou DELETEs) para consultas muito grandes, gerando tabelas temporárias e criando relacionamentos entre elas. Normalmente se usa para lógicas mais complexas do que apenas para duas tabelas, que no caso se utiliza uma JOIN.

**WITH STATEMENTS**

WITH [nome1] AS (

SELECT (campos,) FROM tabela_A [WHERE]), [nome2] AS (

SELECT (campos,) FROM tabela_B [WHERE])



SELECT [nome1].(campos,), [nome2].(campos,) FROM [nome1] JOIN [nome2] ....



***Praticando:***

1. SELECT numero, nome FROM banco;
   SELECT numero, nome, banco_numero FROM agencia;
2. 
   WITH tbl_tmp_banco AS (
   	select numero, nome from banco
   )
   select numero, nome from tbl_tmp_banco;

3. WITH params AS (
   	select 213 AS banco_numero
   ), tbl_tmp_banco AS (
   	Select numero, nome from banco
   	join params on params.banco_numero = banco.numero
   )
   select numero, nome from tbl_tmp_banco;
4. 
   select banco.numero, banco.nome 
   from banco join (SELECT 213 AS banco_numero
   ) params on params.banco_numero = banco.numero

5. WITH clientes_e_transacoes as (
   	select  cliente.nome AS cliente_nome,
   			tipo_transacao.nome AS tipo_transacao_nome,
   			cliente_transacoes.valor AS tipo_transacao_valor
   	FROM cliente_transacoes
   	JOIN cliente ON cliente.numero = cliente_transacoes.cliente_numero
   	join tipo_transacao on tipo_transacao.id = cliente_transacoes.tipo_transacao_id
   )
   select cliente_nome, tipo_transacao_nome, tipo_transacao_valor
   from clientes_e_transacoes;

