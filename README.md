Markdown
# Gestion-Parc-GLPI-OCS
**Projet Académique - Licence 3 ETSE**
**Etablissement : ISM Dakar**

## Présentation du projet
L'objectif de ce projet était de remplacer la gestion manuelle de parc informatique par une infrastructure automatisée. En collaboration avec mon groupe (Oumou et Simon), nous avons mis en place une solution capable de recenser l'intégralité des actifs matériels et logiciels en temps réel, tout en centralisant le support technique via une plateforme de ticketing.

## Stack Technique
L'architecture repose sur une interopérabilité entre environnements Linux et Windows :
* **Serveur Central :** Ubuntu Server (Stack LAMP : Apache, MariaDB, PHP).
* **Annuaire :** Windows Server avec Active Directory (intégration LDAP).
* **Agents :** OCS Inventory installé sur les postes clients Windows.

---

## Réalisation Technique

### 1. Collecte de données avec OCS Inventory NG
Le serveur OCS a été configuré pour recevoir les inventaires via le protocole HTTP. L'agent Windows transmet les configurations matérielles (RAM, CPU, BIOS) et logicielles au serveur Ubuntu situé à l'adresse `http://192.168.142.132/ocsinventory`.

### 2. Gestion et Ticketing avec GLPI v11
Nous avons déployé la version 11 de GLPI pour bénéficier des dernières fonctionnalités de gestion de parc. Cet outil permet de suivre le cycle de vie des équipements et de centraliser les demandes d'assistance.

### 3. Authentification Centralisée (LDAP)
Pour simplifier la gestion des accès, GLPI a été interconnecté avec l'Active Directory. Cela permet une synchronisation des comptes utilisateurs et une authentification unique basée sur les identifiants du domaine Windows.

---

## Difficultés Techniques et Résolution
Le point critique du projet a été l'installation du Plugin OCS. La version stable (2.1.7) présentait une incompatibilité majeure avec le noyau de GLPI 11.

**Solution appliquée :**
Pour résoudre ce blocage, j'ai récupéré la version de développement directement depuis les sources Git du projet :

```bash
# Suppression de la version incompatible
sudo rm -rf /var/www/html/glpi/plugins/ocsinventoryng

# Clonage de la branche de développement compatible
sudo git clone [https://github.com/pluginsGLPI/ocsinventoryng.git](https://github.com/pluginsGLPI/ocsinventoryng.git) ocsinventoryng

# Attribution des permissions au serveur web Apache
sudo chown -R www-data:www-data /var/www/html/glpi/plugins/ocsinventoryng
Cette étape a nécessité une veille technique approfondie pour identifier les dépendances nécessaires au bon fonctionnement de la passerelle.

Bilan du Projet
L'infrastructure est aujourd'hui totalement opérationnelle. Nous avons finalisé la configuration avec un domaine local dylan-oumou-simon.com. L'importation des machines depuis OCS vers GLPI est fluide, garantissant une base de données d'inventaire toujours à jour.

Document technique
Cliquer ici pour télécharger le rapport technique complet (PDF)
