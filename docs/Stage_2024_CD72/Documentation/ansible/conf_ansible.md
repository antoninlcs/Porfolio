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