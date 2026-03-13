# ADR-001 : Choix de la base de données

Date : 2026-03-13  
Statut : en attente de validation

## Contexte

Le projet Decobolizator nécessite de gérer plusieurs types de données : les jobs d'analyse soumis par les utilisateurs, les résultats de parsing COBOL (chunks, variables, métadonnées structurelles), les explications générées par le LLM, ainsi que les états intermédiaires du pipeline de traitement.
Le service doit supporter des accès concurrents et potentiellement un historique des analyses par programme COBOL.

## Options considérées

- **PostgreSQL** : base relationnelle robuste, support JSON natif (JSONB), transactions ACID, écosystème mature en Python et Node.
- **MongoDB** : base documentaire, stockage natif de JSON, schéma flexible mais cohérence moindre.
- **SQLite** : léger, sans serveur, adapté au développement local mais limité en concurrence.
- **Redis seul** : très performant pour du cache et des files de messages, mais pas conçu pour de la persistance longue durée structurée.

## Décision

Utilisation de **PostgreSQL** comme base de données principale, couplé à **Redis** pour la gestion du cache des résultats de parsing et des files de jobs asynchrones.

PostgreSQL est choisi pour sa fiabilité, son support natif du type JSONB (adapté au stockage des chunks et métadonnées COBOL), et sa compatibilité avec SQLAlchemy (Python) et Prisma/node-postgres (Node). Redis complète le dispositif pour éviter de re-parser un même programme COBOL plusieurs fois et pour gérer la queue des traitements asynchrones.

## Conséquences

Avantages :
- Stockage structuré et requêtable des métadonnées COBOL (divisions, variables, chunks)
- Support JSONB pour les colonnes semi-structurées sans sacrifier les requêtes relationnelles
- Redis permet de mettre en cache les résultats de parsing coûteux et de gérer les jobs en file d'attente
- Stack éprouvée, bien supportée dans Docker Compose

Inconvénients :
- Nécessite de maintenir deux services (PostgreSQL + Redis) en infrastructure
- Schema migration à gérer (Alembic pour Python, Prisma Migrate pour Node)
- Surcoût de configuration par rapport à SQLite pour un environnement de dev pur