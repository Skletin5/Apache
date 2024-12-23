# Zonas virtuales
Podemos tener varias paginas distintas en nuestro servidor apache creando zonas virtuales distintas, tenemos distintas formas de crearlas:

## Por nombre
Primero devemos crear los directorios para cada pagina:  
```
$ mkdir /var/www/pagina_1  
$ touch /var/www/pagina_1/index.html  
$ mkdir /var/www/pagina_2  
$ touch /var/www/pagina_1/index.html
```
