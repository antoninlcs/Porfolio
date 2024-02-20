
# Pourquoi créer des templates

Il vous faut crée des templates dans prmox car l'outil cloud-init est compatible qu'en faisant des clones c'est pour cela qu'il faudra créer des templates

## Installer images cloud
Ceci ce fait avec quelques lignes de commandes : 

Tout d'abord il faudra vous connecter en ssh sur votre **serveur proxmox**

Ensuite il faudra télécharger une image cloud de votre disbutrion de votre choix 

Il faudra prendre la version de votre choix 

Il faudra prendre en **.img** ou **qcow2**

exemple : [lien cloud ubuntu](https://cloud-images.ubuntu.com/)
        : [Lien cloud debian](https://cloud.debian.org/images/cloud/bullseye/daily/latest/)   

### Notes 

Si vous êtes en root enlever le sudo 

Pour l'installer faites un wget + **le lien de l'image cloud**

~~~bash 

wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img

~~~

Ensuite il faudra installer une **blibliothèque** pour l'agent :

~~~bash 

sudo apt install guestfs-tools

~~~

Une fois installer il faudra installer le **qemu-guest-agent** pour cela faites : 

~~~bash 

virt-customize -a "nom_de_l'image" --install qemu-guest-agent

~~~

Il est aussi possible de réduire votre disque où se trouvera l'image je conseille fortement 5G au minimum 

~~~bash 

qemu-img resize --shrink "nom de l'image" 5G

~~~
Il est temps de créer votre template.

## Création de la template

Pour cela faites : 

~~~bash

9000 = ID de la machine 

net 0 virtio,bridge=vmbr0 = interface réseau

name = le nom de la machine 

memory = la mémoire 

sudo qm create 9000 --name "nom_de_votre_vm" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0

~~~

Ensuite vous devrez importer un disque sur votre stockage :

~~~bash 

sudo qm importdisk 9000 "nom_de_l'image" pve-data

~~~

Ensuite, il faudra attacher le disque a la vm comme disque virtio

~~~bash 

sudo qm set 9000 --virtio0 pve-data:vm-9000-disk-0

~~~

Ajout d'un disque de type cloud-init : 
 
~~~bash 

sudo qm set 9000 --ide2 pve-data:cloudinit

~~~


Ensuite faudra indiquer a la template le boot :

~~~bash

sudo qm set 9000 --boot c --bootdisk virtio0
~~~


Ensuite pour éviter d'avoir un écran noir sur la template il faut faire cette commande :

~~~bash 

sudo qm set 9000 --serial0 socket --vga serial0

~~~

Ensuite il faudra activé l'agent 

~~~bash

sudo qm set 9000 --agent enabled=1

~~~

Pour finir il vous suffira de la convertir en template

~~~bash

sudo qm template 9000

~~~

# Configuration Cloud-init dans Proxmox :

![cloud-init](/docs/images/template.jpg)

Il faudra renseigner un user et password 

Ensuite mettre votre nom de domaine ainsi que votre dns 

Et enfin ssi vous voulez vous connecter en ssh renseignez votre clé ssh

# Attention 

- N'oubliez pas si vous avez plusieurs template mettre un ID différent  