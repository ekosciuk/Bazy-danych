# Bazy danych

Zadania tu przedstawione zostały wykonane na podstawie tego pliku: 
https://github.com/kropiak/bazy_inf/blob/fb2972e88237d661dbc8cf1b6816975838fe2303/lab_08/lab_08.pdf

##  Zadanie 1.

**a) Treść zadania:**

1. Przekopiuj jeszcze raz z bazy wikingowie rekordy z tabeli kreatura, przekopiuj dodatkowo
tabele: uczestnicy, etapy_wyprawy, sektor, wyprawa, wraz z danymi.
2. Wypisz nazwy kreatur, które nie uczestniczyły w żadnej wyprawie.
3. Dla każdej wyprawy wypisać jej nazwę oraz sumę ilości ekwipunku, jaka została zabrana przez
uczestników tej wyprawy.

**b) Wykonanie:**
Przekopiuj jeszcze raz z bazy wikingowie rekordy z tabeli kreatura, przekopiuj dodatkowo
tabele: uczestnicy, etapy_wyprawy, sektor, wyprawa, wraz z danymi:
~~~~mysql
INSERT INTO kreatura SELECT * FROM wikingowie.kreatura;
CREATE TABLE uczestnicy AS SELECT * FROM wikingowie.uczestnicy;
CREATE TABLE etapy_wyorawy AS SELECT * FROM wikingowie.etapy_wyprawy;
CREATE TABLE sektor AS SELECT * FROM wikingowie.sektor;
CREATE TABLE wyprawa AS SELECT * FROM wikingowie.wyprawa;
~~~~
Wypisanie nazwy kreatur, które nie uczestniczyły w żadnej wyprawie:
~~~~mysql
SELECT * FROM wyprawa;
SELECT k.nazwa FROM kreatura k LEFT JOIN uczestnicy u ON k.idKreatury=u.id_uczestnika WHERE u.id_uczestnika IS NULL;
~~~~
Dla każdej wyprawy wypisać jej nazwę oraz sumę ilości ekwipunku, jaka została zabrana przez
uczestników tej wyprawy:
~~~~mysql
SELECT w.nazwa, SUM(e.ilosc) FROM uczestnicy u INNER JOIN wyprawa w ON u.id_wyprawy=w.id_wyprawy INNER JOIN kreatura k on u.id_uczestnika=k.idKreatury INNER JOIN ekwipunek e ON k.idKreatury=e.idKreatury GROUP BY w.nazwa;
~~~~

## Zadanie 2.

**a) Treść zadania:**

1. Dla każdej wyprawy wypisz jej nazwę, liczbę uczestników, oraz nazwy tych uczestników w
jednej linii.
2. Wypisz kolejne etaty wszystkich wypraw wraz z nazwami sektorów, sortując najpierw według
daty początku wyprawy, a następnie według kolejności występowania etapów. W każdym
etapie określ nazwę kierownika danej wyprawy.

**b) Wykonanie:**
Dla każdej wyprawy wypisz jej nazwę, liczbę uczestników, oraz nazwy tych uczestników w
jednej linii:
~~~~mysql
SELECT w.nazwa, COUNT(k.idKreatury) AS 'liczba uczestnikow', GROUP_CONCAT(k.nazwa separator ',') AS 'uczestnicy' FROM uczestnicy u INNER JOIN wyprawa w ON u.id_wyprawy=w.id_wyprawy INNER JOIN kreatura k ON u.id_uczestnika=k.idKreatury GROUP BY w.nazwa;
~~~~
Wypisanie kolejnych etapów wszystkich wypraw wraz z nazwami sektorów, sortując najpierw według
daty początku wyprawy, a następnie według kolejności występowania etapów. W każdym
etapie określ nazwę kierownika danej wyprawy:
~~~~mysql
SELECT ew.idwyprawy, ew.dziennik, s.nazwa AS 'nazwa sektora', k.nazwa AS 'kierownik' FROM etapy_wyprawy ew INNER JOIN sektor s ON ew.sektor=s.id_sektora INNER JOIN wyprawa w ON ew.idwyprawy=w.id_wyprawy, INNER JOIN kreatura k on w.kierownik=k.idKreatury ORDER BY w.data_rozpoczecia ASC, ew.kolejnosc ASC;
~~~~

tu skończyłam, ale ostatni punkt nie działa!!!
