
# Limbajul de Manipulare a Datelor

>#### 1.1. Crearea înregistrărilor  Instrucțiunea INSERT

> Pentru a adăuga înregistrări într-o tabelă se folosește instrucțiunea INSERT. Aceasta are mai multe variații, pentru a adăuga una sau mai multe înregistări deodata, pentru a da valori doar pentru unele atribute, sau pentru toate atributele dintr-o tabelă.

>> Sintaxa generală: **INSERT INTO <nume_tabelă> VALUES ()**



VARIAȚII:
* adăugarea unei înregistrări, specificând valori pentru toate atributele [pentru exemple, se va folosi tabela produs(id, denumire, pret, descriere)]

```sql
INSERT INTO <nume_tabelă> VALUES(<v1>, <v2>, ..., <vn>);
INSERT INTO produs VALUES(NULL, 'laptop', 3500, 'laptop lenovo cu 8Gb RAM si 512Gb SSD'); -- se adaugă NULL pentru id, pentru a se aplica AUTO_INCREMENTUL```
