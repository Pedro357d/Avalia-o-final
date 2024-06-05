# Avaliacao-final 
# 1 - Cenário
Uma locadora de vídeo chamada “VideoMaster” deseja criar um sistema de banco
de dados para melhor organização de seu sistema. O sistema deve permitir o cadastro de
clientes, funcionários, filmes, diretores e também registrar os empréstimos feitos..
Cada cliente possui um código único, nome, data de nascimento, telefone, endereço,
rua, número e bairro e email. Os funcionários serão cadastrados com um código de
identificação, nome, salário, cargo, e data de contratação. Cada filme terá um código
próprio, título, ano de lançamento, gênero, duração e preço. Os diretores deverão ser
registrados com um número único, nome, nacionalidade e data de nascimento. Por fim, os
empréstimos serão registrados com o número do pedido, forma de pagamento, data de
empréstimo e data de devolução, que deverá ser calculada em uma semana a partir do
empréstimo.
Cada cliente pode executar o empréstimo de vários filmes de uma vez, mas cada
filme só pertence a um cliente. Dessa forma, um empréstimo pode conter vários filmes, mas
um filme só pode estar em um empréstimo. Um diretor pode dirigir diversos filmes, porém
estes apenas terão um diretor.
Todos os funcionários da loja podem gerenciar os filmes e executarem diferentes
empréstimos e cada empréstimo terá um funcionário responsável.
<br/>
# 2 - Modelagem conceitual
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Modelo%20Conceitual.jpg">

# 3 - Demonstração textual do MER e Modelagem lógica
Cliente(**id_cliente**, nome, rua, numero, bairro, data_nasc)
<br/>
Email(**id_email**, email, id_cliente)
<br/>
Telefone(**id_telefone**, numero, id_cliente)
<br/>
Funcionario(**id_funcionario**, nome, data_contratacao, cargo, salario)
<br/>
Diretor(**id_diretor**, nome, nacionalidade, data_nasc)
<br/>
Filme(**id_filme**, titulo, ano_lancamento, duracao, preco, id_diretor)
<br/>
Genero(**id_genero**, tipo_genero, id_filme)
<br/>
Funcionario_Filme(id_funcionario, id_filme)
<br/>
Emprestimo(**id_emprestimo**, forma_pagamento, data_emprestimo, data_devolucao, id_cliente, id_funcionario, id_filme)

## Modelo lógico 
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Modelo%20L%C3%B3gico.png">

# 4 - Modelagem física
```sql
CREATE DATABASE Locadora;
USE Locadora;

CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY IDENTITY,
    nome VARCHAR(80),
    rua VARCHAR(80),
    numero INT,
    bairro VARCHAR(45),
    data_nasc DATE
);
CREATE TABLE Email (
	id_email INT PRIMARY KEY IDENTITY, 
	email VARCHAR(60),
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
 
);
CREATE TABLE Telefone (
	id_telefone INT PRIMARY KEY IDENTITY, 
	numero INT,
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
);
CREATE TABLE Funcionario (
    id_funcionario INT PRIMARY KEY IDENTITY,
    nome VARCHAR(80),
    data_contratacao DATE,
    cargo VARCHAR(40),
    salario DECIMAL(6,2)
);
CREATE TABLE Diretor (
    id_diretor INT PRIMARY KEY IDENTITY,
    nome VARCHAR(80), 
    nacionalidade VARCHAR(50),
    data_nasci DATE 
);
CREATE TABLE Filme (
    id_filme INT PRIMARY KEY IDENTITY,
    titulo VARCHAR(80),
    ano_lancamento INT,
    duracao TIME,
    preco DECIMAL(5,2),
	id_diretor INT,
    FOREIGN KEY (id_diretor) REFERENCES Diretor(id_diretor)
);
CREATE TABLE Genero (
	id_genero INT PRIMARY KEY IDENTITY,
    tipo_genero VARCHAR(40),
    id_filme INT,
    FOREIGN KEY (id_filme) REFERENCES Filme(id_filme)
);
CREATE TABLE Funcionario_Filme (
	id_funcionario INT,
    id_filme INT,
    FOREIGN KEY (id_funcionario) REFERENCES Funcionario (id_funcionario),
    FOREIGN KEY (id_filme) REFERENCES Filme (id_filme)
);


CREATE TABLE Emprestimo (
    id_emprestimo INT PRIMARY KEY IDENTITY,
    forma_pagamento VARCHAR(30),
    data_emprestimo DATE,
	data_devolucao AS DATEADD(DAY,7, data_emprestimo),
    id_cliente INT,
    id_funcionario INT,
    id_filme INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente (id_cliente),
    FOREIGN KEY (id_funcionario) REFERENCES Funcionario (id_funcionario),
    FOREIGN KEY (id_filme) REFERENCES Filme (id_filme)
);
``` 
# 5 - Inserção de dados
```sql
INSERT INTO Cliente (nome, rua, numero, bairro, data_nasc) VALUES 
('Pedro Henrique Cintra Silva', 'Rua Abel de Andrade', 201, 'Jd. Amazonas', '2004-01-15'),
('Maria Silva Oliveira', 'Avenida Brasil', 456, 'Jd. América', '1985-12-20'),
('Pedro Ferreira Santos', 'Rua Franca', 789, 'Vila Nova', '1992-07-10'),
('Ana Pinto Costa', 'Rua dos Pinheiros', 2556, 'Centro', '1988-03-25'),
('Astolfo Robot da Silva', 'Avenida Paulista', 202, 'Bela Vista', '1995-01-30'),
('Karina Neri', 'Rua dos Prazeres', 909, 'Jd. Boa Esperança', '2000-09-14'),
('Bruno Quintana', 'Rua Dr.Rafael Lima', 487, 'Jd. Califórnia', '1987-12-18'),
('Fernanda Oliveira Rodrigues', 'Avenida das Nações', 505, 'Vila Real', '1990-11-05'),
('Gabriel Melo Ferreira', 'Rua Floriano Peixoto', 666, 'Centro', '1996-08-08'),
('Juliana Lima Pereira ', 'Avenida do Contorno', 707, 'Cidade Nova', '1990-02-12'),
('Ricardo Pereira', 'Avenida do Contorno', 707, 'Cidade Nova', '1989-10-22'),
('Mariana Leonardo Carvalho', 'Avenida Independência', 909, 'Parque das Árvores', '1991-04-05'),
('Gustavo Ribeiro Martins', 'Rua das Orquídeas', 1010, 'Centro', '1986-07-16'),
('Patrícia Silva', 'Rua das Rosas', 1111, 'Jardim Primavera', '1993-03-28'),
('Thiago Oliveira', 'Avenida Santos Dumont', 1212, 'Centro', '1988-09-09'),
('Beatriz Fernão Costa', 'Rua Claraval', 1313, 'Vila Maria', '2005-06-21'),
('Felipe Muniz Souza', 'Avenida Brasil', 658, 'Jd. América', '1990-12-30'),
('Larissa Mendes', 'Rua Estavão Silva', 1515, 'Jd. Pauliu', '1992-08-19'),
('Rafael Lima', 'Avenida do Povo', 1616, 'Centro', '1987-05-11'),
('Natália Santos', 'Rua dos Girassóis', 1717, 'Vila Verde', '1991-10-03');

INSERT INTO Email (email, id_cliente) VALUES 
('pedrosilva77@gmail.com', 1),
('mariasoliveira@gmail.com', 2),
('pedrosantos2012@gmail.com', 3),
('anacosta@outlook.com', 4),
('astolforobotsilva@hotmail.com', 5),
('robotoas@gmail.com', 5),
('karinaneri@gmail.com', 6),
('bruninquintana201@outlook.com', 7),
('fernanda.rodrigues@gmail.com', 8),
('gabrielmferreira90@gmail.com', 9),
('jupereira@gmail.com', 10),
('ricardopereira21@outlook.com', 11),
('marianaleo@gmail.com', 12),
('gustavomartins@hotmail.com', 13),
('patriciasilva300@gmail.com', 14),
('thiagoliveira99@gmail.com', 15),
('beatriz.costa@gmail.com', 16),
('felipemunizsouza@yahoo.com.br', 17),
('felipito123@gmail.com', 17),
('larissa.mendes@gmail.com', 18),
('rafaellima@gmail.com', 19),
('nataliaasantos@gmail.com', 20);

INSERT INTO Telefone (numero, id_cliente) VALUES
('987654321', 1),
('998765432', 2),
('997485354', 2),
('912345678', 3),
('923456789', 4),
('934567890', 5),
('945678901', 6),
('956789012', 7),
('947595715', 7),
('967890123', 8),
('978901234', 9),
('989012345', 10),
('990123456', 11),
('901234567', 12),
('912345678', 13),
('988815870', 13),
('923456789', 14),
('934567890', 15),
('945678901', 16),
('956789012', 17),
('967890123', 18),
('978901234', 19),
('989012345', 20);

INSERT INTO Funcionario (nome, data_contratacao, cargo, salario) VALUES 
('Vinicius Lemes', '2015-01-15', 'Gerente', 5500.00),
('Ana Beatriz', '2019-02-20', 'Atendente', 2500.00),
('Fernando Oliveira', '2018-03-25', 'Técnico de Manutenção', 3000.00),
('Juliana Souza', '2023-04-30', 'Atendente', 2500.00),
('Marcelo Lima', '2021-05-10', 'Auxiliar Administrativo', 2800.00),
('Beatriz Costa', '2020-06-18', 'Atendente', 2250.00),
('Rodrigo Martins', '2019-07-22', 'Técnico de Manutenção', 3100.00),
('Patrícia Ferreira', '2016-08-14', 'Gerente de Operações', 6000.00),
('Tiago Mendes', '2017-09-11', 'Atendente', 2250.00),
('Mariana Pereira', '2021-10-05', 'Auxiliar de Limpeza', 2000.00),
('Gustavo Santos', '2020-11-23', 'Atendente', 2650.00),
('Renata Oliveira', '2019-12-02', 'Atendente', 2760.70),
('Leonardo Costa', '2018-01-30', 'Técnico de Manutenção', 3200.90),
('Carla Nunes', '2023-02-17', 'Atendente', 2250.00),
('Ricardo Silva', '2021-03-28', 'Gerente', 5800.00),
('Aline Figueiredo', '2020-04-16', 'Atendente', 2800.00),
('Bruno Almeida', '2019-05-09', 'Técnico de Manutenção', 3300.00),
('Fernanda Castro', '2018-06-07', 'Atendente', 2900.00),
('Rafael Lima', '2024-01-21', 'Atendente', 2250.00),
('Luiza Rocha', '2021-08-19', 'Auxiliar de Limpeza', 2100.00);

INSERT INTO Diretor (nome, nacionalidade, data_nasci) VALUES 
('Steven Spielberg', 'Americana', '1946-12-18'),
('Martin Scorsese', 'Americana', '1942-11-17'),
('Quentin Tarantino', 'Americana', '1963-03-27'),
('Christopher Nolan', 'Britânica', '1970-07-30'),
('Alfred Hitchcock', 'Britânica', '1899-08-13'),
('Stanley Kubrick', 'Americana', '1928-07-26'),
('Francis Ford Coppola', 'Americana', '1939-04-07'),
('Ridley Scott', 'Britânica', '1937-11-30'),
('James Cameron', 'Canadense', '1954-08-16'),
('Peter Jackson', 'Neozelandesa', '1961-10-31'),
('George Lucas', 'Americana', '1944-05-14'),
('Clint Eastwood', 'Americana', '1930-05-31'),
('Woody Allen', 'Americana', '1935-12-01'),
('Guillermo del Toro', 'Mexicana', '1964-10-09'),
('Tim Burton', 'Americana', '1958-08-25'),
('David Fincher', 'Americana', '1962-08-28'),
('Sofia Coppola', 'Americana', '1971-05-14'),
('Ang Lee', 'Taiwanesa', '1954-10-23'),
('Alejandro González Iñárritu', 'Mexicana', '1963-08-15'),
('Wes Anderson', 'Americana', '1969-05-01');

INSERT INTO Filme (titulo, ano_lancamento, duracao, preco, id_diretor) VALUES 
('Jurassic Park', 1993, '02:07:00', 19.99, 1),
('E.T. - O Extraterrestre', 1982, '01:55:00', 14.99, 1),
('Goodfellas', 1990, '02:25:00', 17.99, 2),
('Taxi Driver', 1976, '01:54:00', 16.99, 2),
('Pulp Fiction: Tempo de Violência', 1994, '02:34:00', 18.99, 3),
('A Origem', 2010, '02:28:00', 19.99, 4),
('Interestelar', 2014, '02:49:00', 21.99, 4),
('Psicose', 1960, '01:49:00', 12.99, 5),
('O Iluminado', 1980, '02:26:00', 17.99, 6),
('Laranja Mecânica', 1971, '02:16:00', 15.99, 6),
('O Poderoso Chefão', 1972, '02:55:00', 19.99, 7),
('Alien - O Oitavo Passageiro', 1979, '01:57:00', 14.99, 8),
('Avatar', 2009, '02:42:00', 20.99, 9),
('O Senhor dos Anéis: A Sociedade do Anel', 2001, '02:58:00', 19.99, 10),
('Star Wars: Uma Nova Esperança', 1977, '02:01:00', 16.99, 11),
('Os Imperdoáveis', 1992, '02:10:00', 17.99, 12),
('Noivo Neurótico, Noiva Nervosa', 1977, '01:33:00', 13.99, 13),
('O Labirinto do Fauno', 2006, '01:58:00', 18.99, 14),
('Edward Mãos de Tesoura', 1990, '01:45:00', 15.99, 15),
('Clube da Luta', 1999, '02:19:00', 18.99, 16),
('Encontros e Desencontros', 2003, '01:42:00', 14.99, 17),
('As Aventuras de Pi', 2012, '02:07:00', 16.99, 18),
('Birdman', 2014, '01:59:00', 17.99, 19),
('O Grande Hotel Budapeste', 2014, '01:39:00', 15.99, 20);

INSERT INTO Genero (tipo_genero, id_filme) VALUES 
('Aventura', 1),
('Ficção Científica', 1),
('Ficção Científica', 2),
('Crime', 2),
('Crime', 3),
('Drama', 3),
('Crime', 4),
('Ficção Científica', 4),
('Suspense', 5),
('Terror', 6),
('Ficção Científica', 6),
('Drama', 7),
('Crime', 8),
('Ficção Científica', 8),
('Aventura', 9),
('Fantasia', 10),
('Ficção Científica', 11),
('Drama', 12),
('Comédia', 13),
('Fantasia', 14),
('Drama', 15),
('Ação', 16),
('Drama', 17),
('Aventura', 18),
('Comédia', 19),
('Comédia', 20),
('Ação', 21),
('Aventura', 22),
('Drama', 23),
('Comédia', 24);

INSERT INTO Funcionario_Filme (id_funcionario, id_filme) VALUES
(1, 6),
(2, 1),
(3, 2),
(4, 2),
(4, 15),
(5, 17),
(6, 24),
(7, 4),
(8, 4),
(9, 5),
(19, 7),
(11, 6),
(12, 6),
(12,14),
(12, 22),
(14, 7),
(13, 11),
(15, 8),
(16, 18),
(18, 9),
(18, 19),
(19, 10);
INSERT INTO Emprestimo (forma_pagamento, data_emprestimo, id_cliente, id_funcionario, id_filme) VALUES
('Pix', '2024-05-20', 1, 17, 13),
('Dinheiro', '2024-05-20', 2, 10, 6),
('Cartão de Crédito', '2024-05-20', 6, 6, 18),
('Pix', '2024-05-21', 15, 12, 2),
('Pix', '2024-05-21', 2, 8, 21),
('Cartão de Débito', '2024-05-22', 18, 13, 11),
('Pix', '2024-05-21', 12, 18, 1),
('Dinheiro', '2024-05-25', 19, 15, 7),
('Cartão de Crédito', '2024-05-26', 1, 3, 20),
('Pix', '2024-05-27', 14, 4, 10),
('Cartão de Débito', '2024-05-29', 9, 1, 24),
('Pix', '2024-05-30', 19, 16, 9),
('Dinheiro', '2024-05-30', 8, 20, 3),
('Cartão de Crédito', '2024-05-31', 4, 7, 16),
('Pix', '2024-06-1', 18, 5, 19),
('Pix', '2024-06-2', 6, 11, 4),
('Dinheiro', '2024-06-2', 19, 14, 22),
('Cartão de Crédito', '2024-06-03', 15, 2, 15),
('Pix', '2024-06-03', 11, 9, 23),
('Cartão de Débito', '2024-06-04', 16, 19, 5);
```
## Clientes
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Cliente.png">

## Email 
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Email.png">

## Telefone
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Telefone.png">

## Funcionários
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Funcion%C3%A1rio.png">

## Diretores 
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Diretor.png">

## Filmes
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Filme.png">

## Gêneros 
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/G%C3%AAnero.png">

## Funcionários_Filme
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Funcion%C3%A1rio_Filme.png">

## Empréstimos
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Emprestimo.png">

# 6 - CRUD
## CREATE/INSERT
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Insert.png">

## READ/SELECT
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Select.png">

## UPDATE

### Antes
<img  src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Antes%20update.png">

### Depois
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Depois%20update.png">

## DELETE

### Antes
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Antes%20delete.png">

### Depois
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Depois%20delete.png">

# 7 - Relatórios
## 1 - Listar os filmes que cada diretor dirigiu, ordenando pelo código de diretor.
```sql
SELECT Diretor.id_diretor, Diretor.nome, Filme.titulo FROM Diretor
JOIN Filme ON Diretor.id_diretor = Filme.id_diretor ORDER BY Diretor.id_diretor;
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%201.png">

## 2 - Localizar apenas os filmes do gênero drama.
```sql
SELECT Genero.tipo_genero, Filme.titulo FROM Genero JOIN Filme ON Genero.id_filme = Filme.id_filme
WHERE Genero.tipo_genero = 'Drama';
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%202.png">

## 3 - Mostrar o preço de cada filme ordenando do mais caro ao mais barato.
```sql
SELECT Filme.id_filme, Filme.titulo, Filme.preco FROM Filme
ORDER BY Filme.preco DESC
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%203.png">

## 4 - Listar os filmes alugados no mês de junho.
```sql
SELECT Filme.id_filme, Filme.titulo, Emprestimo.data_emprestimo FROM Filme
JOIN Emprestimo ON Filme.id_filme = Emprestimo.id_filme
WHERE Emprestimo.data_emprestimo BETWEEN '2024-06-01' AND '2024-06-30';
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%204.png">

## 5 - Achar os empréstimos realizados por cada cliente, ordenando pelo código do cliente.
```sql
SELECT Cliente.id_cliente, Cliente.nome, Emprestimo.forma_pagamento, Emprestimo.data_emprestimo, Emprestimo.data_devolucao FROM Cliente
JOIN Emprestimo ON Cliente.id_cliente = Emprestimo.id_cliente
ORDER BY Cliente.id_cliente;
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%205.png">

## 6 - Mostrar somente os filmes com o valor menor de 15 reais.
```sql
SELECT Filme.id_filme, Filme.titulo, Filme.preco FROM Filme
WHERE Filme.preco<15;
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%206.png">

## 7 - Apresentar os diretores com filmes de vários gêneros diferentes.
```sql
SELECT Diretor.id_diretor, Diretor.nome, COUNT(DISTINCT Genero.tipo_genero) AS quantidade_generos FROM Diretor
JOIN Filme ON Diretor.id_diretor = Filme.id_diretor JOIN Genero ON Filme.id_filme = Genero.id_filme
GROUP BY Diretor.id_diretor, Diretor.nome HAVING COUNT(DISTINCT Genero.tipo_genero)>1;
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%207.png">

## 8 - Selecionar os funcionários e filmes que eles gerenciaram.
```sql
SELECT Funcionario.id_funcionario, Funcionario.nome, Filme.titulo FROM Funcionario
JOIN Funcionario_Filme ON Funcionario.id_funcionario = Funcionario_Filme.id_funcionario
JOIN Filme ON Funcionario_Filme.id_filme = Filme.id_filme;
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%208.png">

## 9 - Procurar os filmes que ainda não foram emprestados.
```sql
SELECT Filme.id_filme, Filme.titulo FROM Filme
LEFT JOIN Emprestimo ON Filme.id_filme = Emprestimo.id_filme
WHERE Emprestimo.id_emprestimo IS NULL
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%209.png">

## 10 - Listar os funcionários que foram contratados no ano de 2020.
```sql
SELECT Funcionario.id_funcionario, Funcionario.nome, Funcionario.data_contratacao FROM Funcionario
WHERE YEAR(Funcionario.data_contratacao) = 2020
```
<img src="https://github.com/Pedro357d/Avalia-o-final/blob/main/Demonstra%C3%A7%C3%A3o%2010.png">


