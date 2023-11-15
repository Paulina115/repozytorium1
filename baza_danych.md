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
Records: 0  Duplicates: 0  Warnings: 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 mysql> select*from walizka;                                                                      Empty set (0.01 sec)                                                                                                                                                                                                                               mysql> select*from postac;                                                                                                                                    


```

