# Funções Agregadas

é possível ver várias delas no site do postgre (https://www.postgresql.org/docs/11/functions-aggregate.html) como reaggregate, every, averagge, var_pop, entre outras e outras. Aqui será discutido AVG, COUNT (opção: HAVING), MAX, MIN, SUM

***Praticando:***

1. select numero, nome from banco;
   select banco_numero, numero, nome from agencia;
   select numero, nome, email from cliente;
   select banco_numero, agencia_numero, cliente_numero from cliente_transacoes;

2. select * from conta_corrente;

3. select column_name, data_type from information_schema.columns where table_name = 'banco';

4. -- AVG
   -- COUNT (having)
   --MAX
   -- MIN
   -- SUM

5. select * from cliente_transacoes;

6. select avg(valor) from cliente_transacoes;

7. select count(numero) from cliente where email ilike '%gmail.com' group by email;

8. select max(valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id;
   select min(valor) from cliente_transacoes;

9. select count(id), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id having count(id) > 150;
   -- bom para verificar registros duplicados em um tabela

10. select sum(valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id order by tipo_transacao_id ASC;
    select sum(valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id order by tipo_transacao_id DESC;

