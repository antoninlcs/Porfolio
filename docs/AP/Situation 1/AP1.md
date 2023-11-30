# Etapes de réalisation

## Recherches
Concernant les recherches, notre professeur nous a fourni des fiches pdf. J'ai effectué des recherches sur des navigateurs internet ou des IA comme chatgpt.

## Installation du Serveur AD1

### Le domaine
Nous avons installer Active Directory, pour pouvoir crée le domaine **M2L** pour que les utilisateurs se connecte depuis n'importe quelle postes du siège.

## Les OU
Ensuite on crée des OU (Unité d'organisation) pour pouvoir y intégrer plusieurs types d'utilisateurs comme **Admin**, **Internes**, **Externe** et **Invités**

Dans les OU, on a aussi crée les groupes nécessaires qui était :

- **Rugby**
- **Foot**
- **Equitation**
- **Basket**

## Ajout des différents rôle 
 
 Notre serveur AD possèdent différent rôles comme : 

 - Le DHCP 
 - Le DNS 

## La création de GPO 

- On a crée une gpo pour la gestion des critères de mots de passe 
- On a crée une gpo pour crée des lecteurs réseaux pour chaque utilisateurs et groupes 

## Installation du serveur AD 2

### Pourquoi l'installer 

On a installer un 2ème serveur AD pour répondre aux exigences de la **Tolèrance aux pannes**. Ce qui permet aux client de pouvoir se connecter a leur session même si le DC1 est tomber en panne. 

### Etape à faire 

- Lui mettre des paramètres IP 
- Le joindre au domaine 
- Mettre les DNS 
- Mettre en place un clusteur DHCP 

## Serveur de Fichier 

### Pourquoi l'installer 

Ce serveur permet de partarger des fichiers, controler les accès et gèrer au mieux les ressources et gèrer l'impression


### Etapes à faire 

#### Le faire communiquer 

- Il faudra mettre les bon paramètres IP
- Il faudra le joindre le domaine correspondant 

#### Création des dossiers 

Il y a 2 dossier principal qui sont :

- Perso 
- Groupe 

Dans le dossier Perso, il y a chaque dossier de chaques utilisateurs présent dans le domaine 
Dans le dossier Groupe, il y chaque dossier correspondant aux noms des Groupes (**Rugby**, **Foot**,**Equitation** et **Basket**)

#### Permissions de partage et dossier 
 
- Il y a des droits ntfs qui servent d'avoir les accès en local 
- Il y a des droit cifs qui servent a partager au sein du réseau 


#### Service d'impression 





