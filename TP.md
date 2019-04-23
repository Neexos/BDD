# TP 1
## 1
  Afficher toutes les personnes habitant sur une avenue:  
  **SELECT** prenom,nom,adresse **FROM** personne **WHERE** adresse like '%Avenue%'

## 2
  Affecter comme numéro de téléphone la valeur «N’a pas d’amis» à toutes les personnes qui n’ont pas renseigné leur numéro de téléphone:  
  **SELECT** * **FROM** personne **WHERE** telephone **IS** NULL
