# `📈`︲Doc-Technique-Superviser un réseau avec Zabbix et Grafana.

---

<p align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue?style=for-the-badge)
![Zabbix](https://img.shields.io/badge/Zabbix-6.0%20LTS-CC0000?style=for-the-badge&logo=zabbix&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-9.4-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

</p>

---

Ce dépôt présente un guide complet pour la mise en place d’une infrastructure de supervision réseau avec Zabbix et Grafana sous Debian 13. Il couvre l’installation, la configuration et l’intégration des différents outils de monitoring. Tu y apprendras à déployer le serveur Zabbix, à connecter Grafana pour la visualisation des métriques, et à superviser efficacement des serveurs Linux, Windows ainsi que des équipements réseau grâce au protocole SNMP.

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
> Pour installer Debian 13, vous pouvez, si besoin, vous référer au guide suivant. Il détaille les différentes étapes de l’installation et pourra éventuellement vous être utile comme support : [Lien vers la Documentation](https://github.com/Trq-Builds/Doc-Technique-installation-Debian-13-sans-interface-graphique)

---

<a id="installation-infrastructure"></a>
# `🛠️`︲Installation de l’infrastructure de supervision.

-----

> [\!NOTE]
> Cette section détaille la mise en place du cœur de notre système : **Zabbix** pour la collecte et le traitement des données, et **Grafana** pour une visualisation haute fidélité.
> L'objectif est d'obtenir une stack opérationnelle, sécurisée et interconnectée.

-----

<a id="installation-zabbix"></a>
## `🐧`︲Installation de Zabbix sur Debian 13 (Trixie).

---

> [!IMPORTANT]
> **Note technique :** Pour Debian 13, nous utilisons la version Zabbix 7.0 LTS. L'installation nécessite une attention particulière sur les dépendances `libldap` et la configuration des fonctions MariaDB.

1️⃣︲**Configuration du dépôt et installation**
```bash
# Téléchargement du paquet de configuration du dépôt Zabbix 7.0
wget [https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_7.0-2+debian13_all.deb](https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_7.0-2+debian13_all.deb)
dpkg -i zabbix-release_7.0-2+debian13_all.deb
apt update

# Installation des composants serveurs, web et SQL
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
````

2️⃣︲**Préparation de la base de données MariaDB**

```sql
-- Connexion en root
mysql -u root

-- Création de la base et de l'utilisateur
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER zabbix@localhost IDENTIFIED BY 'password_zabbix';
GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost;

-- Autorisation pour l'importation du schéma (Crucial)
SET GLOBAL log_bin_trust_function_creators = 1;
QUIT;
```

3️⃣︲**Importation du schéma et finalisation**

```bash
# Importation des tables Zabbix
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

# Liaison du mot de passe dans la configuration serveur
sed -i 's/# DBPassword=/DBPassword=password_zabbix/' /etc/zabbix/zabbix_server.conf

# Activation des services
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

---

## `🌐`︲Finalisation via l'interface Web (Setup Wizard).

1️⃣︲**Vérification des prérequis**
Accès via `http://172.16.X.10/zabbix`. L'assistant vérifie la configuration PHP.

<details>
  <summary><strong>📸︲Capture d'écran.</strong></summary>
  <img width="1346" height="816" alt="image" src="https://github.com/user-attachments/assets/a5b1885d-b70b-4fd2-9e74-503d54828c48" />
</details>

2️⃣︲**Connexion à la base de données**

  * **User :** `zabbix`
  * **Password :** `password_zabbix`

3️⃣︲**Validation du tableau de bord**
Connexion avec `Admin` / `zabbix`.

<details>
  <summary><strong>📸︲Capture d'écran.</strong></summary>
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/43e03359-d481-4c08-86d9-95a2f8979a2b" />
</details>


-----

<a id="installation-grafana"></a>
## `📊`︲Installation et interconnexion de Grafana.

-----

1️⃣︲**Installation du service**

```bash
# Ajout de la clé GPG et du dépôt officiel
mkdir -p /etc/apt/keyrings
curl [https://apt.grafana.com/gpg.key](https://apt.grafana.com/gpg.key) | gpg --dearmor | tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] [https://apt.grafana.com](https://apt.grafana.com) stable main" | tee /etc/apt/sources.list.d/grafana.list

# Installation et démarrage
apt update && apt install grafana -y
systemctl enable grafana-server --now
```

2️⃣︲**Configuration du Plugin Zabbix**

  * Installation : `grafana-cli plugins install alexanderzobnin-zabbix-app`
  * Redémarrage : `systemctl restart grafana-server`

3️⃣︲**Liaison Data Source**
Dans Grafana (`port 3000`), ajout de la source Zabbix :

  * **URL :** `http://localhost/zabbix/api_jsonrpc.php`
  * **Identifiants API :** `Admin` / `zabbix`

---

<a id="supervision-fog-zabbix"></a>
# `🖥️`︲Supervision du serveur FOG avec Zabbix.

---

> [!NOTE]
> L'objectif de cette mission est de déployer un agent de supervision sur le serveur FOG (Debian) afin de collecter les métriques de performance (CPU, RAM, Entrées/Sorties disque) et l'état des services critiques.

---

<a id="installation-agent-fog"></a>
## `📦`︲1. Installation de l’agent Zabbix.

L'agent Zabbix est installé directement sur le serveur cible (**srv-fog**) via les dépôts officiels.

```bash
# Mise à jour des dépôts et installation du paquet agent
apt update && apt install zabbix-agent -y
````

---


## `⚙️`︲2. Configuration du fichier zabbix\_agentd.conf.

Pour permettre la communication avec le serveur de supervision, le fichier de configuration de l'agent doit être modifié. Les directives suivantes assurent que l'agent accepte les requêtes provenant de **srv-zabbix** et s'identifie correctement.

```bash
# Édition du fichier de configuration
nano /etc/zabbix/zabbix_agentd.conf
```

**Modifications apportées :**

  * `Server=172.16.X.10` : Autorise le serveur Zabbix à interroger cet agent.
  * `ServerActive=172.16.X.10` : Définit le serveur pour les vérifications actives.
  * `Hostname=srv-fog` : Nom d'hôte unique utilisé pour l'appairage dans l'interface Web.

<!-- end list -->

```bash
# Redémarrage et activation du service au démarrage
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

---

## `➕`︲3. Déclaration de l’hôte dans Zabbix.

L'hôte doit être enregistré manuellement dans le serveur de supervision pour que les données soient traitées.

1.  **Navigation** : Data collection \> Hosts \> Create host.
2.  **Configuration technique** :
      * **Host name** : `srv-fog` (doit correspondre au Hostname du fichier .conf).
      * **Templates** : `Linux by Zabbix agent`.
      * **Host groups** : `Linux servers`.
      * **Interfaces** : `Agent` | IP : `172.16.X.2` | Port : `10050`.

---

## `🧪`︲4. Vérification du statut de supervision.

La validation finale s'effectue dans l'onglet **Hosts**. Après quelques secondes, la disponibilité de l'hôte est confirmée par le passage au vert de l'indicateur de protocole.

<details>
  <summary><strong>📸︲Capture d'écran.</strong></summary>
  <img width="1782" height="1057" alt="image" src="https://github.com/user-attachments/assets/2253cbf2-fca5-41f6-946a-61584c2942e5" />
</details>

---

<a id="supervision-fog-grafana"></a>
# `📊`︲Supervision du serveur FOG avec Grafana.

-----

> [\!NOTE]
> La source de données Zabbix étant configurée, l'objectif est d'exploiter les métriques collectées sur srv-fog au travers d'interfaces visuelles dynamiques et exploitables.

-----

## `⬇️`︲1. Importation du dashboard "Zabbix Full Server Status".

L'utilisation de tableaux de bord préconfigurés optimise le temps de déploiement. Le plugin Zabbix pour Grafana inclut des modèles standards éprouvés.

1.  **Navigation :** Dashboards \> Import.
2.  **ID du Dashboard :** Saisir l'identifiant `5363` (Zabbix - Full Server Status) fourni par la communauté, ou importer le fichier JSON natif du plugin Zabbix.
3.  **Configuration :**
      * **Name :** Zabbix - srv-fog Status.
      * **Zabbix\_Datasource :** Sélectionner la source de données Zabbix précédemment instanciée.
4.  **Validation :** Cliquer sur *Import*.

-----

<a id="personnalisation-panels"></a>
## `🧩`︲2. Personnalisation des panels.

(Le Dashbord est disponible dans le repos au format JSON)
Le dashboard importé est générique. Un filtrage strict est requis pour cibler exclusivement srv-fog et éviter la pollution visuelle.

### P0 : Configuration des Variables de Tri

Dans les paramètres du dashboard (icône engrenage ⚙️) \> Variables :

  * `Group` : Définir la requête sur `Linux servers`.
  * `Host` : Définir la requête sur `srv-fog`.

### P1 : Ajustement des Métriques (Query)

Pour chaque panel critique (CPU, RAM, I/O Disque), la syntaxe de la requête Zabbix doit être alignée :

| Champ Grafana | Valeur Requise |
| :--- | :--- |
| **Group** | `$group` |
| **Host** | `$host` |
| **Application** | Vide (ou spécifique au besoin) |
| **Item** | Élément précis (ex: `CPU utilization`) |

-----

<a id="supervision-ssh"></a>
## `🔐`︲3. Supervision du service SSH.

La surveillance de la disponibilité des processus critiques requiert une approche binaire (Up/Down). Le service SSH est audité via un check TCP natif.

### P1 : Vérification côté Zabbix (Prérequis)

S'assurer que le template `Linux by Zabbix agent` assigné à srv-fog remonte bien la clé d'item standard.

  * **Clé cible :** `net.tcp.service[ssh]`
  * **Type de retour attendu :** Numérique (0 ou 1).

### P2 : Création du Panel Stat dans Grafana

1.  **Création :** Cliquer sur *Add panel* en haut du dashboard.
2.  **Source de données :** Zabbix.
3.  **Paramétrage de la Query :**
      * Group : `Linux servers`
      * Host : `srv-fog`
      * Item : Saisir ou sélectionner `SSH Service Status` (lié à `net.tcp.service[ssh]`).
4.  **Configuration Visuelle (Panneau de droite) :**
      * Type de visualisation : **Stat**.
      * **Value mappings (Mappage des valeurs) :** Règle de décision logique.
          * Condition `1` -\> Texte : `Actif` -\> Couleur : `Vert`.
          * Condition `0` -\> Texte : `Inactif (CRITIQUE)` -\> Couleur : `Rouge`.

-----

