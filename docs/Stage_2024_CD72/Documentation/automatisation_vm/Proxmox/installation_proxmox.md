# Installation Proxmox VE 8.1

Pour cette documentation je me suis inspiré de la documentation fournie par Proxmox

## Configuration requises 

- Intel EMT64 ou AMD64 avec indicateur CPU Intel VT/AMD-V.

- Mémoire : minimum 2 Go pour le système d'exploitation et les services Proxmox VE, plus la mémoire désignée pour les invités. Pour Ceph et ZFS, une mémoire supplémentaire est requise ; environ 1 Go de mémoire pour chaque To de stockage utilisé.

- Stockage rapide et redondant, les meilleurs résultats sont obtenus avec les SSD.

- Stockage du système d'exploitation : utilisez un RAID matériel avec cache en écriture protégé par batterie (« BBU ») ou non-RAID avec ZFS (SSD en option pour ZIL).

### Stockage de la VM :

- Pour le stockage local, utilisez soit un RAID matériel avec cache d'écriture sauvegardé par batterie (BBU), soit un non-RAID pour ZFS et Ceph. Ni ZFS ni Ceph ne sont compatibles avec un contrôleur RAID matériel.

- Le stockage partagé et distribué est possible.

- Cartes réseau (multi) Gbit redondantes, avec cartes réseau supplémentaires en fonction de la technologie de stockage préférée et de la configuration du cluster.

- Pour le relais PCI(e), le processeur doit prendre en charge l'indicateur VT-d/AMD-d.

## Attention 

Il faudra penser a activer la virtualisation dans votre BIOS/UEFI

[Liste touche bios](https://lecrabeinfo.net/liste-des-touches-pour-acceder-au-bios-uefi-acer-asus-dell-lenovo-hp.htm)

Ce paramètre dépendra de votre bios 

## Console d'installation Proxmox

Tout d'abord il faudra vous rendre sur le site proxmox et installer l'iso proxmox [site iso proxmox](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)


Une fois que l'iso sera boot sur votr machine via différent système (Idrac, clé bootable...), il vous sera affiché ceci : 

![console-proxmox](../images/console_proxmox.jpg)

Pour plus de confort selectionner le mode console 

Ensuite il vous faudra accepter le contrat de licence : 

![Licence_proxmox](../images/contrat_licence_prox.jpg)

Il faudra ensuite cliquer sur **I agree**

## Configuration stockage

Ensuite, vous allez arriver sur cette page 

![disque_proxmox](../images/disk_prox.jpg)

Vous allez avoir 2 options qui s'offre à vous :

- 1ère option : Vous ne voulez pas de redondance donc vous utilisez qu'un seul disque mais le risque est que si votre disque tombe vous ne pourrez plus accéder a proxmox et n'y récupérer c'est donner ( Donc utliser de préférence un disque plus petit pour le système ).
  
- 2ème option : Vous voulez faire une backup ou de la redondance il sera préférable d'utliser **ZFS** qui correspond a différents types de **RAID**.

Choissisez l'option qui correspond à vos besoins.

## Configuration Country 

![country_prox](../images/country_prox.jpg)

Il faudra juste choisir dans quel pays vous résidez pour le fuseau horaire et la langue.

## Création compte root 

![compte_prox](../images/compte_prox.jpg)

Il faudra renseignez un mot de passe fort et votre adresse mail pour être prevenu en cas de souci avec proxmox.

## Configuration réseaux 

Pour la configuration réseaux vous aurez ceci :

![reseau_prox](../images/reseau_prox.jpg)
 
Il vous suffira de selectionner votre bonne carte réseau ainsi que le nom de votre serveur proxmox et ses paramètres IP 


## Recap de l'installation 

![récap_prox.jpg](../images/récap_prox.jpg)

Voici le recap de votre installation 

Ensuite, une fois que vous avez bien vérifier votre récapitulatif vous pouvez lancer votre installation en cliquant sur **Install**

## Installation Proxmox 

Une fois que vous allez appuyer sur **Install** vous verrez cette fenêtre :

![install_proxmox](/docs/images/install_prox.jpg)

Proxmox est entrain de s'installer sur votre machine cela prendra quelques minutes


## Fin d'installation 

Une fois votre proxmox installer vous verrez cette fênetre apparaitre : 

![url_prox](../images/url_prox.jpg)

Ceci sera votre url pour accèder pour accèder à l'interface de proxmox sur un navigateur web (Firefox, Chrome)

## Se connecter sur l'interface Web proxmox

Rentrez l'url qui vous avez était donner lors de votre installation : **"http://ip_machine:8086"**

Une fois cela rentrer vous verrez ceci : 

![connexion_web_prox](../images/connection_web_prox.jpg)







