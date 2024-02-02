# Le stockage sur Proxmox 

Pour commencer par défaut à l'installation Proxmox va créer 3 partition en lvm qui sont :

![stockage_prox](/images/stockage_prox.jpg)

## Stockage VM

Il est important de créer un disque différent pour le stockage de VM pour cela je vous conseille de partir sur du **lvm-thin**.

## Pourquoi LVM-THIN

Je vous conseille lvm-thin par rapport a lvm tout simplement var lvm ne prend pas les snapchots en compte ce qui peut être très compliqué en cas de panne de votre vm si vous ne pouvez pas revenir en arrière alors que lvm-thin permet ceci.

## Création stockage lvm-thin 

Tout d'abord rendez-vous sur votre proxmox : **http://ip_de_votre_machine:8006**

Ensuite connectez vous avec votre compte root : 

![compte-root](/images/connection_web_prox.jpg)

Ensuite une fois connecté cliquez sur votre serveur : 

![srv](/images/nom_srv_prox.jpg)

Ensuite allez sur l'onglet **disques** puis **lvm-thin**

![disk](/images/rdv_disk.jpg)

Ensuite cliquez sur **Create:thinpool**

![thinppol](/images/lvm-thin.jpg)

Vous aurez cette fenêtre apparaitre : 

![pool](/images/thin_pool.jpg)

- Il vous proposera un disque qui n'est pas utilisé
- Il faudra lui donné un nom 
- Il vous suffira de appuyer sur **crée**
  
### Résultat 

Une fois l'opération vous pourrez voir le thinpool crée 

![pool_fini](/images/pool_fini.jpg)

Vous pouvez le voir aussi en ligne de commande en faisant la commande suivante : 

~~~bash

lsblk

~~~

![lsblk](/images/lsbl.jpg)


## Pourquoi faire un pool 

Le pool va vous permettre de mieux organiser le stockage de vos VMs

## Création du pool 

Tout d'abord rendez-vous sur votre proxmox : **http://ip_de_votre_machine:8006**

Ensuite connectez vous avec votre compte root : 

![compte-root](/images/connection_web_prox.jpg)


Ensuite, il faut se rendre sur l'onglet **pool**

![pool](/images/pool.jpg)

Une fois sur cette onglet ilo faudra appuyer sur **crée**

![create_pool](/images/create_pool1.jpg)

Vous aurez cette fenêtre apparaitre : 

![create_pool](/images/create_pool2.jpg)

- Il faudra lui renseigner un nom 
- Mettre un commentaire 

Ensuite appuyer sur **ok**

### Résultat

Une fois cela fait vous verrez apparaitre votre pool a 2 endroits : 

- Dans l'onglet pool 
  
![pool](/images/create_pool1.jpg)

- Sur l'onglet général à votre gauche 

![pool](/images/onglet_pool.jpg)



