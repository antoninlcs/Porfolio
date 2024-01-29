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

![api_token](/docs/images/API-Token.jpg)

Puis, cliquez sur **Add** 

![add](/docs/images/add.jpg)

Vous aurez cette fenêtre apparaître : 

![fenetre_token](/docs/images/fenetre_token.jpg)

- Renseigner l'utilisateur associé a ce jeton 
- Lui mettre un nom (ID)
- Décocher **Privilege Separation**
- Faites **add**

Une cela fait vous verrez cette fenêtre apparaitre :

![Secret_id](/docs/images/secret_id.jpg)

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

# Variable pour la clé SSH utilisée pour l'accès aux machines virtuelles
variable "ssh_key" {
  default = "ssh-rsa AAAAB3NzaC1yc2EAAAADA...<clé SSH complète>"
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

# Définition de la ressource Proxmox VM QEMU
resource "proxmox_vm_qemu" "test_vm" {
  # Nom de la machine virtuelle
  name       = "test-vm"

  # Noeud de destination pour la création de la VM
  target_node = "SRV-VIR-PRXMX"

  # Modèle de clonage basé sur une machine virtuelle existante (ubuntu-template)
  clone      = "ubuntu-template"

  # Pool auquel appartient la machine virtuelle
  pool       = "test"

  # Activation de l'agent QEMU Guest
  agent      = 1

  # Type de système d'exploitation avec prise en charge de cloud-init
  os_type    = "cloud-init"

  # Configuration du processeur
  cores      = 2
  sockets    = 1
  cpu        = "host"  # Utilisation des fonctionnalités de CPU de l'hôte

  # Configuration de la mémoire
  memory     = 2048

  # Disque de démarrage
  bootdisk   = "virtio0"

  # Configuration du réseau
  network {
    model  = "virtio"
    bridge = "vmbr0"  # Pont réseau utilisé
  }

  # Configuration de l'adresse IP et de la passerelle pour l'interface réseau 0
  ipconfig0 = "ip=172.16.100.2/16,gw=172.16.255.254"
}

~~~

Une fois vos 3 fichier créer allez sur votre machine est aller sur votre dossier précedemment créer.

Une fois cela fait vous aurez trois commande a effectuer :

- La première commande va permettre d'installer le plugin : 

~~~bash 

sudo terraform init -upgrade 

~~~

- La deuxième sera pour voir si votre fichier est valide 

~~~bash 

sudo terraform validate 

~~~

- Enfin la dernière sera pour créer la vm sur proxmox 

~~~bash 

sudo terraform apply -auto-approuve 

~~~ 

Une fois ce processus effectuer vous pourrez voir votre vm dans le menu de gauche de proxmox 

![vm](/docs/images/vm.jpg)

# Note 

Dans proxmox, une fois le apply fait vous verrez les logs de cette commande dans proxmox en bas de votre écran :

![logs](/docs/images/vm2.jpg)


# Attention 

**Veuillez enlever les commantaires des fichiers cela est juste a titre explicatif, car sinon votre déploiment va mal se passer**

