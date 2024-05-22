```markdown
# Docker-compose, apache, php, mariadb

## C’est quoi Docker-compose :

**<span style="color:red">C’est un outil qui permet de définir et gérer des applications multi-conteneurs. Ce docker utilise un fichier YAML (Représentation des informations lisible par l’Homme).</span>**

## Configurer Docker-compose :

Tout d’abord, il faudra cloner votre dépôt git sur votre VM :

[Revoir mission 24/05](https://antoninlcs.github.io/Porfolio/Stage%20CD%2072/Documentation/GITLAB/)
: lien pour le département.

Allez dans le titre **“Configuration pour “cloner” et “push” sur Ubuntu : ”**

Ensuite, il faudra aller à la racine du dépôt et créer différents dossiers :

## Docker-compose.yml :

~~~bash
version: '3.7'

services: 
  db:
    image: mariadb:10   # Service du container 
    container_name: mariadb  # Son nom
    restart: always    # Il reste toujours allumé
    ports: ['3306:3306']
    volumes:          # Endroit stockant les informations et données du container
      - ./db_data:/var/lib/mysql
      - ./backups:/backups
    environment:
      MYSQL_DATABASE: $MYSQL_DATABASE
      MYSQL_USER: $MYSQL_USER
      MYSQL_PASSWORD: $MYSQL_PASSWORD
      MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
    env_file:
      - .env  # Il va chercher les informations de ce fichier

  web:
    build: .  # Il construit le conteneur à la racine du dépôt
    container_name: apache2
    restart: always
    depends_on: ['db']
    ports: ['80:80'] # numéros de port de sortie du conteneur
    links: ['db:db']
    volumes:
      - './www:/var/www/html' # Endroit où il va chercher le contenu et stocker 
    environment: # Il va récupérer les données de la base de données
      MYSQL_DB_HOST: db
      MYSQL_DATABASE: $MYSQL_DATABASE
      MYSQL_USER: $MYSQL_USER
      MYSQL_PASSWORD: $MYSQL_PASSWORD

    env_file:
      - .env  # Il va chercher les informations de ce fichier
~~~

## Backups, db_data, www :

Il faudra créer ces fichiers dans le dépôt pour les **“Volumes”**.

## Installer WordPress sur le dossier “www” :

**<span style="color:red">WordPress est un système de gestion de contenu gratuit, libre et open source. Ce logiciel écrit en php repose sur une base de données MySQL.</span>**

Pour installer WordPress sur **“www”**, il vous suffira de vous placer dedans :

~~~bash
cd www/
~~~

Et de faire la commande :

~~~bash
wget https://wordpress.org/latest.zip
~~~

**<span style="color:red">Attention :</span>**

Il faut penser à **“unzip“** le fichier **“latest.zip“** 

Pour cela il faudra installer le paquet unzip :

~~~bash
sudo apt install unzip
~~~

Enfin, faites la commande dans **“www”** :

~~~bash
unzip latest.zip
~~~

**Pour que WordPress fonctionne, il faut installer une extension PHP** 

## DockerFiles :

**<span style="color:red">Les Dockerfile vous permettent de configurer et de créer rapidement une image pour la rendre plus partageable facilement.</span>**

Il vous suffira de créer un fichier Dockerfile  toujours à l’intérieur de votre dépôt : 

~~~bash
FROM    php:7.4-apache 
# Il va chercher le PHP Apache 
RUN     docker-php-ext-install mysqli
# Et il va installer l'extension 
~~~

**<span style="color:red">Attention :</span>**

Il faudra modifier les droits de ”www” : ”chmod -R 777.”

## Lancement docker-compose :

Une fois tout cela configuré, vous pourrez lancer la commande : 

~~~bash
sudo docker-compose up
~~~

Cela va lancer votre docker-compose. Il vous suffira d'ouvrir un navigateur et taper le nom de votre machine et vous verrez WordPress. Il faudra renseigner le nom de votre site et les informations de votre BDD précédemment définies dans le fichier **“docker-compose”**.

**<span style="color:red">Voilà votre site sur le serveur Apache sur un docker. Maintenant laissez les devs travailler !</span>**

## Push le dépôt sans avoir accès au ID de connexion et au mot de passe :

**<span style="color:red">Il est important de savoir faire cela car si vous mettez tous vos dossiers dans votre dépôt public et que dans ces dossiers tout le monde peut accéder à vos de mot de passe. Votre site sera en aucun cas sécurisé.</span>**

## .gitignore :

Ce fichier sert à indiquer quels fichiers il va ignorer pendant le push, pour cela il faudra créer ce fichier toujours dans votre dépôt.
Il faudra inscrire dans ce fichier les fichiers sensibles envers votre site :

~~~bash
www/
backups/
db_data/
.env
~~~

Quand vous allez faire un push dans votre GitLab, ces fichiers n'y seront pas.

## .env :

Le fichier .env est un fichier caché sur votre VM qui permet de déclarer des variables pour les informations importantes, par exemple : 

**“MYSQL_PASSWORD = Votre password”**

Votre fichier doit ressembler à cela :

~~~bash
# Declaration Variable 

MYSQL_USER : NOMUTILISATEUR 
MYSQL_PASSWORD : MOTPASSEUSER  
MYSQL_ROOT_PASSWORD : MOTDEPASSEROOT
MYSQL_DATABASE : NOMBASE 
~~~

## .env_sample :

Pour aider les gens à créer leur “.env” sans mettre vos informations, il est possible de faire un exemple et ce fichier contrairement au “.env” sera push dans le dépôt.

**<span style="color:red">Une fois tout cela configuré, il vous suffira de push votre dépôt sur la VM et vos fichiers seront sur le GitLab en ligne.</span>**

**<span style="color:green">Rappel pour push :</span>** Faites un **“git add – A”**, puis un **“git commit –m <message>”**, enfin le **“git push”**.
