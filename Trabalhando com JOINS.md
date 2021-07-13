# Trabalhando com JOINS

no momento que precisa unir uma ou mias tabelas no seu select principal. Temos:

- JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN
- CROSS JOIN

### JOIN ou INNER JOIN 

você vai ter um campo em uma tabela que é idêntico ao campo em outra tabela, e apenas os dados que pertencem a essa relação que serão mostrado em sua query (se cada tabela tem 10 registros mas apenas três em comum, apenas as três surgirão). Se utiliza o ON nesse caso como condição 

***Como funciona:***

SELECT tabela_1.campos, tabela_2.campos FROM tabela_1 JOIN tabela_2 ON tabela_2.campo = tabela_1.campo;

***Exemplo:***

SELECT count(distinct banco.numero) FROM banco JOIN agencia ON agencia.banco_numero = banco.numero;

### LEFT JOIN ou LEFT OUTER JOIN



ao relacionar às tabelas, temos aquelas às esquerdas e à direita. Um left join, as tabelas de relacionamento à esquerda trarão os dados à esquerda, enquanto que aquelas à direita trarão apenas aqueles que se relacionam com a tabela da esquerda 

***Como funciona:***

SELECT tabela_1.campos, tabela_2.campos FROM tabela_1 LEFT JOIN tabela_2 ON tabela2_campo = tabela_1.campo

***Exemplo:***

SELECT banco.numero, banco.nome, agencia.numero, agencia.nome FROM banco LEFT JOIN agencia ON agencia.banco_numero = banco.numero;

### RIGHT JOIN ou RIGHT OUTER JOIN 

ao relacionar às tabelas, temos aquelas às esquerdas e à direita. Um right join, as tabelas de relacionamento à direita trarão os dados à direita, enquanto que aquelas à esquerda trarão apenas aqueles que se relacionam com a tabela da direita 

***Como funciona:***

SELECT tabela_1.campos, tabela_2.campos FROM tabela_1 RIGHT JOIN tabela_2 ON tabela2_campo = tabela_1.campo

***Exemplo:***

SELECT banco.numero, banco.nome, agencia.numero, agencia.nome FROM banco RIGHT JOIN agencia ON banco.numero = agencia.banco_numero;

### FULL JOIN ou FULL OUTER JOIN

vai trazer os dois registros relacionados na tabela 1 e na tabela 2, os oitos registros na tabela 1 sem relação e depois os 18 registros da tabela 2 que não tem relação (não recomendo se precisa de acesso rápido, como a relação é FULL virá mais coisa do que você precisa de fato)

***Como funciona:***

SELECT tabela_1.campos, tabela_2.campos FROM tabela_1 FULL JOIN tabela_2 ON tabela2_campo = tabela_1.campo

a exibição é em linhas: irá mostrar todos os que tem relação, depois mostrará as características que não tem nada relacionado (como uma loja de roupas sem tamanho p, teremos tuplas e tuplas onde o valor 'p' estará sozinho)

***Exemplo:***

SELECT banco.numero, banco.nome, agencia.numero, agencia.nome FROM banco FULL JOIN agencia ON agencia.banco_numero = banco.numero;

### CROSS JOIN

todos os dados de uma tabela serão cruzados com todos os dados da tabela referenciada, criando uma matriz. **CUIDADO**, apenas em situações extremas, pois é um desperdício de recurso na produção e pode impactar toda a produção

***Como funciona:***

SELECT tabela_1.campos, tabela_2.campos FROM tabela_1 CROSS JOIN tabela_2



