# TP3
PostgreSQL 11.2  

## REQUETES:  
1. Sans index:
``` sql
EXPLAIN SELECT * FROM realisateur where ID=280
```
![Img B_Q1](https://github.com/Neexos/BDD/blob/master/img/B_Q1.PNG)  

``` sql
CREATE UNIQUE INDEX IDX_REALISATEUR_ID ON REALISATEUR(ID)
```
![Img B_Q2](https://github.com/Neexos/BDD/blob/master/img/B_Q2.PNG)  

