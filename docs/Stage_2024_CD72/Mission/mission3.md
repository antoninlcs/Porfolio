# Installation d'un serveur OPNsense ( DNS-DHCP-VPN)

## Le contexte

Ce besoin est apparu à la fin du projet car on a decouvert l'option de pouvoir créer des réseaux virtuels et donc d'isoler les VM. 


## Objectifs 

On a crée un SDN ( réseaux virtuel) dans proxmox qui va permettre d'isolé encore plus les vms. C'est pour cela qu'on a choisi ce service qui nous permetait de mettre en place un dhcp ainsi qu'un dns pour ce réseaux virtuel. Seul problème est que notre site ne pouvait pas joindre n'y se connecter en ssh notre réseaux virtuel. C'est pour cela qu'on a décidé moi-même et les administrateurs réseaux et systèmes de mettre en place un VPN ainsi qu'un dhcp et son propre serveur dns.

## Réalisation 

J'ai réaliser une documentation que vous pouvez consulté dans **Stage 2024 CD 72>OPNsense**

Voici aussi un schéma pour rssayer de comprendre le processus 

![schéma_vpn](../../images/schéma_vpn.drawio.jpg)