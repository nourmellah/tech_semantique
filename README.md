# Projet Web Sémantique : Tourisme et Patrimoine

## 📄 Documentation
Ce README détaille :
- le domaine et le modèle ontologique,  
- les namespaces standards utilisés,  
- la structure (classes, propriétés),  
- les requêtes SPARQL et leur interprétation,  
- les règles SWRL et les inférences générées,  
- les conclusions sur les apports des technologies sémantiques.  

## 🧠 Domaine choisi
Ce projet modélise les concepts clés du tourisme (destinations, activités, hébergements, transports, forfaits, événements) et leurs acteurs (touristes, guides, agences, opérateurs).

## 📚 Technologies et standards utilisés
- **Modélisation** : TTL, OWL  
- **Interrogation** : SPARQL  
- **Inférence** : SWRL  
- **Namespaces** :  
  - RDF Schema : `http://www.w3.org/2000/01/rdf-schema#`  
  - OWL : `http://www.w3.org/2002/07/owl#`  
  - XML Schema : `http://www.w3.org/2001/XMLSchema#`  
  - FOAF : `http://xmlns.com/foaf/0.1/`  
  - Dublin Core : `http://purl.org/dc/elements/1.1/`  
  - SKOS : `http://www.w3.org/2004/02/skos/core#`

## 🧩 Structure de l’ontologie

### Classes principales  
- **Tourisme** (racine)  
- **Service**  
  - `Activite` (sous-classes : `Aventure`, `Culturelle`, `Loisir`)  
  - `Hebergement` (sous-classes : `Hotel`, `AubergeDeJeunesse`, `MaisonDHotes`)  
  - `Transport` (sous-classes : `TransportAerien`, `TransportFerroviaire`, `TransportRoutier`)  
  - `Evenement` (sous-classes : `Conference`, `Exposition`, `Festival`)  
  - `ForfaitTouristique`  
- **Destination** (sous-classes : `Pays`, `Region`, `Ville`)  
- **Agent**  
  - `Touriste`  
  - `GuideTouristique`  
- **Organisation** (sous-classes : `AgenceDeVoyage`, `ChaineHoteliere`, `OperateurTouristique`)

### Propriétés d’objet  
- `hasAccommodation` (Touriste → Hebergement)  
- `participatesInActivity` (Touriste → Activite)  
- `hasActivity` (Service → Activite)  
- `hasDestination` (Service/ForfaitTouristique → Destination)  
- `hasTransportation` (Service → Transport)  
- `locatedIn` (Destination → Destination) — *Transitive*  
- `offersService` (OperateurTouristique → Service)  
- `managesTour` (GuideTouristique → Activite)  
- `organizesEvent` (Organisation → Evenement)  

### Propriétés de données  
- `hasName`, `hasDescription` (Tourisme)  
- `hasPrice`, `hasRating` (Service)  
- `startDate`, `endDate`, `capacity` (Evenement)  
- `email`, `telephone`, `website` (Agent)  

## 🔧 Phases et étapes réalisées

1. **Choix du domaine**  
   - Sélection du thème “Tourisme & Patrimoine”  
   - Identification des concepts : destinations, activités, hébergements, transports, forfaits, événements, acteurs.

2. **Modélisation OWL pui TTL**  
   - Définition des classes et propriétés en TTL  

3. **Interrogation SPARQL**  
   - Conception de 4 requêtes :  
     - Activités (nom, localisation, capacité)  
     - Hébergements (nom, prix, localisation)  
     - Touristes et activités (e-mail, hébergement, liste d’activités)  
     - Guides et visites (qui gère quelle activité)  
   - Analyse et capture des résultats.

4. **Formalisation**  
   - Formalisation des sous-classes, disjonctions, restrictions (Transitive, Functional)  

5. **Définition des règles SWRL**  
   - Règles d’inversions (`isAccommodationOf`, `hasParticipant`…)  
   - Propagation de visites (`visits`) et destinations des forfaits  
   - Classification des services “budget” et des hébergements “haute note”

## 🔍 Requêtes SPARQL principales
- **Liste des activités** : `SELECT ?act ?nom ?ville ?capacity`  
- **Hébergements disponibles** : `SELECT ?heb ?nom ?ville ?prix`  
- **Touristes et leurs activités** : `GROUP_CONCAT` pour regrouper les activités  
- **Guides et tours** : `SELECT ?guide ?tour`

## 🤖 Règles SWRL mises en place
- Inversion de propriétés (e.g. `hasAccommodation` → `isAccommodationOf`)  
- Propagation de `hasDestination` depuis activités et transports  
- Inférence de `visits` depuis `participatesInActivity` et `hasAccommodation`  
- Classification automatique :  
  - `HighRatingHebergement` (note > 4.5)  
  - `BudgetService` (prix ≤ 100)  
