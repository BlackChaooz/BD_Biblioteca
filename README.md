# BD_Biblioteca
Projeto da Disciplina Banco de dados II
Para o arquivo ser vizualizado é preciso fazer o download e a adição no seu banco de dados, o projeto em questão foi feito usando o MySql.

Grupo: Vinicius Nunes, Rafael José
Disciplina: Banco de Dados II
Projeto: Biblioteca

Para iniciar o projeto primeiramente precisamos explicar um pouco mais sobre o funcionamento de uma biblioteca, todas bibliotecas possuem livros e esses são divididos por sessões que podem ser divididas em gênero, editora ,autoria etc, para essa livraria vamos fazer as divisões padrões de uma biblioteca na parte dos livros e vamos também fazer um perfil para cada usuário para ter um controle dos empréstimos de cada exemplar, à biblioteca apenas vai lidar com empréstimo sem vendas de exemplares. Para entendermos de forma melhor vamos dar olhada no modelo lógico(MER):
 <img src="" width="350" title="hover text">

É um modelo bem simples, nele pode vemos ver as tabelas que serão desenvolvidas no modelo físico, a tabela “LIVRO” por exemplo está ligada a mais 4 tabelas, a tabela gênero, editora,autoria e exemplar, tudo isso para que seja de mais fácil navegação dentro do Banco de dados. Todo exemplar emprestado ficará vinculado a um usuário que por sua vez tem um perfil com os seus prazos e limites para entrega de livros, como podemos ver é um modelo simples e de prática utilização para qualquer tipo de biblioteca. agora vamos para o modelo físico(MR): 
<img src="" width="350" title="hover text">

Aqui no modelo físico podemos ver o tipo de variável em cada uma das tabelas como por exemplo na tabela livro temos a variável CD com o tipo VARCHAR 45, a variável CD é o código que cada livro vai possuir, cada livro possui um código único para sua identificação, como também o autor,editora,genero entre outros, esse código único serve para facilitar na identificação e procura dos exemplares. Agora vamos falar das principais perguntas relacionadas ao projeto.

1-Para qual tipo de biblioteca esse modelo pode ser aplicado?
	Pode ser aplicado para todos os tipos de bibliotecas, já que com a sua divisão de tabelas ele abrange todo.

2-Como funciona o perfil de cada usuário?
	Cada usuário possui um perfil próprio onde ele mostrará o limite de livros que podem pegar e o prazo de entrega dos livros que possuem.

3-Em qual linguagem o projeto será implementado?
	Em mysql usando o XAMPP.

4-O projeto possui algum limite para tipos de gênero/livro?
	Não, a quantidade de livros se limita a o tamanho da biblioteca.

5-Para que servem os códigos em cada tabela?
	Os códigos servem para uma identificação única, como por exemplo, cada autor vai ter o seu código único que será aplicado em seus livros na parte autor_código.

6-Será possível fazer alterações nas tabelas?
	Sim, através de scripts é possível criar/editar/excluir as tabelas.

7-Após a criação completa do banco de dados será possível adicionar novas entradas de dados?
	Sim, através de scripts é possível adicionar/editar/excluir os dados.
 
8-Como cada usuário ficará vinculado ao livro?
	Ficará vinculado ao cpf de cada usuário.

9-Qual o limite de livros por usuário?
	O limite é de 2 livros

10-Qual o prazo para entrega dos livros?
	7 dias.

11-O prazo pode ser estendido?
	Sim, ao chegar na data limite o usuário deve levar o livro para efetivação de mais 7 dias caso desejar.

12- É possível agendar um livro ?
	Não, devido a ser um modelo simples não possui a funcionalidade para agendamento.

13-O que acontece se o usuário passar do prazo de entrega?
	O responsável pela biblioteca precisa entrar em contato com o mesmo, pois o banco de dados apenas mostra o tempo do prazo.

14- É possível passar do limite de 2 livros?
	Não, pois ao entrar no perfil do usuário o responsável verificará que o mesmo já possui o limite, em caso de erro humano pode ocorrer a vinculação de mais de 2 livros em um único usuário.

15- Qual o principal objetivo do projeto?
	A criação de um modelo simplificado de uma biblioteca que pode ser implementada em qualquer local.

16-Esse modelo pode ser implementado em escolas?
	Sim, é um modelo de fácil implementação.

17-É possível um autor desconhecido ter o seu livro na biblioteca?
	Sim, em casa de autores novos é possível através dos scripts a adição de seus livros.

18-É possível adicionar teses de doutorado?
	Sim, da mesma forma que é possível adicionar novos autores é possível adicionar as teses.

19-É necessário um usuário para poder obter um livro?
	Sim, pois como já foi dito anteriormente o livro fica vinculado ao cpf do usuário.

20-Em caso de crianças que não possuem cpf o que acontece?
	Elas não podem fazer o cadastro, com isso é necessário que um adulto faça o cadastro no seu lugar.

Scripts:
Tabelas

create table `editora` ( `codigo` INT NOT NULL AUTO_INCREMENT , `nome` VARCHAR(255) NOT NULL , `cidade` TEXT NOT NULL , PRIMARY KEY (`codigo`)) ENGINE = InnoDB;

create table `genero` ( `codigo` INT NOT NULL AUTO_INCREMENT , `nome` VARCHAR(255) NOT NULL , PRIMARY KEY (`codigo`)) ENGINE = InnoDB;

create table `livro` ( `CD` INT NOT NULL AUTO_INCREMENT , `titulo` VARCHAR(255) NOT NULL , `edicao` INT NOT NULL ,`volume` INT NOT NULL, `genero_codigo` INT NOT NULL, `editora_codigo` INT NOT NULL, PRIMARY KEY (`CD`)) ENGINE = InnoDB;

create table `autoria` ( `livro_CD` INT NOT NULL AUTO_INCREMENT , `autor_codigo` INT  NOT NULL ,PRIMARY KEY (`livro_CD`)) ENGINE = InnoDB;

create table `autor` ( `codigo` INT NOT NULL AUTO_INCREMENT , `nome` VARCHAR(255) NOT NULL ,PRIMARY KEY (`codigo`)) ENGINE = InnoDB;

create table `exemplar` ( `livro_CD` INT NOT NULL AUTO_INCREMENT , `codigo` INT NOT NULL , `status` VARCHAR(255) NOT NULL , PRIMARY KEY (`livro_CD`)) ENGINE = InnoDB;

create table `emprestimo` ( `exemplar_livro_CD` INT NOT NULL AUTO_INCREMENT , `exemplar_codigo` INT NOT NULL , `usuario_cpf` INT NOT NULL , `data_emprestimo` VARCHAR(255) NOT NULL , `data_entrega` VARCHAR(255) NOT NULL , `prazo` INT NOT NULL ,  PRIMARY KEY (`exemplar_livro_CD`)) ENGINE = InnoDB;

create table `usuario` ( `cpf` VARCHAR(255) NOT NULL , `nome` VARCHAR(255) NOT NULL , `endereco` VARCHAR(255) NOT NULL , `telefone` VARCHAR(255) NOT NULL , `email` VARCHAR(255) NOT NULL , `perfil_codigo` INT NOT NULL , PRIMARY KEY (`perfil_codigo`)) ENGINE = InnoDB;

create table `perfil` ( `codigo` INT NOT NULL AUTO_INCREMENT , `nome` VARCHAR(255) NOT NULL , `limite` INT NOT NULL , `prazo` INT NOT NULL , PRIMARY KEY (`codigo`)) ENGINE = InnoDB;

Alteração de Tabelas caso necessário

ALTER TABLE livro
DROP PRIMARY KEY;

ALTER TABLE autor
ADD  status constraints;

ALTER TABLE livro
ADD  ID_Autor  SMALLINT NOT NULL;

ALTER TABLE livro
ADD CONSTRAINT fk_ID_Autor
FOREIGN KEY (ID_Autor)
REFERENCES tbl_autores (ID_autor)
ON DELETE CASCADE
ON UPDATE CASCADE;

ALTER TABLE livro
ADD CONSTRAINT fk_id_editora
FOREIGN KEY (ID_editora)
REFERENCES tbl_editoras (ID_editora)
ON DELETE CASCADE
ON UPDATE CASCADE;

Excluir tabelas caso necessário

DROP TABLE `nome da tabela`

Inserir dados nas tabelas

INSERT INTO `editora` VALUES (null, “editora IFPE”,”Paulista”);

INSERT INTO `genero` VALUES (null, “Estudo”);

INSERT INTO `livro` VALUES (null, “Python para Leigos”, 1, 1, 1, 1);

INSERT INTO `autoria` VALUES (null,1);

INSERT INTO `autor` VALUES (null,”Francisco da Silva”);

INSERT INTO `exemplar` VALUES (null, 1, “disponivel” );

INSERT INTO `emprestimo` VALUES (null, 1, 12312544126, “11/01/2022”,”18/01/2022”,7 );

INSERT INTO `usuario` VALUES (12312544126, “Pedro da Silva Gomes”,”Rua 10”, “81-988554411”,”pedrosilvagomes@gmail.com”,1);

INSERT INTO `perfil` VALUES (null, “Pedro da Silva Gomes”,2, 7);

Atualizar dados

UPDATE livro
SET titulo = “SSH, o Shell Seguro”
WHERE CD = 1;

UPDATE livro
SET edição = 2
WHERE CD = 1;

UPDATE livro
SET volume = 3
WHERE CD = 2;

UPDATE genero
SET nome = “Fantasia”
WHERE CD = 3;

UPDATE editora
SET nome = “editora PE”
WHERE CD = 2;

UPDATE editora
SET cidade = “Abreu e Lima”
WHERE CD = 4;

UPDATE autor
SET nome = “Machado de Assis”
WHERE CD = 5;

UPDATE exemplar
SET status = “indisponivel”
WHERE CD = 10;

UPDATE emprestimo
SET prazo = “5”
WHERE CD = 2;

UPDATE emprestimo
SET data_entrega = “15/02/2022”
WHERE CD = 15;

UPDATE emprestimo
SET data_emprestimo = “12/03/2022”
WHERE CD = 9;

UPDATE usuario
SET nome = “Pedro Gomes Pereira”
WHERE CD = 5;

UPDATE usuario
SET endereço = “Rua do centro”
WHERE CD = 7;

UPDATE usuario
SET telefone = “83-944112277”
WHERE CD = 1;

UPDATE usuario
SET email = “marcosantoniojunior@gmail.com”
WHERE CD = 13;

UPDATE perfil
SET nome = “Pedro Gomes Pereira”
WHERE CD = 5;

UPDATE perfil
SET limite = 1
WHERE CD = 12;

UPDATE perfil
SET prazo = 5
WHERE CD = 4;

DELETE FROM livro
WHERE CD = 2;

DELETE FROM usuario 
WHERE perfil_codigo = 3;


Realizar Busca

SELECT * from editora;

SELECT * from genero;

SELECT * from livro;

SELECT * from autoria;

SELECT * from autor;

SELECT * exemplar;

SELECT * from emprestimo;

SELECT * from usuario;

SELECT * from perfil

SELECT titulo
FROM livro
INNER JOIN exemplar
ON livro.titulo = exemplar.disponivel;

SELECT titulo
FROM livro
INNER JOIN genero
ON livro.titulo = genero.Estudo;

SELECT titulo
FROM livro
INNER JOIN genero
ON livro.titulo = genero.Fantasia;

SELECT titulo
FROM livro
INNER JOIN genero
ON livro.titulo = genero.Aventura;

SELECT titulo
FROM livro
INNER JOIN genero
ON livro.titulo = genero.Acao;
SELECT titulo
FROM livro
INNER JOIN genero
ON livro.titulo = genero.Manga;

SELECT titulo
FROM livro
WHERE genero_codigo = 1 ( SELECT status FROM exemplar WHERE disponivel)

SELECT titulo
FROM livro
WHERE editora_codigo = 4 ( SELECT status FROM exemplar WHERE disponivel)

SELECT codigo
FROM perfil
WHERE prazo = 0 ( SELECT prazo FROM emprestimo WHERE 0)

SELECT limite
FROM perfil
WHERE limite = 0 ( SELECT data_entrega FROM emprestimo WHERE 12/02/2022)

SELECT status
FROM exemplar
WHERE status = indisponivel ( SELECT data_entrega FROM emprestimo WHERE 15/02/2022)

