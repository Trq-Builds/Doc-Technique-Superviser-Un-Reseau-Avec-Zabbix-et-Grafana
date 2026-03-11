# `📈`︲Doc-Technique-Superviser un réseau avec Zabbix et Grafana.

---

<p align="center">

![Debian](https://img.shields.io/badge/Debian-13-A81D33?style=for-the-badge&logo=debian&logoColor=white)
![Zabbix](https://img.shields.io/badge/Zabbix-Monitoring-CC0000?style=for-the-badge&logo=zabbix&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![SNMP](https://img.shields.io/badge/SNMP-Network%20Monitoring-0078D4?style=for-the-badge)
![Cisco](https://img.shields.io/badge/Cisco-Switch%202960-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)

</p>

---

Ce dépôt présente un guide complet pour la mise en place d’une infrastructure de supervision réseau avec Zabbix et Grafana sous Debian 13. Il couvre l’installation, la configuration et l’intégration des différents outils de monitoring. Tu y apprendras à déployer le serveur Zabbix, à connecter Grafana pour la visualisation des métriques, et à superviser efficacement des serveurs Linux, Windows ainsi que des équipements réseau grâce au protocole SNMP !

---

## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction.](#introduction)
   * [`❔`︲Contexte et problématique.](#contexte-problematique)
   * [`🎯`︲Objectifs de l’intervention.](#objectifs-intervention)
   * [`🧰`︲Présentation des outils utilisés.](#presentation-outils)

---

2. [`🛠️`︲Installation de l’infrastructure de supervision.](#installation-infrastructure)
   * [`🐧`︲Installation de Zabbix sur Debian 13.](#installation-zabbix)
   * [`📊`︲Installation de Grafana.](#installation-grafana)

---

3. [`🖥️`︲Supervision du serveur FOG avec Zabbix.](#supervision-fog-zabbix)
   * [`📦`︲Installation de l’agent Zabbix sur le serveur FOG.](#installation-agent-fog)
   * [`⚙️`︲Configuration de l’agent.](#configuration-agent)
   * [`➕`︲Ajout de l’hôte dans Zabbix.](#ajout-hote-zabbix)

*(La suite du sommaire reste identique...)*

---

<a id="introduction"></a>
# `📘`︲Introduction.

---

<a id="contexte-problematique"></a>
### `❔`︲Contexte et problématique.
Ce projet s'inscrit dans le cadre du centre de formation **Descartes-Bleu**, qui doit assurer la continuité de service de 24 salles de travaux pratiques. Pour garantir le bon fonctionnement des nombreux services (DNS, Web, SSH, MariaDB, etc.) et des équipements réseaux (switchs, routeurs), une solution de supervision proactive et réactive est indispensable.

---

<a id="objectifs-intervention"></a>
### `🎯`︲Objectifs de l'intervention.
L'intervention vise à mettre en place une infrastructure de monitoring complète avec les buts suivants :
* **Installation et configuration** du duo Zabbix et Grafana.
* **Déploiement d'agents** sur des serveurs hétérogènes (Linux Debian et Windows Server).
* **Supervision réseau via SNMP** pour les équipements actifs (Switch Cisco 2960).
* **Visualisation de données** par la création de tableaux de bord personnalisés sur Grafana.

---

<a id="presentation-outils"></a>
### ` 🧰 `︲Présentation des outils et prérequis.

> [!IMPORTANT]
> **Environnement technique :**
> - ` 🏟️ `︲**Zabbix :** *Solution de supervision Enterprise-class* ︲[`🌐`](https://www.zabbix.com/)
> - ` 📊 `︲**Grafana :** *Plateforme d'analyse et de visualisation* ︲[`🌐`](https://grafana.com/)
> - ` 🐧 `︲**Debian 13 :** *OS pour srv-monitoring et srv-fog*
> - ` 🪟 `︲**Windows Server 2022 :** *OS pour srv-ad1*
> - ` 🔌 `︲**Cisco 2960 :** *Matériel actif réseau (Switch)*
> - ` ☕ `︲**Beaucoup de café (ou de patience).**

---

<a id="installation-infrastructure"></a>
# `🛠️`︲Installation de l’infrastructure.

> [!NOTE]
> *Cette section (Mission 1 & 2) est en cours de rédaction. Elle détaillera l'installation des paquets serveurs sur Debian 13.*

---

<a id="supervision-fog-zabbix"></a>
# ` 🖥️ `︲Mission 3 : Superviser le serveur Linux (srv-fog).

> [!NOTE]
> **Objectif :** Déploiement de l'agent Zabbix sur le serveur de déploiement FOG et remontée des premières métriques vers notre serveur de supervision.

<a id="installation-agent-fog"></a>
#### 3.1. Installation de l'agent
Sur le serveur **srv-fog** (Debian) :
```bash
apt update && apt install zabbix-agent -y
