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
  - `AllowOverride`: Indica si se permite el uso de ficheros [.htacces](Apache/htacces) sobre este directorio. `None` lo prohibe, `All` lo permite. 
  - `Order`: Nos permite indicar si queremos permitir o denegar el acceso global y agregar excepciones.  
            `Order allow,deny`: Prohibe le acceso a todo el mundo menos a redes o direcciones en concreto.   
            `Order deny,allow`: Todo el mundo puede acceder menos ciertas redes o direcciones cocretas.  
    Para indicar dichas excepciones debemos escribir los siguiente tras la linea de Order:  
            `Allow from DIRECCION_IP o DIRECCION_DE_RED/MASCARA`: Da permiso de acceso a ciertas direcciones o redes.  
            `Deny from DIRECCION_IP O DIRECCION_DE_RED/MASCARA`: Prohibe el acceso a ciertas direcciones o redes.  
    Para incluir varias direcciones debemos separar cada una con espacios.

    Adicionalmente, tambien podemos controlar el acceso por usuario y contraseña mediante dos tipos de autentificación:
    ### Basic
          
          Order deny,allow
          AuthType Basic
          AuthUserFile "/etc/apache2/claves.txt"
          AuthName "Identifiquese"
          Require valid-user
    
    Tras escribir esto, `AuthUserfile` buscara en la ruta indicada el fichero que deberemos crear de la siguiente manera:  
          `htpasswd -C RUTA_FICHERO.txt NOMBRE_USUARIO`: Este comando nos permitira registrar en el fichero que se creara con la opcion `-c` el usuario indicado.
    
    Tras esto nos permitira que escribamos una contraseña para el usuario, la cual re registrara en hash en el fichero.
    Para agregar mas usuarios al mismo fichero, escribiremos el mismo comando de antes sin la opcion `-c` ya que el fichero ya esta creado.

    Tambien podemos trabajar con grupos pero primero debemos habilitar el mod que nos permitira trabajar con ellos:  
    `a2enmod authz_groupfile`
    Tras esto deberemos hacer un restart.

    Luego crearemos un fichero .txt donde almacenaremos los grupos y sus usuarios de la siguiente forma:

          grupo1: usuario1
          grupo2: usuario2 usuario3
          grupo3: usuario4

    Despues, en nuestra directiva directory, bajo Order escribiremos la siguiente linea:
    `AuthGroupFile "RUTA_DEL_FICHERO"`  
    Y modificaremos la linea `Require`:  
    `Require group GRUPOS_PERMITIDOS`  
    Pondremos los nombres de los grupos con permiso de acceso a la pagina, si son varios, separados por espacios.
    ### Digest
          
          Order deny,allow
          AuthType Digest
          AuthUserFile "/etc/apache2/claves_digest.txt"
          AuthName "Grupo1"
          Require valid-user

    En esta configuración solo cambia el AuthType por Digest y que en el AuthName deberemos poner los grupos con permiso de acceso.
    Para que este metodo de autentificación funcione deberemos habilitar su modulo con el siguiente comando:
    `a2enmod auth_digest`  
    Y tras reiniciar el servicio, comenzaremos a crear el fichero con los grupos y usuarios con el siguiente comando:
    `htdigest -C /etc/apache2/claves_digest.txt grupo1 usuario1`
    Deberemos reintroducir el comando por cada usuario que deseemos introducir, pero muy importante de solo incluir `-c` la primera vez que introduzcamos el comando.
    
  - `Require all`: Si a continuación escribimos `granted` permitiremos a apache mostrar este directorio, `denied` prohibe el acceso.  
  
