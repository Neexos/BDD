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
