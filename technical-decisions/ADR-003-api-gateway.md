# ADR-003 : Choix de l'API Gateway

Date : 2026-03-13
Statut : en discussion

## Contexte

Le projet Decobolizator est composé de plusieurs services internes : un service d'orchestration IA en Python (FastAPI) et potentiellement d'autres micro-services à venir. Le frontend React doit communiquer avec ces services via un point d'entrée unique. L'API Gateway doit gérer le routage, la validation des requêtes, et supporter le streaming SSE vers le client pour relayer les réponses LLM en temps réel.

## Options considérées

- **TypeScript + Fastify** : framework Node.js léger et très performant, syntaxe quasi identique à Express, gestion native de l'async/await, sérialisateur JSON basé sur des schémas.
- **TypeScript + Express** : framework Node.js le plus connu, mature et avec une grande communauté, mais moins performant que Fastify et sans validation de schéma native.
- **TypeScript + NestJS** : framework Node.js structuré et opinionné, inspiré d'Angular, avec injection de dépendances et modules intégrés. Très complet mais avec un overhead d'abstraction et une verbosité élevée pour un service dont le rôle est principalement du routage et du proxy.

## Décision

Utilisation de **TypeScript avec Fastify** comme API Gateway applicatif.

Fastify est un équivalent d'Express avec une syntaxe quasi identique, mais 2 à 3 fois plus rapide grâce à l'utilisation d'un sérialisateur JSON basé sur des schémas. Il gère nativement l'async/await, ce qui simplifie le code de routage et de proxy sans callback imbriqués. NestJS est écarté malgré sa structure solide : son niveau d'abstraction et sa verbosité (décorateurs, modules, injection de dépendances) sont disproportionnés pour un service dont le rôle principal est du routage et du proxy vers FastAPI. Son système de plugins officiels (`@fastify/http-proxy`, `@fastify/cors`, `@fastify/rate-limit`) couvre l'ensemble des besoins sans surcharge architecturale.

## Conséquences

Avantages :
- 2 à 3 fois plus rapide qu'Express grâce au sérialisateur JSON basé sur des schémas (JSON Schema / TypeBox)
- Gestion native de l'async/await : code de routage plus lisible et sans callback hell
- Syntaxe quasi identique à Express : courbe d'apprentissage minimale pour l'équipe
- Plugins officiels pour proxy HTTP, CORS, rate limiting et validation de schéma
- TypeScript first : types natifs sans configuration supplémentaire
- Point d'entrée unique pour le frontend, indépendant de la topologie interne des services

Inconvénients :
- Écosystème de plugins tiers moins vaste qu'Express (les plugins officiels couvrent l'essentiel)