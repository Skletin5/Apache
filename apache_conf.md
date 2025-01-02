# Apache.conf
El fichero 'apache.conf' contiene las directivas de configuración para nuestros servidores apache.

## Directivas
Aqui dispondre una lista de distintas directivas de apache, que hacen, su valor predeterminado y el valor recomendado.  
- `ServerTokens`: Información que se muestra sobre el servidor, como la versión de apache y el SO del servidor.  
      Valor predeterminado: ServerToken Full            Valor recomendado: ServerToken Prod  
- `MaxKeepAliveRequest`: Numero de conexiones simultaneas.  
      Valor predeterminado: MaxKeepAliveRequest 100     Valor recomendado: MaxKeepAliveRequest 500 o mas  
- `Timeout`: Tiempo que el servidor espera para que se completen ciertas tareas.  
      Valor predeterminado: Timeout 60                  Valor recomendado: Timeout 300  
- `LogLevel`: Define el tipo de datos que se escriben en los logs del servidor.
      Valor predeterminado: LogLevel warn               Valor recomendado: LogLevel crit
