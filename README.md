# Gestion-Parc-GLPI-OCS
Projet L3 ETSE - Inventaire automatisé et gestion de parc informatique.
🚀 Gestion de Parc & Inventaire Automatisé (Projet L3 - ETSE)
Projet réalisé à l'ISM Dakar par Dylan B.

💡 Le pourquoi du projet
L'idée était de sortir de la gestion manuelle sur Excel qui devient vite ingérable. Avec mes collègues (Oumou et Simon), on a voulu monter une infra capable de scanner tout un parc en temps réel. Le but : savoir exactement ce qui se passe sur chaque machine sans bouger de son siège, tout en centralisant le support technique.

🛠️ Ma Stack Technique
Pour ce lab, j'ai mixé du monde Linux et du monde Windows pour simuler une vraie entreprise :

Serveur Central : Ubuntu Server (Stack LAMP : Apache, MariaDB, PHP).

Annuaire : Windows Server avec Active Directory (pour que les collègues utilisent leur session Windows habituelle).

Agents : OCS Inventory installé sur les postes clients.

🏗️ Ce que j'ai mis en place
1. La collecte avec OCS Inventory NG
C'est le "moteur" de recherche. L'agent Windows envoie un fichier XML au serveur Ubuntu avec toute la config (RAM, CPU, logiciels installés). J'ai configuré le serveur pour qu'il écoute sur [http://192.168.142.132/ocsinventory](http://192.168.142.132/ocsinventory).

2. Le pilotage avec GLPI v11
C'est ici qu'on gère tout. J'ai choisi la v11 pour avoir l'interface la plus moderne. C'est là qu'on suit le cycle de vie du matos et qu'on gère les tickets de maintenance.

3. L'authentification LDAP
Pas question de créer 50 comptes. J'ai relié GLPI à l'Active Directory. Résultat : une seule base d'utilisateurs et une connexion simplifiée.

🚧 Les galères techniques (et comment je m'en suis sorti)
Le plus gros point de blocage a été le Plugin OCS. La version stable (2.1.7) ne voulait rien savoir avec GLPI 11 (incompatibilité de noyau).

Ma solution : Je suis allé chercher la version de développement directement sur le GitHub des développeurs de GLPI.

Bash
# On nettoie l'ancienne version qui buggait
sudo rm -rf /var/www/html/glpi/plugins/ocsinventoryng

# On récupère la version de dev compatible
sudo git clone https://github.com/pluginsGLPI/ocsinventoryng.git ocsinventoryng

# On remet les bons droits pour qu'Apache puisse l'exécuter
sudo chown -R www-data:www-data /var/www/html/glpi/plugins/ocsinventoryng
Ça a été une bonne leçon sur l'importance de vérifier la compatibilité des versions avant de se lancer !

🎯 Résultat final
Aujourd'hui, tout tourne. On a même configuré un domaine local dylan-oumou-simon.com. C'est super satisfaisant de voir une machine Windows apparaître automatiquement dans l'inventaire GLPI quelques secondes après l'installation de l'agent.
