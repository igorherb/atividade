# Avaliação: Bancos de Dados I
## Grupos

Matheus Vosni e Igor Nicácio.

## Modelagem de bancos de dados relacionais

Você foi contratado por uma grande gravadora para trabalhar como projetista de banco de dados no projeto de desenvolvimento de um site para a comercialização e download de músicas em formato MP3.

Para proporcionar uma interface amigável e poderosa, o site deverá permitir a pesquisa de músicas a partir do autor (que pode ser um artista solo ou uma banda), do álbum, do número da trilha no álbum, do título da música ou do estilo musical (Rock, Blues, Instrumental, etc.) do álbum.

Lembre-se que muitas músicas são disponibilizadas em mais de um álbum (no álbum de lançamento e novamente em coletâneas, por exemplo), mas é importante saber qual a trilha em que elas foram gravadas em cada álbum.

A partir da descrição acima:

1) Desenvolva o Esquema Conceitual de Dados utilizando o Modelo Conceitual Entidade-Relacionamento para os requisitos definidos.

resposta no repositório

2) Considerando o Modelo Conceitual criado anteriormente, desenvolva o Modelo Lógico para os requisitos definidos. Utilize o site [dbdiagram](https://dbdiagram.io/) para fazer a modelagem lógica.

resposta no repositório

3) A partir do modelo lógico, crie o modelo físico do banco de dados, e manipule o BD:

    - Inserindo no mínimo 5 registros para cada uma das tabelas:

INSERT INTO Autores (nome) VALUES
('Luiz Gonzaga'),
('Elba Ramalho'),
('Zé Ramalho'),
('Chico Science & Nação Zumbi'),
('Fagner');

INSERT INTO Estilos (estilo_nome) VALUES
('Baião'),
('Forró'),
('Axé'),
('Frevo'),
('Xote');

INSERT INTO Albuns (id_autor, nome_album, ano_lancamento) VALUES
(1, 'Asa Branca', 1947),  -- Luiz Gonzaga
(2, 'Sete Cores', 1982),  -- Elba Ramalho
(3, 'Cavalo de Pau', 1982),  -- Zé Ramalho
(4, 'Da Lama ao Caos', 1994),  -- Chico Science & Nação Zumbi
(5, 'Cavalo de Pau', 1985);  -- Fagner

INSERT INTO Musicas (id_album, numero_trilha, titulo, id_estilo) VALUES
-- Luiz Gonzaga - Asa Branca (1947)
(1, 1, 'Asa Branca', 1),  -- Baião
(1, 2, 'Qui Nem Jiló', 1),  -- Baião

-- Elba Ramalho - Sete Cores (1982)
(2, 1, 'Anunciação', 3),  -- Axé
(2, 2, 'Banho de Cheiro', 3),  -- Axé

-- Zé Ramalho - Cavalo de Pau (1982)
(3, 1, 'Admirável Gado Novo', 1),  -- Baião
(3, 2, 'Chão de Giz', 1),  -- Baião

-- Chico Science & Nação Zumbi - Da Lama ao Caos (1994)
(4, 1, 'Maracatu Atômico', 4),  -- Frevo
(4, 2, 'Sangue do Norte', 4),  -- Frevo

-- Fagner - Cavalo de Pau (1985)
(5, 1, 'Cavalo de Pau', 2),  -- Xote
(5, 2, 'Morro do Tempo', 2);  -- Xote

    - Atualize 3 dos registros, modificando seus valores

UPDATE Autores
SET nome = 'Rei do Baião'
WHERE nome = 'Luiz Gonzaga';

UPDATE Albuns
SET ano_lancamento = 1950
WHERE nome_album = 'Asa Branca' AND id_autor = (SELECT id_autor FROM Autores WHERE nome = 'Rei do Baião');

UPDATE Musicas
SET titulo = 'Anunciação (Remasterizada)'
WHERE titulo = 'Anunciação' AND id_album = (SELECT id_album FROM Albuns WHERE nome_album = 'Sete Cores' AND id_autor = (SELECT id_autor FROM Autores WHERE nome = 'Elba Ramalho'));

    - Remova 2 registros

DELETE FROM Autores
WHERE nome = 'Rei do Baião';

DELETE FROM Musicas
WHERE titulo = 'Anunciação (Remasterizada)' 
AND id_album = (SELECT id_album FROM Albuns WHERE nome_album = 'Sete Cores' AND id_autor = (SELECT id_autor FROM Autores WHERE nome = 'Elba Ramalho'));

## BANCO FÍSICO
CREATE DATABASE MusicasDB;

USE MusicasDB;

CREATE TABLE Autores (
    id_autor INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL
);

CREATE TABLE Estilos (
    id_estilo INT AUTO_INCREMENT PRIMARY KEY,
    estilo_nome VARCHAR(100) NOT NULL
);

CREATE TABLE Albuns (
    id_album INT AUTO_INCREMENT PRIMARY KEY,
    id_autor INT,
    nome_album VARCHAR(255) NOT NULL,
    ano_lancamento INT,
    FOREIGN KEY (id_autor) REFERENCES Autores(id_autor)
);

CREATE TABLE Musicas (
    id_musica INT AUTO_INCREMENT PRIMARY KEY,
    id_album INT,
    numero_trilha INT NOT NULL,
    titulo VARCHAR(255) NOT NULL,
    id_estilo INT,
    FOREIGN KEY (id_album) REFERENCES Albuns(id_album),
    FOREIGN KEY (id_estilo) REFERENCES Estilos(id_estilo)
);

## COMENTÁRIOS GERAIS: professor, confesso que não estou satisfeito com o meu exercício, senti grande dificuldades na criação dos modelos conceituais, mas por fim espero que pelo menos tenha suprido as necessidades da avaliação ~ Igor.

## Consultas SQL simples e complexas em um banco de dados postgres

O banco de dados disponível no *script* **northwind.sql** foi desenvolvido para representar o contexto de um e-commerce de grande porte, com operações estruturadas para gerenciar diferentes aspectos de sua operação. Ele é composto por uma série de tabelas que armazenam informações sobre produtos, clientes, fornecedores, funcionários e pedidos, entre outras, para fornecer uma visão abrangente de como a empresa lida com o fluxo de mercadorias, atendimento ao cliente e administração interna.

- As tabelas **products** e **categories** contêm informações detalhadas sobre os produtos oferecidos e suas respectivas categorias, incluindo informações de preço, estoque e fornecedores.

- A tabela **customers** armazena dados dos clientes, permitindo associar informações como nome, contato e endereço a cada pedido realizado.

- **Orders** e **order_details** registram cada pedido e os itens que foram comprados, com detalhes de quantidade, preço unitário e descontos aplicados.

- As tabelas **suppliers** e **shippers** são utilizadas para gerenciar o relacionamento com fornecedores e transportadoras, garantindo que cada pedido possa ser processado e enviado adequadamente.

- **Employees** e **employee_territories** contêm dados dos funcionários, inclusive suas regiões de atendimento, o que possibilita analisar o desempenho e alcance da equipe de vendas.

- Tabelas auxiliares como **customer_demographics** e **customer_customer_demo** armazenam detalhes adicionais sobre os clientes, como tipos de clientes e perfis demográficos, enquanto **prospects** registra potenciais clientes que podem vir a se tornar compradores no futuro.

&nbsp;

### Criando e populando o banco de dados

Execute o script **northwind.sql** , disponível na **Pasta Geral** dos **Arquivos**.

&nbsp;

### Consultas

Usando o subconjunto da "structured query language" chamado de DQL, produza consultas de modo a atender cada uma das seguintes solicitações. Os valores financeiros devem ser convertidos para moeda corrente, com R$ e 2 casas após a vírgula.

4) Qual é a receita total gerada por cada categoria de produto? 

SELECT
    c.category_name AS Categoria, 
    CONCAT('R$', FORMAT(SUM(od.unit_price * od.quantity * (1 - od.discount)), 2)) AS Receita_Total
FROM 
    order_details od, products p, categories c
WHERE 
    od.product_id = p.product_id
    AND p.category_id = c.category_id
GROUP BY 
    c.category_name;

5) Qual cliente fez o maior número de pedidos?

SELECT 
    c.customer_id AS Cliente, 
    COUNT(o.order_id) AS Numero_De_Pedidos
FROM 
    orders o, customers c
WHERE 
    o.customer_id = c.customer_id
GROUP BY 
    c.customer_id
ORDER BY 
    Numero_De_Pedidos DESC
LIMIT 1;

6) Qual é o valor médio dos pedidos para cada fornecedor?

SELECT 
    s.supplier_name AS Fornecedor, 
    CONCAT('R$', FORMAT(AVG(od.unit_price * od.quantity * (1 - od.discount)), 2)) AS Valor_Medio_Pedido
FROM 
    order_details od, products p, suppliers s
WHERE 
    od.product_id = p.product_id
    AND p.supplier_id = s.supplier_id
GROUP BY 
    s.supplier_name;

7) Qual produto teve a maior quantidade de unidades vendidas?

SELECT 
    p.product_name AS Produto, 
    SUM(od.quantity) AS Quantidade_Total_Vendida
FROM 
    order_details od, products p
WHERE 
    od.product_id = p.product_id
GROUP BY 
    p.product_name
ORDER BY 
    Quantidade_Total_Vendida DESC
LIMIT 1;

8) Qual a quantidade total de produtos vendidos por cada fornecedor no ano de 2023?

SELECT 
    s.supplier_name AS Fornecedor, 
    SUM(od.quantity) AS Quantidade_Total_Vendida
FROM 
    order_details od, products p, suppliers s, orders o
WHERE 
    od.product_id = p.product_id
    AND p.supplier_id = s.supplier_id
    AND od.order_id = o.order_id
    AND YEAR(o.order_date) = 2023
GROUP BY 
    s.supplier_name;

9) Qual é o preço médio dos produtos em cada categoria?

SELECT 
    c.category_name AS Categoria, 
    CONCAT('R$', FORMAT(AVG(p.unit_price), 2)) AS Preco_Medio
FROM 
    products p, categories c
WHERE 
    p.category_id = c.category_id
GROUP BY 
    c.category_name;

10) Quais clientes fizeram pedidos nos últimos 6 meses?

SELECT DISTINCT 
    c.customer_id AS Cliente
FROM 
    orders o, customers c
WHERE 
    o.customer_id = c.customer_id
    AND o.order_date >= CURDATE() - INTERVAL 6 MONTH;

11) Qual é a receita total gerada por cada fornecedor no mês de junho?

SELECT 
    s.supplier_name AS Fornecedor, 
    CONCAT('R$', FORMAT(SUM(od.unit_price * od.quantity * (1 - od.discount)), 2)) AS Receita_Total
FROM 
    order_details od, products p, suppliers s, orders o
WHERE 
    od.product_id = p.product_id
    AND p.supplier_id = s.supplier_id
    AND od.order_id = o.order_id
    AND MONTH(o.order_date) = 6
GROUP BY 
    s.supplier_name;

12) Qual é o número de pedidos feitos por clientes de cada país?

SELECT 
    c.country AS Pais, 
    COUNT(o.order_id) AS Numero_De_Pedidos
FROM 
    orders o, customers c
WHERE 
    o.customer_id = c.customer_id
GROUP BY 
    c.country;

13) Quais produtos foram pedidos por mais de 30% dos clientes?  Ordene pela porcentagem, do maior ao menor.

SELECT 
    p.product_name AS Produto, 
    CONCAT(ROUND((COUNT(DISTINCT o.customer_id) / (SELECT COUNT(*) FROM customers) * 100), 2), '%') AS Percentual_De_Clientes
FROM 
    order_details od, products p, orders o
WHERE 
    od.product_id = p.product_id
    AND od.order_id = o.order_id
GROUP BY 
    p.product_name
HAVING 
    Percentual_De_Clientes > 30
ORDER BY 
    Percentual_De_Clientes DESC;

14) Quais produtos foram comprados pelo cliente de ID ANTON?

SELECT 
    p.product_name AS Produto
FROM 
    orders o, order_details od, products p
WHERE 
    o.order_id = od.order_id
    AND od.product_id = p.product_id
    AND o.customer_id = 'ANTON';

15) Qual foi o valor total gasto por cada cliente no ano de 2022? Ordene pelo total gasto, do maior para o menor.

SELECT 
    c.customer_id AS Cliente, 
    CONCAT('R$', FORMAT(SUM(od.unit_price * od.quantity * (1 - od.discount)), 2)) AS Total_Gasto
FROM 
    orders o, order_details od, customers c
WHERE 
    o.order_id = od.order_id
    AND o.customer_id = c.customer_id
    AND YEAR(o.order_date) = 2022
GROUP BY 
    c.customer_id
ORDER BY 
    Total_Gasto DESC;

16) Quais são os 5 produtos mais vendidos da categoria Beverages?

SELECT 
    p.product_name AS Produto, 
    SUM(od.quantity) AS Quantidade_Vendida
FROM 
    order_details od, products p, categories c
WHERE 
    od.product_id = p.product_id
    AND p.category_id = c.category_id
    AND c.category_name = 'Beverages'
GROUP BY 
    p.product_name
ORDER BY 
    Quantidade_Vendida DESC
LIMIT 5;

17) Quais são os fornecedores que nunca tiveram um produto vendido?

SELECT 
    s.supplier_name AS Fornecedor
FROM 
    suppliers s
LEFT JOIN 
    products p ON s.supplier_id = p.supplier_id
LEFT JOIN 
    order_details od ON p.product_id = od.product_id
WHERE 
    od.order_id IS NULL;

18) Quais são os 3 meses com maior receita total?

SELECT 
    DATE_FORMAT(o.order_date, '%Y-%m') AS Mes, 
    CONCAT('R$', FORMAT(SUM(od.unit_price * od.quantity * (1 - od.discount)), 2)) AS Receita_Total
FROM 
    order_details od, orders o
WHERE 
    od.order_id = o.order_id
GROUP BY 
    Mes
ORDER BY 
    Receita_Total DESC
LIMIT 3;

19) Qual é o produto mais caro de cada categoria?

SELECT 
    c.category_name AS Categoria, 
    p.product_name AS Produto, 
    CONCAT('R$', FORMAT(MAX(p.unit_price), 2)) AS Preco
FROM 
    products p, categories c
WHERE 
    p.category_id = c.category_id
GROUP BY 
    c.category_name;

20) Qual é a porcentagem de cada categoria em relação à receita total?

SELECT 
    c.category_name AS Categoria, 
    CONCAT(ROUND((SUM(od.unit_price * od.quantity * (1 - od.discount)) / 
                (SELECT SUM(od2.unit_price * od2.quantity * (1 - od2.discount)) FROM order_details od2) * 100), 2), '%') AS Percentual_Receita
FROM 
    order_details od, products p, categories c
WHERE 
    od.product_id = p.product_id
    AND p.category_id = c.category_id
GROUP BY 
    c.category_name;


## COMENTÁRIOS GERAIS: foi utilizado o "JOIN" pra juntar os pedidos dos clientes e usei também o "CONCAT('R$', FORMAT(..., 2))", pra deixar o retorno na formatação que pediu no exercício. "LEFT JOIN" pra registros que não existe e "LIMIT" para as quantidades que foram necessárias quando aplicável. ainda sinto muita dificuldade na agregação dos dados (ahahah), mas tô estudando pra ver se consigo me sair melhor e até resumir os códigos para consultas (espero que esses comentários não impacte na entrega) ~ Igor.