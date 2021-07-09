# Configuração do Programa

Comentário inicial: para usar o PostgreSQL, vc precisa abrir na barra de busca ali no canto inferior esquerdo por "Serviços", e alfabeticamente buscar PostgreSQL para ver que ele está em execução ou não. Em seguida, buscar por pgAdmin4.



O arquivo **PostgreSQL.conf** é o arquivo que tem todas configurações do servidor PostgreSQL: como o servidor vai se comportar, como trabalhar com a memória, quantos usuários simultâneos permitidos, como será feito as replicações, etc. O arquivo PostgreSQL.conf está localizado dentro do diretório de dados conhecido como PGDATA, que é criado assim que inicializa do cluster de banco de dados Alguns parâmetros só podem ser alterados com uma reinicialização completa do banco de dados. Ao criar o primeiro cluster, o primeiro banco de dados, ele cria um diretório com arquivos e lá estará o PostgreSQL, exceto se for pelo Ubuntu, que é um outro diretório: 

*/etc/postgresql/[versão instalada]/[nome do cluster]/postgresql.conf* 

A **view pg_settings**, quando acessada pelo banco de dados, guarda todas as configurações atuais. A view mostra apenas o que está em execução no momento, e apenas com reinicialização a view será alterada com os novos parâmetros. 

É possível acessar todas as configurações através de dois comandos distintos:

"""

SELECT name, setting

FROM pg_settings

"""

"""

SHOW [parâmetro]

"""

Para encontrar no seu computador Vi, vá em Windows (C:) > Arquivos de Programa > PostgreSQL > 11 > data > pg_data 

### Configuração de conexão

**LISTEN_ADDRESSES**

Endereço(s) Terminal Control Protocol da Internet Protocol (TCP/IP) das interfaces que o servidor PostgreSQL vai escutar/liberar conexões (não coloque asterisco como muitos da internet recomendam caso você crie um banco de dados de produção, pois estará liberando acesso para qualquer IP: coloque os IPs corretos que merecem; exceto quando em momento de teste)

**PORT**

A porta TCP que o servidor PostgreSQL vai ouvir. Uma porta canônica é 5432, mas pode altera-lo

**MAX_CONNECTIONS**

Número máximo de conexões simultâneas no servidor PostgreSQL, 1 conexão consome recursos (pense num termômetro quando for *setar* o número total de acessantes simultâneos), mas a questão é o quanto a sua máquina aguenta de fato e o quanto que o servidor tem de memória de processador

**SUPERUSER_RESERVED_CONNECTIONS**

Número de conexões (slots) reservados para conexões ao banco de dados de super usuários (quando acontece alguma coisa como um entrave para que seja garantido um espaço para alguém ir lá resolver)

**AUTHENTICATION_TIMEOUT**

Tempo máximo de segundos para o cliente conseguir uma conexão com o servidor

**PASSWORD_EXCRYPTION**

Algoritmo de criptografia das senhas dos novos usuários criados no banco de dados

**SSL** (Secure Socket Layer)

habilita a conexão criptografada por SSL (importante para a proteção de dados; mas só é possível se o PostgreSQL foi compilado com suporte SSL)



### Configuração de memória

**SHARED_BUFFERS**

tamanho da memória compartilhada do servidor PostgreSQL para cache/buffer de tabelas, índices e demais relações (os dados de rápido acesso; muitas pessoas recomendam que seja 25%: cuidado com esse número, consiga ajustar finamente esse número para a sua máquina)

**WORK_MEM**

Tamanho da memória para operações de agrupamento e ordenação (ORDER BY, DISTINCT, MERGE JOINS) - ao trabalhar com tabelas e objetos, será feito esse comandos uma hora ou outra e ao fazer, há uma memória exclusiva para ordenar melhor esses dados

**MAINTENANCE_WORK_MEM**

Tamanho da memória para operações como VACUUM, INDEX, ALTER TABLE - algumas operações de administração precisa de memória exclusiva (se um objeto for muito grande, você pode aumentar para cria-lo e em seguida diminuir após concluí-lo, recomendo para que acelere o trabalho)



**Arquivo pg_hba.con**

Arquivo responsável pelo controle de autenticação dos usuários no servidor PostgreSQL. Diferença usuários de consultoria, por exemplo (tem o local do acesso conhecido do IP, o banco de dados, qual o usuário e a forma de autenticação)

### métodos de autenticação: 

TRUST (conexão sem requisição de senha)

REJECT (rejeitar conexões)

MD5 (criptografia md5)

PASSWORD (senha sem criptografia)

GSS (*Generic Security Service* application program interface)

SSPI (*Security Support Provider Interface* - only for Windows 😎)

KRB5 (kerberos V5)

IDENT (utiliza o usuário do sistema operacional do cliente via *ident server*)



***manter segura a sua conexão com o banco de dados:***

PEER (utiliza o usuário do sistema operacional do cliente)

LDAP (ldap server)

RADIUS (radius server)

CERT (autenticação via certificado ssl do cliente)

PAM (pluggable authentication modules. O usuário precisa estar no banco de dados)



**Arquivo pg_ident.conf**

arquivo responsável por mapear os usuários do sistema operacional com os usuários do banco de dados. Localizado no diretório de dados PGDATA de sua instalação. A opção ident deve ser utilizada no arquivo pg_hba.conf



### comandos administrativos

###### Ubuntu

pg_lsclusters: lista todos os clusters do PostgreSQL

pg_createcluster **[version]** **[cluster name]**: Cria um novo cluster PostgreSQL

pg_dropcluster **[version]** **[cluster]**: apaga um cluster PostgreSQL

pg_ctlcluster **[version]** **[cluster]** **[action]**:  Start, Stop, Status, Restart de cluster PostgreSQL

###### CentOS

 systemclt **[action]** **[cluster]**:: 

sendo [cluster] = postgresql-11

systemclt start **[cluster]**: inicia o cluster PostgreSQL

systemclt status **[cluster]**: mostra o status do cluster PostgreSQL

systemclt stop **[cluster]**: para o cluster PostgreSQL

systemclt restart **[cluster]**: Restarta o cluster PostgreSQL

###### Windows

ao abrir Servers (Serviços), você identifica a linha que o PostgreSQL ta rodando e se clicar com o botão direito no status, lá tem o Start, Stop, Pause, Restart, Refresh, propriedades específicas



Acaso alguém tenha decidido compilar o Postgre, é necessário administra-lo pelos binários, e alguns mais importantes são:

createdb (cria um banco de dados em seu cluster, por fora, pelo s.o.)

createuser

dropdb

dropuser

initdb (inicia um novo cluster, com novos arquivos e diretórios)

pg_ctl (controlar o seu banco de dados com stop, pause e etc)

pg_basebackup

pg_dump/ pg_dumpall (pseudo-backup, extrair em um formato de texto as informações daquele momento; NÃO SERVE COMO BACKUP, NÃO PODE DAR RESTORE COM O DUMP)

pg_restore (restaurar os arquivos *dumpados*)

psql (entrar no banco de dados via s.o.)

reindexdb

vacuumdb (reorganizar todas as tabelas, indexs, entre outros)



## ARQUITETURA / HIERARQUIA

**Cluster**: coleção de banco de dados que compartilham as mesma configurações (arquivos de configuração) do PostgreSQL e do s.o. (porta, listen_addresses, etc). É uma forma de agrupar banco de dados (um bd pode estar na porta 5432 enquanto outro está na 5433)

**Banco de Dados** (_database_): conjunto de schemas com seus objetivos/relações (tabelas, funções, views, etc)

**Schema**: Conjunto de objetos/relações (tabelas, funções, views, etc). Tenha cuidado que alguns banco de dados interpretam um Schema como um Banco de Dados (e.g. MySQL, where *create a schema* is the same as create a database), no caso do PostgreSQL está tudo certo :smile:.

logo, a ordem de tamanho é

### Cluster > banco de dados > schema



