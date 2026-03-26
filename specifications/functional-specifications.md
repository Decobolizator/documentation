# Functional specification
## Introduction 

Le but de ce document est d'expliciter les spécifications fonctionnelles envisagées et choisies au cours du projet Decobolizator. 

Decobolizator est un outil permettant de faciliter la compréhension de code COBOL via différentes représentations (langage naturel, pseudo-code, schémas).

---

## Hors périmètre (Out of scope)

Le système ne permet pas :
* D’exécuter du code COBOL
* De modifier automatiquement le code COBOL
* De garantir une exactitude parfaite des traductions (les résultats sont assistés par IA)

---

## Liste des fonctionnalités
* F1 : Traduire un fichier COBOL en langage naturel
* F1' : Traduire un projet (composé de plusieurs fichiers) COBOL en langage naturel
* F2 : Traduire du COBOL en pseudo-code
* F3 : Ajouter de la colorisation pour permettre une compréhension plus aisée du code
* F4 : Générer une représentation schématique du COBOL
* F5 : Créer un utilisateur 
* F6 : Se connecter à son compte 
* F7 : Supprimer son compte
* F8 : Accéder à l'historique de ses traductions
* F9 : Lancer la procédure "mot de passe oublié"
* F10 : Se déconnecter
* F11 : Consulter / télécharger une traduction

---

## Règles générales

* Un utilisateur non connecté peut effectuer jusqu’à 2 traductions
* Un utilisateur connecté peut effectuer un nombre illimité de traductions
* L’historique est limité aux 10 dernières traductions
* L’utilisateur ne peut accéder qu’à ses propres données
* Les résultats générés peuvent être incomplets ou partiels

---

## Détail des fonctionnalités 

---

### F1 : Traduire du COBOL en langage naturel
#### Acteur : 
Utilisateur

##### Description :
L'utilisateur peut traduire du code COBOL en langage naturel. La traduction est réalisée par section du code afin de faciliter la compréhension.

##### Input : 
* Code COBOL
* Fichier .cbl, .cob, .ccp

##### Output : 
* Fichier .md

##### Règles : 
* Le code doit être non vide
* Le fichier doit être dans un format supporté
* Si le code est ambigu ou incomplet, une réponse partielle est générée avec un avertissement
* Les noms techniques (variables, sections) doivent être conservés

#### Critères d’acceptation :
* Chaque section du code est expliquée
* La structure globale du programme est compréhensible
* Un avertissement est affiché en cas d’ambiguïté

---

### F1' : Traduire un projet COBOL
#### Acteur :
Utilisateur

#### Description :
L'utilisateur peut importer plusieurs fichiers COBOL constituant un projet. Le système analyse les dépendances (COPY, copybooks) et produit une traduction cohérente.

#### Input :
* Dossier contenant plusieurs fichiers COBOL

#### Output :
* Un fichier global
* Des fichiers séparés par module

#### Règles :
* Les dépendances doivent être prises en compte
* Si un fichier est invalide, il est ignoré mais signalé
* Le reste du projet est tout de même traité

#### Critères d’acceptation :
* Les relations entre fichiers sont correctement interprétées
* Les erreurs sont clairement indiquées sans bloquer le processus

---

### F2 : Traduire du COBOL en pseudo-code
#### Acteur :
Utilisateur

#### Description :
Le système transforme le code COBOL en pseudo-code structuré.

#### Input :
* Code COBOL

#### Output :
* Pseudo-code

#### Règles :
* La logique métier doit être conservée
* Aucune logique supplémentaire ne doit être ajoutée

#### Critères d’acceptation :
* Les structures de contrôle sont lisibles
* Le pseudo-code est compréhensible par un développeur non COBOL

---

### F3 : Ajouter de la colorisation
#### Acteur :
Utilisateur

#### Description :
Le système applique une colorisation syntaxique au code COBOL affiché dans l’interface.

#### Input :
* Code COBOL

#### Output :
* Code colorisé (frontend)

#### Règles :
* La colorisation ne modifie pas le code
* Les mots-clés doivent être distingués

---

### F4 : Générer une représentation schématique
#### Acteur :
Utilisateur

#### Description :
Le système génère une représentation visuelle du code COBOL (flowchart et architecture).

#### Input :
* Code COBOL

#### Output :
* Schéma interactif (web)
* Schéma exportable (mermaid)

#### Règles :
* Les boucles et conditions doivent être visibles
* Une simplification est possible en cas de complexité élevée

#### Critères d’acceptation :
* Le schéma reflète la logique globale du programme

---

### F5 : Créer un utilisateur
#### Acteur :
Utilisateur

#### Description :
Permet à un utilisateur de créer un compte.

#### Input :
* Email
* Mot de passe

#### Output :
* Confirmation

#### Règles :
* Un utilisateur ne peut pas créer deux comptes avec la même adresse email
* Le mot de passe doit respecter des critères de sécurité
* L’email doit être valide

---

### F6 : Se connecter à son compte
#### Acteur :
Utilisateur

#### Description :
Permet à un utilisateur de se connecter.

#### Input :
* Email
* Mot de passe

#### Output :
* Accès au compte

#### Règles :
* Les identifiants doivent être valides

---

### F7 : Supprimer son compte
#### Acteur :
Utilisateur

#### Description :
Permet à un utilisateur de supprimer son compte.

#### Input :
* Confirmation

#### Output :
* Confirmation de suppression

#### Règles :
* Toutes les données doivent être supprimées

---

### F8 : Accéder à l'historique des traductions
#### Acteur :
Utilisateur

#### Description :
Permet à l’utilisateur de consulter ses 10 dernières traductions.

#### Output :
* Liste des traductions

#### Règles :
* Seules les 10 dernières traductions sont conservées
* L’utilisateur peut supprimer une traduction

---

### F9 : Mot de passe oublié
#### Acteur :
Utilisateur

#### Description :
Permet à l’utilisateur de réinitialiser son mot de passe.

#### Input :
* Email

#### Output :
* Lien de réinitialisation

#### Règles :
* Le lien doit être sécurisé et temporaire

---

### F10 : Se déconnecter
#### Acteur :
Utilisateur

#### Description :
Permet à l’utilisateur de se déconnecter de son compte.

---

### F11 : Consulter / télécharger une traduction
#### Acteur :
Utilisateur

#### Description :
Permet à l’utilisateur d’accéder à une traduction existante et de la télécharger.

#### Output :
* Fichier .md

---

## Comportement de l’IA

* L’IA peut proposer des hypothèses en cas d’incertitude
* Un niveau de confiance peut être affiché
* L’IA ne doit pas inventer de logique non présente dans le code
* Une réponse partielle est préférable à une réponse incorrecte

---

## Cas d’erreur

* Fichier vide
* Format non supporté
* Fichier trop volumineux (découpage automatique)
* Code non reconnu
* Données utilisateur invalides
