# Bazy danych

Zadania tu przedstawione zostały wykonane na podstawie tego pliku: 
https://github.com/kropiak/bazy_inf/blob/fb2972e88237d661dbc8cf1b6816975838fe2303/lab_06/lab_06.pdf

##  Zadanie 1.

**a) Treść zadania:**

1. Skopiuj tabele 'kreatura','zasob','ekwipunek' z bazy 'wikingowie' do swojej bazy.
2. Wypisz wszystkie rekordy z tabeli 'zasob'.
3. Wypisz wszystkie rekordy z tabeli 'zasob' gdzie typ to jedzenie.
4. Wypisz 'idZasobu', 'ilosc', dla kreatur o id 1,3,5.

**b) Wykonanie:**

Kopiowanie tabel z bazy 'wikingowie' do swojej bazy:
~~~~mysql
CREATE TABLE kreatura AS SELECT * FROM wikingowie.kreatura;
CREATE TABLE zasob AS SELECT * FROM wikingowie.zasob;
CREATE TABLE ekwipunek AS SELECT * FROM wikingowie.ekwipunek;
~~~~
Wypisywanie rekordów z tabeli 'zasob':
~~~~mysql
SELECT * FROM zasob;
~~~~
Wypisywanie rekordów z tabeli 'zasob', gdzie typ to jedzenie:
~~~~mysql
SELECT * FROM zasob WHERE rodzaj='jedzenie';
~~~~
Wypisaywanie 'idZasobu', 'ilosc', dla kreatur o id 1,3,5:
~~~~mysql
SELECT idZasobu, ilosc FROM zasob WHERE id_kreatury in (1,3,5);
~~~~

## Zadanie 2.

**a) Treść zadania:**

1. Wyświetl kreatury, które nie są wiedźmą i dźwigają co najmniej 50kg.
2. Wyświetl zasoby, które ważą pomiędzy 2 a 5 kg.
3. Wyświetl kreatury, których nazwa zawiera 'or' i które dźwigają między 30kg a 70kg

**b) Wykonanie:**

Wyświetlanie kreatur, które nie sa wiedźmą i dźwigają co najmniej 50 kg:
~~~~mysql
SELECT * FROM kreatura WHERE rodzaj <> 'wiedzma' AND udzwig>50 or udzwig=50;
~~~~
Wyświetlanie zasobów, które ważą pomiędzy 2 a 5 kg:
~~~~mysql
SELECT * FROM zasob WHERE waga BETWEEN 2 AND 5;
~~~~
Wyświetlanie kreatur, których nazwa zawiera 'or' i które dźwigają między 30 a 70 kg:
~~~~mysql
SELECT * FROM kreatura WHERE nazwa='%or' AND udzwig BETWEEN 30 AND 70;
~~~~

## Zadanie 3.

**a) Treść zadania:**

1. Wyświetl zasoby, które zostały pozyskane w miesiącach lipcu i sierpniu.
2. Wyświetl zasoby, które mają zdefiniowany rodzaj od najlżejszego do najcięższego.
3. Wyświetl 5 najstarszych kreatur.

**b) Wykonanie:**

Wyświetlanie zasobów, które zostały pozyskane w miesiącach lipcu i sierpniu:
~~~~mysql
SELECT * FROM zasob WHERE MONTH(dataPozyskania) =7 OR MONTH(dataPozyskania)=8;
~~~~
Wyświetlanie zasobów, które mają zdefiniowany rodzaj od najlżejszego do najcięższego:
~~~~mysql
SELECT * FROM zasob WHERE rodzaj IS NOT NULL ORDER BY waga ASC;
~~~~
Wyświetalnie 5 najstarszych wikingów:
~~~~mysql
SELECT * FROM kreatura WHERE dataUr IS NOT NULL ORDER BY dataUr ASC LIMIT 5;
~~~~

## Zadanie 4.

**a) Treść zadania:**

1. Wyświetl unikalne rodzaje zasobów.
2. Wyświetl jako jedną kolumnę nazwę i rodzaj kreatury (w postaci: nazwa - rodzaj), gdzie
rodzaj rozpoczyna się od 'wi'.
3. Wyświetl zasoby z całkowitą wagą danego zasobu (ilość * waga) dla zasobów pozyskanych w
latach 2000-2007.

**b) Wykonanie:**

Wyświetlanie unikalnych rodzajów zasobów:
~~~~mysql
SELECT DISTINCT rodzaj AS 'unikalne rodzaje' FROM zasob;
~~~~
Wyświetlanie jednej kolumny, zgodnie z warunkami z 2).:
~~~~mysql
SELECT CONCAT(nazwa, ' - ', rodzaj) AS 'nazwa i rodzaj kreatury' FROM kreatura WHERE rodzaj LIKE 'wi%';
~~~~
Wyświetlanie zasobów z całkowitą wagą danego zasobu (ilość * waga) dla zasobów pozyskanych w latach 2000-2007:
~~~~mysql
SELECT *, (ilosc*waga) AS 'calkowita waga zasobu' FROM zasob WHERE YEAR(dataPozyskania) BETWEEN 2000 and 2007;
~~~~

## Zadanie 5.

**a) Treść zadania:**

1. Zakładając, że każdy rodzaj jedzenia to 30% odpadu, wyświetl masę właściwego jedzenia
(netto) oraz wagę odpadków.
2. Wyświetl zasoby, które nie mają rodzaju.
3. Wyświetl wszystkie unikalne rodzaje zasobów, których nazwa zaczyna się od 'Ba' lub kończy się na 'os'. Dane posortuj alfabetycznie.

**b) Wykonanie:**

Wyświetlanie masy właściwego jedzenia (netto) oraz wagi odpadków, zakładając że każdy rodzaj jedzenia to 30% odpadu:
~~~~mysql
SELECT nazwa, (waga*ilosc*0.7) AS 'waga netto', (waga*ilosc*0.3) AS 'waga odpadkow' FROM zasob WHERE rodzaj='jedzenie';
~~~~
Wyświetlanie zasobów, które nie mają rodzaju:
~~~~mysql
SELECT * FROM zasob WHERE rodzaj IS NULL;
~~~~
Wyświetlanie wszystkich unikalnych rodzajów zasobów, których nazwa zaczyna się od 'Ba' lub kończy się na 'os' i sortowanie alfabetyczne:
~~~~mysql
SELECT DISTINCT rodzaj, nazwa FROM zasob WHERE nazwa LIKE 'Ba%' OR nazwa LIKE '%os' ORDER BY nazwa;
~~~~
