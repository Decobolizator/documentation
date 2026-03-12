# Organisation JIRA
## Types de tickets
- Epic
- UserStory
- Feature
- Task
- Sub-task

## Structure d’un JIRA
Votre projet est séparé en plusieurs SPRINT de 2 semaines
Vous devez séparer votre projet en différents EPIC
- Une EPIC regroupe des US (User Story) qui sont liées par un thème
- Par exemple : vous avez un projet de système de traduction. Les différentes thèmes correspondent aux différents blocs qui construisent votre projet
- Back
- Front
- IA …

## Description des tickets

Une user story va vous permettre de décrire le besoin du point de vue de l’utilisateur final de votre application.
Elle doit définir **une seule fonctionnalité** de votre application et doit contenir les informations suivantes :  
- Résumé (titre de l’US) -> Doit être explicite (concis / clair) pour que vous vous y retrouviez facilement lorsque votre projet sera rempli d’US
- Description -> Doit définir le plus précisément possible votre US :
- Contexte
  - Comportement actuel (si fonctionnalité déjà existante)
  - Spécifications
  - Développements (si non création de tâches)
  - Maquettes
  - **How to demo**
- Par exemple : une user story front end qui concerne la page d’accueil
  - Résumé : [DESIGN][ACCUEIL] Navigation
  - Description :
    - Contexte : En tant que client, je souhaite changer le design de la page d’accueil afin qu’elle respecte la nouvelle identité visuelle de ma marque.
    - Comportement actuel : Aujourd’hui, la page d’accueil est au anciennes couleurs de la marque.
    - Spécifications :
      - Police d’écriture :
      - Titre -> Arial …
      - Couleur :
      - Fond -> CODEHEX
    - Développements :
      - 1. Changer les codes couleurs
      - 2. Changer les polices
      - 3. …
  - Maquettes : mettre les photos des maquettes
  - How to demo :
    - Le testeur ouvre la page d’accueil et vérifie que ça correspond bien à son attente

Si vous créez des tâches (recommandé pour le suivi de l’avancement du dev de la fonctionnalité), créez une tâche par étape -> C’est votre TODO. Exemple :
- [DEV] Changer les codes couleurs
  - Description, soit :
    - Soit vous mettez un lien vers la spec de l’US
    - Soit vous remettez les specs dans le ticket
- [DEV] Changer les polices
- [REC] Vérifier les codes couleurs
  - Soit vous mettez un lien vers la spec de l’US
  - La personne qui teste vérifie que toutes les couleurs sont OK
Pour les tâches de recette, l’exemple n’est pas le meilleur, mais l’idée ce de mettre en détail ce que la personne doit tester et les résultats qu’elle doit avoir. Cela doit être rédigé en prenant en compte que le testeur n’y connait rien -> Le plus précis possible.
Si vous voulez ajouter une modification à votre description d’US, il est préférable d’indiquer que c’est un EDIT pour tracer aux mieux les changements.
Par exemple :
- Avant : Je veux que la barre de navigation soit bleue
- Après : Je veux que la barre de navigation soit bleue (EDIT JJ/MM/AAAA) et rouge
Ou alors :
- Avant : Je veux que la barre de navigation soit bleue
- Après : Je veux que la barre de navigation soit bleue (EDIT JJ/MM/AAAA) rouge

### Statut du ticket
Selon votre organisation, les workflows des tickets peuvent être différents. Sur votre version de JIRA, vous avez un workflow simple. Et c’est OK ! L’important c’est de bien tracer vos tickets pour que tout le monde puisse se rendre de l’avancement de votre projet. Votre workflow peut ressembler à :
`Backlog -> Dev in progress -> Ready to be tested -> Test in progress -> Validation -> Terminé`
Ready to be tested -> Vous n’avez pas cette étape, mais ça peut être une bonne idée de l’ajouter. En effet, quand vous aurez une grosse liste de tickets en « Dev in progress » il vous sera dur de savoir quels tickets ont été fini de dev et sont prêts à être testés, et lesquels sont encore en cours.
À chaque fin début ou fin d’étape, il vous faut mettre à jour le ticket.
Par exemple, vous commencez les dev d’un ticket qui était dans le backlog, vous le passez en Dev in progress. Vous avez fini de dev un ticket, il est prêt à être testés, vous le mettez en « Ready to be tested »

### Assignation des tickets
Un ticket doit être assigné à une personne -> celle en charge du statut dans laquelle est le ticket.
Par exemple : dans votre projet, Personne A doit dev ticket A. Et personne B va recetter (= tester) ticket A.
Quand Personne A va commencer les devs, il prend le ticket A qui est en statut « Backlog » et le passe en « Dev in progress », puis se l’assigne.
Il fait sa vie de dev et il fini les devs. Il met le ticket en « Ready to be tested” et il l’assigne à la personne qui doit tester (Personne B). Et ensuite personne B fait les tests, elle valide et change le statut en « Validation ».
Si personne B valide pas, elle peut mettre un commentaire pour dire « Je valide pas pcq faut changer … » et repasser le ticket en « Dev in progress » et réassigne le ticket à personne A.
Si vous avez des tâches, il faut fermer les tâches au fur et à mesure que les étapes passent. 
