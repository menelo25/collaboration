# Proposition Améliorée pour la Gestion de Livres dans la Bibliothèque

La bibliothèque gère un grand nombre de livres usagés, dont beaucoup ont été publiés avant 1972 et ne possèdent pas d'ISBN. L'objectif est de simplifier le processus d'ajout tout en assurant l'unicité des livres.

## Modifications Proposées

### Saisie des Détails du Livre

- Lors de l'ajout d'un livre, l'utilisateur saisit les informations nécessaires, telles que le titre, l'auteur, l'année de publication, etc.

### Frontend (Vue.js)

- Modification de l'interface utilisateur pour inclure un champ de saisie du titre en plus de celui de l'ISBN lors de l'ajout d'un livre.
- L'interface utilisateur doit rester intuitive, guidant l'utilisateur en fonction du scénario (ISBN ou titre).

### Vérification de l'Existence

- Le système vérifie si un livre similaire existe déjà dans la base de données en se basant sur le titre, l'auteur, et d'autres détails pertinents.

### Gestion des Livres sans ISBN

- Si l'année de publication est antérieure à 1972, l'ISBN ne sera pas obligatoire.
- Le système utilise le titre comme identifiant unique dans ce cas.

### Confirmation de l'Ajout

- Si un livre similaire est trouvé, le système informe l'utilisateur de l'existence du livre et propose de mettre à jour les informations existantes si nécessaire.
- Si aucun livre similaire n'est trouvé, le système ajoute le nouveau livre à la base de données.

### Backend (Express.js)

- Modification des routes et des contrôleurs pour gérer les deux scénarios : ajout de livre avec ISBN et ajout de livre sans ISBN.
- Implémentation d'une logique de vérification pour s'assurer que l'ISBN est valide, s'il est fourni, et que le titre est unique s'il est utilisé comme identifiant.
- Gestion appropriée des erreurs, avec des messages clairs pour guider l'utilisateur en cas de problème.

### Validation de l'ISBN

- Utilisation d'une bibliothèque de validation d'ISBN pour garantir la conformité aux normes des ISBN fournis.
- Affichage d'un message d'erreur si l'ISBN n'est pas valide, incitant l'utilisateur à vérifier et à corriger les informations.

### Tests et Documentation

- Mise en place de tests unitaires et d'intégration pour garantir le bon fonctionnement du système.
- Mise à jour de la documentation pour inclure les nouvelles fonctionnalités et les changements apportés au système.

```mermaid
sequenceDiagram

    Utilisateur->>+Système: Saisir ISBN ou Titre
    alt ISBN fourni
        Système->>+Utilisateur: Vérifier Existence ISBN
        alt ISBN invalide ou existant
            Système-->>-Utilisateur: Afficher Messages d'Erreur
        else
            Utilisateur->>+Système: Fournir Détails sur Livres
            Utilisateur-->>-Système: Ajouter Livre sans ISBN
        end
    end

```

```mermaid
  usecase Utilisateur as U
  usecase AjouterLivre as AL
  usecase ConsulterLivre as CL
  usecase MettreAJourLivre as MJL
  usecase SupprimerLivre as SL

  U --> (Ajouter Livre) : <<include>>
  U --> (Consulter Livre) : <<include>>
  U --> (Mettre à Jour Livre) : <<include>>
  U --> (Supprimer Livre) : <<include>>

  (Ajouter Livre) --> (Fournir Détails Livre)
  (Fournir Détails Livre) --> (Valider ISBN)
  (Fournir Détails Livre) --> (Valider Titre)
  (Ajouter Livre) --> (Vérifier Existence)
  (Vérifier Existence) --> |Livre Existe| (Informations Livre)
  (Vérifier Existence) --> |Livre n'Existe pas| (Ajouter Livre)

  (Consulter Livre) --> |Rechercher par Titre| (Liste Livres)
  (Consulter Livre) --> |Rechercher par Auteur| (Liste Livres)
  (Consulter Livre) --> |Rechercher par Année| (Liste Livres)

  (Mettre à Jour Livre) --> (Sélectionner Livre)
  (Sélectionner Livre) --> (Détails Livre)
  (Détails Livre) --> (Valider ISBN)
  (Détails Livre) --> (Valider Titre)
  (Mettre à Jour Livre) --> (Vérifier Existence)
  (Vérifier Existence) --> |Livre Existe| (Informations Livre)
  (Vérifier Existence) --> |Livre n'Existe pas| (Mettre à Jour Livre)

  (Supprimer Livre) --> (Sélectionner Livre)
  (Sélectionner Livre) --> (Détails Livre)
  (Détails Livre) --> (Confirmer Suppression)
  (Confirmer Suppression) --> (Supprimer Livre)
  (Supprimer Livre) --> |Livre Supprimé| (Confirmation Suppression)



    end

```
