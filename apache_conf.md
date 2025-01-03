# Apache.conf
El fichero 'apache.conf' contiene las directivas de configuración para nuestros servidores apache.

## Directivas
Aqui dispondre una lista de distintas directivas de apache, que hacen, su valor predeterminado y el valor recomendado.  
- `ServerTokens`: Información que se muestra sobre el servidor, como la versión de apache y el SO del servidor.  
      Valor predeterminado: ServerToken Full
      Valor recomendado: ServerToken Prod
  
- `MaxKeepAliveRequest`: Numero de conexiones simultaneas.  
      Valor predeterminado: MaxKeepAliveRequest 100
      Valor recomendado: MaxKeepAliveRequest 500 o mas
  
- `Timeout`: Tiempo que el servidor espera para que se completen ciertas tareas.  
      Valor predeterminado: Timeout 60
      Valor recomendado: Timeout 300
  
- `LogLevel`: Define el tipo de datos que se escriben en los logs del servidor.  
      Valor predeterminado: LogLevel warn
      Valor recomendado: LogLevel crit

- `DirectoryIndex`: Define el fichero que apache mostrara al acceder desde el navegador, por defecto es el `index.html`.  
      `DirectoryIndex NOMBRE_DEL_FICHERO`  
  Podemos agregar varios ficheros para que muestre el siguiente en caso de que el primero no se encuentre.

- `Alias`: podemos definir un alias para una ruta.  
      `Alias /NOMBRE_ALIAS /RUTA_REAL`  
  Por ejemplo is tubieramos la ruta `/var/www/sitio/contacto` y pusieramos el alias `/info`
  al escribir en el navegador `www.sitio.com/info` accederiamos contactos.  

  Otro uso para alias seria escribir un alias para otra ruta fuera del directorio original como `/var/www/enlaces`.
  
## Directory
Este parrafo nos permite delimitar un directorio al cual apache podra acceder o no, mas algunas opciones que definiran su funcionamiento.  
  
      <Directory /var/www/>
              Options Indexes FollowSymLinks
              AllowOverride None
              Require all granted
      </Directory>

  A continuacion mostrare el significado de cada linea:  
  - `Options Indexes`: Si apache no encuentra un fichero que mostrar al realizarse la busqueda, la opcion indexes nos mostrara una lista con el contenido del directorio accedido.
  - `Options FollowSymLinks`: Permite que los enlaces simbolicos funcionen.
  - `Options Multiviews`: Permite que al existir varias versiones por idiomas del `index.hmtl` el navegador escoja el del idioma que tenga definido como prioritario. En caso de que no disponga de ninguno, apache mostrar el primero indicado en el fichero `/etc/apache2/mods-enabled/negotiation.conf`.  
  - `Require all`: Si a continuación escribimos `granted` permitiremos a apache mostrar este directorio, `denied` prohibe el acceso.  
  
