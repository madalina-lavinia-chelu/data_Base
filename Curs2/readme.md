
# Limbajul de Manipulare a Datelor

>#### 1.1. Crearea înregistrărilor  Instrucțiunea INSERT

      Pentru a adăuga înregistrări într-o tabelă se folosește instrucțiunea INSERT. Aceasta are mai multe variații, pentru a adăuga una sau mai multe înregistări deodata, pentru a da valori doar pentru unele atribute, sau pentru toate atributele dintr-o tabelă.

>> Sintaxa generală: 

>>**INSERT INTO <nume_tabelă> VALUES ()**


 
 
VARIAȚII:

* adăugarea unei înregistrări, specificând valori pentru toate atributele 

[pentru exemple, se va folosi tabela produs(id, denumire, pret, descriere)]

```sql
INSERT INTO <nume_tabelă> VALUES(<v1>, <v2>, ..., <vn>);
INSERT INTO produs VALUES(NULL, 'laptop', 3500, 'laptop lenovo cu 8Gb RAM si 512Gb SSD');
-- se adaugă NULL pentru id, pentru a se aplica AUTO_INCREMENTUL
```

* adăugarea unei înregistrări, specificând valori doar pentru anumite atribute

```sql
INSERT INTO <nume_tabelă>(<a1>, <a2>, ..., <an>) VALUES(<v1>, <v2>, ..., <vn>);
INSERT INTO produs(denumire, pret) VALUES('laptop', 3500); 
```

* adăugarea mai multor înregistrări, specificând valori pentru toate atributele

```sql
INSERT INTO <nume_tabelă> 
VALUES
(<v1>, <v2>, ..., <vn>),
(<v1>, <v2>, ..., <vn>);
INSERT INTO produs 
VALUES
(NULL, 'laptop', 3500, 'laptop lenovo cu 8Gb RAM si 512Gb SSD'),
(NULL, 'casti', 150, 'casti wireless');```

* adăugarea mai multor înregistrări, specificând valori doar pentru unele atribute

```sql
INSERT INTO <nume_tabelă>(<a1>, <a2>, ..., <an>) 
VALUES
(<v1>, <v2>, ..., <vn>),
(<v1>, <v2>, ..., <vn>);
INSERT INTO produs(denumire, pret) 
VALUES
('laptop', 3500),
('casti', 150);```

* adăugarea unei înregistrări, folosind instrucțiunea SET

```sql
INSERT INTO <nume_tabelă>
SET <a1> = <v1>, <a2>=<v2>;
INSERT INTO produs
SET denumire = 'laptop', pret = 3500;```

* adăugarea unei înregistrări, doar cu valori default/NULL

```sql
INSERT INTO <nume_tabelă> VALUES();
INSERT INTO produs VALUES();```


 
 
>#### 1.2 Modificarea înregistrărilor - Instrucțiunea UPDATE 

      Pentru actualizarea unei înregistrări dintr-o tabelă, se folosește instrucțiunea UPDATE. Folosind această instrucțiune, se pot actualiza oricâte atribute deodată, iar înregistrările vizate de update vor fi filtrate cu ajutorul clauzei WHERE.


 
Condițiile de pe clauza WHERE sunt optionale, dar dacă este adăugată nicio condiție, se vor actualiza toate înregistrările din tabelă.

VARIAȚII:

* update general

```sql
UPDATE <nume_tabelă> SET <atribut> = <valoare>;
UPDATE produs SET reducere = 10;
UPDATE produs SET pret = pret * 0.90;
-- se aplică o reducere de 10% pentru toate produsele```

* update cu condiții

```sql
UPDATE <nume_tabelă> SET <atribut> = <valoare> WHERE <condiții>; 
UPDATE produs SET pret = pret*0.90 WHERE denumire = 'laptop';
UPDATE produs SET nume = 'laptop', descriere = 'laptop lenovo' WHERE id = 5;
```
 

Restricții
> La actualizarea înregistrărilor, trebuie urmate câteva reguli simple:

* condiția după care se face actualizarea ar trebui să conțină un atribut cu valori unice; astfel se evită actualizarea mai multor înregistrări, din greșeală
    * ex: se face actualizarea prețului unui produs după condiția denumire = 'laptop' - sunt șanse foarte mari să existe mai multe produse care îndeplinesc această condiție, așa că vor fi actualizate toate aceste produse; dacă se voia de fapt actualizarea unui anume produs, aceasta ar fi trebuit făcută dupa condiția id = <id_produs> - la actualizarea după Primary Key, este clar că se actualizează un singur produs 
    * implicit, MySQL are o setare care nu permite actualizările după atribute non-unice, tocmai pentru a veni cu o atenționare în plus, în cazul în care utilizatorul dorește să facă astfel de operații - această setare se numește safe updates și se activează/dezactivează setând *sql_safe_updates* cu valoarea 1-true/0-false: **SET SQL_SAFE_UPDATES = 0;**
* nu se recomană actualizarea cheii primare (fie ea adăgată manual sau cu AUTO_INCREMENT)


 
 
>#### 1.3. Crearea înregistrărilor - Instrucțiunea DELETE

      Pentru a șterge înregistrări dintr-o tabelă, se folosește instrucțiunea DELETE. Aceasta se poate folosi împreună cu clauza WHERE pentru a șterge anumite înregistrări, sau fără condiții pentru a șterge toate înregistrările.
> Dacă se șterge o anumită înregistrare, id-ul acesteia nu va mai fi refolosit automat de către AUTO_INCREMENT, dar poate fi asociat manual unei înregistrări. De asemenea, dacă se șterg toate înregistrările, AUTO_INCREMENT-ul nu se resetează automat. Totuși, acesta poate fi resetat cu instrucțiunea ALTER TABLE <nume_tabelă> AUTO_INCREMENT = <valoare>
    
>> Sintaxa generală: 

>>** DELETE FROM <nume_tabelă> WHERE <condiții>**

VARIAȚII:

* ștergerea tuturor înregistrărilor

```sql
DELETE FROM <nume_tabelă>
DELETE FROM produs;
ALTER TABLE produs AUTO_INCREMENT = 1; 
-- resetare auto increment, pentru ca noile înregistrări să înceapă iar de la 1```

* ștergerea unor anumite înregistrări

```sql
DELETE FROM <nume_tabelă> WHERE <condiții>
DELETE FROM produs WHERE denumire = 'laptop'; 
-- nu se știe câte înregistrări vor fi șterse, pentru că nu se pune condiția după atribut unic
DELETE FROM produs WHERE id = 2;
-- se șterge o singură înregistrare, id-ul cu valoarea 2 nu va mai fi refolosit automat
```
 

Restricții

> La ștergerea înregistrarilor dintr-o tabelă, se ține cont de următoarele reguli:

* nu se poate șterge o înregistrare de care depinde un Foreign Key
* nu se poate rula DELETE fără condiții dacă există cel puțin o înregistrare în acea tabelă de care depinde un Foreign Key
* o altă instrcțiune pentru a șterge toate datele dintr-o tabelă este TRUNCATE <nume_tabelă> - aceasta șterge si re-crează tabela (se resetează și AUTO_INCREMENT) - dar nu poate fi folosită pentru o tabelă care este referențiată de o altă tabelă (nu contează daca există sau nu înregistrări adăugate, ci doar structura tabelelor)

![plot](C:\Users\Lavinia\Desktop\data_Base\Curs2\delete_r.png)
 


 
 
>#### 1.3. Crearea înregistrărilor - Instrucțiunea SELECT

      Pentru a prelua înregistrări dintr-o tabelă se folosește instrucțiunea SELECT. În aceasta se specifică ce atribute se preiau, iar dacă se dorește preluarea tuturor atributelor, se folosește caracterul * (se citește ALL). Această instrucțiune este întotdeauna urmată de clauza FROM, unde se specifică tabela din care se preiau datele.

    

VARIAȚII:

* selectarea tuturor atributelor

```sql 
SELECT * FROM <nume_tabelă>;
SELECT * FROM produs;```

* selectarea unor anumite atribute

```sql 
SELECT <a1>, <a2>, ..., <an> FROM <nume_tabelă>;
SELECT denumire, pret FROM produs;```

* asocierea unui alias pentru atribute - dacă se dorește ca la afișare să aiba un alt nume

```sql 
SELECT <a1> AS <alias1>, <a2>, <a3> AS <alias3> FROM <nume_tabelă>; 
-- nu toate atributele să aibă un alias, iar cuvântul cheie AS se poate omite
SELECT denumire AS nume_produs, pret `pret recomandat`, descriere FROM produs; 
-- dacă se omite AS se pune doar un spațiu între atribut și alias, iar dacă alias-ul conține spații, acesta se pune între backticks ``(apostroafe înclinate)```
 


 
 
>#### 1.3. Crearea înregistrărilor - Filtrarea datelor

      Instrucțiunea SELECT conține de multe ori și clauza WHERE folosită pentru a filtra datele.

      Pe această clauză se pot adăuga oricâte filtrări conectate prin AND (și), respectiv OR (ori). Atunci când se face o combinație de AND/OR, AND-ul are prioritate, iar daca se dorește efectuarea filtrărilor într-o anumită ordine, se folosesc paranteze rotunde. 



```sql
SELECT <a1>, <a2>, ..., <an>
FROM <nume_tabelă>
WHERE <exp1> AND/OR <exp2> ... AND/OR <expn>;```


Principalele operații și filtrări ce se pot face pe where sunt:

* verificare cu operatori de comparare < > <= >= != 
    * pret >= 100  denumire != 'lenovo' (!= - diferit)

* verificare cu operatorii BETWEEN, IN
    * pret BETWEEN 100 AND 500 - echivalent cu pret >= 100 AND pret <= 500
    
    * pret IN (100, 500) - pretul are valoarea 100 sau 500, nu mai este definit un interval - echivalent cu pret = 100 || pret = 500

* verificare cu LIKE - pentru a verifica daca un șir de caractere respectă un anumit șablon: se folosește % pentru "0 sau mai multe caractere", _ pentru "fix un caracter"
    * denumire LIKE '%laptop%' - denumire contine laptop - denumiri valide: laptop, laptop lenovo, suport laptop, suport laptop 13 inch
    
    * denumire LIKE '% lenovo' - denumirea se termina cu lenovo - denumiri valide: laptop lenovo, mouse lenovo
    
    * denumire LIKE 'l%' - denumirea incepe cu litera l - denumiri valide: laptop, lenovo
    
    * denumire LIKE '_t%' - denumirea are a doua litera t - denumiri valide: it, stereo

* verificare nu NULL/NOT NULL

    * descriere IS NULL - atributul descriere nu are o valoare
    
    * descriere IS NOT NULL - atributul descriere este neapărat completat
    
Pe lângă SELECT - FROM - WHERE se mai pot adăuga următoarele clauze, în ordinea listată:

* ORDER BY - se ordonează înregistrările după unul sau mai multe atribute

    * direcția implicită este ASC, pentru ordonare descendentă se folosește DESC  
    
    * dacă se speficiă mai multe atribute în ORDER BY, se face ordonarea după primul, iar daca sunt mai multe înregistrări cu valori identice pentru primul atribut, se face ordonarea după al doilea, șamd
    
* LIMIT - limitează numărul de rezultate

    * LIMIT n - se preiau doar primele n rezultate
    
    * LIMIT n, m - se sare peste primele n rezultate și se preiau următoarele m


Exemplu query valid:

```sql
SELECT denumire, pret 
FROM produs
WHERE denumire LIKE '%laptop%' AND pret <= 5000
ORDER BY pret DESC, denumire
LIMIT 10;

-- se preia denumirea și prețul primelor 10 produse, care conțin în denumire laptop și au prețul mai mic sau egal cu 5000. acestea sunt ordonate după preț descendent, iar dacă au același preț, sunt ordonate după denumire, ascendent```