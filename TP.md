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
  
