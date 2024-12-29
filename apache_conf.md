# Zonas virtuales
Podemos tener varias paginas distintas en nuestro servidor apache creando zonas virtuales distintas, tenemos distintas formas de crearlas:

## Por IP
Para crear un zona virtual por ip necesitaremos diponer de al menos dos interfaces de red. Podemos configurarlas en `/etc/networking/interfaces`.  
Tras esto, debemos crear los directorios para las paginas:
```
$ mkdir /var/www/pagina_1  
$ touch /var/www/pagina_1/index.html  
$ mkdir /var/www/pagina_2  
$ touch /var/www/pagina_1/index.html
```
Escribimos un texto en cada index para que podamos saber que pagina estamos consultando al probarlo  
/html1/index.html  
`<h1>Soy la html1</h1>`

/html2/index.html  
`<h1>Soy la html2</h1>`

tras esto, configuraremos `html1.conf` y `html2.conf`.  
```bash
<VirtualHost IP_INTERFAZ:80>
        ServerAdmin webmaster@localhost
        ServerName www.example.org
        DocumentRoot /var/www/DIRECTORIO_HTML

        <Directory /var/www/DIRECTORIO_HTML>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </directory>
        ErrorLog ${APACHE_LOG_DIR}/html1_error.log
        CustomLog ${APACHE_LOG_DIR}/html1_access.log combined
</VirtualHost>
```
Tras conbfigurar ambos fichero, habilitamos las paginas con los siguientes comandos  
`$ sudo a2ensite html1.conf`  
`$ sudo a2ensite html2.conf`

Tambien recomiendo deshabilitar la pagina predeterminado en `000-default.conf`.  
`$ sudo a2dissite 000-default.conf`  

Luego reiniciamos el servicio apache2.  
`$ sudo systemctl restart apache2`

Tras esto podemos comprobar que funciona buscando las direcciones ip asociadas a las paginas en nuestro navegador.  

## Por Puerto
Para esto podemos aprovechar las dos paginas configuradas, solo deberemos cambiar unas pocas cosas.  
Primero volvemos a los ficheros de configuración de las zonas virtuales y modificamos el encabezado:  
```bash
<VirtualHost *:PUERTO_DESEADO>
...
```  
Tras configurar un puerto en cada pagina, deberemos dirigirnos al fichero de coniguración `/etc/apache2/ports.conf`. Aquí solo tendremos que agregar puertos a la lista:
```bash
Listen 80
Listen (puerto pagina 1)
Listen (puerto pagina 2)
```
Tras esto reiniciamos el servicio apache con `$ systemctl restart apache2` y comprobamos que funcionan accediendo a la dirección IP del servidor seguido por ":PUERTO_DE_LA_PAGINA".

## Por nombre
Primero devemos crear los directorios para cada pagina:  
```
$ mkdir /var/www/pagina_1  
$ touch /var/www/pagina_1/index.html  
$ mkdir /var/www/pagina_2  
$ touch /var/www/pagina_1/index.html
```
