- criação do banco de dados Ecommerce

create database ecommerce;
use ecommerce;
-- tabela cliente

create table client(
IdCliente int auto_increment primary key,
Fname varchar(10),
Minit char(3),
Lname varchar(20),
CPF char(11),
Address varchar(45),
constraint unique_cpf_client unique (CPF)
);

-- tabela produto
-- size dimensão do produto
create table produto(
IdProduto int auto_increment primary key,
Pname varchar(10),
Classification_kids bool default false,
category enum('eletronico','vestimenta','brimquedos','alimentos', 'moveis') not null,
avaliação float default 0,
size varchar(10)

);

-- tabela pagamento

-- implentar tabela e crar conexoes
insert into pagamento values (1,10,'pix'),
(2,12,'cartão de debito'),
(3,14,'boleto');
alter table pagamento drop validadecartao;
create table pagamento(
idCliente int,
id_pagamento int,
tipo_de_pagamento enum('Boleto', 'Cartao de Credito', 'Cartao de Debito', 'pix'),
primary key(idCliente, id_pagamento),
constraint fk_idCliente foreign key (idCliente) references client(idCliente),
constraint fk_id_pagamento foreign key (id_pagamento) references pedido(idPedido)

);
desc pagamento;
select*from pagamento;


-- tabela pedido

insert into pedido values ( 10,1,'cancelado','fones',50.0),
(12,2,'confirmado','tv',20),
(14,3,'em processamento','vestido',50.0);
select* from pedido;
create table pedido (
idPedido int auto_increment primary key,
idPedidoCliente int,
pedidoStatus enum('cancelado','confirmado', 'em processamento') default ('em processamento'),
descricaoPedido varchar (255),
frete float default 10,
constraint fk_pedido_cliente foreign key (idPedidoCliente) references client(idCliente),
constraint fk_pedido foreign key (idPedido) references produto(idProduto)

);

-- tabela estoque
alter table estoque auto_increment =1;
insert into estoque values(1,'sao paulo',200),
(2,'rio de janeiro',300),
(3,'sao luis',100);
create table estoque(
idProdEstoque int auto_increment primary key,
local_estoque varchar (45) not null,
quantidade int default 0
);

-- table fornecedor

create table fornecedor(
id_Fornecedor int auto_increment primary key,
razao_social varchar(255) not null,
CNPJ char(15) not null,
contato char(11) default 0,
constraint unique_cnpj UNIQUE (cnpj)
);
 -- tabela vendedor
 
 insert INTO VENDEDOR values (2,'sao paulo', 'teste1',default,01234567845,'teste3', 90909090909);
 select*from vendedor;
 create table vendedor(
 id_Vendedor int auto_increment primary key,
 Local_vendedor varchar(45),
razao_social varchar(255) not null,
CNPJ char(15),
CPF char(11),
nome_Fantasia varchar (255),
contato char(11) default 0,
constraint unique_cnpj UNIQUE (cnpj),
constraint unique_cpf UNIQUE (CPF)
 
 );
 insert into produtoVendedor values (1, 10, 2),
 (2, 12, 5);
 create table produtoVendedor(
 idVendedor int ,
 idProduto int,
 quantidadeproduto int default 1,
 primary key (idVendedor, idProduto),
 constraint fk_id_vendedor foreign key (idVendedor) references vendedor(id_Vendedor),
 constraint fk_id_produto_vendedor foreign key (idProduto) references produto(idProduto)
 );
 desc produtoVendedor;
 select*from produtoVendedor;
 
 -- tabela pagamento pedido
 
 insert into disponibila_produto values (1,10),
 (2,14);
 
 create table disponibila_produto(
 id_Fornecedor int,
 idProduto int ,
 constraint fk_idfornecedor foreign key (id_Fornecedor) references fornecedor(id_Fornecedor),
  constraint fk_idproduto foreign key (idProduto) references produto(idProduto)
 
 );
 
 insert into produto_em_estoque values (1, 10),
 (2, 14);
 
 create table produto_em_estoque(
 idProdEstoque int primary key,
 idProduto int,
  constraint fk_idprodestoque foreign key (idProdEstoque) references estoque(idProdEstoque),
 constraint fk_id_produto foreign key (idProduto) references produto(idProduto)
 );
 insert into envioProduto values (10,1,'sequoia',123,'saindo_entrega'),
 (12,2,'amazon',124,'entregue');
 create table envioProduto(
 idPedido int,
 idEntrega int auto_increment primary key,
 transportadora varchar(20),
 codigo_rastreio varchar (20),
 status_entrega enum('saindo_entrega','em_deslocamento','entregue'),
 constraint fk_idpedido foreign key (idPedido) references pedido(idPedido)
 
 );
 show tables;
 use ecommerce;
 desc FORNECEDOR;
 desc referential_constraints;
select * from referential_constraints where constraint_schema = 'ecommerce';
alter table client auto_increment =1;
insert into client (FNAME, MINIT, LNAME, CPF, ADDRESS) 
values('MARIA', 'A','SANTOS', 00123725373, 'RUA DOS ANZOIS, SAO LUIS'),
('JOSE', 'B','SOUSAS', 25123725373, 'RUA DOS ANJOSS, SAO PAULO'),
('ANDRE', 'P','OLIVEIRAS', 40023725373, 'RUA 5, LUIS'),
('MAGNO', 'H','MARTINS', 50123725373, 'RUA 8, BELEM'),
('MAURO', 'P','CARVALHO', 90123725373, 'RUA JOAO, PARANA');
SELECT * FROM client;
DESC VENDEDOR;
alter table VENDEDOR auto_increment =1;




insert into FORNECEDOR 
values('1', 'LIMAJR', 098760012372537, '86999912623'),
('2', 'JR ASSOCIADOS', 123760012372537, '99999918923'),
('3', 'SANTOSLTDA', 345760012372537, '51999912698');

insert into PRODUTO 
values('10', 'FONES', 1, 'ELETRONICO', '1', '2'),
('12', 'CELULAR', 1, 'ELETRONICO', '1', '3'),
('14', 'TV', 1, 'ELETRONICO', '1', '6');

SELECT * FROM Produto;
select count(*) from client;
select fname, lname, Pedidostatus from client c, Pedido p where c.idCliente = idPedidoCliente;

-- Quantidade de produtos vendidos por vendedor
select count(*) as Venda_vendedor from vendedor, produtovendedor
where idProduto=10;

-- Ordenação por category
select idEntrega, idProduto, count(*) as Quantidade_eletronico
from envioProduto, Produto
where idEntrega = 2
order by category;

-- having
select idEntrega, idProduto, count(*) as Quantidade_eletronico
from envioProduto, Produto
where idEntrega = 2
having count(*)>2
order by category;

select*from produto;


select*from produtovendedor;


select*from pedido;
-- where para encontar a quantidade do estoque
    select distinct * from estoque
    where (local_estoque, quantidade) IN (select local_estoque, quantidade from  estoque where idProdEstoque=2);


select * from client inner join pedido  ;

select * from client cross join vendedor;

select * from  client  inner join pedido p
inner join envioProduto e on e.status_entrega='entregue'
group by status_entrega;

select * from fornecedor join client ;
select * from client;
