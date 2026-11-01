# Déploiement et administration de Microsoft Exchange Server 2019

## Présentation du projet

Ce projet consiste à déployer, configurer et administrer une infrastructure de messagerie d'entreprise basée sur **Microsoft Exchange Server 2019 CU15**, intégrée à un environnement Windows Server 2022 et Active Directory.

L'objectif est de mettre en place une infrastructure centralisée permettant la gestion des comptes utilisateurs, des boîtes aux lettres, des communications internes et externes, des ressources de calendrier ainsi que des politiques de sécurité liées à la messagerie.

Le projet couvre également l'administration avancée d'Exchange Management Shell (EMS), la gestion des autorisations RBAC, la configuration des protocoles de messagerie et la sauvegarde des bases de données.

## 1. Architecture de l'infrastructure

L'environnement est constitué de deux machines virtuelles Windows Server 2022 déployées sous VMware.

| Composant | Configuration |
|---|---|
| Contrôleur de domaine | DC23 |
| Serveur de messagerie | Exchange23 |
| Système d'exploitation | Windows Server 2022 Standard |
| Serveur de messagerie | Microsoft Exchange Server 2019 CU15 |
| Domaine Active Directory | contosoX.com |
| Réseau privé | 192.168.1.X/24 |
| Adresse IP du DC23 | 192.168.1.X |
| Adresse IP interne Exchange23 | 192.168.1.X|
| Espace de noms Exchange | webmail.contosoX.com |
| Environnement de virtualisation | VMware Workstation |

### Topologie logique

- **DC23 :** contrôleur de domaine Active Directory, serveur DNS et autorité de certification.
- **Exchange23 :** serveur Exchange, membre du domaine Active Directory, disposant de deux interfaces réseau : une interface privée et une interface connectée au réseau externe.

## 2. Technologies et outils utilisés

- Windows Server 2022
- Microsoft Exchange Server 2019 CU15
- Active Directory Domain Services (AD DS)
- Active Directory Certificate Services (AD CS)
- DNS
- Exchange Admin Center (EAC)
- Exchange Management Shell (EMS)
- Microsoft Outlook 2024
- Outlook on the Web (OWA)
- Exchange ActiveSync
- SMTP, POP3, IMAP4, MAPI over HTTP
- VMware Workstation
- Windows Server Backup
- Group Policy Management (GPO)

## 3. Travaux réalisés

### Phase 1 - Préparation et configuration des serveurs

- Création de deux machines virtuelles Windows Server 2022.
- Configuration des ressources matérielles et des interfaces réseau.
- Attribution des adresses IP statiques et configuration DNS.
- Configuration des interfaces réseau privée et externe.
- Désactivation d'IPv6 conformément aux exigences du laboratoire.
- Configuration des paramètres de sécurité Windows Server.
- Installation des VMware Tools.

### Phase 2 - Déploiement d'Active Directory

- Installation et configuration d'Active Directory Domain Services.
- Promotion de DC23 en contrôleur de domaine.
- Création du domaine `contosoX.com`.
- Configuration du service DNS.
- Modification des stratégies de groupe (GPO).
- Configuration des droits de connexion locale.
- Installation et configuration d'Active Directory Certificate Services.
- Déploiement d'une autorité de certification d'entreprise.

### Phase 3 - Préparation et installation d'Exchange Server

- Intégration d'Exchange23 au domaine Active Directory.
- Préparation de l'environnement pour Exchange Server 2019.
- Installation d'Exchange Server 2019 CU15.
- Création des enregistrements DNS A et MX.
- Configuration de l'espace de noms Exchange.
- Configuration des URL internes des services Exchange.
- Création et déploiement d'un certificat SSL/TLS.
- Affectation du certificat aux services IIS, SMTP, POP3 et IMAP4.
- Configuration d'Outlook Web Access (OWA).

### Phase 4 - Gestion des bases de données et des boîtes aux lettres

- Création des répertoires de stockage des bases de données et des journaux.
- Création de la base de données de boîtes aux lettres `DB23`.
- Configuration d'un quota de stockage de 3 Go.
- Création et activation des boîtes aux lettres utilisateurs.
- Création d'une boîte aux lettres de salle (`Room3309`).
- Configuration des approbations de réservation de salles.
- Création d'une boîte aux lettres partagée (`SharedMBX`).
- Attribution des autorisations d'accès.
- Création de boîtes aux lettres de dossiers publics.

### Phase 5 - Gestion des dossiers publics et des groupes de distribution

- Création d'une hiérarchie de dossiers publics.
- Activation de la messagerie pour le dossier public `Assurances`.
- Publication de messages et de pièces jointes dans les dossiers publics.
- Création de groupes de distribution dynamiques :
  - `Networking_teachers`
  - `Programming_teachers`
  - `Sales_Representatives`
- Configuration des filtres dynamiques selon l'attribut Active Directory `Department`.
- Utilisation d'Exchange Management Shell pour afficher les membres des groupes.

### Phase 6 - Gestion des listes d'adresses et des stratégies

- Création d'une liste d'adresses globale (`Teachers_GAL`).
- Création de listes d'adresses personnalisées.
- Création d'un carnet d'adresses hors ligne (`IT_OAB`).
- Configuration d'une stratégie de carnet d'adresses (`IT_ABP`).
- Attribution de la stratégie à une boîte aux lettres utilisateur.
- Création d'un domaine accepté supplémentaire (`nwtraders.msft`).
- Configuration d'une stratégie d'adresse e-mail.
- Création et attribution d'une stratégie OWA personnalisée.
- Configuration des MailTips pour certains utilisateurs.

### Phase 7 - Gestion des permissions et du contrôle d'accès

- Configuration des autorisations Full Access.
- Attribution des permissions Send on Behalf.
- Configuration des permissions d'administration des destinataires.
- Mise en œuvre du modèle Role-Based Access Control (RBAC).
- Attribution de permissions spécifiques à un utilisateur pour administrer certains attributs des comptes Sales.

### Phase 8 - Configuration des protocoles et des clients de messagerie

- Configuration et démarrage des services POP3 et IMAP4.
- Configuration de profils de messagerie Microsoft Outlook :
  - MAPI pour Fateh23.
  - POP3 pour Andrew23.
  - IMAP4 pour Hamid23.
- Configuration d'un connecteur d'envoi SMTP.
- Tests d'envoi et de réception de courriels.
- Vérification des communications entre utilisateurs via OWA.

### Phase 9 - Sécurité et gestion des appareils mobiles

- Configuration des paramètres d'accès Exchange ActiveSync.
- Mise en quarantaine des nouveaux appareils mobiles.
- Configuration des notifications destinées aux administrateurs.
- Création de règles d'accès pour autoriser ou bloquer des appareils selon leurs caractéristiques.
- Configuration des politiques de sécurité des appareils mobiles.

### Phase 10 - Sauvegarde et restauration

- Installation de la fonctionnalité Windows Server Backup.
- Configuration d'une sauvegarde de la base de données Exchange `DB23`.
- Vérification des opérations de sauvegarde.

## 4. Compétences techniques développées

**Administration systèmes**
- Installation et administration de Windows Server 2022.
- Gestion des services Active Directory et DNS.
- Gestion des utilisateurs, groupes et unités organisationnelles.
- Configuration des stratégies de groupe.

**Administration de messagerie**
- Déploiement et configuration de Microsoft Exchange Server.
- Gestion des bases de données et des boîtes aux lettres.
- Administration des protocoles SMTP, POP3, IMAP4 et MAPI.
- Gestion des dossiers publics et des ressources de calendrier.

**Sécurité informatique**
- Gestion des certificats numériques.
- Configuration des politiques de sécurité des appareils mobiles.
- Gestion des permissions et du contrôle d'accès.
- Administration basée sur les rôles (RBAC).

**Automatisation et administration**
- Utilisation d'Exchange Management Shell.
- Administration PowerShell des objets Exchange.
- Configuration de règles et de stratégies de messagerie.
- Gestion des sauvegardes de données.

## 5. Validation et tests

Les fonctionnalités de l'infrastructure sont validées à travers différents scénarios :

- Vérification de la résolution DNS.
- Vérification de l'intégration au domaine Active Directory.
- Connexion aux boîtes aux lettres via OWA.
- Échanges de courriels entre utilisateurs.
- Tests des profils Outlook MAPI, POP3 et IMAP4.
- Validation des réservations de salles.
- Vérification des permissions des boîtes aux lettres.
- Vérification des groupes de distribution dynamiques.
- Vérification des stratégies d'accès mobile.
- Validation de la sauvegarde de la base de données.


