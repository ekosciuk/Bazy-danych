# Bazy danych

Zadania tu przedstawione zostały wykonane na podstawie tego pliku: 
https://github.com/kropiak/bazy_inf/blob/fb2972e88237d661dbc8cf1b6816975838fe2303/lab_06/lab_06.pdf

##  Zadanie 1.

**a) Treść zadania:**

1. Wyświetl średnią wagę wszystkich wikingów.
2. Wyświetl średnią wagę oraz liczbę kreatur dla każdego rodzaju.
3. Wyświetl średni wiek dla każdego rodzaju kreatury.

**b) Wykonanie:**

Wyświetl średnią wagę wszystkich wikingów:
~~~~mysql
SELECT AVG(waga) AS srednia_waga FROM wikingowie;
~~~~
Wyświetl średnią wagę oraz liczbę kreatur dla każdego rodzaju:
~~~~mysql
SELECT rodzaj, AVG(waga) AS srednia_waga, COUNT(*) AS liczba_kreatur FROM kreatura GROUP BY rodzaj;
~~~~
Wyświetl średni wiek dla każdego rodzaju kreatury:
~~~~mysql
SELECT rodzaj, AVG(wiek) AS sredni_wiek FROM kreatura GROUP BY rodzaj;
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
SELECT rodzaj_zasobu, SUM(waga) AS suma_wag FROM zasob GROUP BY rodzaj_zasobu;
~~~~
Dla każdej nazwy zasobu wyświetlić średnią wagę, jeśli ilość jest równa co najmniej 4 oraz
jeśli ta suma wag jest większa od 10:
~~~~mysql
SELECT nazwa_zasobu, AVG(waga) AS srednia_waga FROM zasob GROUP BY nazwa zasobu HAVING COUNT(*) >= 4 AND SUM (waga) > 10;
~~~~
Wyświetlić ile jest różnych nazw dla każdego rodzaju zasobu, jeśli minimalna liczba zasobu
jest większa od 1:
~~~~mysql
SELECT 


