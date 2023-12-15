# Bazy danych
Zadania tu przedstawione zostały wykonane na podstawie tego pliku: 
https://github.com/kropiak/bazy_inf/blob/573cc6a82dde109acd27761d9c15a86a53e7661e/lab_05/lab_05.pdf

##  Zadanie 1.
**a) Treść zadania:**

1. Uśmierć dwóch najstarszych wikingów, ale to nie może być Bjorn (DELETE z WHERE).
2. Usuń klucz główny z tabeli postac - może być potrzebnych kilka komend (np. odłączenie kluczy
obcych).

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
ALTER TABLE przetwory DROP FOREIGN KEY przetwory_ibfk_2;
ALTER TABLE postac DROP PRIMARY KEY;
~~~
## Zadanie 2.
**a) Treść zadania:**

1. Do tabeli postać dodaj pole pesel składające się z 11 znaków i ustaw to pole na klucz główny.
2. W tabeli postać zmień pole rodzaj, tak, aby możliwe było dodanie syreny.
3. Wstaw do tabeli syrenę o nazwie Gertruda Nieszczera.

**b) Wykonanie:**

Dodawania pola pesel do tabeli postac:
~~~mysql
ALTER TABLE postac ADD COLUMN pesel CHAR(11) FIRST;
ALTER TABLE postac ADD PRIMARY KEY (pesel);
~~~
Dodanie rodzaju syrena do tabeli postać:
~~~mysql
ALTER TABLE postac CHANGE rodzaj rodzaj ENUM('wiking','ptak','kobieta','syrena') DEFAULT NULL;
~~~
Wstawianie syreny:
~~~mysql
INSERT INTO postac VALUES(21029283299, 9, 'Gertruda Nieszczera', 'syrena', '1940-11-11', 14, DEFAULT, DEFAULT);
~~~

## Zadanie 3.
**a) Treść zadania:**

1. Wszystkie postacie, które mają w swojej nazwie 'a', wsadź na statek Bjorna.
2. Zmniejsz ładowność wszystkim statkom o 30%, których data wodowania była w XX wieku.
3. Ustaw warunek sprawdzający czy wiek postaci nie jest większy od 1000.

**b) Wykonanie:**

Wstawianie postaci z "a" w nazwie na statek Bjorna:
~~~mysql
UPDATE postac SET statek = 'Rębacz' WHERE nazwa LIKE '%a%';
~~~
Zmniejszanie ładowności statku w zależności od daty wodowania:
~~~mysql
UPDATE statek SET max_ladownosc = max_ladownosc * 0.7 WHERE YEAR(data_wodowania) BETWEEN 1901 AND 2000;
~~~
Sprawdzanie wieku postaci:
~~~mysql
ALTER TABLE postac ADD CHECK(wiek < 1000);
~~~

## Zadanie 4.
**a) Treść zadania:**

1. Do postaci dodaj węża Loko, tylko nie wsadzaj go na statek.
2. Stwórz nową tabelę na podstawie tabeli Postacie (dokładnie takie same pola), nazwij ją Marynarz -
wrzuć do tej tabeli wszystkie postacie które mają zdefiniowany statek.
3. Dostosuj odpowiednio klucze główne i obce.

**b) Wykonanie:**

Dodawanie węża:
~~~mysql
ALTER TABLE postac CHANGE rodzaj rodzaj ENUM('wiking', 'ptak', 'kobieta', 'syrena', 'waz') DEFAULT NULL;
INSERT INTO postac VALUES(38294823493, 10, 'Loko', 'waz', '1932-11-10', 100, DEFAULT, DEFAULT);
~~~
Tworzenie tabeli marynarz:
~~~mysql
CREATE TABLE marynarz LIKE postac;
INSERT INTO marynarz SELECT * FROM postac WHERE STATEK IS NOT NULL;
~~~
Dostosowanie kluczy:
~~~mysql
ALTER TABLE marynarz ADD FOREIGN KEY (statek) REFERENCES statek(nazwa_statku);
~~~

## Zadanie 5.

**a) Treść zadania:**

1. Wysadź wszystkich ze statku.
2. Uśmierć jednego wikinga.
3. Zniszcz wszystkie statki.
4. Usuń tabelę statek.
5. Utwórz tabelę zwierz z polami id - klucz główny samo zwiększający się, nazwa - ciąg znaków, wiek -
liczba.
6. Przekopiuj z tabeli postac wszystkie zwierzaki.

**b) Wykonanie:**

Wysadzanie:
~~~mysql
UPDATE postac SET id_statku = NULL;
~~~
Uśmiercanie jednego wikinga:
~~~mysql
DELETE FROM postac WHERE rodzaj = 'wiking' LIMIT 1;
~~~
Niszczenie wszystkich statków:
~~~mysql
UPDATE postac SET id_statku = NULL WHERE id_statku IS NOT NULL;
DELETE FROM statek;
~~~
Usuwanie tabeli statek:
~~~mysql
DROP TABLE statek;
~~~~
Tworzenie tabeli zwierz:
~~~mysql
CREATE TABLE zwierz (id INT AUTO_INCREMENT PRIMARY KEY, nazwa VARCHAR(255), wiek INT);
~~~
Kopiowanie zwierzaków z tabeli postac:
~~~mysql
INSERT INTO zwierz SELECT id_postaci, nazwa, wiek FROM postac WHERE rodzaj = 'ptak';
INSERT INTO zwierz SELECT id_postaci, nazwa, wiek from postac WHERE rodzaj = 'wąż';
~~~
