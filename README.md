
### Commande création image docker 
 
~~~bash
sudo docker build -t monsite-mkdocs .
~~~

### Commande start docker nginx

~~~bash
sudo docker run -d -p 8080:80 monsite-mkdocs
~~~