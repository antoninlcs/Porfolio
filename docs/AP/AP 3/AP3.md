# Présentation de l’organisation 

 


<span style="color:red"> La société M2L gère les clubs sportifs et fournit toute la logistique informatique nécessaire aux différentes ligues. 

L’organisation du système informatique est constituée du siège à BLOIS et de différentes agences réparties abritant les différentes ligues. 

La société M2L installe en fonction de l’agence, le système informatique de base qui est constitué en général des éléments suivants : 

	
- Des postes client de l’agence (30 à 100 postes). Fonctionnement sous linux 

- Poste Administrateur 

- Serveur WEB en DMZ qui héberge les informations de la ligue locale qui n’est pas implanté 

- Serveur Proxy qui filtre et journalise les accès 

- Tous les serveurs sont accessibles en SSH depuis le LAN 

- un pare feu sous pfsense pour le filrage périmétrique LAN INTERNET DMZ 


Un VPN relie les agences aux sièges. 

	 
Le siège fournit les services suivants aux agences :

	- Gestion du parc et des tickets avec GLPI 

	- Supervision avec Centreon 

	Gestion centralisé des logs avec GRAYlog 

	Gestion de la sauvegarde – OpenMediaVault  

 
 

- Le siège lui-même est composé de poste de travail sous Windows 10 , de deux contrôleurs de domaine – M2L.loc -et d’un serveur de fichiers qui stocke les dossier utilisateurs et des groupes de travail du siège. </span>

 
# Présentation du groupe :  

Pour cette situation, on était 3, on s'est réparti les tâches en fonction des serveurs. Personnellement j’ai travaillé principalement sur l’AD1 et l’AD2. Pour s’organiser on a utilisé l’outil Trello pour se répartir les tâches. Après chaque tâche, nous avons réalisé une doc et suite à cela ont procédé à une étape de validation pour valider les tâches. 

 

Objectif : 

 

Sur le siège niveau sureté et sécurité il n’y avait aucun moyen mis en place pour cela. C’est pour cela qu’on nous a demandé d’installer un deuxième AD pour répondre aux tolérances aux pannes. Ensuite on devait réorganiser l’AD1 en créant des OU, des GPO. 

 

 

Présentation de la situation 

 

1. Organisation 

   - J'ai consulté des fiches PDF fournies par mon professeur et effectué des recherches sur des sujets tels que les navigateurs internet et les intelligences artificielles comme ChatGPT. 

 

2. Création des nouveaux OU (Unités d'Organisation) : 

   - J'ai créé des unités d'organisation pour regrouper différents types d'utilisateurs tels que Admin, Internes, Externe et Invités. 

   - J'ai également créé des groupes tels que Rugby, Foot, Équitation et Basket dans ces unités. 

   - Dans ces OU j’ai pu intégrer mes utilisateurs  

 

3. Ajout des différents rôles : 

   - Mon serveur AD a des rôles tels que DHCP et DNS. 

 

3.1 La plage dhcp :  

 

192.168.10.34 à 192.168.10.40 

 

3.2 Les dns :  

 

192.168.10.17 et 192.168.10.19 

 

3.3 L’agent relais :  

 

 

 

4. Création de GPO (Objets de Stratégie de Groupe): 

   - J'ai mis en place des GPO pour gérer les critères de mots de passe et créer des lecteurs réseau pour les utilisateurs et les groupes 

 

4.1 La stratégie des mots de passe  

 

 

5. Installation du Serveur AD 2: 192.168.10.19 

   - J'ai installé un deuxième serveur AD pour assurer la tolérance aux pannes. 

   - J'ai effectué des étapes telles que la configuration IP, la jonction au domaine, la configuration DNS et la mise en place d'un cluster DHCP. 

    - Tous les rôles installer ce réplique sur l’AD1 en cas de modification  

 

6. Serveur de Fichiers: 

   - Mon collègue a installé un serveur permettant de partager des fichiers, de contrôler les accès et de gérer les ressources, y compris l'impression. 

 

7.1 Étapes pour le Serveur de Fichiers : 

   - Mon collègue a configuré les paramètres IP (192.168.10.18) et ajouter le serveur au domaine. 

   - Mon collègue a créé les dossiers principaux "Perso" et "Groupe". 

   - Mon collègue a attribué des permissions (NTFS et CIFS) pour gérer l'accès local et le partage réseau. 

 

7. Période de test :  

 

7.1 La réplication :  

 

On a fait une modification par exemple sur les OU sur l’AD2 pour voir si l’AD1 le prenait en compte. 

 

7.2. La tolérance aux pannes :  

 

On a éteint l’AD principale pour voir si les utilisateurs peuvent toujours joindre le domaine grâce au AD2 et vérifier s'il ont bien une adresse IP grâce au cluster de DHCP qu’on a pu mettre en place  

 

7.3 L’agent relais  

 

On a modifié le fichier de configuration en enlevant 1 des DNS pour savoir si les AD reprenait bien un des deux DNS 

 

 

7.4 DNS  

 

Effectuer un nslookup  pour vérifier les différents DNS 