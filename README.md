# Prova-BCD

# Desafio 2: Estoque da Loja

# Mer e Der:

![atividade](./loja_de_roupas.drawio.png)

## Dicionário de Dados

| Entidade | Atributo | Tipo | Tamanho| Descrição |
|-|-|-|-|-|
|Produto | id | Int | 11 | Identificador, PK, Auto incrementável |
|Produto| nome | varchar | 100 | Nome do produto|
|Produto |Descrição| varchar | 100| Informações sobre o produto|
|Produto | Preço | Decimal | 10,2 | Valor do produto|
|Produto | Marca| varchar | 20 | Criadora do produto |
|Produto | id_categoria | int | 30 | Identificador de categoria |
|Produto | id_fornecedor | int | 30 | identificador do fornecedor |
| Categoria | id | Inteiro | 11 | Identificador, PK, Auto incrementável |
| Categoria| nome | varchar | 100 | Nome da categoria|
| Categoria | descrição | varchar | 100 | Descrição da categoria |
|Fornecedor| id | int | 11 | identificador do fornecedor|
|Fornecedor| Razão_soical| varchar | 50 | Nome oficial jurídico de uma empresa |
|Fornecedor | Nome_fantasia | varchar | 50 | Nome comercial da empresa |
|Fornecedor | CNPJ | varchar | 20 | Cadastro Nacional da Pessoa Jurídica |
|Fornecedor| Telefone | varchar | 20 | Telefone da empresa|
|Fornecedor| email | varchar | 100 | Email da empresa|
|Fornecedor| endereço | varchar| 150 | endereço da empresa |
|Estoque | id_estoque| int| 11 | identificação de estoque |
|Estoque| id_produto| int| 50 |identificação do produto|
|Estoque| quantidade | int| 100 | Quantidade do estoque|
|Estoque| quantidade_minima|int| 20 | quantidade minima de produto|
|movimentaçaõ_de_estoque| id | int| 11 | Identificação da movimentação de estoque |
|movimentaçaõ_de_estoque | Id_produto| int| 50| Identificação do produto |
|movimentaçaõ_de_estoque| tipo| enum|  |Identificação do produto|
|movimentaçaõ_de_estoque| quantidade | int| | Quantidade do produtos que restam no estoque|
|movimentaçaõ_de_estoque| data|date| 20 | Data da entrada e saida de produtos|

## Dados em CSV:

- ![Categoria.csv](./Categoria.csv)
- ![fornecedor.csv](./fornecedor.csv)
- ![produto.csv](./produto.csv)
- ![estoque.csv](./estoque.csv)
- ![movimentação_de_estoque.csv](./movimentação_de_estoque.csv)

## DDL.SQUL

```
drop database if exists estoque_roupa;
create database estoque_roupa;

use estoque_roupa;

create table categoria(
    id int (10) primary key not null auto_increment,
    nome varchar (100) not null,
    descricao varchar (100) not null
);

create table fornecedor (
    id int (10) primary key not null auto_increment,
    razao_social varchar(50) not null,
    nome_fantasia varchar(50)not null,
    cnpj varchar(20) not null,
    telefone varchar(20) not null,
    email varchar(100) not null,
    endereco varchar(150) not null
);

create table produto(
    id int (10) primary key not null auto_increment,
    nome varchar (100) not null,
    descricao varchar (100) not null,
    preco decimal(10,2) not null,
    marca varchar (20) not null,
    id_categoria int not null,
    id_fornecedor int not null
);

create table estoque (
    id_estoque int primary key not null auto_increment,
    id_produto int not null,
    quantidade int not null,
    quantidade_minima int not null
);

create table movimentacao_de_estoque(
     id int (10) primary key not null auto_increment,
    id_produto int not null,
    tipo enum('entrada', 'saida') not null,
    quantidade int not null,
    data date not null
);

alter table produto add constraint fornece foreign key (id_categoria) references categoria(id);
alter table produto add constraint possui foreign key (id_fornecedor) references fornecedor(id);
alter table estoque add constraint estoque foreign key (id_produto) references produto(id);
````
## DML.SQL

````
use estoque_roupa;

insert into
categoria
values
(null,'Camisetas', 'Camisetas masculinas e femininas'),
(null,'Calças', 'Calças jeans, sociais e casuais'),
(null,'Vestidos', 'Vestidos femininos diversos'),
(null,'Tênis', 'Tênis masculino e feminino');
select * from categoria;

insert into
fornecedor
values
(null,'Moda Brasil LTDA', 'Moda Brasil', '12.345.678/0001-90', '(11) 99999-1111', 'contato@modabrasil.com', 'Rua das Flores, 100 - São Paulo - SP'),
(null,'Estilo Fashion LTDA', 'Estilo Fashion', '23.456.789/0001-81', '(19) 98888-7777', 'contato@estilofashion.com', 'Rua Central, 250 - Campinas - SP'),
(null,'Roupas & Cia LTDA', 'Roupas & Cia', '34.567.890/0001-72', '(21) 97777-3322', 'contato@roupasecia.com', 'Av. Brasil, 500 - Rio de Janeiro - RJ');
select * from fornecedor;

insert into
produto
values
(1,'Camiseta Básica Branca','Camiseta de algodão branca',39.90, 'Nike', 1, 1),
(2,'Camiseta Preta','Camiseta básica preta',44.90, 'Adidas', 1, 2),
(3,'Calça Jeans Masculina', 'Calça jeans azul masculina',129.90, 'Levis', 2, 1),
(4,'Calça Jeans Feminina','Calça jeans feminina skinny',119.90, 'Colcci', 2, 2),
(5,'Vestido Floral','Vestido feminino estampado',159.90, 'Zara', 3, 3),
(6,'Tênis Esportivo','Tênis para atividades físicas',299.90, 'Nike', 4, 1),
(7,'Tênis Casual','Tênis casual para uso diário', 199.90, 'Adidas', 4, 2);
select * from produto;

insert into
estoque
values
(null,1, 50, 10),
(null,2, 35, 10),
(null,3, 20, 5),
(null,4, 25, 5),
(null,5, 15, 5),
(null,6, 18, 5),
(null,7, 12, 3);
select * from estoque;

insert into
movimentacao_de_estoque
values
(1, 'entrada', 50, '2026-09-01'),
(2, 'entrada', 35, '2026-09-02'),
(3, 'entrada', 20, '2026-09-03'),
(4, 'entrada', 25, '2026-09-03'),
(5, 'entrada', 15, '2026-09-04'),
(6, 'entrada', 18, '2026-09-04'),
(7, 'entrada', 12, '2026-09-05'),

(1, 'saida', 5, '2026-09-10'),
(2, 'saida', 3, '2026-09-11'),
(3, 'saida', 2, '2026-09-12'),
(5, 'saida', 4, '2026-09-13'),
(7, 'saida', 3, '2026-09-14');
select * from movimentacao_de_estoque;
````
