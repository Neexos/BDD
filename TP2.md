# TP2
## 1
```drop table if exists DETAILEMPRUNT cascade;

drop table if exists EDITEUR cascade;

drop table if exists EMPRUNT cascade;

drop table if exists EXEMPLAIRE cascade;

drop table if exists GENRE cascade;

drop table if exists MEMBRE cascade;

drop table if exists OUVRAGE cascade;

/*==============================================================*/
/* Table : DETAILEMPRUNT                                        */
/*==============================================================*/
create table DETAILEMPRUNT 
(
   NUM_EMPRUNT          NUMERIC(10)           not null,
   NUMERO               INTEGER               not null,
   ISBN                 NUMERIC(10)           not null,
   NUM_EXEMPLAIRE       NUMERIC(3)            not null,
   RENDULE              DATE,
   constraint PK_DETAILEMPRUNT primary key (NUM_EMPRUNT, NUMERO),
   constraint UQ_DETAILEMPRUNT unique (NUM_EMPRUNT, ISBN, NUM_EXEMPLAIRE)
);

/*==============================================================*/
/* Table : EDITEUR                                              */
/*==============================================================*/
create table EDITEUR 
(
   NUM_EDITEUR          INTEGER              not null,
   NOM_EDITEUR          VARCHAR(80)          not null,
   constraint PK_EDITEUR primary key (NUM_EDITEUR)
);

/*==============================================================*/
/* Table : EMPRUNT                                              */
/*==============================================================*/
create table EMPRUNT 
(
   NUM_EMPRUNT          NUMERIC(10)           not null,
   NUM_MEMBRE           INTEGER           not null,
   CREELE               DATE                  default current_date not null, -- ATTENTION default current_date se met avant not null
   constraint PK_EMPRUNT primary key (NUM_EMPRUNT)
);

/*==============================================================*/
/* Table : EXEMPLAIRE                                           */
/*==============================================================*/
create table EXEMPLAIRE 
(
   ISBN                 NUMERIC(10)           not null,
   NUM_EXEMPLAIRE       NUMERIC(3)            not null,
   CODE_ETAT            CHAR(5)               not null,
   constraint PK_EXEMPLAIRE primary key (ISBN, NUM_EXEMPLAIRE),
   constraint CK_EXEMPLAIRE_CODE_ETAT check (CODE_ETAT IN ('NE','BO','MO','MA'))
);

/*==============================================================*/
/* Table : GENRE                                                */
/*==============================================================*/
create table GENRE 
(
   CODE_GENRE           CHAR(5)              not null,
   LIBELLE_GENRE        VARCHAR(80)          not null,
   constraint PK_GENRE primary key (CODE_GENRE)
);

/*==============================================================*/
/* Table : MEMBRE                                               */
/*==============================================================*/
create table MEMBRE 
(
   NUM_MEMBRE           SERIAL,
   NOM                  VARCHAR(80)           not null,
   PRENOM               VARCHAR(80)           not null,
   RUE                  VARCHAR(100)          not null,
   CP                   CHAR(5)               not null,
   VILLE                VARCHAR(50)           not null,
   TELEPHONE            CHAR(10),
   ADHESION             DATE                  not null,
   DUREE                NUMERIC(2)            not null,
   constraint PK_MEMBRE primary key (NUM_MEMBRE),
   constraint CK_MEMBRES_DUREE check (DUREE>0)
);

/*==============================================================*/
/* Table : OUVRAGE                                              */
/*==============================================================*/
create table OUVRAGE 
(
   ISBN                 NUMERIC(10)           not null,
   CODE_GENRE           CHAR(5)               not null,
   NUM_EDITEUR          INTEGER               not null,
   TITRE                VARCHAR(80)           not null,
   AUTEUR               VARCHAR(80),
   constraint PK_OUVRAGE primary key (ISBN)
);

alter table DETAILEMPRUNT
   add constraint FK_DETAILEMPRUNT_EMPRUNT foreign key (NUM_EMPRUNT)
      references EMPRUNT (NUM_EMPRUNT);

alter table DETAILEMPRUNT
   add constraint FK_DETAILEMPRUNT_EXEMPLAIRE foreign key (ISBN, NUM_EXEMPLAIRE)
      references EXEMPLAIRE (ISBN, NUM_EXEMPLAIRE);

alter table EMPRUNT
   add constraint FK_EMPRUNT_MEMBRE foreign key (NUM_MEMBRE)
      references MEMBRE (NUM_MEMBRE);

alter table EXEMPLAIRE
   add constraint FK_EXEMPLAIRE_OUVRAGE foreign key (ISBN)
      references OUVRAGE (ISBN);

alter table OUVRAGE
   add constraint FK_OUVRAGE_CODE_GENRE foreign key (CODE_GENRE)
      references GENRE (CODE_GENRE);

alter table OUVRAGE
   add constraint FK_OUVRAGE_EDITEUR foreign key (NUM_EDITEUR)
      references EDITEUR (NUM_EDITEUR);
```

## Question 2  
  Les membres sont très nombreux et si certains fréquentent souvent la bibliothèque, d'autres au contraire ne renouvellent pas leur adhésion tous les ans. Pour ces derniers, il n'est pas souhaitable d'avoir des informations en double dans la base. Aussi il ne doit pas être possible d'avoir deux membres qui possèdent mêmes nom, prénom et numéro de téléphone fixe. Définissez une contrainte afin de satisfaire cette nouvelle exigence. La contrainte sera ajoutée sur la table des membres (instruction alter table).   
  
  **ALTER TABLE** membre  
  **ADD CONSTRAINT** num_membre **UNIQUE** (nom,prenom,telephone)  
  
## Question 3  
  De plus en plus de membres possèdent deux numéros de téléphone : un pour le poste fixe de leur domicile et un pour leur téléphone portable. Or, la base nous permet de stocker un seul numéro de téléphone. Apportez les modifications de structure nécessaires pour prendre en compte cette modification.Comme cette nouvelle colonne, nommée MOBILE, va contenir des informations relatives à un numéro de téléphone portable, mettez en place une contrainte d'intégrité afin de vous assurer que le numéro de téléphone saisi commence bien par 06 ou par 07 (contrainte de type check. Utilisez des fonctions de manipulation de chaines et notamment la commande like)  
  
  **ALTER TABLE** membre  
  **ADD COLUMN** MOBILE **VARCHAR(10) CHECK**(mobile **LIKE** '^0[6-7]') **UNIQUE**  
  
## Question 4  
  Afin d'améliorer les performances d'accès aux données, définissez un index sur toutes les colonnes de type clé étrangère (les opérations de jointure seront plus rapides; nous y reviendrons lors d’un prochain TP sur l’optimisation).  
  
  **CREATE INDEX** index_membre **ON** emprunt (num_membre)  
  **CREATE INDEX** index_detail **ON** detailemprunt (num_emprunt, isbn, num_exemplaire)  
  **CREATE INDEX** index_ouvrage **ON** ouvrage (code_genre,num_editeur)  
  
  
## Question 5  
  À l'usage, on se rend compte que lorsque l'on souhaite supprimer une fiche d'emprunt, il faut nécessairement supprimer toutes les lignes présentes dans la table DetailEmprunt qui font référence à la ligne de la table Emprunt que l'on souhaite supprimer. Modifiez le comportement de la contrainte de clé étrangère en cas de suppression de la ligne maîtrepour rendre automatique une telle suppression.  
  
  **ALTER TABLE** detailemprunt  
   **ADD CONSTRAINT** num_emprunt   
	**FOREIGN KEY** (num_emprunt)  
	**REFERENCES** emprunt (num_emprunt)  
	**ON DELETE CASCADE**  
  
## Question 6  
  Modifiez la table des exemplaires afin que la colonne code_etat prenne par défaut la valeur "NE" ce qui signifie que l'état d'un nouvel exemplaire est par défaut neuf (clause DEFAULT; cf. commande ALTER TABLE).  
  
  **ALTER TABLE** exemplaire 
	**ALTER** code_etat **SET DEFAULT** 'NE'
  
## Question 7  
  La table DetailEmprunt n'est pas bien nommée. Le nom Detail semble préférable. Renommez la table afin de prendre en compte cette nouvelle exigence (cf. commande ALTER TABLE).  
  
  **ALTER TABLE** detailemprunt **RENAME TO** detail
  
