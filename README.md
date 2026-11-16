# JobBoard 

JobBoard AWD est une plateforme de recrutement qui centralise les annonces, les candidatures et la planification des entretiens. Le candidat peut consulter les offres, postuler avec son CV et suivre sa candidature. Le recruteur peut publier des annonces, examiner les dossiers et organiser un entretien avec les candidats présélectionnés.

## Fonctionnalités principales

- Authentification et gestion des rôles candidat et recruteur.
- Publication et consultation des annonces.
- Dépôt de candidatures avec CV et suivi de leur statut.
- Planification et annulation des rendez-vous d’entretien.
- Notification du candidat en cas de changement lié à son rendez-vous.

## Architecture et technologies

L’interface **Angular** communique avec les microservices **Spring Boot** via une **API Gateway**. **Eureka** permet la découverte des services, **OpenFeign** assure leurs appels REST synchrones et **RabbitMQ** transmet les événements de façon asynchrone. Les données sont stockées dans **MySQL** ou **MongoDB** selon le service.

## Démarrage local

1. Configurer les connexions aux bases de données, à RabbitMQ et les paramètres d’authentification.
2. Démarrer les bases de données et RabbitMQ, puis Eureka.
3. Lancer les microservices et l’API Gateway.
4. Démarrer l’interface Angular et vérifier le parcours de connexion.

Consulter la [documentation technique](docs/documentation.md) pour le rôle des services, le déroulement d’un entretien et les scénarios de test.

## Équipe

Adem Raies · Mohamed Fares · Issra Aouainia · Mohamed Amine Ghzaiel · Sarra Ben Nejma
