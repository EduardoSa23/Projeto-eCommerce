# Projeto-eCommerce

![eCommerce](https://user-images.githubusercontent.com/111080797/192079225-50c7f3d1-71fa-4026-8440-41d3a957cef1.png)

-- Criação do banco de dados para e-Commerce
-- drop database ecommerce;
create database ecommerce;
use ecommerce;

-- criar tabela cliente

create table clients (
	id_Client int auto_increment primary key,
    Fname Varchar(20),
    Minit char(3),
    Lname varchar(20),
    CPF char(11) not null,
    Address varchar(45),
    constraint unique_cpf_client unique (CPF)
);
	
alter table clients auto_increment=1;

desc clients;
-- criar tabela prodto
-- size eqivale a dimensão do produto

create table product (
	id_Product int auto_increment primary key,
    Category enum('Eletronico','Vestimento','Brinquedos','Alimentos') not null,
    Pname varchar(45) not null,
    Codigo varchar(20) not null,
    classification_kids bool default false,
    Avaliation float default 0,
    size varchar(10)
);
alter table product auto_increment=1;

desc product;
drop table entrega;
create table entrega(
	idEntrega int primary key,
    StatusEntrega ENUM('Em separação', 'Em transporte', 'Saiu para a entrega', 'Entregue') default 'Em separação',
    CodRastreio varchar(15) not null
);
drop table entregaOrder;
create table entregaOrder(
	idEnEntrega int,
    IdEnOrder int,
    primary key (idEnEntrega, idEnOrder),
    constraint idEn_Entrega foreign key (idEnEntrega) references entrega(IdEntrega),
    constraint idEn_Order foreign key (idEnOrder) references orders(IdOrder)
);

-- criar tabela pedido

create table orders (
	idOrder int auto_increment primary key,
    idOrderClient int,
    OrderStatus enum('Cancelado','Confirmado','Em processamento') default 'Em processamento', 
    OrderDescription varchar(255),
    sendValue float default 10,
    paymentCash bool default false,
    constraint fk_orders_client foreign key (idOrderClient) references clients(id_Client)
			on update cascade
);

create table Payments(
idPayment int auto_increment primary key,
idPayOrder int,
idPayProduct int,
typePayment enum('Cash','CreditCard','Ticket') default 'CreditCard',
totalPrice decimal(5,2) not null,
paymentStatus enum('Authorized','Not Authorized','Processing','Chargeback') default 'Processing',
constraint fk_pay_order foreign key(idPayOrder) references Orders(idOrder),
constraint fk_pay_product foreign key(idPayProduct) references Product(id_Product)
);

alter table payments auto_increment=1;

desc orders;
-- criar tabela estoque


create table storages (
	idStorage int auto_increment primary key,
    StorageLocation varchar(20),
    Quantity int default 0
 );

-- criar tabela fornecedor

create table suplaier(
	idSuplaier int auto_increment primary key,
    Cnpj char(14) not null,
    SocialName varchar(255) not null,
    contact varchar(11) not null,
    NameFantasySuplaier varchar(45) default 'Não Possui',
    constraint unique_suplaier unique (Cnpj),
    constraint unique_SocialName_suplaier unique (SocialName)
);
    
desc suplaier;
-- criar tabela vendedor

create table seller(
	idSeller int auto_increment primary key,
    Cnpj char(14),
    CPF char(11),
    SocialName varchar(255),
    contact varchar(11) not null,
    Address varchar(60) not null,
    NameFantasySeller varchar(45) default 'Não Possui',
    constraint unique_cnpj_seller unique (Cnpj),
    constraint unique_cpf_seller unique (CPF),
    constraint unique_SocialName_seller unique (SocialName)
);

desc seller;
-- criar tabela produto vendedor

create table productSeller(
	idPsSeller int,
    idPsProduct int,
    ProductQuantity int default 1,
    primary key (idPsProduct, idPsSeller),
    constraint fk_product_Seller foreign key (idPsSeller) references seller (idSeller),
    constraint fk_product_product foreign key (idPsProduct) references product(id_Product)
);

desc productSeller;

-- criar tabela produto predido

create table productOrder(
	idPoOrder int,
    idPoProduct int,
    PoQuantity int default 1,
    poStatus enum('Disponível','Sem estoque') default 'Disponível',
    primary key (idPoProduct, idPoOrder),
    constraint fk_product_order foreign key (idPoOrder) references orders(IdOrder),
    constraint fk_product_order_product foreign key (idPoProduct) references product(id_Product)
);

-- Criar tabela prodto em estoque

create table storagelocation(
	idSlStorage int,
    idSlProduct int,
    location varchar (255),
    primary key (idSlStorage, idSlProduct),
    constraint fk_storage_product foreign key (idSlProduct) references product(id_Product),
    constraint fk_product_storage foreign key (idSlStorage) references storages(idStorage)
);

-- criar tabela produto fornecedor

create table productSuplaier (
	idPsSuplaier int,
    idPsProduct int,
    Quantify int not null,
    primary key (idPsSuplaier, idPsProduct),
    constraint fk_product_Suplaier foreign key (idPsSuplaier) references suplaier(idSuplaier),
    constraint fk_product_ProdSuplaier foreign key (idPsProduct) references product(id_Product)
);


show tables;

show databases;
use information_schema;
show tables;
desc referential_constraints;
select * from referential_constraints where constraint_schema = 'ecommerce';

_______________________________________________________________________________________________________________________________________________________________

use ecommerce;

show tables;
desc clients;

-- id_client, Fname, Minit, Lname, CPF, Address

insert into Clients (Fname, Minit, Lname, CPF, Address)
	values('Eduardo','A','Sá','44587603969','rua sobral 35, Calmon Viana, Poá, SP'),
		  ('Carlos','E','Oliveira','57860984732','rua solé 856, Centro, Suzano, SP'),
          ('Denis','S','Silva','84673625419','rua cadiz 635, Vila romana, São Paulo, SP'),
          ('Camila','D','Santos','46574893047','rua Mariguela 345, Jd Oliveiras, Rio, RJ'),
          ('Palmira','A','Rubens','57483957483','Av Rueda 1467, Centro, Uberlandia, MG');

select * from clients;
select * from orders;

insert into product(Category, Pname, Codigo, classification_kids, Avaliation, size)
	values('Eletronico','Fone de ouvido','457',false,4,null),
		  ('Brinquedos','Barbie','786',true,5,'3x57x80'),
          ('Vestimento','Camisa Social','236',false,2,null),
          ('Eletronico','Capa de celular','986',false,1,null);
          
          

select * from orders;
insert into Orders (idOrderClient, OrderStatus, OrderDescription, SendValue, paymentCash)
	values(1, default,'compra via aplicativo',null,1),
		  (2, default,'compra via aplicativo',50,0),
          (3, 'Confirmado',null,null,1),
          (4, default,'compra via web site',150,0);

insert into entrega(idEntrega, StatusEntrega, CodRastreio)
	values(1,'Entregue','7564839276'),
		  (2,'Em Separação','4837295647'),
          (3,'Entregue','0574836273'),
          (4,'Em transporte','9574628394'),
          (5,'Em transporte','8504836408'),
          (6,'Saiu para a entrega','8504836408');
          
insert into entregaOrder(idEnEntrega, idEnOrder)
	values(1,1),
		  (6,3),
          (2,2),
          (3,1),
          (5,3);
          

insert into payments(idPayOrder, idPayProduct, typePayment, totalPrice, paymentStatus)
	values(1,1,'CreditCard',179.00,'Authorized'),
    (2,2,'Cash',360.00,'Authorized'),
    (3,3,'CreditCard',559.00,'Not Authorized'),
    (2,1,'Ticket',769.00,'Chargeback');
    
insert into productOrder(idPoOrder, idPoProduct, PoQuantity, poStatus)
	values(1,1,2,default),
		  (2,2,1,default),
          (3,3,1,default);
          
desc storageLocation;

insert into storages (StorageLocation,Quantity)
	values('São Paulo',1140),
		  ('São Paulo',3780),
          ('Rio de Janeiro',560),
          ('Minas Gerais',1200);
          
insert into storageLocation(idSlStorage, idSlProduct, location)
	values(1,1,'RJ'),
		  (2,2,'SP');
    
desc productSuplaier;

insert into suplaier(CNPJ,SocialName,contact,NameFantasySuplaier)
	values('23463234000134','José Olviveira Soares','11995746352','Barar do Zé'),
		  ('23678490000113','Eletronicos e Cia','11995746352','Bar do Zé'),
          ('56748398000164','Outlet São joão','21987645362','Outlet São João');
          
insert into productSuplaier(idPsSuplaier,idPsProduct, Quantify)
	values(1,1,500),
		  (1,2,400),
          (2,4,560),
          (3,3,5),
          (2,3,10);
          
desc productseller;

insert into seller(Cnpj, CPF, SocialName, contact, address, NameFantasySeller)
	values(null,'47594873625',null,'11987645362','São Paulo',null),
		  (13896059000145,null,'Modas da Flor','21975847362','Rio de Janeiro',default),
          (47389556000113,null,'Central Tech','34997658493','Minas Gerais','Central Tech');
          
insert into productSeller(idPsSeller,idPsProduct, ProductQuantity)
	values(1,3,60),
		  (2,2,10),
          (2,4,15),
          (3,1,40);

select count(*) from clients;

select * from clients c, orders o where c.id_Client = o.idOrderClient;

select Fname,Lname,idOrder, orderStatus from clients c, orders o where c.id_Client = o.idOrderClient;
select concat(Fname,'',Lname) as Client,idOrder as Request, orderStatus as Status from clients c, orders o where c.id_Client = o.idOrderClient;

insert into orders (idOrderClient, orderStatus, orderDescription, sendValue, paymentCash)
	values(2,default, 'compra via aplicativo',null,1);


select count(*) from clients c, orders o 
					where c.id_Client = o.idOrderClient;

-- Recuperar quantos pedidos foram realizados pelos cliente

select c.id_Client, Fname, count(*) as Number_of_Orders from clients c inner join orders o on c.id_Client = o.idOrderClient
			inner join productOrder p on p.idPoOrder = idOrder
		group by id_Client; 

-- Valores ordenados
select * from payments order by totalPrice;

-- Clientes cadastrados
select * from clients;

-- Quantos estoques tem em São paulo
select * from storages where StorageLocation = 'São Paulo';

select distinct idPsProduct from productSuplaier
	where idPsProduct in
    (select se.idPsProduct from productSuplaier su, productSeller se, product
    where su.idPsProduct = se.idPsProduct);
    
-- Quantos eletronicos foram vendidos
select count(*) as Eletronicos_vendidos from product,productOrder
	where category = 'Eletronico';
