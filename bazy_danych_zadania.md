Zadanie1
```
mysql> create table postac(
    -> id_postaci int auto_increment primary key,
    -> nazwa varchar(40),
    -> rodzaj enum('wiking','ptak','kobieta'),
    -> data_ur date
    -> ,wiek int unsigned);
Query OK, 0 rows affected (0.06 sec)
mysql> select*from postac;                                                                                              
Empty set (0.00 sec)                                                                                                                                                                                                                            
mysql> insert into postac(nazwa,rodzaj,data_ur,wiek) values('Bjorn','wiking','1700-11-09',323);                         
Query OK, 1 row affected (0.01 sec)                                                                                                                        
mysql> select*from postac;                                                                                                                                                                           
+------------+-------+--------+------------+------+                                                                                                                                                  
| id_postaci | nazwa | rodzaj | data_ur    | wiek |                                                                                                                                                 
+------------+-------+--------+------------+------+                                                                                                                                                  
|          1 | Bjorn | wiking | 1700-11-09 |  323 |                                                                                                                                                  
+------------+-------+--------+------------+------+                                                                                                                                                 
1 row in set (0.00 sec) 
mysql> insert into postac(nazwa,rodzaj,data_ur,wiek) values('drozd','ptak','2000-11-03',20);                            
Query OK, 1 row affected (0.01 sec)                                                                                                                                                                                                             
mysql> insert into postac(nazwa,rodzaj,data_ur,wiek) values('Tesciowa','kobieta','1970-10-07',54);                      
Query OK, 1 row affected (0.01 sec)   
mysql> select*from postac;                                                                                              
+------------+----------+---------+------------+------+                                                                 
| id_postaci | nazwa    | rodzaj  | data_ur    | wiek |                                                                 
+------------+----------+---------+------------+------+                                                                 
|          1 | Bjorn    | wiking  | 1700-11-09 |  323 |                                                                
|          2 | drozd    | ptak    | 2000-11-03 |   20 |                                                                 
|          3 | Tesciowa | kobieta | 1970-10-07 |   54 |                                                                 
+------------+----------+---------+------------+------+                                                                 
3 rows in set (0.00 sec)                                                                                                                                                                                                                        
mysql> update postac                                                                                                        
-> set data_ur = '1935-10-07' where id_postaci = 3                                                                      
-> set wiek = 88 where id_postaci = 3;                                                                              
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'set wiek = 88 where id_postaci = 3' at line 3                                        
mysql> update postac                                                                                                        
-> set data_ur = '1935-10-07' where id_postaci = 3;                                                                 
Query OK, 1 row affected (0.01 sec)                                                                                     
Rows matched: 1  Changed: 1  Warnings: 0                                                                                                                                                                                                        
mysql> update postac                                                                                                      
-> set wiek = 88 where id_postaci = 3;                                                                             
Query OK, 1 row affected (0.01 sec)                                                                                     
Rows matched: 1  Changed: 1  Warnings: 0 
mysql> select*from postac;                                                                                             
+------------+----------+---------+------------+------+                                                                 
| id_postaci | nazwa    | rodzaj  | data_ur    | wiek |                                                                 
+------------+----------+---------+------------+------+                                                                
|          1 | Bjorn    | wiking  | 1700-11-09 |  323 |                                                                 
|          2 | drozd    | ptak    | 2000-11-03 |   20 |                                                                 
|          3 | Tesciowa | kobieta | 1935-10-07 |   88 |                                                                 
+------------+----------+---------+------------+------+                                                                 
3 rows in set (0.00 sec)
```
zadanie2
```
mysql> create table walizka(                                                                         
-> id_walizki int auto_increment primary key,                                                    
-> pojemnosc int unsigned,                                                                       
-> kolor enum('rozowy','czerwony','teczowy','zolty'),                                            
-> id_wlasciciel int,                                                                            
-> foreign key (id_wlasciciela) references postac(id_postaci) on delate cascade);            
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'delate cascade)' at line 6             
mysql> foreign key (id_wlasciciela) references postac(id_postaci) on delete cascade);            
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'foreign key (id_wlasciciela) references postac(id_postaci) on delete cascade)' at line 1                                                
mysql> create table walizka(                                                                         
-> id_walizki int auto_increment primary key,                                                    
-> pojemnosc int unsigned,                                                                       
-> kolor enum('rozowy','czerwony','teczowy','zolty'),                                            
-> id_wlasciciel int,                                                                            
-> foreign key (id_wlasciciela) references postac(id_postaci) on delete cascade);           
 ERROR 1072 (42000): Key column 'id_wlasciciela' doesn't exist in table                           
mysql> create table walizka(                                                                         
-> id_walizki int auto_increment primary key,                                                    
-> pojemnosc int unsigned,                                                                       
-> kolor enum('rozowy','czerwony','teczowy','zolty'),                                            
-> id_wlasciciela int,                                                                           
-> foreign key (id_wlasciciela) references postac(id_postaci) on delete cascade);           
 Query OK, 0 rows affected (0.08 sec)   
mysql> select*from walizka;                                                                      
Empty set (0.01 sec)                                                                                                                                                                              
mysql> alter table walizka                                                                           
-> alter column kolor set default 'rozowy';                                                  
Query OK, 0 rows affected (0.02 sec)                                                             
Records: 0  Duplicates: 0  Warnings: 0 
                                  
mysql> insert into walizka(pojemnosc,id_wlasciciela) values(50,(select id_postaci from postac where id_postaci = 1));                                                                             
Query OK, 1 row affected (0.01 sec)                                                                                                                                                               
mysql> insert into walizka(pojemnosc,id_wlasciciela) values(70,(select id_postaci from postac where id_postaci = 3));                                                                             
Query OK, 1 row affected (0.01 sec) 
mysql> select*from walizka                                                                           
-> ;                                                                                         
+------------+-----------+--------+----------------+                                             
| id_walizki | pojemnosc | kolor  | id_wlasciciela |                                             
+------------+-----------+--------+----------------+                                             
|          1 |        50 | rozowy |              1 |                                             
|          2 |        70 | rozowy |              3 |                                             
+------------+-----------+--------+----------------+                                             
2 rows in set (0.00 sec)
```
zadanie3
```
mysql> create table izba(                                                                            
-> adres_budynku varchar(50)                                                                     
-> , nazwa_izby varchar(50),                                                                     
-> metraz int unsigned,                                                                          
-> wlasciciel int,                                                                               
-> primary key(adres_budynku,nazwa_izby),                                                        
-> foreign key (wlasciciel) references postac(id_postaci) on delete set null);               
Query OK, 0 rows affected (0.05 sec)
mysql> alter table izba                                                                              
-> add column kolor varchar(30) after metraz;                                                
Query OK, 0 rows affected (0.04 sec)                                                            
 Records: 0  Duplicates: 0  Warnings: 0                                                                                                                                                            
mysql> alter table izba                                                                              
-> alter column kolor set default 'czarny';                                                  
Query OK, 0 rows affected (0.02 sec)                                                             
Records: 0  Duplicates: 0  Warnings: 0
mysql> alter table izba                                                                                                                                                                                                               
-> alter column kolor set default 'czarny';                                                                                                                                                                                   
Query OK, 0 rows affected (0.02 sec)                                                                                                                                                                                              
Records: 0  Duplicates: 0  Warnings: 0
mysql> select*from walizka;
Empty set (0.01 sec)
mysql> select*from postac;                                                                                                                                    
```
zadanie4
```
use infs_jurewiczp;
create table przetwory(
id_przetworu int auto_increment primary key,
rok_produkcji int default 1654,
id_wykonawcy int,
zawartosc varchar(225),
dodatek varchar(225) default 'chilli',
id_konsumenta int,
foreign key(id_wykonawcy,id_konsumenta) references postac(id_postaci));
insert into przetwory(id_przetworu,id_wykonawcy,zawartosc,id_konsumenta) values(1,(select id_postaci from postac where id_postaci = 1), 'bigos',(select id_postaci from postac where id_postaci = 3));
select*from przetwory;
select*from postac;
insert into postac(nazwa,rodzaj,data_ur,wiek) values('Wiking1','wiking','1701-05-11',322);
insert into postac(nazwa,rodzaj,data_ur,wiek) values('Wiking2','wiking','1702-12-11',321);
insert into postac(nazwa,rodzaj,data_ur,wiek) values('Wiking3','wiking','1703-04-01',320);
insert into postac(nazwa,rodzaj,data_ur,wiek) values('Wiking4','wiking','1704-03-13',319);
insert into postac(nazwa,rodzaj,data_ur,wiek) values('Wiking5','wiking','1705-07-11',318);
```
zadanie5
```
create table statek(
nazwa_statku varchar(60) primary key,
rodzaj_statku enum('maly','sredni','duzy'),
data_wodowania date,
max_ladownosc int unsigned);
select*from statek;
insert into statek(nazwa_statku,rodzaj_statku,data_wodowania,max_ladownosc) values('statek1','maly','2023-12-12',20);
insert into statek(nazwa_statku,rodzaj_statku,data_wodowania,max_ladownosc) values('statek2','sredni','2023-12-12',30);
alter table postac add column funkcja varchar(60);
select*from postac;
update postac set funkcja = 'kapitan' where id_postaci = 1;
alter table postac add column statek varchar(100);
alter table postac add foreign key(statek) references statek(nazwa_statku);
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek1') where id_postaci = 1;
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek1') where id_postaci = 2;
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek2') where id_postaci = 4;
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek2') where id_postaci = 5;
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek2') where id_postaci = 6;
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek2') where id_postaci = 7;
update postac set statek = (select nazwa_statku from statek where nazwa_statku = 'statek2') where id_postaci = 8;
delete from izba where nazwa_izby = 'spizarnia';
select*from izba;
drop table izba;
```
show tables
alter table postac modify id_postaci int;
alter table walizka drop foreign key walizka_ibfk_1;
alter table przetwory drop foreign key przetwory_ibfk_1;
desc postac;
alter table przetwory drop foreign key przetwory_ibfk_2;
alter table postac add column pesel char(11);
alter table postac drop primary key;
select* fron postac;
alter table postac change id postaci id postaci int;
update postac set pesel = "35426719281" + id_postaci;
alter table postec add primary key(pesel);
alter table postac change rodzaj rodzaj enum("wiking','ptak', 'kobieta','syrena', 'waz');
desc przetwory;
insert into postac(id_postaci,nazwa,rodzaj,data_ur,wiek,pesel) values(11, 'Gertruda Nieszczera','syrena'
select naze from postac where nazwa like '%ak'y
select nazwa from postac where nazwa regexp '^d';
update postac set statek = 'Statek1' where nazwa like '%ar';
select: from statek where data_wodowania >= "1901-01-91' and data wodowania <= '2023-12-31'
select*from statek where year (data _wodowania) between 1901 and 2023;
update statek set max_ladownosc = max_ladownosc*0.7 where year(data_wodowania) between 1901 and 2023;
select*from statek;
alter table postac add check (wiek <= 1009);
C3
1663-19-83',323, *35426719281"
id_postaci);
update postac set wiek
2000 where nazwa = 'Bjorn
insert into postac(id_postaci,nazwa,rodzaj,data_ur,wiek,pesel) valuis(12,'Loko', 'waz', '1506-09-03',523, '35426719281" + id_postaci);
create table Marynarz like postac;
select*from Marynarz;
desc Maryna-z;
desc postac;
ej na
OS
post
id_postaci);
zrobil
iej
tr2e
Jchy ni
dobny
28
29
30
31
32
33
34
Hministration Schemas
formation:
No object selected
desc Marynarz;
insert into Marynarz select
from postac where statek is not null:
