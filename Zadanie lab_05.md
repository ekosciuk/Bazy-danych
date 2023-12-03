# Bazy danych
Zadania tu przedstawione zostały wykonane na podstawie tego pliku: https://github.com/kropiak/bazy_inf/blob/573cc6a82dde109acd27761d9c15a86a53e7661e/lab_04/lab_04.pdf

##  Zadanie 1.
**a) Treść zadania:**

1. uśmierć dwóch najstarszych wikingów, ale to nie może być Bjorn (DELETE z WHERE)
2. usuń klucz główny z tabeli postac - może być potrzebnych kilka komend (np. odłączenie kluczy
obcych)

**b) Wykonanie:**
Znajdowanie dwóch najstarszych wikingów (oprócz Bjorna):
~~~mysql
SELECT * FROM postac WHERE rodzaj = 'wiking' AND nazwa != 'Bjorn' ORDER BY wiek DESC LIMIT 2;
~~~
Usuwanie dwóch najstarszych wikingów (oprócz Bjorna):
~~~mysql
DELETE FROM postac WHERE id_postaci IN (SELECT id_postaci FROM (SELECT id_postaci FROM postac WHERE rodzaj = 'wiking' AND nazwa != 'Bjorn' ORDER BY wiek DESC LIMIT 2) AS temp);
~~~
Usuwanie klucza głównego z tabeli postac:
~~~mysql

## Zadanie 2.
**a) Treść zadania:**
1) Stwórz tabelę walizka:
* id_walizki - liczba samozwiększająca się, klucz główny
* pojemnosc - liczba nieujemna
* kolor - typ wyliczeniowy (różowy, czerwony, tęczowy, żółty)
* id_wlasciciela - klucz obcy odwołujący się do tabeli postać, ustawione kaskadowe
usuwanie.
2) Dodaj do pola kolor wartość domyślną – różowy.
3) Dodaj jedną walizkę dla Bjorna i jedną walizkę dla teściowej

**b) Wykonanie:**

Tworzenie tabeli walizka:
~~~mysql
CREATE TABLE walizka (id_walizki INT AUTO_INCREMENT PRIMARY KEY, pojemnosc INT UNSIGNED, kolor ENUM('różowy', 'czerwony', 'tęczowy', 'żółty') DEFAULT 'różowy', id_wlasciciela INT, FOREIGN KEY (id_wlasciciela) REFERENCES postac(id_postaci) ON DELETE CASCADE);
~~~
Dodawanie jednej walizki dla Bjorna i jednej dla teściowej:
~~~mysql
INSERT INTO walizka (pojemnosc, id_wlasciciela) VALUES (20, (SELECT id_postaci FROM postac WHERE nazwa = 'Bjorn'));
INSERT INTO walizka (pojemnosc, id_wlasciciela) VALUES (15, (SELECT id_postaci FROM postac WHERE nazwa = 'Tesciowa'));
~~~
## Zadanie 3.
**a) Treść zadania:**
1. Stwórz tabelę izba z polami:
* adres_budynku - część klucza głównego
* nazwa_izby - część klucza głównego
* metraz - liczba nieujemna
* wlasciciel - klucz obcy do tabeli postać, ustaw null w razie usunięcia.
2. Za pomocą oddzielnego polecenia dodaj pole kolor izby po polu metraż. Ustaw domyślny kolor na czarny.
3. Stwórz izbę spiżarnia.

**b) Wykonanie:**

Tworzenie tabeli izba:
~~~mysql
CREATE TABLE izba (adres_budynku VARCHAR(255), nazwa_izby VARCHAR(255), metraz INT UNSIGNED, wlasciciel INT, PRIMARY KEY (adres_budynku, nazwa_izby), FOREIGN KEY (wlasciciel) REFERENCES postac(id_postaci) ON DELETE SET NULL);
~~~
Dodawanie pola kolor izby po polu metraz:
~~~mysql
ALTER TABLE izba ADD COLUMN kolor_izby VARCHAR(20) DEFAULT 'czarny' AFTER metraz;
~~~
Stworzenie izby spiżarnia:
~~~mysql
INSERT INTO izba (adres_budynku, nazwa_izby, metraz, wlasciciel) VALUES ('Bjornowska 125', 'spiżarnia', 15, NULL);
~~~
## Zadanie 4.
**a) Treść zadania:**
1. Stwórz tabelę przetwory z polami:
* id_przetworu - klucz główny
* rok_produkcji - typ roku, domyślnie 1654
* id_wykonawcy - klucz obcy do tabeli postać
* zawartosc - ciąg znaków
* dodatek - ciąg znaków - domyślnie papryczka chilli
* id_konsumenta - klucz obcy do tabeli postać
2. Wstaw bigos z papryczką chilli do tabeli przetwory.

**b) Wykonanie:**

Tworzenie tabeli przetwory:
~~~mysql
CREATE TABLE przetwory (id_przetworu INT AUTO_INCREMENT PRIMARY KEY, rok_produkcji YEAR DEFAULT 1654, id_wykonawcy INT, zawartosc VARCHAR(255), dodatek VARCHAR(255) DEFAULT 'papryczka chilli', id_konsumenta INT, FOREIGN KEY (id_wykonawcy) REFERENCES postac(id_postaci), FOREIGN KEY (id_konsumenta) REFERENCES postac(id_postaci));
~~~
Wstawienie bigosu z papryczką chilli do tabeli przetwory:
~~~mysql
INSERT INTO przetwory (rok_produkcji, id_wykonawcy, zawartosc, id_konsumenta) VALUES (2023, (SELECT id_postaci FROM postac WHERE nazwa = 'Bjorn'), 'bigos', (SELECT id_postaci FROM postac WHERE nazwa = 'Tesciowa'));
~~~
## Zadanie 5.
**a) Treść zadania:**
1. Wstaw 5 wikingów do tabeli postaci.
2. Stwórz tabelę statek z polami:
* nazwa statku - klucz główny
* rodzaj statku - typ wyliczeniowy
* data wodowania - typ daty
* max_ladownosc - liczba dodatnia
3. Dodaj dwa statki do tabeli.
4. Dodaj pola do tabeli postac:
* funkcja - ciąg znaków.
5. Zmień funkcję u Bjorna na kapitan.
6. Dodaj klucz obcy w tabeli postać odwołujący się do statku.
7. Powsadzaj wikingów oraz drozda na statki.
8. Usuń izbę spiżarnia z tabeli izba.
9. Usuń tabelę izba.

**b) Wykonanie:**

Wstawienie 5 wikingów do tabeli postac:
~~~mysql
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Akjurg', 'wiking', '1990-01-01', 30), ('Krunog', 'wiking', '1985-02-15', 35), ('Rijavik', 'wiking', '1980-03-20', 40), ('Kijan', 'wiking', '1975-04-25', 45), ('Kranyg', 'wiking', '1970-05-30', 50);
~~~
Tworzenie tabeli statek:
~~~mysql
CREATE TABLE statek ( nazwa_statku VARCHAR(255) PRIMARY KEY, rodzaj_statku ENUM('żaglowiec', 'okręt wojenny', 'łódź podwodna'), data_wodowania DATE, max_ladownosc INT UNSIGNED);
~~~
Dodanie dwóch statków do tabeli statek:
~~~mysql
INSERT INTO statek (nazwa_statku, rodzaj_statku, data_wodowania, max_ladownosc) VALUES ('Rębacz', 'żaglowiec', '2000-01-01', 100), ('Krwiożerca', 'okręt wojenny', '2010-02-15', 500);
~~~
Dodanie pola funkcja do tabeli postac:
~~~mysql
ALTER TABLE postac ADD COLUMN funkcja VARCHAR(255);
~~~
Przypisanie wikingów oraz Drozda do statków:
~~~mysql
UPDATE postac SET id_statku = 'Rębacz' WHERE rodzaj = 'wiking';
UPDATE postac SET id_statku = 'Krwiożerca' WHERE nazwa = 'Drozd';
~~~
Usunięcie izby spiżarnia z tabeli izba:
~~~mysql
DELETE FROM izba WHERE nazwa_izby = 'spiżarnia';
~~~
Usunięcie tabeli izba:
~~~mysql
DROP TABLE izba;
~~~
