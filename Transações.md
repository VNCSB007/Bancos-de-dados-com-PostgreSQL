# Transações

### Definição

Conceito fundamental de todo sistema db relacional. Conceito de múltiplas etapas/códigos reunidos em apenas 1 transação, onde o resultado precisa ser **tudo ou nada.** 

Imagine que tem um db de transação financeira, então UPDATE conta SET valor = valor -100 WHERE nome =  'Alice' e fazer a mesma lógica só que de transferência para Bob, só que o update acontece com a Alice mas não para o Bob, sumindo com o "dinheiro" que estava sendo negociado. A transação, se tiver erro, precisa de um *Hold Back*, o que desfaz todas as transações que foram executadas entre BEGIN e COMMIT; se tudo der certo, o comando final é o COMMIT.

***Como Funciona:***

**BEGIN;**

​			UPDATE conta SET valor = valor - 100 WHERE nome = 'Alice';

​			UPDATE conta SET valor = valor + 100 WHERE nome = 'Bob';

**[ COMMIT ou ROLLBACK ];**

nesse caso, uma forma de garantir que nenhum dinheiro seja perdido, o rollback acontecerá caso tudo dê certo com o script.

### SAVEPOINT

tem também a ferramenta **SAVEPOINT** para salvar o progresso até certo momento do código. caso dê erro, o savepoint em conjunção com o rollback **pulam** os comandos que deram erro e continuam com o programa

***Como Funciona:***

**BEGIN;**

​			UPDATE conta SET valor = valor - 100 WHERE nome = 'Alice';

**SAVEPOINT my_savepoint;**

​			UPDATE conta SET valor = valor + 100 WHERE nome = 'Ronaldo';

-- Ops -- não é para Ronaldo, é para Bob!!

**ROLLBACK TO my_savepoint;**

UPDATE conta SET valor = valor + 100 WHERE nome = 'Bob';

**COMMIT;**





