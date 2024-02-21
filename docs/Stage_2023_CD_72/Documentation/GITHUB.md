#  Création d'un dépôt GITHUB pour héberger mon Portfolio

## Comment crée un dépot GITHUB 

Tout d'abord il faudra vous rendre sur ce site : [GITHUB](https://github.com/)

Il faudra vous connectez avec vos codes fourni aux préalables :

![Image connexion GITHUB](../../images/conexiongit.jpg)

Une fois connecté il faudra **Cliquer sur "new" en haut à droite de la page**

![new dépôt](../../images/new.jpg)

Ensuite renseigner les informations suivante :

![dépôt](../../images/dépôt.jpg)

**<span style="color:red">Voilà votre dépôt GITHUB créer !!</span>**

## Activer la page web 

Aller dans l'onglet **"settings"** 

Puis aller dans l'onglet pages :

![Page](../../images/page.jpg)

Vous devriez arriver sur cet onglet : 

![Page_2.0](../../images/page_2.0.jpg)

## Création fichier "ci.yml"

<span style="color:red">Ce fichier est un script qui permet de construire et de déployer son site automatiquement</span>

Pour ce faire :

~~~bash
mkdir .github
mkdir workflows
vim ci.yml
~~~

Ensuite il faudra inscrire ceci dans le fichier :

~~~bash
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.x

      - name: Install dependencies
        run: |
          pip install mkdocs mkdocs-material mkdocs-mermaid2-plugin

      - name: Build site
        run: mkdocs build --verbose --clean

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: site
~~~
# **<span style="color:red"> Attention </span>**

**Il faudra penser à mettre le deploy dans la bonne branche**

Pour ce faire aller dans **"settings"**

Puis aller sur l'onget **"pages"** :

![Onglet page](../../images/page.jpg)

Ensuite sélectionner github action

![page](../../images/page_2.0.jpg)

et vous allez mettre **"Deploy from a branch"** :

![deploy](../../images/deploy_ghpage.jpg)

# **<span style="color:red"> Attention </span>**

**Il faudra penser à mettre les bons droits pour le workflows**

Pour ce faire aller dans **"settings"**

Ensuite aller dans l'onglet **"actions"** :

![actions](../../images/actions.jpg)

Ensuite il faudra descendre jusqu'à trouver ceci : 

![Droit Workflows](../../images/droit.jpg)

Cocher les mêmes choses que moi !

**Pensez à "save"**

**<span style="color:red">Voilà dépôt prêt à être utiliser !!! </span>**

