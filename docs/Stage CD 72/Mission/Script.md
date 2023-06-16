# Mission du 26/05

#### <span style="color:green">Création Script pour récupérer le volume de la partitions “home” et mettre cette information dans la base de données MariaDB</span> 

## Exposer le port MariaDB :

### C’est quoi un port ? 

**<span style="color:red">Un port est un point virtuel où les connexions réseau commencent et se terminent.</span>**

Tout d’abord, il vous faudra vous rendre sur votre dossier **“docker-compose.yml”**. 
Il faudra exposer le port **“3306”** du conteneur MariaDB pour qu’il puisse être accessible depuis l’extérieur.

## Script “sh” :

### C’est quoi sh ?

**<span style="color:red">Sh est l'extension utilisé pour les scripts shell qui peuvent être écrit en bash, fish, zsh etc.</span>**

Pour pouvoir connaitre le volume de la partition il vous faut un script exécutable. Il faudra pour cela utiliser l’extension en sh.

Tout d’abord, dans votre dépôt local créer un fichier en sh :

~~~bash
vim <nom fichier>.sh
~~~

Et mettre ceci dans le fichier :

~~~bash
#!/bin/bash


# Vérifier si le client MYSQL est installé
if ! type mysql >/dev/null 2>&1; then
  #Installer le client MySQL
  sudo apt update
  sudo apt-get install -y mysql-client

fi

# Récupérer la taille de la partition
partition_size=$(du -s /home | awk '{print $1}')
timestamp=$(date +%Y-%m-%d\ %H:%M:%S)



# Insérer les données dans la bdd
mysql -h 127.0.0.1 -P 3306 -u $MYSQL_USER -p$MYSQL_PASSWORD -D $MYSQL_DATABASE -e "INSERT INTO partition_home (size, timestamp) VALUES ($partition_size,'$timestamp');"


#Charger les variables d'environnement à partir du fichier .env

source .env 
~~~

### **<span style="color:red">Attention</span>**

Assurer vous que votre script a les permissions d’exécution :

~~~bash
”chmod +x <Chemin du script>”
~~~

## Crontab : 

### C’est quoi “crontab” ?

**<span style="color:red">Crontab est le nom du programme sous Unix (et Linux) qui permet d'éditer des tables de configuration du programme cron. Ces tables spécifient les tâches à exécuter et leur horaire d'exécution.</span>**

Tout d’abord, il faudra taper la commande crontab –e cela vous ouvrira un fichier.

Ensuite en bas il vous faudra indiquer tout les combien de temps un fichier doit s’exécuter : 

~~~bash
<période>  <Chemin du fichier>
~~~

## Création de la table dans la base de données : 

Tout d’abord, il faudra vous connecter à votre docker :

~~~bash
“docker exec –it <nom du conteneur> /bin/bash”
~~~

Pour cela connecter vous en tant qu'Administrateur de la Base de Données :

~~~bash
“mysql -u root –p”
~~~

Une fois connecté à la base de données MariaDB il va falloir indiquer sur quelle base de données créer la table :

~~~bash
“USE <nom de la base de données>”
~~~

Une fois la base de données sélectionne vous pouvez créer votre table. Voici un exemple :

~~~bash
“CREATE TABLE partitions_home (
  id INT AUTO_INCREMENT PRIMARY KEY,
  size VARCHAR(20),
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);”
~~~

Ensuite pour voir si votre table est créée, il vous suffira de faire un :

~~~bash
“SHOW TABLES”
~~~

**Vous verrez votre table normalement.** 

## Voir si le script fonctionne 

Pour voir si le script fonctionne, il vous suffira de faire :

~~~bash
“SELECT * FROM <nom de la table crée>”
~~~

**<span style="color:red"> Voilà vos informations de votre script seront sur votre Base De Données MariaDB !!</span>**




