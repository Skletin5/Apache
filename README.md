# Apache
## Indice
1. Introducción
2. Instalación
3. Configuración
4. Personalización

## Introducción
El servicio Apache, conocido formalmente como Apache HTTP Server, es un servicio web de código abierto que se utiliza para subir contenido en la web.
Es conocido por su flexibilidad y capacidad de configuración, lo que lo hace ideal para una amplia variedad de aplicaciones web.
En esta guia explicaré los pasos a seguir para configurar este servicio y como personalizarlo.

## Instalación
Para instalar apache debemos introducir el sigueinte comando en nuestra terminal:  
`$ sudo apt install apache2`

Tras instalarlo ya tendremos disponible y funcionando nuestro servidor apache2, podemos comprobar su funcionamiento accediendo a localhost desde nuestro navegador o escribiendo la ip del servidor si quereis acceder remotamente.  
![localhost](/img/prueba.PNG)

## Configuración
Disponemos de varios ficheros de configuración para personalizar nuestro servidor:  
- [/etc/apache/sites-available/000-default.conf](/zonas_virtuales.md): Configuración de las zonas virtuales.  
- [/etc/apache/apache.conf](/apache_conf.md): Directivas de control de conexión.  

## Personalización
Podemos personalizar nuestra página editando el fichero index.html que viene en la zona virtual por defecto.
