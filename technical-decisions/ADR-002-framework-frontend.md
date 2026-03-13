# ADR-002 : Choix du framework frontend

Date : 2026-03-13  
Statut : en attente de validation

## Contexte

Le projet Decobolizator expose une interface web permettant à un utilisateur de soumettre un fichier ou du code COBOL, de suivre l'avancement du traitement, et de consulter l'explication générée en langage naturel.
L'interface doit gérer un flux asynchrone avec affichage progressif des résultats (SSE).

## Options considérées

- **React** : bibliothèque UI dominante, vaste écosystème, flexibilité maximale, gestion fine du state.
- **Next.js** : framework React avec SSR/SSG, routing intégré, mais complexité accrue pour une SPA pure.
- **Vue 3** : courbe d'apprentissage plus douce, bonne réactivité, écosystème plus petit qu'React.
- **Svelte** : compilé, très performant, mais moins de ressources et de composants disponibles.

## Décision

Utilisation de **React** (avec Vite comme bundler) en mode SPA.

Next.js est écarté car le projet ne nécessite pas de SSR.
React offre le meilleur rapport entre flexibilité, disponibilité de composants (gestion du streaming SSE, affichage de code syntaxé), et familiarité de l'équipe.

## Conséquences

Avantages :
- Gestion native du streaming SSE via `EventSource` ou `fetch` avec `ReadableStream`
- Écosystème riche : `react-syntax-highlighter` pour l'affichage du code COBOL
- Séparation claire frontend/backend via API REST + SSE
- Compatible avec la stack Docker Compose

Inconvénients :
- Boilerplate plus verbeux que Vue ou Svelte pour les composants simples
- Gestion du state global à définir
- Pas de routing SSR natif (acceptable pour ce cas d'usage)