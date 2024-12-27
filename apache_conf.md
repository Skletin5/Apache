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
/html11/index.html  
`<h1>Soy la html1</h1>`

/html2/index.html  
`<h1>Soy la html2</h1>`

tras esto, configuraremos `html1.conf` y `html2.conf`.  
```
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
## Por nombre
Primero devemos crear los directorios para cada pagina:  
```
$ mkdir /var/www/pagina_1  
$ touch /var/www/pagina_1/index.html  
$ mkdir /var/www/pagina_2  
$ touch /var/www/pagina_1/index.html
```
