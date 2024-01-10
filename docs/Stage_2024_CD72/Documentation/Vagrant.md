# Le fonctionnement de Vagrant 

Vagrant est un logiciel anciennement libre et open-source pour la création et la configuration des environnements de développement virtuels. Il peut être considéré comme un wrapper autour de logiciels de virtualisation comme VirtualBox.
Depuis la version 1.1, Vagrant n'impose plus VirtualBox, mais fonctionne également avec d'autres logiciels de virtualisation tels que VMware. Depuis la version 1.62 Vagrant prend nativement en charge les 
conteneurs Docker à l'exécution, au lieu d'un système d'exploitation entièrement virtualisé. Cela permet de réduire la charge en ressources puisque Docker utilise des conteneurs Linux légers.
 
## Les "box":
 
Vagrant utilise des "box" qui fonctionne comme un registry, c'esy-à'dire qu'au lieu de repartir de 0 en recréant une machine de A à Z il va chercher dans les différentes "box dans le catalogue d'HashiCorp.
 
Fichiers synchronisé : 
Vagrant synchronise automatiquement les fichiers vers et depuis la machine invitée.
 
## Le partage d'environnement :
 
Vagrant Share est un plugin qui vous permet de partager l'environnement Vagrant avec n’importe qui autour de la monde entier avec une connexion Internet. 
Il vous donnera une URL qui sera acheminée directement à votre environnement Vagrant à partir de n’importe quel appareil dans le monde connecté à Internet.

## Les providers

Un provider est l'emplacement dans lequel l'environnement virtuel s'exécute. Il peut être local (la valeur par défaut est d'utiliser VirtualBox) ou distant.

## Les provisions / Aprovisionement 

Un approvisionneur est un outil pour configurer l'environnement virtuel. Il peut être aussi simple qu'un script shell, mais en variante, un outil plus avancé comme Chef, Puppet ou Ansible peut être utilisé.

Les approvisionneurs de Vagrant vous permettent d'installer automatiquement des logiciels, de modifier les configurations et plus encore sur la machine dans le processus de démarrage.

# Installation de Vagrant 

Tout d'abord, il faudra vous rendre sur le [site de vagrant](https://developer.hashicorp.com/vagrant/install?product_intent=vagrant)

Il faudra choisir le **packages** correspondant a votre système d'exploitation (MacOs,Windows,Linux)

![exemple de packages](../images/packages_vagrant.jpg)

Plus qu'a télécharger !! 

Une fois télécharger, il faudra taper en ligne de commande la commande suivante : 

~~~bash

vagrant init 

~~~

Cette commande va vous générer un fichier nommé **Vagrantfile**

![générer fichier Vagrantfiles](../images/Fichier_Vagrantfile.jpg)

Ce fichier va être la clé de votre projet 

# Le fichier Vagrantfile 

Ce fichier va vous permettre de configurer votre environnement 

Ceci est un exemple de fichier que j'ai pu réaliser :

~~~bash
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

require 'yaml'

# Chargez les informations depuis le fichier config.yml
config_data = YAML.load_file('config.yml')

# Toute la configuration Vagrant se trouve ci-dessous. Le "2" dans Vagrant.configure
# configure la version de configuration (nous prenons en charge les anciens styles pour
# la compatibilité ascendante). Ne le changez pas à moins de savoir ce que
# vous faites.

Vagrant.configure("2") do |config|

  # Définit la machine virtuelle avec le nom "srv-test"
  config.vm.define "srv-test" do |machine_config|

    # Définit la boîte de base pour la machine virtuelle
    machine_config.vm.box = "generic/ubuntu2204"
    
    # Spécifie le fournisseur pour la machine virtuelle (Hyper-V)
    machine_config.vm.provider "hyperv" do |h|

      # Active les extensions de virtualisation
      h.enable_virtualization_extensions = true

      # Utilise un clonage lié
      h.linked_clone = true
    end

    # Configure le réseau public pour la machine virtuelle
    machine_config.vm.network "public_network", dns: ["DNS1", "DNS2"]




    # Configuration du proxy pour la machine virtuelle
    machine_config.proxy.http     = "ADRESSE PROXY"
    machine_config.proxy.https    = "ADRESSE PROXY"
    machine_config.proxy.no_proxy = "localhost,127.0.0.1"

    
    # Configuration du proxy pour l'environnement
    config.proxy.http     = "ADRESSE PROXY"
    config.proxy.https    = "ADRESSE PROXY"
    config.proxy.no_proxy = "localhost,127.0.0.1"

    # Configure le dossier partagé pour Nginx en utilisant SMB
    machine_config.vm.synced_folder "Chemin du dossier machine hôte", "chemin du dossier VM", type: "smb", smb_username: config_data['smb_username'], smb_password: config_data['smb_password'], mfsymlinks: true
  end
end

Pour les chemin mettre / au lieu de \

~~~

# Configuration Proxy 

Le proxy risque de vous pose problèmes pour aller chercher des box, ou faire des apt-update etc...

C'est pour cela qui faudra effectuer quelques commandes avant de lancer votre environnement :

Tout d'abord il faudra que vous ouvrez un **cmd en admin**

et ensuite faire c'est trois commandes :

~~~bash 

set http_proxy=ADRESSE PROXY

set https_proxy=ADRESSE PROXY

vagrant plugin install vagrant-proxyconf

~~~

Si vous voyez ceci s'afficher c'est que c'est bon !!

![Verif Proxy](../images/vérif-proxy.jpg)


# Conf fichier.yml 

Comme vous le voyez il y a des variables pour le partage de fichier ceci permet a vagrant d'accèder a nos fichiers sauf qu'il faut mettre notre login P@ssword en clair ce qui n'est pas une bonne pratiques. C'est pour cela qui vous faudra crée des variables ainsi que ce fichier 

Il faudra créer un fichier **yml** dans votre **dossier d'environnement** 

Voici la configuration : 

~~~bash 

# config.yml
smb_username: "votre User"
smb_password: "Votre P@ssword"

~~~

Ainsi le fichier Vagrantfile ira chercher les valeurs des variables qu'on lui a mis pour la partage de fichier

# Les commandes de bases 

**vagrant init** : crée un fichier Vagrantfile dans le répertoire courant. Ce fichier contient la configuration de la Vagrant Box à utiliser. Tu peux utiliser cette commande pour créer un nouveau projet Vagrant.

**vagrant up** : démarre la Vagrant Box. Si la box n’est pas encore installée, Vagrant la téléchargera et la configura avant de la démarrer.

**vagrant ssh**: se connecte à la Vagrant Box en utilisant SSH. Tu peux utiliser cette commande pour accéder à la console de la box et exécuter des commandes sur le système d’exploitation de la box.

**vagrant halt** : arrête la Vagrant Box. Tu peux utiliser cette commande pour mettre la box en pause ou pour l’arrêter complètement.

**vagrant destroy** : supprime la Vagrant Box. Cette commande arrête la box et la supprime de ta machine. Tu peux utiliser cette commande pour nettoyer ton environnement de développement

**Vagrant reload** : redémarrer l'environnement 