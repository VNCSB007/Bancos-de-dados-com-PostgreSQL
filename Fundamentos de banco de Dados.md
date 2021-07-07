# Fundamentos de banco de Dados( ***Fundamentos do banco de dados; modelo relacional; introdução ao PostgreSQL***)

### tenha sempre conhecimento de novas tecnologias que podem implementar o seu trabalho

gerenciamento de banco de dados

Introdução ao mundo dos bancos de dados

O que são **dados**? valores e fatos brutos, documentados, recolhidos e armazenados sem qualquer tratamento (54646464 6549845498 5498764 987945 98764648 6546 64648 8; Emergencia fdaniel devanildo postgard italia; ppp ooo hjhj hjhj hjhj jjjj hhhh). 

**Informação** é o contexto com o qual vários dados fazem sentido: quando o sentido é estabelecido para os dados (2002... idai? 2002 é o ano que roberto nasceu... ah, entendi!)

Dado -> informação -> compreensão

***Modelo relacional***

modelar: criar um modelo que explica as funcionalidades de um programa que será escrito em um script qualquer. O modelo de dados tem ferramentas que permitem dizer como os dados estão organizados e relacionados. Todos os dados armazenados serão assim feito em tabelas de linhas e colunas.

O modelo relacional classifica os dados em linhas e colunas. As linhas (**tuplas**; não confundir com as listas imutáveis do Python) são os dados organizados da tabela, e as colunas são os atributos destes dados.

No modelo relacional, no exemplo dos dados Telefone e Nomes exemplificado pelo professor, não há uma relação inicial entre os números inicialmente aleatórios e os nomes, mas sabemos entre todos aqueles dados obtidos o que é um némero e o que é um nome (tirando a filha do Elon Musk, mas enfim). Só depois de uma análise saberemos de quem é quem. (lembrando que nem toda informação resgatada para ser analisada necessariamente vai ter o seu dado correspondente naquele banco de dados; ademais, talvez não haja dados suficientes para ser dado o seu contexto, como um número sem dono ou uma pessoa que não tem um telefone, assim como uma pessoa pode ter vários telefones, ou até mesmo um telefone ser propriedade de várias pessoas).

Tipos de Tabelas: **coisas tangíveis** (carro, moto, produto, animal); **funções**; perfis de usuário, status de compra; **evento ou ocorrências**; Produtos de um pedido, histórico de dados

*- Mas quais colunas utilizar na tabela?* (**importante!**)

Chave Primária / Primary Key / **PK**: 

(Coluna Importante) é um conjunto ou mais de campos que nunca se repete. Identidade da tabela; são utilizados como índice de referência na criação de relacionamentos entre tabelas.

Chave estrangeira / Foreign Key / **FK**:

valor de referência a uma PK de outra tabela para criar um relacionamento entre aquela e a tabela que se está utilizando

Um exemplo de modelo relacional:

<img src="C:\Users\vinic\AppData\Roaming\Typora\typora-user-images\image-20210707021736658.png" alt="image-20210707021736658" style="zoom:%;" />

**SGBD** (Sistema de Gerenciamento de Banco de Dados ou Sistema de Gestão de Base de Dados): Conjunto de programas ou softwares responsáveis pelo gerenciamento de um banco de dados. Programas de facilitam a administração de um banco de dados

### **Introdução ao PostgreSQL: um SGBD de dados objeto relacional (OPen Source)**

arquitetura dele: multiprocessos (ao instala-lo, vários processos serão iniciados no back-end que atenderão a diversas tarefas, como o Post Master que é o processo principal, e as cópias do Post Master que são os childs que gerenciam as conexões que entram e saem do banco de dados. Toda atividade custa um pedaço da memória do banco de dados; além disso tudo, precisa-se de espaço para gravar em disco no storage, espaço em disco, para depois solicitar processos específicos de escrita em disco).

O modelo do POstgreSQL é um modelo cliente/servidor: há processos que só acontecem na maquina de cliente (interface gráfica, terminal unix, aplicação web ou mobile) versus o que acontece no servidor.

A principais características: opensource; POint in time recovery (se houver problemas às 13:00, é possível recuperar como estava o sistema às 12:59; linguagem procedural com suporte a várias linguagens de programação (perl, python...); códigos robustos como Views, functions, procedures, triggers; Consultas complexas e **CTE** (Commom table expressions); suporte a dados geográficos (PostGIS); Controle de concorrências multi-versão (cada transação de dados é separada, um mecanismo complexo para que estas transações não se confundam)



Instalação e documentação do POstgreSQL:

Site Oficial:

https://www.postgresql.org/

Download com instruções passo a passo:

https://www.postgresql.org/download/

Documentação Completa:

https://www.postgresql.org/docs/manuals/

A fim de criar servidores e baixar pacotes, se usa o executable pgAdmin4

Ferramentas gráficas para interagir com o banco de dados:

https://www.pgadmin.org/

