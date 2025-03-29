# Diffuseur d'Odeur Intelligent - Documentation Technique

Ce projet concerne le développement d'un diffuseur d'odeur intelligent avec une application mobile Flutter pour son contrôle. Le système comprend :
- Contrôle de 3 ventilateurs avec différentes vitesses
- Gestion d'un tube LED pour l'éclairage
- Batterie 3000mAh rechargeable USB-C
- Interface tactile 3x3cm sur le diffuseur
- Communication Bluetooth/WiFi

## Table des Matières
1. [Spécifications Techniques](#1-spécifications-techniques)
   - [Exigences Mécaniques](#exigences-mécaniques)
   - [Exigences Électroniques](#exigences-électroniques)
   - [Exigences Logiciel Embarqué](#exigences-logiciel-embarqué)
2. [Communication Sans Fil](#2-communication-sans-fil)
   - [Bluetooth](#bluetooth)
   - [WiFi](#wifi)
3. [Architecture](#3-architecture)
   - [Stack Technologique](#stack-technologique)
   - [Clean Architecture](#clean-architecture)
   - [Structure du Projet](#structure-du-projet)
   - [Composants Partagés](#composants-partagés)
4. [Fonctionnalités](#4-fonctionnalités)
   - [Authentification](#authentification)
   - [Profil Utilisateur](#profil-utilisateur)
   - [Contrôle des Ventilateurs](#contrôle-des-ventilateurs)
   - [Gestion de l'Éclairage](#gestion-de-léclairage)
   - [Monitoring de la Batterie](#monitoring-de-la-batterie)
5. [Planning](#5-planning)
   - [Phases de Développement](#phases-de-développement)
   - [Analyse de Complexité](#analyse-de-complexité-des-features)
   - [Tests et Qualité](#tests-et-qualité)
6. [Profil du Développeur](#profil-du-développeur)
   - [Présentation](#présentation)
   - [Expertise Technique](#expertise-technique)
   - [Applications Notables](#applications-notables)
   - [Leadership et Expertise](#leadership-et-expertise)
   - [Contact](#contact)

## 1. Spécifications Techniques

### Exigences Mécaniques
- **Forme et Ergonomie**
  * Boîtier sphérique cylindrique (ou courbe)
  * Façade adaptée pour interface tactile
  * Accès facile au port USB-C
  * Conception démontable pour maintenance
  * Espace dédié pour le tube LED avec éclairage homogène et anti-éblouissement

- **Ventilation et Thermique**
  * Volume dégagé autour des 3 ventilateurs
  * Canaux d'aération optimisés
  * Gestion thermique efficace
  * Orifices de ventilation intégrés pour éviter la surchauffe

- **Interface Tactile**
  * Surface tactile 3x3cm
  * Fonctions :
    - Marche/Arrêt (On/Off)
    - Réglage ventilation (vitesse, choix du ventilateur actif : 1, 2 ou 3)
    - Réglage lumière (intensité du tube LED)
    - Boutons +/- pour ajustement (vitesse, intensité, sélection ventilateur)
  * Sensibilité optimisée pour conditions variables (humidité, température)
  * Logique de priorisation pour commandes simultanées

### Exigences Électroniques
- **Carte Électronique**
  * Adaptée à la forme cylindrique
  * Pilotage précis des 3 ventilateurs
  * Gestion de la batterie 3000mAh
  * Interface écran tactile
  * Communication sans fil (Bluetooth/WiFi)
  * Conformité aux normes CE/UL et directives (CEM, sécurité électrique, CB)

- **Alimentation**
  * Batterie 3000mAh
  * Recharge USB-C (5V)
  * Supervision tension/courant/température
  * Objectif autonomie : 10 heures en continu
  * Circuits de charge optimisés
  * Protection contre surcharge et décharge profonde

- **Éclairage**
  * Tube LED intégré
  * Contrôle d'intensité
  * Éclairage homogène
  * Protection contre l'éblouissement
  * Modes prédéfinis disponibles

### Exigences Logiciel Embarqué
- **Gestion des Ventilateurs**
  * Activation indépendante (1, 2 ou 3)
  * 3 niveaux de vitesse
  * Monitoring et sécurité
  * Journalisation des erreurs

- **Gestion de l'Éclairage**
  * Contrôle d'intensité
  * Modes prédéfinis
  * Synchronisation avec ventilateurs

- **Supervision Batterie**
  * Niveau de charge
  * Temps d'autonomie
  * Alertes batterie faible
  * Protection contre surcharge/décharge

- **Interface Utilisateur**
  * Sensibilité tactile optimisée
  * Gestion des conditions variables
  * Priorisation des commandes
  * Mises à jour OTA (si WiFi disponible)

## 2. Communication Sans Fil

### Bluetooth
- **Version** : Bluetooth 6.0 (2024)
  * Débit jusqu'à 4 Mbps
  * Latence réduite à 1ms
  * Portée jusqu'à 500m en extérieur
  * [Source: Bluetooth SIG](https://www.bluetooth.com/specifications/bluetooth-core-specification/)

- **Technologie** : BLE (Bluetooth Low Energy) avec LE Audio 2.0
  * Optimisé pour les appareils IoT
  * Consommation énergétique réduite de 50%
  * Compatible avec les appareils mobiles modernes
  * Support du codec LC3+ pour une qualité audio améliorée
  * [Source: Difference entre Bluetotth et BLE](https://elainnovation.com/difference-entre-bluetooth-et-bluetooth-low-energy/)
  * [Source: Bluetooth BLE](https://www.bluetooth.com/learn-about-bluetooth/bluetooth-technology/le-audio/)

- **Optimisation des Performances**
  * Utilisation de la modulation PHY 4M pour les données
  * Coded PHY amélioré pour la portée étendue
  * Adaptive Frequency Hopping de nouvelle génération
  * Isochronous Channels avec multi-stream
  * [Source: Bluetooth Technology Overview](https://www.bluetooth.com/learn-about-bluetooth/tech-overview/)

- **Découverte du Matériel**
  * Service Discovery Protocol (SDP) 2.0
  * GATT (Generic Attribute Profile) amélioré
  * Caractéristiques personnalisées pour le diffuseur
  * [Source: Bluetooth Portam](https://www.bluetooth.com/wp-content/uploads/Files/Specification/HTML/Core-54/out/en/host/service-discovery-protocol--sdp--specification.html)

- **Sécurité**
  * Cryptage AES-256
  * Appairage sécurisé avec numérico-comparaison
  * Protection contre les attaques MITM et replay
  * [Source: kiteworks - Bluetooth Security](https://www.kiteworks.com/fr/glossaire/tout-ce-que-vous-devez-savoir-sur-le-chiffrement-aes-256/#:~:text=Le%20chiffrement%20AES%2D256%20est%20bas%C3%A9%20sur%20un%20r%C3%A9seau%20de,le%20chiffrement%20et%20le%20d%C3%A9chiffrement.)

- **Types de Données Échangées**
  * Commandes de contrôle (JSON)
  * Télémétrie en temps réel
  * Mises à jour de firmware
  * [Source: Bluetooth Core Specification](https://www.bluetooth.com/specifications/bluetooth-core-specification/)

### WiFi
- **Version** : WiFi 7 (802.11be)
  * Débit maximal théorique : 46 Gbps
  * Latence ultra-faible < 1ms
  * Support tri-bande (2.4 GHz, 5 GHz, 6 GHz)
  * Bande 6 GHz optimisée pour IoT
  * [Source: WiFi Alliance](https://www.wi-fi.org/discover-wi-fi)

- **Technologie** : 802.11be
  * Multi-Link Operation (MLO) pour utilisation simultanée de plusieurs bandes
  * Multi-RU (Resource Unit) pour allocation dynamique des ressources
  * 4K QAM pour modulation améliorée
  * Preamble Puncturing pour éviter les interférences
  * [Source: IEEE 802.11be](https://standards.ieee.org/standard/802_11be-2024.html)

- **Optimisation des Performances**
  * Dynamic Fragmentation amélioré pour paquets de taille variable
  * Spatial Reuse avec BSS Coloring pour réutilisation spatiale
  * Enhanced Multi-User MIMO jusqu'à 16 flux
  * Gestion intelligente de la congestion
  * [Source: Cisco](https://www.cisco.com/c/en/us/products/collateral/wireless/catalyst-9100ax-access-points/wireless-wi-fi-7-access-points-aag.html)

- **Découverte du Matériel**
  * mDNS (multicast DNS) optimisé pour IoT
  * DNS-SD (DNS Service Discovery) avec cache local
  * DPP 2.0 (Device Provisioning Protocol) pour configuration rapide
  * Fast Initial Link Setup (FILS)
  * [Source: IONOS: Multicast DNS](https://www.ionos.fr/digitalguide/serveur/know-how/multicast-dns/)

- **Sécurité**
  * WPA3 Enterprise avec authentification SAE
  * Opportunistic Wireless Encryption (OWE) pour réseaux ouverts
  * Protected Management Frames (PMF) obligatoire
  * Enhanced Open avec chiffrement individualisé
  * [Source: WiFi Alliance](https://community.fs.com/fr/article/wpa3-security-why-your-enterprise-business-needs-it.html)

- **Types de Données Échangées**
  * Streaming temps réel ultra-HD
  * Mises à jour OTA avec vérification d'intégrité
  * Télémétrie et analytics en temps réel
  * Communication IoT optimisée pour basse consommation
  * [Source: mises à jour Over-the-air (OTA)](https://www.ninjaone.com/fr/blog/que-sont-les-mises-a-jour-over-the-air/)

## 3. Architecture

### Stack Technologique
- **Framework** : Flutter (v 3.27.2)  - Dart (v3.6.1) -  DevTools (v2.40.2)
- **Architecture** : [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- **State Management** : [flutter_bloc](https://pub.dev/packages/flutter_bloc) (BLoC)
- **Navigation** : [go_router](https://pub.dev/packages/go_router)
- **Base de données locale** : [hive](https://pub.dev/packages/hive)
- **Bluetooth** : [flutter_blue_plus](https://pub.dev/packages/flutter_blue_plus)
- **WiFi** : [wifi_iot](https://pub.dev/packages/wifi_iot) (optionnel)
- **HTTP Client** : [generic_requester](https://github.com/Jewelch/generic_requester) (API générique basé sur Dio développé par Jewel CHERIAA)
- **Cryptographie** : [encrypt](https://pub.dev/packages/encrypt)
- **Mesure Performance** : [flutter_performance_metrics](https://pub.dev/packages/flutter_performance_metrics)
- **Animations** : [lottie](https://pub.dev/packages/lottie)
Monitoring de la Batterie (10 jours)


### Clean Architecture
Chaque feature suit les principes de Clean Architecture :
- **Data Layer** : Gestion des données et communication avec les sources externes
- **Domain Layer** : Logique métier et règles de l'application
- **Presentation Layer** : Interface utilisateur et gestion des états

### Structure du Projet
```
lib/
├── core/
│   ├── config/
│   │   ├── app_config.dart
│   │   └── env_config.dart
│   ├── theme/
│   │   ├── app_theme.dart
│   │   └── theme_data.dart
│   └── utils/
│       ├── extensions/
│       └── helpers/
├── features/
│   ├── auth/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   ├── auth_local_datasource.dart
│   │   │   │   └── auth_google_datasource.dart
│   │   │   ├── models/
│   │   │   │   └── user_model.dart
│   │   │   └── repositories/
│   │   │       └── auth_repository_impl.dart
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   └── user.dart
│   │   │   ├── repositories/
│   │   │   │   └── auth_repository.dart
│   │   │   └── usecases/
│   │   │       ├── login_usecase.dart
│   │   │       └── logout_usecase.dart
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   ├── auth_bloc.dart
│   │       │   ├── auth_event.dart
│   │       │   └── auth_state.dart
│   │       ├── pages/
│   │       │   ├── login_page.dart
│   │       │   └── register_page.dart
│   │       └── widgets/
│   │           ├── login_form.dart
│   │           └── register_form.dart
│   ├── profile/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   └── repositories/
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   └── usecases/
│   │   └── presentation/
│   │       ├── bloc/
│   │       ├── pages/
│   │       └── widgets/
│   ├── fan_control/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   ├── lighting_control/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   └── battery_monitoring/
│       ├── data/
│       ├── domain/
│       └── presentation/
└── shared/
    ├── widgets/
    └── services/
```

### Composants Partagés
- **Core** : Configuration et utilitaires de base
  ```
  core/
  ├── config/
  ├── theme/
  └── utils/
  ```

- **Shared** : Widgets et services réutilisables
  ```
  shared/
  ├── widgets/
  └── services/
  ```

- **Design System** : Système de design cohérent
  ```
  design_system/
  ├── theme/
  ├── components/
  └── animations/
  ```

- **API** : Interfaces et services d'API basés sur generic_requester
  ```
  api/
  ├── interfaces/
  │   └── api_client.dart
  ├── models/
  │   ├── request/
  │   └── response/
  └── services/
      ├── bluetooth_service.dart
      └── wifi_service.dart
  ```

## 4. Fonctionnalités

### Authentification
- **Méthodes d'Authentification**
  * Google Sign-In
    - Authentification sécurisée via OAuth 2.0
    - Récupération des informations de profil
    - Gestion des tokens d'accès
    - Support multi-plateformes (iOS/Android)
  * Authentification Email/Mot de passe
    - Inscription avec validation d'email
    - Récupération de mot de passe
    - Validation de force du mot de passe
    - Protection contre les attaques par force brute
  * Authentification biométrique (optionnelle)
    - Support Face ID/Touch ID sur iOS
    - Support Fingerprint sur Android
    - Fallback sur code PIN

### Profil Utilisateur
- Gestion complète du profil :
  * Informations personnelles :
    - Nom et prénom
    - Photo de profil
    - Date de création
    - Dernière modification
  * Personnalisation de l'interface :
    - Thème (clair/sombre/automatique)
    - Langue (FR/EN)
  * Préférences de fonctionnement :
    - Vitesses par défaut des ventilateurs
    - Intensité lumineuse par défaut
    - Horaires de fonctionnement
  * Historique et statistiques :
    - Temps d'utilisation par jour
    - Consommation d'énergie
    - Utilisation des différents modes
    - Alertes et notifications

- Export/Import avancé des profils :
  * Export complet en JSON incluant :
    - Toutes les informations personnelles
    - Toutes les préférences d'interface
    - Tous les paramètres de fonctionnement
    - L'historique complet d'utilisation
    - Les statistiques d'utilisation
    - Les programmes personnalisés
  * Import depuis :
    - Fichier JSON
    - QR Code
    - Lien de partage
  * Sauvegarde locale :
    - Stockage sécurisé sur l'appareil
    - Sauvegarde automatique
    - Versioning des sauvegardes
    - Restauration des points de sauvegarde
  * Partage entre appareils :
    - Export en fichier JSON
    - Génération de QR Code
    - Lien de partage sécurisé
    - Partage direct entre appareils
    - Synchronisation automatique

- Gestion des préférences :
  * Stockage local avec Hive :
    - Chiffrement des données
    - Compression des données
    - Gestion des versions
    - Nettoyage automatique
  * Sauvegarde automatique :
    - Sauvegarde périodique
    - Sauvegarde après modifications
    - Sauvegarde avant déconnexion
    - Gestion des erreurs
  * Restauration des paramètres :
    - Restauration complète
    - Restauration partielle
    - Restauration sélective
    - Annulation des modifications
  * Synchronisation :
    - Entre appareils
    - Entre sessions
    - En temps réel
    - Gestion des conflits

- Interface utilisateur :
  * Pages dédiées :
    - Page de profil principal
    - Page des préférences
    - Page d'historique
    - Page de statistiques
    - Page de sauvegarde
  * Widgets interactifs :
    - Formulaire de profil
    - Sélecteur de thème
    - Sélecteur de langue
    - Formulaire de préférences
    - Visualisation d'historique
    - Export/Import de profil
    - Partage de profil
  * Visualisations :
    - Graphiques d'utilisation
    - Statistiques en temps réel
    - Historique détaillé
    - État de la sauvegarde

### Contrôle des Ventilateurs
   - Sélection du ventilateur (1, 2 ou 3)
   - Réglage de la vitesse (3 niveaux)
   - Monitoring de l'état
- Communication Bluetooth/WiFi pour le contrôle
- Programmes personnalisés :
  * Création de séquences de ventilation
  * Planification horaire
  * Déclenchement automatique
- Statistiques d'utilisation :
  * Temps de fonctionnement par ventilateur
  * Consommation d'énergie
  * Efficacité de la diffusion

### Gestion de l'Éclairage
   - Contrôle de l'intensité LED
   - Modes prédéfinis
   - Synchronisation avec les ventilateurs
- Communication Bluetooth/WiFi pour le contrôle
- Effets lumineux :
  * Mode ambiance
  * Mode lecture
  * Mode nuit
  * Mode fête
- Programmation :
  * Horaires d'allumage/extinction
  * Transitions progressives
  * Scènes personnalisées

### Monitoring de la Batterie
   - Niveau de charge
   - Temps d'autonomie estimé
   - Alertes de batterie faible
- Communication Bluetooth/WiFi pour le monitoring
- Optimisation énergétique :
  * Mode économie d'énergie
  * Ajustement automatique des performances
  * Recommandations d'utilisation
- Historique de charge :
  * Cycles de charge
  * Durée d'autonomie
  * Consommation par fonction

## 5. Planning

### Phases de Développement
1. **Phase 1 - Setup Initial (3 jours)**
   - Mise en place de l'architecture Clean Architecture
   - Configuration de l'environnement de développement
   - Mise en place des packages core :
     * Core (utilitaires, constantes, erreurs)
     * API (interfaces et services)
     * Design System (composants UI réutilisables)

2. **Phase 2 - Développement Feature par Feature (50 jours)**
   1. **Authentification (5 jours)**
      - Couche Data
        * Implémentation du stockage local
        * Gestion des sessions
      - Couche Domain
        * Use cases de login/logout
        * Gestion des états
      - Couche Presentation
        * Interface de login
        * Gestion des états
        * Tests UI

   2. **Profil Utilisateur (10 jours)**
      - Couche Data
        * Modèles de données
        * Stockage local avec Hive
        * Gestion des préférences
      - Couche Domain
        * Use cases de gestion de profil
        * Export/Import
        * Synchronisation
      - Couche Presentation
        * Interface de profil
        * Gestion des préférences
        * Visualisations
        * Tests UI

   3. **Monitoring de la Batterie (10 jours)**
      - Couche Data
        * Communication Bluetooth/WiFi avec le diffuseur
        * Stockage des données de batterie
        * Gestion des alertes
      - Couche Domain
        * GetBatteryLevelUC (Récupère le niveau de charge actuel)
        * GetBatteryAutonomyUC (Calcule le temps d'autonomie restant)
        * SetBatteryAlertThresholdUC (Configure les seuils d'alerte)
        * OptimizeBatteryConsumptionUC (Optimise la consommation énergétique)
      - Couche Presentation
        * Affichage du niveau de batterie
        * Visualisation de l'autonomie
        * Gestion des notifications
        * Interface de configuration

   4. **Gestion de l'Éclairage (10 jours)**
      - Couche Data
        * Communication Bluetooth/WiFi avec le diffuseur
        * Stockage des préférences d'éclairage
        * Gestion des modes prédéfinis
      - Couche Domain
        * SetLightIntensityUC (Définit l'intensité lumineuse)
        * GetLightModeUC (Récupère le mode d'éclairage actuel)
        * SetLightModeUC (Configure le mode d'éclairage)
        * SetLightScheduleUC (Configure la programmation horaire)
        * SyncLightWithFanUC (Synchronise l'éclairage avec les ventilateurs)
      - Couche Presentation
        * Interface de contrôle de l'éclairage
        * Sélection des modes prédéfinis
        * Configuration des horaires
        * Visualisation des états

   5. **Contrôle des Ventilateurs (15 jours)**
      - Couche Data
        * Communication Bluetooth/WiFi avec le diffuseur
        * Stockage des configurations
        * Gestion des programmes
      - Couche Domain
        * SetFanSpeedUC (Définit la vitesse d'un ventilateur)
        * GetFanStatusUC (Récupère l'état d'un ventilateur)
        * SetFanProgramUC (Configure un programme personnalisé)
        * SetFanScheduleUC (Configure la programmation horaire)
        * SyncFansUC (Synchronise plusieurs ventilateurs)
      - Couche Presentation
        * Interface de contrôle des ventilateurs
        * Création de programmes
        * Visualisation des états
        * Configuration des séquences

### Analyse de Complexité des Features

1. **Contrôle des Ventilateurs** (Complexité Élevée)
   - Gestion de 3 ventilateurs indépendants
   - 3 niveaux de vitesse pour chaque ventilateur
   - Communication Bluetooth/WiFi en temps réel
   - Programmes personnalisés et planification
   - Monitoring et statistiques d'utilisation
   - Gestion des erreurs et sécurité
   - Synchronisation avec l'interface tactile du diffuseur
   - Temps estimé : 15 jours

2. **Gestion de l'Éclairage** (Complexité Moyenne)
   - Contrôle d'intensité simple
   - Modes prédéfinis
   - Effets lumineux basiques
   - Programmation horaire
   - Communication Bluetooth/WiFi
   - Synchronisation avec les ventilateurs
   - Temps estimé : 10 jours

3. **Monitoring de la Batterie** (Complexité Moyenne)
   - Lecture des données de batterie en temps réel
   - Calculs d'autonomie avec prédictions
   - Alertes configurables
   - Communication Bluetooth/WiFi
   - Optimisation énergétique avancée
   - Temps estimé : 10 jours

**Durée totale estimée : 65 jours**

### Tests et Qualité
Les tests sont intégrés à chaque étape du cycle de développement :
- **Data Layer** : Tests des modèles et datasources
- **Domain Layer** : Tests des use cases et entities
- **Presentation Layer** : Tests des BLoCs et UI
- **Tests Finaux** : Tests de performance, sécurité et UX

---
*Dernière mise à jour : 2025*

## Contact
**Jewel Cheriaa**
- Email: jewelcheriaa@gmail.com
- LinkedIn: [Jewel Cheriaa](https://www.linkedin.com/in/jewel-cheriaa/)
- Mobile: +216 24 226 712
- WhatsApp: +33 7 43 10 44 25

Pour plus d'informations sur mon expertise Flutter, consultez mon projet [Generic Requester](https://github.com/Jewelch/generic_requester).

## 6. Profil du Développeur {#profil-du-développeur}

### Présentation
Développeur Senior iOS & Flutter (TUNISIA) - Bilingue en Français et en Anglais  
Tech Lead et Architecte Logiciel  
Formateur Agréé chez IQClass et VosCours  
9 années d'expérience | 50+ applications créées | 35+ toujours en ligne sur l'App Store

### Expertise Technique
Passionné et axé sur les résultats, avec un solide historique de création d'applications de haute qualité. Expert en :
- Programmation générique
- Clean Architecture
- UIKit, SwiftUI, Combine (iOS)
- State Management Patterns (BLoC, Riverpod, Provider, Getx, Mobx, Modular)
- Automatisation et tests (Unitaires, UI, Intégration, E2E)
- Optimisation des performances
- Maintenabilité du code
- Intégration de systèmes complexes

### Applications Notables
* My Swiss Keeper (Swisse) - [App Store](https://apps.apple.com/fr/app/my-swiss-keeper/id1617620449)
* RTA Dubai (UAE Roads & Transport Authority) (+1M utilisateurs) - [App Store](https://apps.apple.com/ae/app/rta-dubai/id426109507)
* Maskan (UAE Federal Tax Authority) (+1K utilisateurs) - [App Store](https://apps.apple.com/us/app/maskan-fta/id6478710219)
* IRP AUTO Santé (FRANCE) (+100K utilisateurs) - [App Store](https://apps.apple.com/fr/app/irp-auto-sant%C3%A9/id948623366?l=en) | [Play Store](https://play.google.com/store/apps/details?id=com.irpauto.sante&hl=en_US)
* Ville de Marseille (FRANCE) (+10K utilisateurs) - [App Store](https://apps.apple.com/fr/app/ville-de-marseille/id1267540404?platform=iphone)
* Zenpark - Parkings (FRANCE) (+500K utilisateurs) - [App Store](https://apps.apple.com/fr/app/zenpark-parkings/id757934388)
* iHealth MyVitals (FRANCE) (+100K utilisateurs) - [App Store](https://apps.apple.com/us/app/ihealth-myvitals/id1532014748)
* Rides2U (USA) (+4K utilisateurs) - [App Store](https://apps.apple.com/us/developer/rides2u-llc/id1616957681) | [Play Store](https://play.google.com/store/apps/dev?id=7213610952804159810)
* Halal App (KSA) - [App Store](https://apps.apple.com/us/app/halal-app-%D8%AD%D9%84%D8%A7%D9%84/id1570293278)
* WALLPOST Software (USA) (+2K utilisateurs) - [App Store](https://apps.apple.com/fr/app/wallpost-software/id1044979110)
* TLFnet (+2K utilisateurs) - [Play Store](https://play.google.com/store/apps/details?id=com.tlfnet)
* Sketch AI: Drawing to Art (TURKEY) - [App Store](https://apps.apple.com/us/app/sketch-ai-drawing-to-art/id6447612551)
* Aligneurs Français (FRANCE) - [App Store](https://apps.apple.com/fr/app/aligneurs-francais/id1630781596?platform=iphone)
* Turaqi Captain & Client (KSA) - [App Store](https://apps.apple.com/fr/developer/atallah-almotairi/id1535395336)
* CODE: QR & Barcode Reader (FRANCE) - [App Store](https://apps.apple.com/us/app/code-qr-and-barcode-reader/id1073953713)
* KingaSafety (USA) - [App Store](https://apps.apple.com/us/app/kinga-safety/id6443869502)

### Leadership et Expertise
En plus de mon expertise technique, j'apporte une forte capacité de leadership et de travail en équipe, ayant occupé des postes clés dans plusieurs organisations :
- QoreVirtual (USA) - Architecte Software iOS, Android et Flutter
- BMW (Allemagne) - Développeur Android Senior
- SCUB (France) - Tech Lead Flutter
- Be-ys Software (France) - Tech Lead Flutter & iOS
- WiMobi (Tunisie) - Tech Lead iOS

### Contact
Développeur d'applications Mobile Senior  
Tech Lead / Architecte Logiciel / Formateur agréé  
Mobile: +216 24 226 712  
WhatsApp: +33 7 43 10 44 25  
Email: jewelcheriaa@gmail.com
