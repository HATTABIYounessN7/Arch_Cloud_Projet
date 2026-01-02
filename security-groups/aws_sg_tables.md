# Règles des Security Groups AWS

Ce document décrit les règles de sécurité appliquées aux différentes instances EC2 du lab Wazuh.

---

## 1. Security Group – Wazuh Server

### Inbound Rules

| Type       | Protocole | Port | Source     | Description           |
| ---------- | --------- | ---- | ---------- | --------------------- |
| SSH        | TCP       | 22   | IP Admin   | Accès administrateur  |
| HTTPS      | TCP       | 443  | IP Admin   | Accès Dashboard Wazuh |
| Custom TCP | TCP       | 1514 | SG Clients | Communication agents  |
| Custom TCP | TCP       | 1515 | SG Clients | Enrôlement agents     |

### Outbound Rules

| Type        | Protocole | Port | Destination | Description      |
| ----------- | --------- | ---- | ----------- | ---------------- |
| All traffic | All       | All  | 0.0.0.0/0   | Sortant autorisé |

---

## 2. Security Group – Linux Client

### Inbound Rules

| Type | Protocole | Port | Source   | Description          |
| ---- | --------- | ---- | -------- | -------------------- |
| SSH  | TCP       | 22   | IP Admin | Accès administrateur |

### Outbound Rules

| Type        | Protocole | Port | Destination | Description      |
| ----------- | --------- | ---- | ----------- | ---------------- |
| Custom TCP  | TCP       | 1514 | SG Wazuh    | Envoi logs       |
| Custom TCP  | TCP       | 1515 | SG Wazuh    | Enrôlement       |
| All traffic | All       | All  | 0.0.0.0/0   | Sortant autorisé |

---

## 3. Security Group – Windows Client

### Inbound Rules

| Type | Protocole | Port | Source   | Description          |
| ---- | --------- | ---- | -------- | -------------------- |
| RDP  | TCP       | 3389 | IP Admin | Accès administrateur |

### Outbound Rules

| Type        | Protocole | Port | Destination | Description      |
| ----------- | --------- | ---- | ----------- | ---------------- |
| Custom TCP  | TCP       | 1514 | SG Wazuh    | Envoi logs       |
| Custom TCP  | TCP       | 1515 | SG Wazuh    | Enrôlement       |
| All traffic | All       | All  | 0.0.0.0/0   | Sortant autorisé |

---
