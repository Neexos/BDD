# TP 1
## 1
  Afficher toutes les personnes habitant sur une avenue:  
  **SELECT** prenom,nom,adresse **FROM** personne **WHERE** adresse **LIKE** '%Avenue%'

## 2
  Affecter comme numéro de téléphone la valeur «N’a pas d’amis» à toutes les personnes qui n’ont pas renseigné leur numéro de téléphone:  
  **SELECT** * **FROM** personne **WHERE** telephone **IS** NULL

## 3
  Afficher les noms des réalisateurs habitant une ville commençant par la lettre ’N’:  
  **SELECT DISTINCT** prenom,nom,ville **FROM** personne **JOIN** film **ON** numpersonne = realisateur **WHERE** ville **LIKE** 'N%'
  
## 4
  Afficher tous les films (TITRE, ANNEE, REALISATEUR) qui n’ont pas été réalisés par Spielberg:  
  **SELECT** titre,annee,nom **FROM** film **JOIN** personne **ON** realisateur = numpersonne **WHERE** realisateur **NOT IN** (**SELECT** numpersonne **FROM** personne **WHERE** nom='Spielberg')

## 5
  Afficher pour chaque réalisateur (nom, prénom) et chaque film (titre) son salaire à la minute de film:  
  **SELECT** nom,prenom,titre,(salaire_real/longueur) **FROM** film **LEFT OUTER JOIN** personne **ON** realisateur=numpersonne

## 6
  Afficher  pour  chaque film, les nom et prénom des  acteurs  et leur  salaire  (afficher  le  titre  du film par ordre alphabétique et le salaire par ordre décroissant):  
  **SELECT** film.titre, personne.nom, personne.prenom, distribution.salaire **FROM** film **JOIN** distribution **ON** film.numfilm = distribution.numfilm **JOIN** acteur **ON** distribution.numacteur = acteur.numacteur **JOIN** personne **ON** personne.numpersonne = acteur.numpersonne **ORDER BY** titre **ASC**, salaire **DESC**;
  
## 7
  Quels sont les acteurs dramatiques (nom, prénom) qui ont joué dans un film de Spielberg:  
  **SELECT** test.prenom, test.nom  
  **FROM** film  
  **JOIN** distribution  
	  **ON** distribution.numfilm = film.numfilm  
  **JOIN** acteur  
	  **ON** acteur.numacteur = distribution.numacteur  
  **JOIN** genre  
	  **ON** genre.numgenre = acteur.specialite  
  **JOIN** personne AS "test"  
	  **ON** test.numpersonne = acteur.numpersonne  
  **JOIN** personne AS "real"  
	  **ON** film.realisateur = real.numpersonne  
	**WHERE** genre.libellegenre='Drame'  
	**AND** real.nom='Spielberg'  
