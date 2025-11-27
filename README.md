# Moonzs
Banco de Dados da Moonzs
-- Criação do banco e uso
CREATE DATABASE IF NOT EXISTS LojaMoonzs;
USE LojaMoonzs;

-- Tabela Cliente
CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(120) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    endereco VARCHAR(150)
);

-- Tabela Produto
CREATE TABLE Produto (
    id_produto INT PRIMARY KEY AUTO_INCREMENT,
    nome_produto VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    estoque INT NOT NULL
);

-- Tabela Pedido
CREATE TABLE Pedido (
    id_pedido INT PRIMARY KEY AUTO_INCREMENT,
    id_cliente INT NOT NULL,
    data_pedido DATE NOT NULL,
    valor_total DECIMAL(10,2),
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
);

-- Tabela ItemPedido (associação Pedido x Produto)
CREATE TABLE ItemPedido (
    id_item_pedido INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    id_produto INT NOT NULL,
    quantidade INT NOT NULL,
    subtotal DECIMAL(10,2),
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_produto) REFERENCES Produto(id_produto)
);
USE LojaMoonzs;

-- Clientes
INSERT INTO Cliente (nome, email, telefone, endereco) VALUES
('Ana Oliveira', 'ana@mail.com', '119999999', 'Rua A, 123'),
('João Silva', 'joao@mail.com', '119888888', 'Av. B, 456'),
('Mariana Costa', 'mariana@mail.com', '119777777', 'Rua C, 789');

-- Produtos
INSERT INTO Produto (nome_produto, preco, estoque) VALUES
('Camiseta Oversized', 79.90, 50),
('Calça Cargo', 129.90, 30),
('Jaqueta Jeans', 199.90, 20);

-- Pedido
INSERT INTO Pedido (id_cliente, data_pedido, valor_total) VALUES
(1, '2024-11-01', 399.80),
(2, '2024-11-05', 79.90);

-- Itens dos pedidos
INSERT INTO ItemPedido (id_pedido, id_produto, quantidade, subtotal) VALUES
(1, 3, 2, 399.80),
(2, 1, 1, 79.90);
USE LojaMoonzs;

-- 1. Consultar todos os clientes
SELECT * FROM Cliente;

-- 2. Listar produtos com estoque baixo
SELECT nome_produto, estoque
FROM Produto
WHERE estoque < 30
ORDER BY estoque ASC;

-- 3. Consulta com JOIN
SELECT p.id_pedido, c.nome AS cliente, pr.nome_produto, ip.quantidade
FROM Pedido p
JOIN Cliente c ON p.id_cliente = c.id_cliente
JOIN ItemPedido ip ON p.id_pedido = ip.id_pedido
JOIN Produto pr ON ip.id_produto = pr.id_produto
LIMIT 10;

-- 4. Contagem de pedidos por cliente
SELECT c.nome, COUNT(p.id_pedido) AS total_pedidos
FROM Cliente c
LEFT JOIN Pedido p ON p.id_cliente = c.id_cliente
GROUP BY c.nome;

-- 5. Produtos com maior valor
SELECT nome_produto, preco FROM Produto ORDER BY preco DESC LIMIT 3;
USE LojaMoonzs;

-- UPDATE
UPDATE Produto SET estoque = 45 WHERE id_produto = 1;
UPDATE Cliente SET telefone = '119666666' WHERE email = 'joao@mail.com';
UPDATE Pedido SET valor_total = 299.90 WHERE id_pedido = 1;

-- DELETE
DELETE FROM Produto WHERE estoque = 0;
DELETE FROM Cliente WHERE id_cliente = 3;
DELETE FROM ItemPedido WHERE quantidade = 0;
