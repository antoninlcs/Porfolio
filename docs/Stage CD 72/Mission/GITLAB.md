# Mission 24/05

## SSH : Clé privée, clé publique, gitlab

### Générer clé privée, clé publique (Windows) : 

Pour se faire il faudra ouvrir un powershell, puis taper la commande : 

~~~bash 
ssh-keygen
~~~

Ensuite, il va vous générer une clé privée et publique dans cet emplacement :

~~~bash
“C:\Users\<votre User>/.ssh/id_rsa”
~~~

**<span style="color:red">Voilà vos clées créent</span>**

### Générer clé privée, clé publique (Ubuntu) :

Pour se faire, il faudra taper la même commande que celle de windows :

~~~bash 
ssh-keygen
~~~

Ensuite, il va vous générer une clé privée et publique dans cet emplacement :

~~~bash
“/home/ubuntu/.ssh/id_rsa”
~~~ 

### Utiliser la VM sur le Powershell de Windows :

Il vous suffira de copier la clé publique de Windows sur le fichier **“authorized_keys”** qui se trouve dans **“/home/ubuntu/.ssh”** et de sauvegarder le fichier.

Pour utiliser la VM sur le powershell de Windows, il faudra taper la commande :

~~~bash
“ssh <user>@<adresse Ip de la VM> ou <le nom de domaine>”
~~~

Ensuite, il vous posera une question : Il faudra répondre **“yes”**

**<span style="color:red">Enfin vous serez connecté en ssh à la VM</span>** 

### Connecter la VM sur Visual Code :

Pour connecter sa VM, il faut commencer par installer une extension sur Visual Code. Pour cela il faut aller dans extension.

Ensuite il faudra rechercher “remote“ et installer le premier qui apparaît.
   
Une fois installer, il y aura une **<span style="color:blue">icône bleue en bas</span>** à gauche de l’application.

Il faudra cliquer dessus, ensuite 

~~~bash
Cliquer sur “Connect to Host...”
Ensuite cliquer sur “+ Add New SSH Host...”
~~~ 

Ensuite il faudra renseigner :

~~~bash
“ssh <user>@<adresse Ip de la VM> ou <le nom de domaine>”
~~~

Ensuite au même endroit il vous demandera la distribution utiliser : **(Linux, Windows, Mac)**, choisissez ce dont vous avez besoin. 

**<span style="color:red">Vous voilà connectez à la VM sur Visual Code !!</span>**

### Créer un dépôt sur Git-lab et le pouvoir “Cloner” et “push” grâce à “GIT” sur UBUNTU :

Il vous suffira de vous connecter au git-lab du conseil départemental : [Lien GitLab](https://git.sarthe.fr/users/sign_in)

Avec vos codes fournit au préalable.

#### Création du dépôt :  

Cliquez sur **“Create blank project”**.

Ensuite donner un nom au projet et appuyer sur **“Create project”**.

#### Configuration pour “cloner” et “push” sur Ubuntu :

Une fois le dépôt créer, il faudra ajouter vos clés publiques (Ubuntu et Windows) sur Git-Lab.

Pour ce faire, il faut se rendre sur votre profil (Icone en haut à droite) et cliquer sur **préférences**.
 
En cliquant sur préférences, sur la gauche de l’écran il y aura **“SSH keys”** cliquez dessus. 

Ensuite, copier-coller une des deux clés publiques **“dans Key”**, donnez-lui un titre et ajoutez-la.

Faites la même manipulation pour la deuxième clé publique.
 
Ensuite retournez sur votre dépôt et copier le lien ssh.  

Puis retournez sur votre VM coller ce que vous venez copier en rajoutant au début **"git clone"**.

Suite à cette commande, votre dépôt sera dans votre VM.


**<span style="color:red">Maintenant depuis votre dépôt vous pourrez travailler sur votre VM et mettre vos travaux sur ce dépôt.</span>**

#### Maintenant, nous allons voir comment “push” nos dossier de travail qui sont présent dans notre dépôt sur la VM directement dans Git-lab.

Après avoir mis un fichier dans le dépôt il vous suffira de faire 3 commandes à la suite :

~~~bash
git add -A
( Il va ajouter le nouveau fichier créer dans le dépôt)
~~~

~~~bash
git commit -m rapport
(C’est pour indiquer un message pour le fichier push) moi par exemple c’est ”rapport”
~~~

~~~bash
git push
(La dernière commande permet d’envoyer ce fichier dans le git-lab)
~~~

**<span style="color:red">Voilà comment créer un dépôt dans Git-lab et de pouvoir le cloner et le push sur git-lab sous Ubuntu.</span>**


#### Configuration pour “cloner” et “push” sur Windows :

Sur Windows, il vous suffira d’installer Git pour Windows : [Lien téléchargement Git](https://git-scm.com/download/win)

Une fois installer, les commandes seront les mêmes que celles de Linux 




