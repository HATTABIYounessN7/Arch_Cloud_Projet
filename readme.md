# Atelier Sécurité des Endpoints & Supervision SIEM

### Étude de cas multi-OS (Linux & Windows) avec Wazuh

## Présentation du projet

Ce projet s’inscrit dans le cadre d’un atelier universitaire portant sur la **sécurité des endpoints**, la **supervision SIEM** et les **concepts EDR**, déployés dans un environnement **Cloud AWS**.

L’objectif principal est de mettre en place une architecture fonctionnelle intégrant :

- Un serveur **Wazuh All-in-One**,
- Un client **Linux (Ubuntu)**,
- Un client **Windows Server**,
  afin d’observer, centraliser et analyser des événements de sécurité réels à travers un tableau de bord SIEM.

Le projet met l’accent sur une approche **pratique et opérationnelle**, proche des environnements utilisés dans un SOC moderne.

---

## Architecture du lab

L’architecture repose sur une infrastructure AWS simple mais réaliste :

- **VPC AWS** avec subnet public
- **Internet Gateway** pour l’accès externe
- **3 instances EC2** :
  - Wazuh-Server (Ubuntu 22.04)
  - Linux-Client (Ubuntu 22.04)
  - Windows-Client (Windows Server)

Le schéma réseau détaillé est disponible dans le dossier [`/diagrams`](./diagrams).

---

## Composants techniques

### Wazuh Server (All-in-One)

- OS : Ubuntu 22.04 LTS
- Rôles :
  - Wazuh Manager
  - Wazuh Indexer
  - Wazuh Dashboard
- Fonction :
  - Centralisation des logs
  - Corrélation et détection des événements
  - Visualisation SIEM

### Linux Client

- OS : Ubuntu 22.04
- Agent : Wazuh Agent
- Événements observés :
  - Authentification SSH
  - Élévation de privilèges
  - Modifications de fichiers sensibles

### Windows Client

- OS : Windows Server
- Agent : Wazuh Agent
- Événements observés :
  - Échecs de connexion
  - Création d’utilisateurs
  - (Optionnel) Logs Sysmon pour EDR avancé

---

## Security Groups & Réseau

Les règles de sécurité AWS ont été configurées selon le principe du **moindre privilège**.

Les tableaux détaillés des Security Groups sont disponibles dans le dossier [`/security-groups`](./security-groups).

---

## Installation & Déploiement

> Les commandes ont été exécutées manuellement dans le cadre du lab.  
> Les étapes sont néanmoins documentées pour reproductibilité.

### Étapes principales :

1. Création des instances EC2 (AWS Learner Lab)
2. Configuration des Security Groups
3. Installation Wazuh All-in-One sur le serveur
4. Enrôlement des agents Linux et Windows via le Dashboard
5. Vérification des communications (ports 1514 / 1515)

Voir le dossier [`/installation`](./installation) pour le détail des étapes.

---

## Wazuh Dashboard

Le tableau de bord Wazuh permet de :

- Visualiser l’état des agents,
- Surveiller les événements de sécurité en temps réel,
- Effectuer des recherches avancées (Threat Hunting).

Des captures du Dashboard sont disponibles dans [`/screenshots`](./screenshots).

---

## SIEM vs EDR

Le projet illustre la complémentarité entre :

- **SIEM** : centralisation, corrélation et analyse des logs,
- **EDR** : surveillance approfondie des endpoints.

Wazuh combine ces deux approches en fournissant :

- Une visibilité globale (SIEM),
- Une détection fine des comportements suspects sur les hôtes (EDR).

---

## IAM / PAM – Scénarios de sécurité

Des scénarios de test ont été volontairement générés afin d’illustrer :

- Les échecs d’authentification,
- Les élévations de privilèges,
- Les changements d’utilisateurs et de groupes.

Ces événements sont détectés et remontés automatiquement par Wazuh.

---

## Threat Hunting

Trois requêtes de Threat Hunting ont été utilisées dans le Dashboard :

```text
agent.name: "Linux-Client" AND rule.groups: "authentication_failed"
agent.name: "Windows-Client" AND rule.id: "60122"
agent.name: "Windows-Client" AND data.win.system.eventID: "4720"
```
