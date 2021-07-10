# Ferramenta PGAdmin

visão geral; configurar acesso ao servidor; visão geral ao cluster 

Site oficial:

https://www.pgadmin.org/

Download com instruções passo a passo:

https://www.pgadmin.org/download

Documentação completa:

https://www.pgadmin.org/docs/pgadmin4/latest/index.html

Importante para conexão:

- Liberar acesso ao cluster em postgresql (através do arquivo postgresql.conf)
- Liberar acesso ao cluster para o usuário do banco de dados em pg_hba.conf
- criar/editar usuários



(ele vai explicar sobre como utilizar o PostgreSQL no terminal, enquanto isso haverá alguns comentários durante a sua utilização): 

- o psql aceita diversas opções como "psql -h 127.0.0.1 -p 5432 -U postgres", que são host, porta e usuário; mas isso é o que ele já fará naturalmente, ou seja, isso é caso queiramos fazer algo de diferente na entrada ao banco de dados.
- ALTER USER retorna ALTER ROLE 
- forçar com que o sistema exija uma senha, mesmo que o usuário criador seja o mesmo usuário que está requisitando acesso, é possível ao apagar o peer em pgadmin.conf  > desce até as letras brancas > apagar os peer's > salva o arquivo 
- alterar a configuração pgadmin não necessita que precise restartar o banco todo: basta dar um reload: "ctlcluster 11 aula reload"
- Sabe todas aquelas ferramentas administrativas? ao criar um banco de dados, todas elas são interagíeis pelo Tools no PGAdmin4 (detalhe que tem um Grant Wizard... nome legal :open_mouth:)













