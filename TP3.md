# TP3
PostgreSQL 11.2  

## REQUETES:  
### A  
1. Sans index:
``` sql
EXPLAIN SELECT * FROM realisateur where ID=280
```
![Img B_Q1](https://github.com/Neexos/BDD/blob/master/img/B_Q1.PNG)  
Coût de la requête compris entre 0 et 112,70.  

2. Création d'un index sur l'id réalisateur:  
``` sql
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID);
EXPLAIN SELECT * FROM realisateur where ID=280;
```
![Img B_Q2](https://github.com/Neexos/BDD/blob/master/img/B_Q2.PNG)  

La requête coûte désormais entre 0,28 et 8,30 !!!  
Cette différence s'explique car lors de la première requête, il faut balayer toute la table realisateur (SEQ SCAN) puis trouver l'ID=280. Après ajout d'un index, la recherche est plus rapide car il sait où se trouve la donnée (indexée) et réalise un INDEX SCAN.  


3. Suppression de l'index:  
``` sql 
DROP INDEX IDX_REALISATEUR_I
```

### B  
1. Sans index:  
``` sql 
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM ='spielberg
```
![Img B_Q3](https://github.com/Neexos/BDD/blob/master/img/B_Q3.PNG)  
Coût de la requête compris entre 0 et 127,64  

2. Création d'un index sur l'id réalisateur:  
``` sql
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID);
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM='spielberg';
```
![Img B_Q4](https://github.com/Neexos/BDD/blob/master/img/B_Q4.PNG)  
La requête coûte désormais entre 0,28 et 8,30 !!!  
idem que précédemment.

3. Ajout d'un  index  non  unique  sur  NOM (il  peut  y  avoir  des  doublons!):  
``` sql
CREATE INDEX IDX_REALISATEUR_NOM ON REALISATEUR(NOM);
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM='spielberg';
``` 
![Img B_Q5](https://github.com/Neexos/BDD/blob/master/img/B_Q5.PNG)  
idem que précédemment malgré le fait d'avoir 2 index.

4. Suppression des index précédents:  
``` sql
DROP INDEX IDX_REALISATEUR_NOM;
DROP INDEX IDX_REALISATEUR_ID;
```  
5. Création de l'index sur NOM puis sur ID:  
``` sql
CREATE INDEX IDX_REALISATEUR_NOM ON REALISATEUR(NOM);
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID);
EXPLAIN SELECT * FROM realisateur where ID=2800 AND NOM='spielberg';
```
![Img B_Q6](https://github.com/Neexos/BDD/blob/master/img/B_Q6.PNG)  
idem que précédemment.
