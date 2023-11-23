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

### Ajout des différents rôle 
 
 Notre serveur AD possèdent différent rôles comme : 

 - Le DHCP 
 - Le DNS 

### La création de GPO 

- On a crée une gpo pour la gestion des critères de mots de passe 
- On a crée une gpo pour crée des lecteurs réseaux pour chaque utilisateurs et groupes 

## Installation du serveur AD 2

## Pourquoi l'installer 

On a installer un 2ème serveur AD pour répondre aux exigences de la **Tolèrance aux pannes**. Ce qui permet aux client de pouvoir se connecter a leur session même si le DC1 est tomber en panne. 

## Etape à faire 

- Lui mettre des paramètres IP 
- Le joindre aux domaines 
- Mettre les DNS 
- Mettre en place un clusteur DHCP 


