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
- `Directory`: Da permisos de acceso a directorios, en este caso mostrare como dar y revocar acceso a directorios con ejemplos del propio fichero.  
      Conceder permisos:  
      ```bash
      <Directory /var/www/>
              Options Indexes FollowSymLinks
              AllowOverride None
              Require all granted
      </Directory>
      ```
Este parrafo concederia permiso para acceder al contenido del directorio `/var/www/` donde se hayan los `index.html`.

      Revocar permisos:  
      ```bash
      <Directory />
            Options FollowSymLinks
            AllowOverride None
            Require all denied
      </Directory>
      ```
Este parrafo deniega el acceso al directorio raiz para asegurar que solo se puede acceder al contenido especificado en otras directivas Directory.  
