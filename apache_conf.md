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

- `Alias`: Podemos definir un alias para una ruta.  
      `Alias /NOMBRE_ALIAS /RUTA_REAL`  
  Por ejemplo is tubieramos la ruta `/var/www/sitio/contacto` y pusieramos el alias `/info`
  al escribir en el navegador `www.sitio.com/info` accederiamos contactos.  

  Otro uso para alias seria escribir un alias para otra ruta fuera del directorio original como `/var/www/enlaces`.

- `Redirect`: Similar a los alias por en este caso se REDIRIGE a otra enlace, tanto ajeno como del propio servidor.
      `Redirect /NOMBRE_REDIRECT ENLACE_A_REDIRIGIR`  
  Por ejemplo: `Redirect /navegador https://www.google.es`

- `ErrorDocument`: Nos permite modificar el mensaje que muestra la pagina ante errores.  
      `ErrorDocument COD_ERROR 'MENSAJE DE ERROR' o /PAGINA_DE_ERROR_PERSONALIZADA`
  Por ejemplo: `ErrorDocument 404 'Pagina no encontrada' o /var/www/error/pagina_no_encontrada.html
  
## Directory
Este parrafo nos permite delimitar un directorio al cual apache podra acceder o no, mas algunas opciones que definiran su funcionamiento.  
  
      <Directory /var/www/>
              Options Indexes FollowSymLinks
              AllowOverride None
              Order allow,deny
              Allow from 192.168.1.0/24
              Require all granted
      </Directory>

  A continuacion mostrare el significado de cada linea:  
  - `Options Indexes`: Si apache no encuentra un fichero que mostrar al realizarse la busqueda, la opcion indexes nos mostrara una lista con el contenido del directorio accedido.
  - `Options FollowSymLinks`: Permite que los enlaces simbolicos funcionen.
  - `Options Multiviews`: Permite que al existir varias versiones por idiomas del `index.hmtl` el navegador escoja el del idioma que tenga definido como prioritario. En caso de que no disponga de ninguno, apache mostrar el primero indicado en el fichero `/etc/apache2/mods-enabled/negotiation.conf`.
  - `Order`: Nos permite indicar si queremos permitir o denegar el acceso global y agregar excepciones.  
            `Order allow,deny`: Prohibe le acceso a todo el mundo menos a redes o direcciones en concreto.   
            `Order deny,allow`: Todo el mundo puede acceder menos ciertas redes o direcciones cocretas.  
    Para indicar dichas excepciones debemos escribir los siguiente tras la linea de Order:  
            `Allow from DIRECCION_IP o DIRECCION_DE_RED/MASCARA`: Da permiso de acceso a ciertas direcciones o redes.  
            `Deny from DIRECCION_IP O DIRECCION_DE_RED/MASCARA`: Prohibe el acceso a ciertas direcciones o redes.  
    Para incluir varias direcciones debemos separar cada una con espacios.

    Adicionalmente, tambien podemos controlar el acceso por usuario y contraseña con las siguientes lineas:  
          
          Order deny,allow
          AuthUserfile "/etc/apache2/claves.txt"
          AuthName "Identifiquese"
          AuthType Basic
          Require valid-user
    
    Tras escribir esto, `AuthUserfile` buscara en la ruta indicada el fichero que deberemos crear de la siguiente manera:  
          `htpasswd -C RUTA_FICHERO.txt NOMBRE_USUARIO`: Este comando nos permitira registrar en el fichero que se creara con la opcion `-c` el usuario indicado.
    
    Tras esto nos permitira que escribamos una contraseña para el usuario, la cual re registrara en hash en el fichero.
    Para agregar mas usuarios al mismo fichero, escribiremos el mismo comando de antes sin la opcion `-c` ya que el fichero ya esta creado.
    
    `AuthName` enseña el mensaje entre comillas en la ventana que mostrara al navegador para pedir le usuario y contraseña.
    
  - `Require all`: Si a continuación escribimos `granted` permitiremos a apache mostrar este directorio, `denied` prohibe el acceso.  
  
