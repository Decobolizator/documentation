# ADR-005 : Choix de l'API d'orchestration IA

Date : 2026-03-13
Statut : en attente de validation

## Contexte

Le cœur du projet Decobolizator est un pipeline de traitement du code COBOL en trois passes : extraction du contexte global, résumé parallèle des chunks, et synthèse finale en langage naturel. Ce service doit assurer le parsing du code COBOL (extraction des divisions, variables, paragraphes), le chunking sémantique, la construction des prompts, et l'orchestration des appels vers l'API LLM externe. Le choix du langage et du framework est structurant car il conditionne l'accès aux bibliothèques de traitement du code.

## Options considérées

- **Python + FastAPI** : Python dispose de bibliothèques de parsing COBOL et de chunking qui facilitent le travail de traitement du code. FastAPI est un framework async moderne, performant, avec validation native via Pydantic et génération automatique de documentation OpenAPI.
- **Python + Flask** : plus simple que FastAPI, mais synchrone par défaut et sans validation de schéma native, inadapté à un pipeline avec des appels LLM potentiellement longs.
- **Node.js + Fastify** : cohérent avec le reste de la stack JS, mais l'écosystème de parsing COBOL et de traitement de texte est nettement moins riche qu'en Python.
- **Node.js + Express** : même limitation que Fastify sur l'écosystème de parsing, sans le gain de performance.

## Décision

Utilisation de **Python avec FastAPI** comme service d'orchestration IA.

Python est le langage qui dispose des bibliothèques les plus matures pour le parsing de code (notamment COBOL), le chunking sémantique et le traitement de texte. Ces bibliothèques faciliteront directement le travail de traitement du code dans le pipeline three-pass. FastAPI est choisi comme framework car il gère nativement l'async/await (essentiel pour les appels LLM parallèles en phase MAP), intègre la validation des données via Pydantic, et génère automatiquement une documentation OpenAPI utilisable par les autres services.

## Conséquences

Avantages :
- Accès aux bibliothèques Python de parsing COBOL et de chunking, absentes ou immatures dans l'écosystème Node.js
- FastAPI gère nativement l'async/await : les appels LLM parallèles (phase MAP du three-pass) s'écrivent avec `asyncio.gather`
- Validation des données entrantes et sortantes via Pydantic, avec schémas réutilisables entre les services
- Documentation OpenAPI générée automatiquement, consommable par le service Fastify
- Streaming de la réponse finale vers le client possible via `StreamingResponse` de FastAPI
- Intégration naturelle avec les bibliothèques de ML et de NLP si le projet évolue vers du traitement plus avancé

Inconvénients :
- Introduit Python dans une stack majoritairement TypeScript, ce qui implique deux environnements de développement à maintenir
- Nécessite une interface REST claire et documentée entre FastAPI et Fastify pour éviter les régressions inter-services
- Gestion des dépendances Python (virtualenv ou Poetry) à standardiser dans le Docker Compose