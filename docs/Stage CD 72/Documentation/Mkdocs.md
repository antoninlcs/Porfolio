
## Intallation MkDocs
Tout d'abord, il faudra installer le paquet "pip"

~~~bash
sudo apt install pip
~~~

Ensuite installer MkDocs

~~~bash
 pip install MkDocs
~~~

Après l'installation, dans votre répertoire de travail, exécutez la commande suivante pour initialiser un site :

~~~bash
mkdocs new mkdocspro
~~~

Ensuite placer vous dans le dossier :

~~~bash
cd mkdocspro
~~~

Ensuite faite cette commande pour savoir l'url du site :

~~~bash
mkdocs serve
~~~

 <span style="color:red">**Si tous est fait correctement vous devriez arriver sur une page comme celle ci !**</span>

## Installation Docker par script 

Sur le **GitLab** du département : [Lien GitLab](https://git.sarthe.fr/users/sign_in)

Dans la barre de recherche taper **"ansible localhost"** 

**Cloner** sur votre Vm le dépôt.

Ensuite aller dans ce dossier et faite :

~~~bash
 sudo ansible-galaxy install -f -r requirements.yml
~~~
~~~bash
  sudo sudo ansible-playbook  pb_install_docker.yml
~~~


## Héberger le site avec Nginx avec un dockerfile 

#### Créer un dépôt sur git et le cloner

Pour ce faire aller voir [La mission du 24.05](https://antoninlcs.github.io/cd72/Stage%20CD%2072/Mission/Mission%20du%2024.05/) !

#### Mettre le contenu de MkDocs dans le dépôt

Il faudra faire :

~~~bash
mv <nom du fichier> <la destination>
~~~

#### Création du Dockerfile

Il faudra créer un **"Dockerfile"** dans le dépôt précedemment crée 

~~~bash
vim Dockerfile
~~~

Une fois créer il faudra insérer ceci :

~~~bash
FROM nginx:latest

# Copiez les fichiers générés de MkDocs dans le répertoire du serveur Nginx
COPY site /usr/share/nginx/html

# Copiez la configuration Nginx personnalisée
COPY nginx.conf /etc/nginx/conf.d/default.conf
~~~

#### Création du fichier conf Nginx
 
 Il faudra crée ce fichier toujours dans le dépôt 

~~~bash
vim nginx.conf
~~~

Une fois créer il faudra insérer ceci :

~~~bash
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
~~~

#### Création de l'image Docker 

Pour construire l'image il faudra taper cette commande : 

~~~bash
docker build -t monsite-mkdocs .
~~~

Il vous suffira d'éxécuter le conteneur :

~~~bash
docker run -d -p 8080:0 monsite-mkdocs
~~~

<span style="color:red">**Si tous est fait correctement vous devriez arriver sur une page comme celle ci ! Et votre site est héberger sur un docker nginx**</span>



## Héberger le site avec Nginx avec docker-compose 

Tout d'abord supprimer le fichier **"Dockerfiles"**

Ensuite dans le dépôt, il faudra créer le fichier **"docker-compose.yml"** et mettre ceci :

~~~bash
version: '3'

services:
  nginx-server:
    container_name: nginx  #Son nom
    #restart: always    #Il reste toujours allumer
    image: nginx:latest
    ports:
      - 8080:80
    volumes:
     ## - ./nginx.conf:/etc/nginx/conf.d/default.conf
      - ./site:/usr/share/nginx/html
~~~

<span style="color:red">**Si tous est fait correctement vous devriez arriver sur une page comme celle ci ! Et votre site est héberger sur un docker nginx en permanance**</span>


## Bonus 

### Installer un navigateur en ligne de commande 

Faite un : 

~~~bash
sudo apt install lynx
~~~

Pour l'utiliser par exemple "Google ":

~~~bash
lynx https://google.com
~~~

Répondez "A" aux questions qu'il vous pose.

<span style="color:red">**Et voilà vous avez un navigateur internet en ligne de commande**</span>







