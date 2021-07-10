# Administração de Usuários

users, roles e groups; administrar estes; GRANTS (administrar acessos)

**Definição**: 

Roles (papeis ou funções), users e grupo de usuários são "contas", perfis de atuação em um banco de dados, que possuem permissões em comum ou específicas. Nas versões anteriores do PostgreSQL 8.1, users e roles eram distintos; hoje são alias

É possível que roles pertençam a outras roles



***Exemplo***: Criou role de administrador, com acessos ilimitados; depois uma de professores, que tem permissões em escrever duas tabelas, mas pode ler todas as outras; cria uma última, os alunos, que podem apenas ler as tabelas. 

Agora cria-se uma Role, Daniel, que pertence aos professores, assim como a role Robert, outra role das role dos professores.

Perceba que a role Daniel e Robert não tem permissões exclusivas dentro desse sistema específico, mas eles estando dentro de outra role conhecida como Professores, eles tem mais concessões

## administração dessas categorias 

Sobre o script:

**CREATE ROLE name [[WITH] option [ ... ]]**

**ALTER ROLE role_specification [ WITH ] option [ ... ]**

### tipos de options:

- **SUPERUSER | NOSUPERUSER** (se é um superusuário, o padrão é o NOSUPERUSER; um superusuário não tem restrições no banco de dados, então cuidado, habilitem ela só se extremamente necessário, melhor será se um usuário do próprio administrador tiver essa possibilidade)
- **CREATEDB | NOCREATEDB** (Se a role pode criar um banco de dados)
- **CREATEROLE | NOCREATEROLE** (se a role pode criar outras roles)
- **INHERIT | NOINHERIT** (Sempre que roles estiverem dentro de uma mesma role, todas elas terão as mesmas concessões [e.g. a role Robert e Daniel estando na Role professores])
- **LOGIN | NOLOGIN** (A role pode ou não se coonectar ao banco de dados)
- **REPLICATION | NOREPLICATION** (serve mais se pode fazer um backup ou não)
- **BYPASSRLS | NOBYPASSRLS** (bypasss role level security, trabalha com a segurança dos roles)
- **CONNECTION LIMIT connlimit** (se pode ser ou não criptografado; mais ao fazer operações de restore e backup)
- **[ENCRYPTED] PASSWORD 'password' | PASSWORD NULL** (todo password ele já tem a criptografia predita nas configurações, como o md5, mas é possível mudar)
- **VALID UNTIL 'timestamp'** (até que data a role tem acesso ao banco de dados, como 5 dias)
- **IN ROLE role_name [, ...]** (na criação da role, ao criar uma role com a linha de código lá em cima, você ja consegue realocar ela para outra role [e.g. CREATE ROLE Daniel || IN ROLE professores])
- **IN GROUP role_name [, ...]** (na criação da role, ao criar uma role com a linha de código lá em cima, você ja consegue realocar ela para outro grupo [e.g. CREATE ROLE Daniel || IN GROUP ???])
- **ROLE role_name [, ...]** (na criação da role, ao criar uma role com a linha de código lá em cima, você ja consegue alocar várias outras roles para dentro dela [e.g. CREATE ROLE Daniel || IN ROLE batata, cenoura, frango])
- **ADMIN role_name [, ...]** (passarão a fazer parte da nova role e terão acessos administrativos)
- **USER role_name [, ...]** 
- **SYSID uid**
- Funciona após a criação da role: **GRANT [role a ser concedida] TO [role que recebrá novas concessões]** 
- **REVOKE [role a ser revogada] FROM [role que terá suas permissões revogadas]** (e.g. REVOKE professores FROM daniel)
- **DROP ROLE role_specification** (e.g. DROP ROLE daniel = daniel não faz mais parte do nosso db)



***Praticando***

1. CREATE ROLE professores NOCREATEDB NOCREATEROLE INHERIT NOLOGIN NOBYPASSRLS CONNECTION LIMIT 10;
   ALTER ROLE professores PASSWORD '123';
   CREATE ROLE daniel LOGIN PASSWORD '123';
2. DROP ROLE daniel;
   CREATE ROLE daniel LOGIN PASSWORD '123' IN ROLE professores;

3. CREATE ROLE daniel LOGIN PASSWORD '123' ROLE professores;

## Administrando acessos (GRANT)

GRANTS são privilégios de acesso aos objetos do banco de dados

#### Objetos que os Privilégios concedem

**tabela** 

coluna

sequence

**database**

domain

foreign data wrapper

foreign server

**function**

language

large object

**schema**

tablespacce

type

#### Códigos para os objetos destacados 

**DATABASE**

- GRANT {{CREATE | CONNECT | TEMPORARY | TEMP} [, ...] | ALL [PRIVILEGES]};
- ON DATABASE database_name [, ...];
- TO role_specification [, ...] [WITH GRANT OPTION];

**SCHEMA**

- GRANT {{CREATE | USAGE} [, ...] | ALL [PRIVILEGES]}; 
- ON SCHEMA schema_name [, ...];
- TO role_specification [, ...] [WITH GRANT OPTION];

**TABLE**

GRANT {{SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER} [, ...] | ALL [PRIVILEGES]};

ON {[TABEL] table_name [, ...] | ALL TABLES IN SCHEMA schema_name[, ...]};

TO role_specification [, ...] [WITH GRANT OPTION];



#### REVOKE 

[role a ser revogada] FROM [role que terá suas permissões revogadas] (e.g. REVOKE professores FROM daniel)

há também como revogar todas as permissões de forma simplificada :

- REVOKE ALL ON ALL TABLES IN SCHEMA [schema] FROM [role];
- REVOKE ALL ON [SCHEMA | DATABASE] [schema | database] FROM [role];

***praticando***

1. CREATE TABLE teste (nome varchar);
   GRANT ALL ON TABLE teste TO professores;
   CREATE ROLE daniel LOGIN PASSWORD '123';
2. DROP ROLE daniel;
   CREATE ROLE daniel INHERIT LOGIN PASSWORD '123' IN ROLE professores;

3. REVOKE professores FROM daniel;