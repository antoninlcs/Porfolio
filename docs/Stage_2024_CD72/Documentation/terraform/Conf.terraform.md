 
  
# Création du projet terraform 

Tout d'abord, il faudra installer terraform sur votre machine 

Il y aura quelques a effectuer tout d'abord assurez-vous que votre système est à jour et que vous avez installé les packages **gnupg**, **software-properties-commonet** installés **.curl**. Vous utiliserez ces packages pour vérifier la signature GPG de HashiCorp et installer le référentiel de packages Debian de HashiCorp :

~~~bash 

 sudo  apt-get update &&  sudo  apt-get  install -y gnupg software-properties-common

 ~~~

Ensuite, Installez la **clé GPG HashiCorp**.

 ~~~bash 

wget -O- https://apt.releases.hashicorp.com/gpg |  \
gpg --chermor |  \ 
sudo  tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

~~~

La commande **gpg** rapportera l'empreinte de la clé 


Puis, ajoutez le référentiel officiel HashiCorp à votre système. La commande **La lsb_release -cs** recherche le nom de code de la version de distribution de votre système actuel, tel que **buster**, **groovy** ou **id**.

~~~bash 

 echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

~~~

Maintenant, il faudra mettre a jour vos paquets 

~~~bash 

sudo apt-get update

~~~

Enfin vous pourrez installer **Terraform**

~~~bash 

 sudo  apt-get  install terraform

~~~

# Création de l'API JETON PROXMOX

Tout d'abord, allez sur votre proxmox : **http://ip_de_votre_machine:8006**

Ensuite, allez sur l'onglet **API Tokens**

![api_token](../images/API-Token.jpg)

Puis, cliquez sur **Add** 

![add](/images/add.jpg)

Vous aurez cette fenêtre apparaître : 

![fenetre_token](/images/fenetre_token.jpg)

- Renseigner l'utilisateur associé a ce jeton 
- Lui mettre un nom (ID)
- Décocher **Privilege Separation**
- Faites **add**

Une cela fait vous verrez cette fenêtre apparaitre :

![Secret_id](/images/secret_id.jpg)

**NOTEZ CES INFORMATIONS QUELQUES PART CAR VOUS EN AUREZ BESOIN ET CES INFORMATIONS NE PEUVENT PAS ETRE RELUE PLUSIEURS FOIS**


# Création des fichier terraform 

Tout dd'abord créer un dossier dans lequel vous mettrez vos fichiers terraform :

~~~bash 

mkdir terraform

~~~
Créer vos fichier sur vscode se sera plus pratique

[lien connection ssh](http://srv-tst-dock.cg72.fr:8080/Doc_stage_2023/GITLAB/)

Ce lien vous explique comment connecter votre machine en ssh sur vscode

Ensuite vous aller créer un premier fichier confidentiel qui va permettre de stocker le lien de votre proxmox ainsi que les informations de votre API TOKEN

Pour cela nommez ce fichier "credentials.auto.tfvars"

~~~bash 

proxmox_api_url = "http://ip_de_votre_machine:8006/api2/json"
proxmox_api_token_id = "Votre ID jeton"
proxmox_api_token_secret = "Votre sercret"

~~~

Une fois cela fait faites **contol+s**

Ensuite créer un fichier **provider.tf** : 

Il faudra mettre ces informations : 

~~~bash 

# Configuration de Terraform et spécification des versions des fournisseurs requis
terraform { 
  required_providers {
    proxmox = {
      source  = "TheGameProfi/proxmox"
      version = "2.9.16"
    }
  }
}

# Variables pour les informations d'authentification et de configuration Proxmox
variable "proxmox_api_url" {
    type = string 
}

variable "proxmox_api_token_id" {
    type = string
    sensitive = true  # Marque la variable comme sensible (sensible) pour masquer sa valeur
}

variable "proxmox_api_token_secret" {
    type = string 
    sensitive = true   # Marque la variable comme sensible pour masquer sa valeur
}


# Fournisseur Proxmox avec les détails d'authentification
provider "proxmox" {
  pm_api_url           = var.proxmox_api_url
  pm_api_token_id      = var.proxmox_api_token_id
  pm_api_token_secret  = var.proxmox_api_token_secret
  pm_tls_insecure      = true    # Désactive la vérification TLS pour les connexions (utilisé dans les environnements de test)
  pm_debug             = true    # Active le mode débogage pour le fournisseur Proxmox
}
~~~

Ce fichier va permettre a terraform de joindre l'api de proxmox

Enfin, il faudra créer un dernier fichier qui va permettre de configurer votre vm 

Il s'appelera généralement **main.tf**

Voici ce qu'on peut retrouver dans ce fichier 

~~~bash 

# Définition de la ressource de la machine virtuelle Proxmox
resource "proxmox_vm_qemu" "vm_basic" {
  # Nom de la machine virtuelle
  name       = "test-vm-anto-tst-6"
  # Noeud cible sur lequel déployer la machine virtuelle
  target_node = "SRV-VIR-PRXMX"
  # Modèle à utiliser pour le clonage
  clone      = "tmplt-ubt2204-propre"
  # Pool dans lequel la machine virtuelle sera placée
  pool       = "test"
  # Activation de l'agent de Proxmox pour la gestion de la machine virtuelle
  agent      = 1
  # Type de système d'exploitation utilisé
  os_type    = "cloud-init"
  # Nombre de cœurs CPU
  cores      = 2
  # Nombre de sockets CPU
  sockets    = 1
  # Configuration CPU pour correspondre au modèle de CPU de l'hôte
  cpu        = "host"
  # Mémoire RAM de la machine virtuelle (en Mo)
  memory     = 2048
  # Disque de démarrage
  bootdisk   = "virtio0"
  
  # Configuration du réseau
  network {
    # Modèle de carte réseau virtuelle
    model  = "virtio"
    # Pont réseau sur lequel la machine virtuelle sera connectée
    bridge = "vmbr0"
  }

  # Configuration de l'adresse IP et de la passerelle
  ipconfig0    = "ip=172.16.100.6/16,gw=172.16.255.254"
  # Serveur de noms DNS à utiliser
  nameserver   = "172.16.20.160"
  # Domaine de recherche DNS
  searchdomain = "cg72.fr"
  # Nom d'utilisateur pour l'initialisation cloud-init
  ciuser       = "root"
  # Mot de passe pour l'initialisation cloud-init
  cipassword   = "root"

  # Provisionnement avec un exécuteur local
  provisioner "local-exec" {
    # Commande à exécuter après le déploiement de la machine virtuelle
    command = "sleep 1m && ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i '172.16.100.6,' -u root   /home/anto/automatisation/ansible/setup_vm.yml"
  }
}

~~~


# Utilsation de Ansible pour installer des services sur la vm 

Tout d'abord créer un dossier ansible 

~~~bash

mkdir ansible

~~~

Il faudra créer un dossier task où il y aura votre playbook

~~~bash

mkdir task

~~~

Il faudra créer un dossier template on vous y mettrez vos template 

~~~bash

mkdir template

~~~

Ensuite dans ce dossier il faudra créer un playbook dans lequel vous allez définire les tâches à réaliser 

~~~bash 

- name: Configure Proxy and Timezone, Update Packages, Install and Start Apache
  hosts: all
  become: yes  # Autoriser l'exécution des tâches en tant que superutilisateur

  tasks:

    - name: COPY SET PROXY CONF
      become: yes  # Exécuter cette tâche en tant que superutilisateur
      ansible.builtin.template:  # Utilisation du module template pour copier un fichier template
        src: /home/anto/automatisation/ansible/90curtin-aptproxy.j2
        dest: '/etc/apt/apt.conf.d/90curtin-aptproxy'  # Destination du fichier généré
        owner: root  # Propriétaire du fichier
        group: root  # Groupe du fichier
        mode: '0644'  # Permissions du fichier


    - name: Configure proxy in /etc/environment for HTTP
      lineinfile:  # Utilisation du module lineinfile pour gérer les lignes dans un fichier texte
        path: /etc/environment  # Chemin du fichier
        regexp: '^http_proxy='  # Expression régulière pour rechercher une ligne
        line: 'http_proxy=proxy'  # Ligne à ajouter ou modifier
        state: present  # État de la ligne (présent dans ce cas)


    - name: Configure proxy in /etc/environment for HTTPS
      lineinfile:  # Utilisation du module lineinfile pour gérer les lignes dans un fichier texte
        path: /etc/environment  # Chemin du fichier
        regexp: '^https_proxy='  # Expression régulière pour rechercher une ligne
        line: 'https_proxy=proxy'  # Ligne à ajouter ou modifier
        state: present  # État de la ligne (présent dans ce cas)


    - name: Set timezone
      timezone:  # Utilisation du module timezone pour configurer le fuseau horaire
        name: "Europe/Paris"  # Nom du fuseau horaire à configurer

    
    - name: Update packages
      apt:  # Utilisation du module apt pour gérer les packages sous Debian/Ubuntu
        update_cache: yes  # Mise à jour de la cache des paquets
        upgrade: yes  # Mise à jour des packages installés


    - name: Install Apache web server
      apt:  # Utilisation du module apt pour gérer les packages sous Debian/Ubuntu
        name: apache2  # Nom du package à installer
        state: present  # État souhaité du package (présent dans ce cas)

    - name: Ensure Apache service is started and enabled
      service:  # Utilisation du module service pour gérer les services système
        name: apache2  # Nom du service à gérer (Apache dans ce cas)
        state: started  # État souhaité du service (démarré dans ce cas)
        enabled: yes  # Activer le service au démarrage du système

~~~

Ensuite pour indiquer le proxy, il faudra créer un template comme indiquer dans le code. Dans ce fichier vous allez indiquer l'addresse de votre proxy : 

~~~bash

Acquire::http::Proxy "{{ http_proxy }}";
Acquire::https::Proxy "{{ https_proxy }}";

~~~

Enfin dans votre fichier **main.tf** il faudra renseigner cette ligne :

~~~bash 

 # Commande à exécuter après le déploiement de la machine virtuelle
    command = "sleep 1m && ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i '172.16.100.6,' -u root   /home/anto/automatisation/ansible/setup_vm.yml"

~~~

Elle va permettre d'executer l'ansible sur la vm provisionnée automatiquement.


Une fois les fichiers dont a besoin terraform et ansible sont crée allez sur votre machine est aller sur votre dossier **terraform**.

Une fois cela fait vous aurez 5 commandes a effectuer :

- Les deux premières commandes va servir a renseigner la clé ssh parce que ansible ne peut pas la renseigné tout seul

~~~bash 

ssh-agent $SHELL

ssh-add 
~~~

Ensuite renseigner votre clé ssh 

- La troisième commande va permettre d'installer le plugin : 

~~~bash 

sudo terraform init -upgrade 

~~~

- La quatrième sera pour voir si votre fichier est valide 

~~~bash 

sudo terraform validate 

~~~

- Enfin la dernière sera pour créer la vm sur proxmox 

~~~bash 

sudo terraform apply -auto-approuve 

~~~ 

Une fois ce processus effectuer vous pourrez voir votre vm dans le menu de gauche de proxmox 

![vm](/images/vm.jpg)

# Note 

Dans proxmox, une fois le apply fait vous verrez les logs de cette commande dans proxmox en bas de votre écran :

![logs](/images/vm2.jpg)




# Attention 

**Veuillez enlever les commantaires des fichiers cela est juste a titre explicatif, car sinon votre déploiment va mal se passer**

