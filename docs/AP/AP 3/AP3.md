# Mission agencement de Windows

## Présentation de l'organisation

<span style="color:red"> La société M2L gère les clubs sportifs et fournit toute la logistique informatique nécessaire aux différentes ligues.</span>

<span style="color:red">L’organisation du système informatique est constituée du siège à BLOIS et de différentes agences réparties abritant les différentes ligues.</span> 

<span style="color:red">La société M2L installe en fonction de l’agence, le système informatique de base qui est constitué en général des éléments suivants :</span>

	
- <span style="color:red">Des postes client de l’agence (30 à 100 postes). Fonctionnement sous linux </span>

- <span style="color:red">Poste Administrateur </span>

- <span style="color:red">Serveur WEB en DMZ qui héberge les informations de la ligue locale qui n’est pas implanté </span>

- <span style="color:red">Serveur Proxy qui filtre et journalise les accès </span>

- <span style="color:red">Tous les serveurs sont accessibles en SSH depuis le LAN </span>

- <span style="color:red">un pare feu sous pfsense pour le filrage périmétrique LAN INTERNET DMZ </span>


<span style="color:red">Un VPN relie les agences aux sièges.</span>

	 
<span style="color:red">Le siège fournit les services suivants aux agences :</span>

	- Gestion du parc et des tickets avec GLPI 

	- Supervision avec Centreon 

	Gestion centralisé des logs avec GRAYlog 

	Gestion de la sauvegarde – OpenMediaVault  



- <span style="color:red">Le siège lui-même est composé de poste de travail sous Windows 10 , de deux contrôleurs de domaine – M2L.loc -et d’un serveur de fichiers qui stocke les dossier utilisateurs et des groupes de travail du siège.</span>

## Schéma du SI 

![Schema Si](../images/Schema_SI.JPG)
 
## Présentation du groupe et le travaille en mode projet  :  

<span style="color:green">Pour cette situation, on était 3, on s'est réparti les tâches en fonction des serveurs. Personnellement j’ai travaillé principalement sur l’AD1 et l’AD2. Pour s’organiser on a utilisé l’outil Trello pour se répartir les tâches. Après chaque tâche, nous avons réalisé une doc et suite à cela ont procédé à une étape de validation pour valider les tâches.</span>

 

## Objectif : 

<span style="color:orange">Sur le siège niveau sureté et sécurité il n’y avait aucun moyen mis en place pour cela. C’est pour cela qu’on nous a demandé d’installer un deuxième AD pour répondre aux tolérances aux pannes. Ensuite on devait réorganiser l’AD1 en créant des OU, des GPO.</span>

## Présentation de la situation 

### Organisation 

   - J'ai consulté des fiches PDF fournies par mon professeur et effectué des recherches sur des sujets tels que les navigateurs internet et les intelligences artificielles comme ChatGPT. 

 

### Création des nouveaux OU (Unités d'Organisation) : 

   - J'ai créé des unités d'organisation pour regrouper différents types d'utilisateurs tels que Admin, Internes, Externe et Invités. 

   - J'ai également créé des groupes tels que Rugby, Foot, Équitation et Basket dans ces unités. 

   - Dans ces OU j’ai pu intégrer mes utilisateurs  

 

### Ajout des différents rôles : 

   - Mon serveur AD a des rôles tels que DHCP et DNS. 

 

#### La plage dhcp :  

 

192.168.10.34 à 192.168.10.40 

 

#### Les dns :  

 

192.168.10.17 et 192.168.10.19 


### Création de GPO (Objets de Stratégie de Groupe): 

   - J'ai mis en place des GPO pour gérer les critères de mots de passe et créer des lecteurs réseau pour les utilisateurs et les groupes 

 

### Installation du Serveur AD 2: 192.168.10.19 

   - J'ai installé un deuxième serveur AD pour assurer la tolérance aux pannes. 

   - J'ai effectué des étapes telles que la configuration IP, la jonction au domaine, la configuration DNS et la mise en place d'un cluster DHCP. 

    - Tous les rôles installer ce réplique sur l’AD1 en cas de modification  

 

### Serveur de Fichiers: 

   - Mon collègue a installé un serveur permettant de partager des fichiers, de contrôler les accès et de gérer les ressources, y compris l'impression. 

 

#### Étapes pour le Serveur de Fichiers : 

   - Mon collègue a configuré les paramètres IP (192.168.10.18) et ajouter le serveur au domaine. 

   - Mon collègue a créé les dossiers principaux "Perso" et "Groupe". 

   - Mon collègue a attribué des permissions (NTFS et CIFS) pour gérer l'accès local et le partage réseau. 

 

### Période de test :  

#### La réplication :   

On a fait une modification par exemple sur les OU sur l’AD2 pour voir si l’AD1 le prenait en compte. 


#### La tolérance aux pannes :  

On a éteint l’AD principale pour voir si les utilisateurs peuvent toujours joindre le domaine grâce au AD2 et vérifier s'il ont bien une adresse IP grâce au cluster de DHCP qu’on a pu mettre en place  


#### L’agent relais : 

On a modifié le fichier de configuration en enlevant 1 des DNS pour savoir si les AD reprenait bien un des deux DNS 


#### DNS :

Effectuer un nslookup  pour vérifier les différents DNS 



