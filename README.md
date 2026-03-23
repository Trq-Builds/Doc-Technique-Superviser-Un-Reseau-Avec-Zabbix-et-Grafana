# `📈`︲Doc-Technique-Superviser un réseau avec Zabbix et Grafana.

---

<p align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue?style=for-the-badge)
![Zabbix](https://img.shields.io/badge/Zabbix-6.0%20LTS-CC0000?style=for-the-badge&logo=zabbix&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-9.4-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

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
   * [`🔌`︲Ajout et configuration du plugin Zabbix pour Grafana.](#plugin-zabbix-grafana)

   ---

3. [`🖥️`︲Supervision du serveur FOG avec Zabbix.](#supervision-fog-zabbix)

   * [`📦`︲Installation de l’agent Zabbix sur le serveur FOG.](#installation-agent-fog)
   * [`⚙️`︲Configuration du fichier zabbix_agentd.conf.](#configuration-agent)
   * [`➕`︲Ajout de l’hôte dans l’interface Zabbix.](#ajout-hote-zabbix)

   ---

4. [`📊`︲Supervision du serveur FOG avec Grafana.](#supervision-fog-grafana)

   * [`⬇️`︲Importation du dashboard "Zabbix Full Server Status".](#import-dashboard)
   * [`🧩`︲Personnalisation des panels.](#personnalisation-panels)
   * [`🔐`︲Supervision du service SSH.](#supervision-ssh)

   ---

5. [`🗄️`︲Supervision du service MariaDB.](#supervision-mariadb)

   * [`🔗`︲Ajout du template MySQL by Zabbix Agent.](#template-mysql)
   * [`📈`︲Importation du dashboard MySQL dans Grafana.](#dashboard-mysql)
   * [`⚙️`︲Adaptation des panels.](#adaptation-panels)

   ---

6. [`🪟`︲Supervision du serveur Windows SRV-AD1 dans Zabbix.](#supervision-windows-zabbix)

   * [`⬇️`︲Installation de l’agent Zabbix sur Windows Server 2022.](#installation-agent-windows)
   * [`👥`︲Création du groupe d’hôtes Windows.](#groupe-windows)
   * [`➕`︲Ajout du serveur dans Zabbix.](#ajout-serveur-windows)
   * [`🧪`︲Tests sur l’état du service DNS.](#tests-dns)

   ---

7. [`📊`︲Supervision du serveur SRV-AD1 dans Grafana.](#supervision-windows-grafana)

   * [`📈`︲Ajout des panels dans le dashboard Monitoring réseau.](#panels-windows)
   * [`⚙️`︲Configuration des métriques surveillées.](#configuration-metriques)

   ---

8. [`🌐`︲Préparation de la supervision SNMP.](#preparation-snmp)

   * [`📦`︲Installation des paquets SNMP sur srv-zabbix.](#installation-snmp)
   * [`⚙️`︲Configuration du fichier snmpd.conf.](#configuration-snmp)
   * [`🧪`︲Test du fonctionnement avec snmpwalk.](#test-snmp)

   ---

9. [`🔌`︲Préparation du switch Cisco 2960.](#preparation-switch)

   * [`🔗`︲Connexion au switch via console.](#connexion-switch)
   * [`🌐`︲Configuration de l’adresse IP.](#configuration-ip-switch)
   * [`🔐`︲Configuration de la communauté SNMP.](#configuration-communaute)

   ---

10. [`📡`︲Supervision du switch Cisco dans Zabbix.](#supervision-switch-zabbix)

   * [`👥`︲Création du groupe d’hôtes Switchs CISCO.](#groupe-switch)
   * [`➕`︲Ajout du switch dans Zabbix.](#ajout-switch)
   * [`🧩`︲Application du template CISCO IOS by SNMP.](#template-cisco)

   ---

11. [`📊`︲Supervision du switch Cisco dans Grafana.](#supervision-switch-grafana)

   * [`📈`︲Création du dashboard Switchs CISCO.](#dashboard-switch)
   * [`🧩`︲Ajout et configuration des panels.](#panels-switch)

   ---

12. [`⚡`︲Automatisation du dashboard de supervision.](#automatisation-dashboard)

   * [`📂`︲Création de la variable Group.](#variable-group)
   * [`💻`︲Création de la variable Host.](#variable-host)
   * [`🔄`︲Dashboard interactif multi-switchs.](#dashboard-interactif)

   ---

13. [`🧰`︲Outils et ressources utilisées.](#outils-ressources)

---

<a id="introduction"></a>
# `📘`︲Introduction.

---

<a id="contexte-problematique"></a>
### `❔`︲Contexte et objectifs du TP.
> [!NOTE]
> Ce projet s'inscrit dans le cadre du centre de formation Descartes-Bleu, qui doit assurer la continuité de service de 24 salles de travaux pratiques. Pour garantir le bon fonctionnement des nombreux services (DNS, Web, SSH, MariaDB, etc.) et des équipements réseaux (switchs, routeurs), une solution de supervision proactive et réactive est indispensable.

---

<a id="objectifs-intervention"></a>
### `🎯`︲Objectifs de l'intervention

> [!NOTE]
> L'intervention vise à mettre en place une infrastructure de monitoring complète avec les buts suivants :
> - Installation et configuration du duo Zabbix et Grafana.
> - Déploiement d'agents sur des serveurs hétérogènes (Linux Debian et Windows Server).
> - Supervision réseau via SNMP pour les équipements actifs comme le switch Cisco 2960.
> - Visualisation de données par la création de tableaux de bord personnalisés et interactifs sur Grafana.

---

<a id="presentation-outils"></a>
### ` 🧰 `︲Présentation des outils et prérequis.

> [!IMPORTANT]
> **Présentation des outils et prérequis :**
> - ` 🏟️ `︲**Zabbix :** Solution de supervision Enterprise-class ︲[`🌐`](https://www.zabbix.com/)
> - ` 📊 `︲**Grafana :** Plateforme d'analyse et de visualisation ︲[`🌐`](https://grafana.com/)
> - ` 🐬 `︲**MariaDB :** Système de gestion de base de données (MySQL) ︲[`🌐`](https://mariadb.org/)
> - ` 🐧 `︲**Debian 13 :** OS pour srv-zabbix ︲[`🌐`](https://www.debian.org/)
> - ` 🪟 `︲**Windows Server 2025 :** OS pour srv-ad1 ︲[`🌐`](https://www.microsoft.com/fr-fr/evalcenter/evaluate-windows-server-2022)
> - ` 🔌 `︲**Cisco 2960 :** Matériel actif réseau (Switch) ︲[`🌐`](https://www.cisco.com/)
> - ` 📦 `︲**Virtualisation :** VMware ︲[`🌐`](https://www.vmware.com/)

---

> [!NOTE]
> Pour installer Debian 13, vous pouvez, si besoin, vous référer au guide suivant. Il détaille les différentes étapes de l’installation et pourra éventuellement vous être utile comme support : [https://github.com/Trq-Builds/Doc-Technique-installation-Debian-13-sans-interface-graphique](https://github.com/Trq-Builds/Doc-Technique-installation-Debian-13-sans-interface-graphique)

---

<a id="installation-infrastructure"></a>
# `🛠️`︲Installation de l’infrastructure de supervision.

-----

> [\!NOTE]
> Cette section détaille la mise en place du cœur de notre système : **Zabbix** pour la collecte et le traitement des données, et **Grafana** pour une visualisation haute fidélité.
> L'objectif est d'obtenir une stack opérationnelle, sécurisée et interconnectée.

-----

<a id="installation-zabbix"></a>
## `🐧`︲Installation de Zabbix sur Debian 13.

> [\!IMPORTANT]
> **Prérequis Base de données :** Nous utiliserons **MariaDB**. Assure-toi que le service est installé et actif avant de lancer le script de schéma Zabbix.

## `🐬`︲Installation et configuration de MariaDB.

> [!NOTE]
> **Prérequis :** La mise en place de Zabbix nécessite une base de données relationnelle fonctionnelle (Étape C de la documentation officielle).

#### 1.1. Installation du service

Le serveur de base de données est installé sur la machine **srv-zabbix**.

```bash
# Mise à jour des dépôts et installation de MariaDB
apt update && apt install mariadb-server -y
systemctl enable --now mariadb
```

---

1️⃣︲**Ajout du dépôt officiel Zabbix (Debian 13)** On récupère la configuration spécifique pour Debian 13 (Trixie) pour éviter les conflits de dépendances :

```bash
# Téléchargement et installation du paquet de configuration du dépôt
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_7.0-2+debian13_all.deb
dpkg -i zabbix-release_7.0-2+debian13_all.deb
apt update
```

2️⃣︲**Installation du serveur, du frontend et de l'agent**

```bash
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
```

3️⃣︲**Initialisation de la base de données**
Connecte-toi à MariaDB pour créer l'utilisateur et la base de données dédiée :

```sql
mysql -u root
-- Dans la console MariaDB :
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password_zabbix';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```

4️⃣︲**Importation du schéma initial**
Cette commande structure la base de données avec les tables nécessaires au fonctionnement de Zabbix :

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

5️⃣︲**Configuration et démarrage du serveur**
Édite le fichier `/etc/zabbix/zabbix_server.conf` pour renseigner `DBPassword=password_zabbix`, puis redémarre les services :

```bash
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

---

<a id="installation-grafana"></a>
## `📊`︲Installation de Grafana.

---

> [\!TIP]
> Grafana permet de transformer vos données Zabbix en tableaux de bord dynamiques et esthétiques. L'installation via le dépôt officiel garantit les mises à jour de sécurité.

1️⃣︲**Installation des dépendances et du dépôt**

```bash
apt install -y apt-transport-https software-properties-common wget
mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | tee /etc/apt/sources.list.d/grafana.list
apt update
```

2️⃣︲**Installation et démarrage du service**

```bash
apt install grafana -y
systemctl enable grafana-server --now
```

---

<a id="plugin-zabbix-grafana"></a>
## `🔌`︲Ajout et configuration du plugin Zabbix.

-----

> [\!IMPORTANT]
> Pour que Grafana puisse "parler" à Zabbix, nous devons utiliser l'API de Zabbix via un plugin dédié.

1️⃣︲**Installation du plugin en ligne de commande**

```bash
grafana-cli plugins install alexanderzobnin-zabbix-app
systemctl restart grafana-server
```

2️⃣︲**Configuration de la Data Source (Interface Web)**
Rendez-vous sur `http://<IP_SERVEUR>:3000` (identifiants par défaut : `admin` / `admin`) :

  * **Menu :** `Connections` \> `Data Sources` \> `Add data source`.
  * **Type :** Rechercher `Zabbix`.
  * **URL :** `http://localhost/zabbix/api_jsonrpc.php`
  * **Auth :** Renseigne ton login/pass Zabbix (Admin / zabbix par défaut).
  * **Enable :** N'oublie pas d'activer le plugin dans Grafana (Administration \> Plugins \> Zabbix \> Enable).

---

> [!TIP]
> Ton infrastructure est maintenant prête. Les deux services communiquent.

---

<a id="finalisation-zabbix-web"></a>
## `🌐`︲Finalisation de la configuration via l'interface Web.

> [!IMPORTANT]
> Avant de commencer, assure-toi que le serveur Zabbix est lié à la base de données en éditant le fichier `/etc/zabbix/zabbix_server.conf` avec le paramètre `DBPassword=password_zabbix`.

1️⃣︲**Assistant d'installation (Setup Wizard)**
Accède à l'interface via `http://172.16.X.10/zabbix`. L'assistant vérifie les prérequis PHP.
* **Database connection** : Renseigne l'utilisateur `zabbix` et son mot de passe.
* **Zabbix server details** : Donne un nom à ton serveur (ex: `Zabbix-Descartes`).

<details>
  <summary>📸︲.</summary>
  <img width="1543" height="862" alt="image" src="https://github.com/user-attachments/assets/44074d14-0870-4e7b-93d3-0cd98cd43a66" />
</details>

2️⃣︲**Premier Dashboard et vérification du statut**
Connecte-toi avec les identifiants par défaut : `Admin` / `zabbix`.

> [!IMPORTANT]
> **Capture d'écran n°2 :** Le Widget "System information" sur le tableau de bord principal montrant que "Zabbix server is running" est à "Yes".

---

<a id="supervision-fog-zabbix"></a>
# `🖥️`︲Supervision du serveur FOG avec Zabbix.

---

<a id="installation-agent-fog"></a>
## `📦`︲Installation de l’agent Zabbix sur le serveur FOG.

---

> [!NOTE]
> Le serveur FOG étant une machine Debian, nous utilisons l'agent Zabbix classique pour faire remonter les métriques système (CPU, RAM, Disque).

1️⃣︲**Installation du paquet**
Sur la console du serveur FOG :
```bash
apt update && apt install zabbix-agent -y
```

2️⃣︲**Configuration du fichier agent**
Modification du fichier `/etc/zabbix/zabbix_agentd.conf` pour autoriser la connexion depuis le serveur de supervision :
* `Server=172.16.X.10`
* `Hostname=srv-fog`

```bash
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

3️⃣︲**Déclaration de l'Hôte dans Zabbix**
Dans l'interface Web : **Data collection** > **Hosts** > **Create host**.
* **Template utilisé** : `Linux by Zabbix agent`.

> [!IMPORTANT]
> **Capture d'écran n°3 :** La liste des hôtes montrant `srv-fog` avec l'icône **ZBX** allumée en vert.

---

<img width="1346" height="816" alt="image" src="https://github.com/user-attachments/assets/a5b1885d-b70b-4fd2-9e74-503d54828c48" />


Vrac : MDPs : Username : Admin (Majuscule importante)

Password : zabbix

