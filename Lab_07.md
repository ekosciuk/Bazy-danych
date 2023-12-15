# Bazy danych

Zadania tu przedstawione zostały wykonane na podstawie tego pliku: 
https://github.com/kropiak/bazy_inf/blob/fb2972e88237d661dbc8cf1b6816975838fe2303/lab_07/lab_07.pdf

##  Zadanie 1.

**a) Treść zadania:**

1. Wyświetl średnią wagę wszystkich wikingów.
2. Wyświetl średnią wagę oraz liczbę kreatur dla każdego rodzaju.
3. Wyświetl średni wiek dla każdego rodzaju kreatury.

**b) Wykonanie:**

Wyświetl średnią wagę wszystkich wikingów:
~~~~mysql
SELECT AVG(waga) FROM kreatura WHERE rodzaj='wiking';
~~~~
Wyświetl średnią wagę oraz liczbę kreatur dla każdego rodzaju:
~~~~mysql
SELECT rodzaj, AVG(waga), COUNT(waga), COUNT(*) FROM kreatura GROUP BY rodzaj;
~~~~
Wyświetl średni wiek dla każdego rodzaju kreatury:
~~~~mysql
SELECT rodzaj, AVG(year(curdate())-year(dataur)) AS 'sredni wiek' FROM kreatura GROUP BY rodzaj;
~~~~

## Zadanie 2.

**a) Treść zadania:**

1. Dla każdego rodzaju zasobu wyświetlić sumę wag tego zasobu.
2. Dla każdej nazwy zasobu wyświetlić średnią wagę, jeśli ilość jest równa co najmniej 4 oraz
jeśli ta suma wag jest większa od 10.
3. Wyświetlić ile jest różnych nazw dla każdego rodzaju zasobu, jeśli minimalna liczba zasobu
jest większa od 1.

**b) Wykonanie:**

Dla każdego rodzaju zasobu wyświetlić sumę wag tego zasobu:
~~~~mysql
SELECT rodzaj, SUM(waga*ilosc) FROM zasbo GROUP BY rodzaj;
~~~~
Dla każdej nazwy zasobu wyświetlić średnią wagę, jeśli ilość jest równa co najmniej 4 oraz
jeśli ta suma wag jest większa od 10:
~~~~mysql
SELECT nazwa, AVG(waga) FROM zasob WHERE ilosc >= 4 GROUP BY nazwa HAVING SUM(waga) >10;
~~~~
Wyświetlić ile jest różnych nazw dla każdego rodzaju zasobu, jeśli minimalna liczba zasobu
jest większa od 1:
~~~~mysql
SELECT rodzaj, COUNT(DISTINCT(nazwa)) AS liczba FROM zasob GROUP BY rodzaj HAVING liczba >1;
~~~~

## Zadanie 3.

**a) Treść zadania:**

1. Wyświetlić dla każdej kreatury ilości zasobów jakie niesie.
2. Wyświetlić dla każdej kreatury nazwy zasobów jakie posiada.
3. Wyświetlić kreatury, które nie posiadają żadnego ekwipunku.

**b) Wykonanie:**
Wyświetlanie dla każdej kreatury ilości zasobów jakie niesie:
~~~~mysql
SELECT k.nazwa, e.idZasobu, e.ilosc from kreatura k inner join ekwipunek e on k.idKreatury=e.id.Kreatury;
~~~~
Wyświetlenie dla każdej kreatury nazwy zasobów jakie posiada:
~~~~mysql
SELECT k.nazwa, z.nazwa FROM kreatura k INNER JOIN ekwipunek e ON k.idKreatury=e.idKreatury INNER JOIN zasob z ON z.idZasobu=e.idZasobu;
~~~~
Wyświetlenie kreatury, które nie posiadają żadnego ekwipunku:
~~~~mysql
SELECT k.nazwa FROM kreatura k LEFT JOIN ekwipunek e ON k.idKreatury=e.idKreatury WHERE e.idKreatury IS NULL;
~~~~
## Zadanie 4.

**a) Treść zadania:**

1. Wyświetlić nazwy wikingów, którzy urodzili się w latach 70-tych XVII wieku oraz nazwy
zasobów, które posiadają (użyj natural joina jeśli się da).
2. Wyświetlić nazwy 5 najmłodszych kreatur, które w ekwipunku posiadają jedzenie.
3. Wypisz obok siebie nazwy kreatur, których numer idKreatury różni się o 5 (np. Bjorn - Astrid, Brutal - Ibra itd.).

**b) Wykonanie:**

Wyświetlanie nazwy wikingów, którzy urodzili się w latach 70-tych XVII wieku oraz nazwy
zasobów, które posiadają (użyj natural joina jeśli się da):
~~~~mysql
SELECT k.nazwa, z.nazwa FROM kreatura k, ekwipunek e NATURAL JOIN zasob z WHERE k.rodzaj='wiking' AND YEAR(k.dataUr) BETWEEN 1670 AND 1679;
~~~~
Wyświetlanie nazwy 5 najmłodszych kreatur, które w ekwipunku posiadają jedzenie:
~~~~mysql
SELECT * FROM kreatura k INNER JOIN ekwipunek e ON k.idKreatury=e.idKreatury INNER JOIN zasob z on z.idZasobu=e.idZasobu WHERE z.rodzaj='jedzenie' ORDER BY k.dataUr DESC LIMIT 5;
~~~~
Wypisanie obok siebie nazwy kreatur, których numer idKreatury różni się o 5 (np. Bjorn - Astrid, Brutal - Ibra itd.):
~~~~mysql
SELECT CONCAT(k1.nazwa,'-', k2.nazwa) FROM kreatura k1 INNER JOIN kreatura k2 WHERE k1.idKreatury - k2.idKreatury = 5;
~~~~

##Zadanie 5.

**a) Treść zadania:**

1. Dla każdego rodzaju kreatury wyświetlić średnią wagę zasobów, jaką posiadają w ekwipunku,
jeśli kreatura nie jest małpą ani wężem i ilość ekwipunku jest poniżej 30.
2. Dla każdego rodzaju kreatury wyświetlić nazwę, datę urodzenia i rodzaj najmłodszej i
najstarszej kreatury.

**b) Wykonanie:**

Dla każdego rodzaju kreatury wyświetlić średnią wagę zasobów, jaką posiadają w ekwipunku,
jeśli kreatura nie jest małpą ani wężem i ilość ekwipunku jest poniżej 30:
~~~~mysql
SELECT k.rodzaj, AVG(e.ilosc * z.waga) FROM kreatura k INNER JOIN ekwipunek e ON k.idKreatury=e.idKreatury INNER JOIN zasob z on e.idZasobu=z.idZasobu WHERE k.rodzaj NOT IN ('malpa','waz') GROUP BY k.rodzaj HAVING SUM(e.ilosc) <30;
~~~~
Dla każdego rodzaju kreatury wyświetlić nazwę, datę urodzenia i rodzaj najmłodszej i
najstarszej kreatury:
~~~~mysql
SELECT a.nazwa, a.rodzaj, a.dataUr FROM kreatura a, (SELECT MIN(dataUr) MIN, MAX(dataUr) MAX FROM kreatura GROUP BY rodzaj) b WHERE b.min = a.dataUr or b.max=a.dataUr;
~~~~
