# Installation et déploiement du lab Wazuh (AWS)

Ce document décrit les étapes d’installation et de déploiement du lab de supervision SIEM/EDR basé sur **Wazuh**, déployé sur **AWS Learner Lab**.  
Les commandes ont été exécutées manuellement durant le TP, mais sont regroupées ici à des fins de documentation et de reproductibilité.

---

## 1. Pré-requis

- Compte AWS (AWS Learner Lab)
- Accès à la console EC2
- Une paire de clés SSH
- Une IP publique fixe (ou connue) pour l’accès administrateur
- Connaissances de base en Linux et Windows Server

---

## 2. Déploiement de l’infrastructure AWS

### 2.1 Instances EC2

Trois instances EC2 sont créées dans le **même VPC** et le **même subnet public** :

| Instance       | OS               | Rôle                                     |
| -------------- | ---------------- | ---------------------------------------- |
| Wazuh-Server   | Ubuntu 22.04 LTS | SIEM / EDR (Manager, Indexer, Dashboard) |
| Linux-Client   | Ubuntu 22.04     | Endpoint Linux                           |
| Windows-Client | Windows Server   | Endpoint Windows                         |

---

### 2.2 Groupes de sécurité

- Un **Security Group dédié au serveur Wazuh**
- Un **Security Group pour les clients (Linux & Windows)**

Les règles détaillées sont documentées dans `aws_sg_tables.md`.

---

## 3. Installation de Wazuh (All-in-One)

Sur l’instance **Wazuh-Server (Ubuntu)** :

```bash
sudo apt update && sudo apt -y upgrade
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

**À la fin de l’installation, le script fournit** :

- L’**URL du Wazuh Dashboard**
- Le **compte administrateur** (admin)
- Le **mot de passe** généré automatiquement

### 3.1 Vérification des services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

## 4. Enrôlement du client Linux

Méthode recommandée : via le **Wazuh Dashboard**

1. Accéder au Dashboard
2. Agents Management → Summary → Deploy new agent
3. Sélectionner Linux
4. Copier et exécuter les commandes proposées sur le client Linux

Vérification :

```bash
sudo systemctl status wazuh-agent
```

## 5. Enrôlement du client Windows

### 5.1 Installation de l’agent Wazuh

Depuis le **Dashboard** :

- > Agents Management → Deploy new agent
- Sélectionner **Windows**
- Exécuter les commandes fournies dans **PowerShell (Admin)**

### 5.2 Vérification

Sur Windows :

- > Services → Wazuh Agent
- Statut : **Running**

## 6. Vérification des communications réseau

Ports essentiels :

| Port     | Usage               |
| -------- | ------------------- |
| 1514/TCP | Communication agent |
| 1515/TCP | Enrôlement          |
| 443/TCP  | Accès Dashboard     |
| 22/TCP   | SSH (Linux)         |
| 3389/TCP | RDP (Windows)       |

## 7. Validation du déploiement

Le lab est considéré fonctionnel lorsque :

- Les deux agents apparaissent **Active** dans le Dashboard
- Les événements Linux et Windows remontent correctement
- Les alertes SIEM sont visibles dans l’interface

---
