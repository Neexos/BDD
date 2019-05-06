# TP3
PostgreSQL 11.2  

## Contributors
 - MEYNET Léo
 - PETIT Alexandre

## REQUETES:  
### 1  
a. Sans index:
``` sql
EXPLAIN SELECT * FROM realisateur where ID=280
```
![Img B_Q1](https://github.com/Neexos/BDD/blob/master/img/B_Q1.PNG)  
Coût de la requête compris entre 0 et 112,70.  

b. Création d'un index sur l'id réalisateur:  
``` sql
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID);
EXPLAIN SELECT * FROM realisateur where ID=280;
```
![Img B_Q2](https://github.com/Neexos/BDD/blob/master/img/B_Q2.PNG)  

La requête coûte désormais entre 0,28 et 8,30 !!!  
Cette différence s'explique car lors de la première requête, il faut balayer toute la table realisateur (SEQ SCAN) puis trouver l'ID=280. Après ajout d'un index, la recherche est plus rapide car il sait où se trouve la donnée (indexée) et réalise un INDEX SCAN.  


c. Suppression de l'index:  
``` sql 
DROP INDEX IDX_REALISATEUR_I
```

### 2
a. Sans index:  
``` sql 
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM ='spielberg
```
![Img B_Q3](https://github.com/Neexos/BDD/blob/master/img/B_Q3.PNG)  
Coût de la requête compris entre 0 et 127,64  

b. Création d'un index sur l'id réalisateur:  
``` sql
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID);
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM='spielberg';
```
![Img B_Q4](https://github.com/Neexos/BDD/blob/master/img/B_Q4.PNG)  6.
La requête coûte désormais entre 0,28 et 8,30 !!!  
idem que précédemment.

c. Ajout d'un  index  non  unique  sur  NOM (il  peut  y  avoir  des  doublons!):  
``` sql
CREATE INDEX IDX_REALISATEUR_NOM ON REALISATEUR(NOM);
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM='spielberg';
``` 
![Img B_Q5](https://github.com/Neexos/BDD/blob/master/img/B_Q5.PNG)  
idem que précédemment malgré le fait d'avoir 2 index.

d. Suppression des index précédents:  
``` sql
DROP INDEX IDX_REALISATEUR_NOM;
DROP INDEX IDX_REALISATEUR_ID;
```  
e. Création de l'index sur NOM puis sur ID:  
``` sql
CREATE INDEX IDX_REALISATEUR_NOM ON REALISATEUR(NOM);
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID);
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM='spielberg';
```
![Img B_Q6](https://github.com/Neexos/BDD/blob/master/img/B_Q6.PNG)  
idem que précédemment.
![Img B_Q7](https://github.com/Neexos/BDD/blob/master/img/B_Q7.PNG)  
On voit que l'index sur le nom est plus gros d'une soixantaine de kB.

### 3
``` sql
SELECT * FROM realisateur where ID=2800 OR NOM ='spielberg';
```
#### a) Sans index
![Img 3_Qa](https://github.com/Neexos/BDD/blob/master/img/3_a.PNG)    
Le coût de la rêquete est compris entre 0 et 127,64

#### b) Création d'un index sur le réalisateur ID
![Img 3_Qa](https://github.com/Neexos/BDD/blob/master/img/3_a.PNG)  
Le coût est le même que celui sans index car la requête comporte un OR, le champs nom n'est pas indexé donc SEQ SCAN.

#### c) Ajout de l'ID nom sur la table réalisateur
![Img 3_Qc](https://github.com/Neexos/BDD/blob/master/img/3_c.PNG)  
Le coût est cette fois bien moins important; il est compris entre 8,58 et 15,23

### 4
```sql 
SELECT * FROM realisateur where ID>1000
```
#### a) Sans ID

![Img 4_Qa](https://github.com/Neexos/BDD/blob/master/img/4_a.PNG)  
Le coût est cette fois compris entre 0 et 112,7
#### b) Avec index sur réalisateur ID
![Img 4_Qa](https://github.com/Neexos/BDD/blob/master/img/4_a.PNG)  
Le coût est identique car le nombre de valeur à récupérer est important. On met finalement moins de temps avec un SEQ SCAN qu'avec un INDEX SCAN

### 5
```sql
SELECT * FROM realisateur where ID>4000;
```
#### a) Avec ID
![Img 5_Qa](https://github.com/Neexos/BDD/blob/master/img/5_a.PNG)  
On peut voir qu'avec un nombre de valeur à récupérer moins important, le SGBD utilise donc un INDEX SCAN

### 6
#### a) Après ajout de l'index unique IDX_TITRES(id_film, titre, langue)
```sql
CREATE  UNIQUE  INDEX IDX_TITRES ON TITRES(id_film, titre, langue);
SELECT * FROM TITRES WHERE titre = 'Char';
```
![Img 6_Qa](https://github.com/Neexos/BDD/blob/master/img/6_a.PNG)  
LE SGBD fait un SEQ SCAN avec un coût compris entre 0 et 395,09 (très long car l'index est composé de 3 champs et le premier champs est différent de celui utilisé dans la condition)

#### b) Après suppresion et ajout de l'index unique IDX_TITRES(titre, id_film, langue)
```sql
CREATE UNIQUE INDEX IDX_TITRES ON TITRES(titre, id_film, langue);
SELECT * FROM TITRES WHERE titre = 'Char';
```
![Img 6_Qb](https://github.com/Neexos/BDD/blob/master/img/6_b.PNG)  
LE SGBD fait un INDEX ONLY SCAN avec un coût compris entre 0,29 et 8,3 (c'est moins long car postgreSQL utilise seulement le premier champs "titre" lors du scan, et par chance ici la condition est faite sur le titre)

#### c) Après suppresion et ajout d'un index non unique
```sql
DROP INDEX IDX_TITRES;
CREATE INDEX IDX_TITRES_TITRE ON TITRES(titre);
```
![Img 6_Qc](https://github.com/Neexos/BDD/blob/master/img/6_c.PNG)  
idem que la question précèdente.

##### d) Synthèse
```sql
DROP INDEX IDX_TITRES_TITRE;
```
Il vaut mieux créer un index sur un seul champs car la plupart du temps c'est plus optimisé.

### 7
```sql
SELECT * FROM TITRES WHERE titre = 'Char' AND ID_FILM=1000;
```
#### a) Création d'un index unique
```sql
CREATE UNIQUE INDEX IDX_TITRES ON TITRES(id_film, titre, langue);
```
#### b) Plan d'éxécutiuon
```sql
SELECT * FROM TITRES WHERE titre = 'Char' ORID_FILM=1000;
```
