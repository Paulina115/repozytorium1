 # LAB4
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
# LAB5
zadanie1
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
alter table postac change id_postaci id_postaci int;
```
zadanie2
```
update postac set pesel = "35426719281" + id_postaci;
alter table postec add primary key(pesel);
alter table postac change rodzaj rodzaj enum("wiking','ptak', 'kobieta','syrena', 'waz');
desc przetwory;
insert into postac(id_postaci,nazwa,rodzaj,data_ur,wiek,pesel) values(11, 'Gertruda Nieszczera','syrena','1600-10-08',323,'35426719281'+id_postaci);
```
zadanie3
```
select nazwa from postac where nazwa like '%a%';
select nazwa from postac where nazwa regexp '^d';
update postac set statek = 'Statek1' where nazwa like '%a%';
select: from statek where data_wodowania >= "1901-01-91' and data wodowania <= '2023-12-31'
select*from statek where year (data _wodowania) between 1901 and 2023;
update statek set max_ladownosc = max_ladownosc*0.7 where year(data_wodowania) between 1901 and 2023;
select*from statek;
alter table postac add check (wiek <= 1009); 
update postac set wiek = 2000 where nazwa = 'Bjorn;
```
zadanie4
```
insert into postac(id_postaci,nazwa,rodzaj,data_ur,wiek,pesel) values(12,'Loko', 'waz', '1506-09-03',523, '35426719281" + id_postaci);
create table Marynarz like postac;
select*from Marynarz;
desc Marynarz;
desc postac;
desc Marynarz;
insert into Marynarz select from postac where statek is not null;
```
zadanie5
```
uptade postc set statek = null;
delete from postac where nazwa = 'wiking3';
select*from statek;
delete from statek where nazwa_statku = 'statek1'
delete from statek where nazwa_statku = 'statek2'
drop table statek;
alter table postac drop foreign key postac_ibfk_1;
create table zwierz(
id_zwierza int auto_increment primary key,
nazwa varchar(10)
wiek int);
insert into zwierz (nazwa select*from postac where rodzaj = 'ptak')
```
# LAB6
zadanie1
```
create table kreatura as select*from wikingowie.kreatura;
select*from kreatura;
create table zasob as select*from wikingowie.zasob;
select*from zasob;
create table ekwipunek as select*from wikingowie.ekwipunek;
select*from ekwipunek;
select*from zasob where rodzaj = 'jedzenie';
select*from zasob where kreatura.idKreatury = 1 or kreatura.idKreatury=3 or kreatura.idKreatury=5 ;
```
zadanie2
```
select*from kreatura where rodzaj = 'wiedzma' and udzwig >= 50;
select*from zasob where waga between 2 and 5;
select*from kreatura where nazwa like '%or%'and udzwig between 30 and 70;
```
zadanie3
```
select*from zasob where month(dataPozyskania) = 07 or  month(dataPozyskania) = 08 ;
select rodzaj from zasob where rodzaj is not null order by waga asc;
select*from kreatura order by dataUr desc limit 5;
```
zadanie4
```
select distinct rodzaj from zasob;
select distinct ilosc,rodzaj from zasob where rodzaj = 'jedzenie' and ilosc = 1;
select concat(nazwa , rodzaj ) from kreatura where rodzaj like '%wi%';
select concat(ilosc*waga) from zasob where year(dataPozyskania) between 200 and 2007;
```
zadanie5
```
select concat(0.7*waga,'|',0.3*waga) from zasob;
select*from zasob where rodzaj is null;
select distinct rodzaj from zasob where nazwa like '%Ba%' or nazwa like '%os%' order by nazwa;
```
# LAB7
zadanie 1
```
select*from kreatura;
select avg(waga) from kreatura where rodzaj = wiking';
select avg(waga),rodzaj,count(*) from kreatura group by rodzaj
select avg(2023-year (dataur)) from kreatura group by rodzaj;
select*from ekwipunek;
```
zadanie 2
```
select sum(waga*ilosc) from zasob group by rodzaj;
select*from zasob;
select avg(waga*ilosc) from zasob where ilosc >= 4"
group by nazwa having sum(waga)>=10
select count(distinct nazwa) from zasob group by rodzaj having min(ilosc) > 1;
```
zadanie3
```
select k.nazwa,sum( e.ilosc) from ekwipunek e inner join kreatura k on e.idKreatury = k.idKreatury group by k.nazwa ;

select k.nazwa, z.nazwa from zasob z inner join ekwipunek e on z.idZasobu=e.idZasobu inner join kreatura k on k.idKreatury = e.idKreatury;
select k.nazwa from kreatura k left join ekwipunek e on k.idKreatury= e.idKreatury  where e.idKreatury is  null;


```
zadanie 4
```
select k.nazwa, z.nazwa from kreatura k inner join ekwipunek e on k.idKreatury = e.idKreatury inner join zasob z on e.idZasobu = z.idZasobu where year(k.dataUr) between 1670 and 1680;

select k.nazwa from kreatura k inner join ekwipunek e on k.idKreatury=e.idKreatury inner join zasob z on z.idZasobu = e.idZasobu where z.rodzaj = 'jedzenie' order by k.dataUr desc limit 5;

select concat(k1.nazwa,'-',k2.nazwa) from kreatura k1 inner join kreatura k2 on k1.idKreatury - k2.idKreatury = 5;
```
zadanie5
```
select avg(z.waga*z.ilosc) from zasob z inner join ekwipunek e on z.idZasobu = e.idZasobu inner join kreatura k on k.idkreatury=e.idKreatury where k.rodzaj<>'waz' or k.rodzaj<> 'malpa'and e.ilosc <30 group by k.rodzaj;

select k.rodzaj,k.nazwa,n.najstarsza,n.najmlodsza from (select rodzaj, max(dataUr) as najstarsza, min(dataUr) as najmlodsza from kreatura group by rodzaj) n inner join kreatura k on n.najstarsza=k.dataUr;
```
# LAB8
zadanie1
```
create table uczestnicy as select*from wikingowie.uczestnicy;
create table etapy_wyprawy as select*from wikingowie.etapy_wyprawy;
create table sektor as select*from wikingowie.sektor;
create table wyprawa as select*from wikingowie.wyprawa;

select k.nazwa, k.idKreatury  from kreatura k left join uczestnicy u on k.idKreatury = u.id_uczestnika
where u.id_uczestnika is null;

select w.nazwa, sum(e.ilosc) from wyprawa w inner join uczestnicy u on w.id_wyprawy = u.id_wyprawy inner join ekwipunek e on e.idKreatury = u.id_uczestnika group by w.nazwa;
```
zadanie2
```
select w.nazwa,count( u.id_wyprawy),group_concat(k.nazwa) from wyprawa w inner join uczestnicy u on w.id_wyprawy = u.id_wyprawy inner join kreatura k on u.id_uczestnika=k.idKreatury group by u.id_wyprawy;

select w.nazwa, ew.idEtapu, s.nazwa, k.nazwa as nazwa_kierownika from wyprawa w
inner join etapy_wyprawy ew on w.id_wyprawy = ew.idWyprawy
inner join kreatura k on w.kierownik = k.idKreatury
inner join sektor s on s.id_sektora = ew.sektor
order by w.data_rozpoczecia asc, ew.kolejnosc asc;
```
zadanie3
```
select s.nazwa, count(ew.sektor) as ilosc_odwiedzin from sektor s left join etapy_wyprawy ew on s.id_sektora=ew.sektor group by s.nazwa;

select k.nazwa, if(count(u.id_uczestnika)>0,'Brala udzial','Nie brala udzialu') as udzial
from kreatura k left join uczestnicy u on k.idKreatury=u.id_uczestnika group by k.nazwa;

```
zadanie4
```
 select w.nazwa,sum(length(ew.dziennik))from wyprawa w inner join etapy_wyprawy ew on w.id_wyprawy=ew.idWyprawy group by w.nazwa having sum(length(ew.dziennik))<400;

select w.nazwa,sum(z.waga*z.ilosc)/count(u.id_uczestnika)from wyprawa w inner join uczestnicy u on w.id_wyprawy=u.id_wyprawy inner join ekwipunek e on u.id_uczestnika=e.idKreatury inner join zasob z on e.idZasobu=z.idZasobu group by w.nazwa;
```
zadanie5
```
select w.nazwa,k.nazwa,datediff(k.dataUr,w.data_rozpoczecia)from kreatura k inner join uczestnicy u on k.idKreatury = u.id_uczestnika inner join wyprawa w on u.id_wyprawy=w.id_wyprawy inner join etapy_wyprawy ew on w.id_wyprawy=ew.idWyprawy where ew.sektor=7;












