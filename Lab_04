# Zadania lab04
## Zadanie 1
* punkt 1 stwórz tabele postac
```sql
create table postac (
    -> id_postaci int auto_increment primary key,
    -> nazwa varchar(40),
    -> rodzaj enum('wiking','ptak','kobieta'),
    -> data_ur date,
    -> wiek int unsigned);
```
* punkt 2 do tabeli dodaj rekordy
```sql
 insert into postac values
(default, 'Bjorn','wiking','1700-11-09',323),
(default, 'Drozd','ptak','1696-12-12',360),
(default, 'Tesciowa','kobieta','1820-10-20',100);

 select * from postac;

 +------------+----------+---------+------------+------+
| id_postaci | nazwa    | rodzaj  | data_ur    | wiek |
+------------+----------+---------+------------+------+
|          1 | Bjorn    | wiking  | 1700-11-09 |  323 |
|          2 | Drozd    | ptak    | 1696-12-12 |  360 |
|          3 | Tesciowa | kobieta | 1820-10-20 |  100 |
+------------+----------+---------+------------+------+
```
* punkt 3 zmodyfikuj wiek tesciowej na 88 lat
```sql
update postac set wiek=88 where id_postaci=3;
```
## Zadanie 2
* punkt 1 stwórz tabele walizka
```sql
mysql> create table walizka (
    -> id_walizki int auto_increment primary key,
    -> pojemnosc int unsigned,
    -> kolor enum ('rozowy', 'czerwony', 'teczowy', 'zolty'),
    -> id_wlasciciela int,
    -> foreign key(id_wlasciciela) references postac(id_postaci) on delete cascade);
```
* punkt 2 dodaj do pola kolor warość domyśną-różowy
```sql
alter table walizka alter kolor set default 'rozowy';
```
* punkt 3 dodaj jedna walizke dla bjonra, jedną dla teściowej
```sql
insert into walizka values (default,50,default,3),
(default,50,default,1);
```
## Zadanie 3
* punkt 1 stwórz tabele izba
```sql
create table izba(
adres_budynku varchar(50) not null, 
nazwa_izby varchar(50) not null,
metraz mediumint unsigned,
wlasciciel int,
foreign key (wlasciciel)
references postac(id_postaci) on delete set null);
#on delete lub on update -> restrict|set null|cascade
#dodanie klucza głównego do tabeli izba
alter table izba add primary key(adres_budynku, nazwa_izby);
```
* punkt 2 dodaj pole kolor po polu metraż, ustaw domyślny kolor na czarny
```sql
alter table izba add column
kolor varchar(30) default 'czarny'
after metraz;
```
* punkt 3 stwórz izbę spiżarnia
```sql
instert into izba values(
'Kolejowa 23', 'spiżarnia',10,default,1);
```
## Zadanie 4
* punkt 1 stwórz tabelę przetrwory
```sql
create table przetwory(
id_przetworu int not null auto_increment primary key,
rok_produkcji smallint default 1654,
id_wykonawcy int, foreign key(id_wykonawcy) references postac(id_postaci),
zawartosc varchar(40),
dodatek varchar(40) default 'papryczka chilli',
id_konsumenta int, foreign key(id_konsumenta) references postac(id_postaci));
```
* punkt 2 wstaw bigos z papryczką chilli do tabeli przetwory
```sql
insert into przetwory values
(default, default, 1, 'bigos', default, 3);
```
## Zadanie 5
* punkt 1 wstaw 5 wikingów do tabeli postac
```sql
insert into postac values
(default, 'Bram','wiking', '1690-10-9', 50),
(default, 'Asgaut','wiking', '1710-11-20', 40),
(default,'Eyjolf', 'wiking','1702-11-21',38),
(default,'Galti', 'wiking','1720-11-24',20),
(default,'Hallstein', 'wiking','1712-11-27',28);
```
* punkt 2 stworz tabele statek
```sql
create table statek(
nazwa_statku varchar(40) primary key,
rodzaj_statku enum('slop','brigantine','galleon'),
data_wodowania date,
max_ladownosc int unsigned
);
```
* punkt 3 dodaj dwa statki do tabeli
```sql
insert into statek values
('Hakon', 'sloop', '1600-10-9', 40000),
('Kollskegg', 'galleon', '1620-02-20', 90000);
```
* punkt 4 dodanie pola funkcja do tabeli postac
```sql
alter table postac add column
funkcja varchar(40);
```
* punkt 5 funkcja Bjorna na kapitan
```sql
update postac set funkcja = 'kapitan' where id_postaci = 1;
```
* punkt 6 dodanie klucza obcego w tabeli postac odwolujacego sie do statku
```sql
alter table postac add column statek varchar(40);
alter table postac add foreign key(statek) references statek(nazwa_statku);
```
* punkt 7 powsadzaj wikingow i drozda na statki
```sql
update postac set statek="Hakon" where id_postaci=1;
update postac set statek="Hakon" where id_postaci=2;
update postac set statek="Hakon" where id_postaci=4;
update postac set statek="Kollskegg" where id_postaci=5;
update postac set statek="Kollskegg" where id_postaci=6;
update postac set statek="Kollskegg" where id_postaci=7;
update postac set statek="Kollskegg" where id_postaci=8;
```
* punkt 8 usuń izbe spiżarmnia z tabeli izba
```sql
delete from izba;
```
* punkt 9
```
drop table izba;
```
## komendy pomocnicze
* pokazuje jak coś jest zbudowane
```sql
show create table ....;
decribe .....;
```
