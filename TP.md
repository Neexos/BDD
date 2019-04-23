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
	
## 8
  Quels sont les cinémas indépendants Bordelais, avec le film correspondant, qui passent un film de Woody Allen à partir de 22 heures dans une salle d'au moins 100 places et d'écran de taille supérieure à 30 m2:  
  **SELECT** *  
  **FROM** cinema  
  **JOIN** salle  
  **ON** salle.numcinema = cinema.numcinema  
  **JOIN** programmation  
  **ON**(programmation.numcinema, programmation.numsalle) = (salle.numcinema, salle.numsalle)  
  **JOIN** film  
  **ON** film.numfilm = programmation.numfilm  
  **JOIN** personne  
  **ON** personne.numpersonne = film.realisateur  
  **WHERE** cinema.ville='Bordeaux' **AND** cinema.compagnie='indep' **AND** salle.taille_ecran > 30 **AND** salle.nbplaces > 100 **AND** personne.nom='Allen' **AND** programmation.horaire >= '22:00'  

## 9
  Afficher tous les genres de film et les titres des films associés à chaque genre (jointure externe à gauche ou à droite):  
  **SELECT** libellegenre, titre **FROM** genre  
  **LEFT OUTER JOIN** film **ON** film.genre = genre.numgenre  
  
## 10
  Afficher les cinémas dont les salles n’ont pas été saisies dans la base:
  **SELECT** cinema.nom **FROM** cinema
  **LEFT JOIN** salle **ON** cinema.numcinema = salle.numcinema
  **WHERE** salle.numcinema IS NULL

## 11
  Ajouter la salle N°1 de 150 places, avec 1 écran de 40 m2 au cinéma Decavision (et non pas au cinéma N° 5...):  
  **INSERT INTO** salle **SELECT** cinema.numcinema, '1' **AS** numsalle, '40' **AS** taille_ecran, '150' **AS** nbplaces **FROM** cinema **WHERE** cinema.nom='Decavision'
  
## 12
  Afficher  les  programmations  dans  toutes les  salles  de tous les  cinémas  (même  si la  salle n’a pas de programmation) :  
  **SELECT** cinema.nom,salle.numsalle,film.titre,programmation.date_deb,programmation.date_fin,horaire,prix  
  **FROM** programmation  
  **RIGHT OUTER JOIN** salle **ON** (salle.numcinema,salle.numsalle) = (programmation.numcinema,programmation.numsalle)  
  **LEFT OUTER JOIN** cinema **ON** cinema.numcinema = salle.numcinema  
  **LEFT OUTER JOIN** film **ON** programmation.numfilm = film.numfilm  
  
## 13
  Quel est le total des salaires des acteurs du film «Jurassic Parc»:  
  **SELECT SUM**(salaire) **FROM** distribution  
  **JOIN** film **ON** film.numfilm = distribution.numfilm  
  **JOIN** acteur **ON** acteur.numacteur = distribution.numacteur  
  **WHERE** film.titre='Jurassic Parc'  
  
## 14
  Donner le nombre de films par genre:  
  **SELECT** genre.libellegenre, COUNT(\*) **FROM** genre  
  **JOIN** film **ON** film.genre = genre.numgenre  
  **GROUP BY** libellegenre  
  
## 15 
  Supprimer de la table Genre ceux qui ne sont relatifs à aucun film:  
  
