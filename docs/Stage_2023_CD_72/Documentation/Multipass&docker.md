
# Multipass, docker 


## Installer multipass : 

 Multipass c’est un outil comme VirtualBox cela permet d'ouvrir un shell avec la distribution souhaiter. [Lien pour installer Multipass](https://multipass.run/install)

## Hyper G : 

C’est un logiciel qui permet de gérer les VM. C’est un gestionnaire 
Voici le chemin pour s’y rendre : 

~~~bash
C:\Windows\System32\virtmgmt.msc
~~~


## Configurer une nouvelle VM :

Ceci affiche le shell :

~~~bash
multipass shell
~~~

~~~bash
multipass stop primary

Il faudra penser tout d’abord supprimer la machine par défaut “Primary” 
~~~

~~~bash
multipass launch -c 4 -m 8G -d 20G -n servernew 20.04

Ensuite il faudra lancer la VM avec “Launch”, le “-c” c’est pour les cœurs de la VM, “–m” c’est pour la mémoire, “-d” pour le stockage du disque dur, “-n” le nom de la VM et “20.04” la version de la distribution et à la fin rajouter “--cloud-init <Chemin de ce fichier>”
~~~


Pour lancer notre VM il suffira de faire la commande :

~~~bash
“multipass start < nom de la machine >” 
~~~

## Installation Docker :

Tout d’abord, il faut mettre à jour les paquets : 

~~~bash
sudo apt update
~~~

Ensuite, il faudra installer des paquets prérequis pour HTTPS :

~~~bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
~~~

Ensuite il faudra ajouter la clé GPG du dépôt de docker sur votre système :

~~~bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
~~~

Ajoutez le référenciel Docker aux sources APT :

~~~bash
sudo add-apt-repository "deb [arch-amd64] https://download.docker.com/linux/ubuntu focal stable
~~~


Grâce à cela, il faudra mettre à jour la base de données : 

~~~bash
sudo apt update
~~~

Il faudra config l’installation sur le dépôt de docker et non sur celui d’ubuntu : 

~~~bash
apt-cache policy docker-ce
~~~

Enfin, vous pourrez installer docker : 

~~~bash
sudo apt install docker-ce
~~~

Ensuite, vous devrez activer docker :  

~~~bash
sudo systemctl status docker
~~~

## Configuration docker : 

Il vous suffira de vous connecter au git-lab du conseil départemental : [GitLab département](https://git.sarthe.fr/users/sign_in) 
Avec vos codes fournit au préalable.

Ensuite dans la barre de recherche :  taper **”localhost”**

Ensuite cliquer sur clone et copier le lien https. 


Ensuite, une fois le lien copier, il faudra le coller sur la VM :

~~~bash
git clone <lien ssh>
~~~

Une fois cela fait il faudra vous rendre dans le dossier **“ansible_localhost”** : **“cd ansible_localhost”**

Ensuite, faite la commande **“cat README.md”** cela vous amènera à l’intérieur du script :

~~~bash
sudo ansible-galaxy install -f -r requirements.yml

sudo ansible-playbook pb_install_docker.yml
~~~

**Puis copier la ligne juste en-dessous de bash en rajoutant sudo avant.**

Même chose pour le 2ème Bash.

## Installer une image Docker :

Il vous suffira de faire un **“sudo docker run <nom de l’image souhaité >”**

## Héberger une page :

Tout d’abord créer un dossier “Docker”, ensuite dans ce dossier créer en fichier dockerfile et un fichier .html.

Dans le fichier “dockerfile” il faut inscrire du code :

~~~bash
FROM nginx

COPY . /usr/share/nginx/html
~~~

Cela permet que à chaque fois qu’on lance le docker nginx le fichier html se lance et cela permet que à chaque fois que on quitte le docker on ne perd ce qu’on a fait.

Et inscrivez dans le fichier html votre code html

Ensuite placer vous dans le dossier Docker et taper cette commande : 


~~~bash
docker build -t nginx-myapplication
~~~

Pour finir, taper la commande :

~~~bash
docker run --name <nom conteneur> -p <port> -d <nom image>
~~~

Pour vérifier que cela fonctionne, allez sur un navigateur et taper : **<Votre adresse ip > : <numéro de port >/<fichier html>.**

Vous devriez voir votre page html.

## **<span style="color:red">Conclusion :</span>**

**Cette mission m’a permis de découvrir deux logiciels qui sont : Multipass et Hyper V**

**Elle m’a aussi permis de configurer une VM sur cette plateforme.**

**Elle m’a permis de voir différentes possibilités avec docker. Comment le configurer ? Comment l’installer ?** 

