# Diagrammes UML — JobBoard AWD

## 1. Diagramme de classes par microservice

Les namespaces regroupent les classes par microservice. Les liens en pointillés représentent des références par identifiant entre services.

```mermaid
classDiagram
    direction TB

    namespace Identite {
        class Compte {
            +UUID id
            +String email
            +String role
            +String statutAcces
        }

        class JetonRenouvellement {
            +UUID id
            +UUID compteId
            +DateTime dateExpiration
        }
    }

    namespace Dossiers {
        class DossierCandidat {
            +UUID id
            +UUID candidatId
            +List~String~ competences
            +List~String~ experiences
            +DateTime dateModification
        }

        class VersionCV {
            +UUID id
            +UUID dossierId
            +String referenceFichier
            +Integer numeroVersion
            +DateTime dateDepot
        }
    }

    namespace Annonces {
        class Annonce {
            +UUID id
            +UUID recruteurId
            +String titre
            +String description
            +String statut
            +DateTime datePublication
        }

        class CriterePoste {
            +UUID id
            +UUID annonceId
            +String libelle
            +String description
        }
    }

    namespace Depots {
        class Candidature {
            +UUID id
            +UUID annonceId
            +UUID candidatId
            +String statut
            +DateTime dateDepot
        }

        class HistoriqueStatut {
            +UUID id
            +UUID candidatureId
            +String ancienStatut
            +String nouveauStatut
            +DateTime dateChangement
        }
    }

    namespace Entretiens {
        class RendezVous {
            +UUID id
            +UUID candidatureId
            +DateTime debut
            +DateTime fin
            +String mode
            +String lieu
            +String lienVisio
            +String statut
        }

        class ParticipantRdv {
            +UUID id
            +UUID rendezVousId
            +UUID compteId
            +String role
        }
    }

    namespace CentreDeMessages {
        class Message {
            +UUID id
            +UUID eventId
            +UUID destinataireId
            +String contenu
            +DateTime dateCreation
        }

        class ModeleMessage {
            +UUID id
            +String typeEvenement
            +String contenuModele
        }
    }

    Compte "1" -- "0..*" JetonRenouvellement : renouvelle sa session
    DossierCandidat "1" -- "0..*" VersionCV : conserve
    Annonce "1" -- "0..*" CriterePoste : définit
    Candidature "1" -- "0..*" HistoriqueStatut : historise
    RendezVous "1" -- "1..*" ParticipantRdv : regroupe
    ModeleMessage "0..1" -- "0..*" Message : sert à générer

    DossierCandidat ..> Compte : candidatId
    Annonce ..> Compte : recruteurId
    Candidature ..> Compte : candidatId
    Candidature ..> Annonce : annonceId
    RendezVous ..> Candidature : candidatureId
    ParticipantRdv ..> Compte : compteId
    Message ..> Compte : destinataireId

    note for Candidature "Un seul dépôt par couple annonceId et candidatId."
    note for RendezVous "La planification exige une candidature présélectionnée."
    note for Message "Déduplication par couple eventId et destinataireId."
```

## 2. Diagramme de cas d’utilisation

```mermaid
flowchart LR
    V["Visiteur"]
    C["Candidat"]
    R["Recruteur"]
    A["Administrateur"]

    subgraph JOB["Système JobBoard AWD"]
        subgraph PUBLIC["Consultation et inscription"]
            VOIR(["Consulter les annonces"])
            FILTRER(["Filtrer les annonces"])
            INSCRIPTION(["Créer un compte"])
        end

        subgraph CAND["Espace candidat"]
            DOSSIER(["Gérer son dossier, ses compétences et ses expériences"])
            CV(["Déposer une nouvelle version du CV"])
            POSTULER(["Postuler à une annonce"])
            SUIVI(["Suivre le statut de sa candidature"])
        end

        subgraph RECR["Espace recruteur"]
            PUBLIER(["Publier une annonce"])
            CLOTURER(["Clôturer une annonce"])
            EXAMINER(["Examiner une candidature"])
            DECIDER(["Prendre une décision sur une candidature"])
            PLANIFIER(["Planifier un rendez-vous"])
            ANNULER(["Annuler un rendez-vous"])
        end

        subgraph ADMIN["Administration"]
            ROLES(["Attribuer les rôles"])
            ACCES(["Gérer les accès aux comptes"])
        end

        subgraph CONTROLES["Contrôles métier"]
            OUVERTURE(["Vérifier que l’annonce est ouverte"])
            DOUBLON(["Vérifier l’absence de candidature en double"])
            TRANSITION(["Vérifier la transition de statut"])
            PRESELECTION(["Vérifier que la candidature est présélectionnée"])
            AUTORISATION(["Vérifier les droits du recruteur sur le rendez-vous"])
        end
    end

    V --- VOIR
    V --- FILTRER
    V --- INSCRIPTION

    C --- DOSSIER
    C --- CV
    C --- POSTULER
    C --- SUIVI

    R --- PUBLIER
    R --- CLOTURER
    R --- EXAMINER
    R --- DECIDER
    R --- PLANIFIER
    R --- ANNULER

    A --- ROLES
    A --- ACCES

    FILTRER -. "« extend »" .-> VOIR
    POSTULER -. "« include »" .-> OUVERTURE
    POSTULER -. "« include »" .-> DOUBLON
    DECIDER -. "« include »" .-> TRANSITION
    PLANIFIER -. "« include »" .-> PRESELECTION
    PLANIFIER -. "« include »" .-> AUTORISATION
    ANNULER -. "« include »" .-> AUTORISATION
```
