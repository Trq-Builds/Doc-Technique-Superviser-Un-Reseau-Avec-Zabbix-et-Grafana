# `📈`︲Doc-Technique-Superviser un réseau avec Zabbix et Grafana.

---

<p align="center">

![Debian](https://img.shields.io/badge/Debian-13-A81D33?style=for-the-badge&logo=debian&logoColor=white)
![Zabbix](https://img.shields.io/badge/Zabbix-Monitoring-CC0000?style=for-the-badge&logo=zabbix&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![SNMP](https://img.shields.io/badge/SNMP-Network%20Monitoring-0078D4?style=for-the-badge)
![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D6?style=for-the-badge&logo=windows&logoColor=white)
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

<a id="contexte-et-objectifs"></a>
### `❔`︲Contexte et objectifs du TP.
> [!NOTE]
> Ce projet s'inscrit dans le cadre du centre de formation Descartes-Bleu, qui doit assurer la continuité de service de 24 salles de travaux pratiques. Pour garantir le bon fonctionnement des nombreux services (DNS, Web, SSH, MariaDB, etc.) et des équipements réseaux (switchs, routeurs), une solution de supervision proactive et réactive est indispensable.
