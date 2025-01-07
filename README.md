# Apache
## Indice
1. Introducción
2. Instalación
3. Configuración
4. AWstats
5. Personalización

## Introducción
El servicio Apache, conocido formalmente como Apache HTTP Server, es un servicio web de código abierto que se utiliza para subir contenido en la web.
Es conocido por su flexibilidad y capacidad de configuración, lo que lo hace ideal para una amplia variedad de aplicaciones web.
En esta guia explicaré los pasos a seguir para configurar este servicio y como personalizarlo.

## Instalación
Para instalar apache debemos introducir el sigueinte comando en nuestra terminal:  
`$ sudo apt install apache2`

Tras instalarlo ya tendremos disponible y funcionando nuestro servidor apache2, podemos comprobar su funcionamiento accediendo a localhost desde nuestro navegador o escribiendo la ip del servidor si quereis acceder remotamente.  
![localhost](/img/localhost.PNG)

## Configuración
Disponemos de varios ficheros de configuración para personalizar nuestro servidor:  
- [/etc/apache/sites-available/000-default.conf](/zonas_virtuales.md): Configuración de las zonas virtuales.  
- [/etc/apache/apache.conf](/apache_conf.md): Directivas de control de conexión.  
- .htaccess: Ficheros de configuración para los directory de los ficheros de configuración que sobreescriben a los mismos.

## AWstats
AWstats es una herramienta que usaremos para que recoja los logs de apache y los transforme en estadisticas. Para su instalacion usaremos el siguiente comando:  
`sudo apt install awstats`  
Tras esto deberemos copiar un fichero en una nueva ubicación agregando a su nombre nuestro nombre de dominio:  
`cp /etc/awstats/awstats.conf /etc/awstats/awstats.TU_DOMINIO.conf`  
Por ejmplo: `awstats.sitio.com.conf`  

Tras esto, iremos a modificarlo. Con control + W podremos buscar las lineas que espicificaré.  
- `LogFile`= /var/log/apache2/access.log  
- `SiteDomain`= Nombre de nuestro dominio "Sitio1.com"  
- `HostAliases`= localhost 127.0.0.1 sitio.com
Guardamos el fichero y ejecutamos el siguiente script:  
`/usr/lib/cgi-bin/awstats.pl -config=TU_DOMINIO -update`  
Awstats ya esta configurado, ahora trabajaremos con apache2.

Primero habilitamos el siguiente mod:  
`sudo a2enmod cgi`  
Tras esto, vamos al fichero /etc/apach2/TU_DOMINIO.conf y agregamos estas lineas:
```bash
Alias /awstatsclasses "/usr/share/awstats/lib/"
Alias /awstats-icon "/usr/share/awstats/icon/"
Alias /awstatscss "/usr/share/doc/awstats/examples/css"
ScriptAlias /awstats/ /usr/lib/cgi-bin/
Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
```
Tras recargar apache, al acceder a esta dirección `http://www.TU_DOMINIO/awstats/awstats.pl?config=TU_DOMINIO` accederemos a una pagina que nos motrara las estadisticas de nuestro servidor como esta:  
![awstats](/img/awstats.PNG)  
Si quisieramos realizar varias consultas para comprobar que las estadisticas funcionan, podemnos usar el comando `ab -n 5000 http://www.sitio1.com/` para realizar 5000 peticiones al servidor. El unico problemas es que tendremos que ejecutar el mismo script que ejecutamos antes para que se actualizen las estadisticas, pero podemos solucionar esto configurando un crontab.  

Escribimos `crontab -e` y seleccionamos un editor. Nos vamos al final de la pagina y escribimos la siguiente linea:  
`0 */3 * * * /usr/lib/cgi-bin/awstats.pl -config=sitio1.com -update > /dev/null`  
Con esto se repetira el script cada tres horas y no se mostrara en el terminal.

## Personalización
Podemos personalizar nuestra página editando el fichero index.html que viene en la zona virtual por defecto.
