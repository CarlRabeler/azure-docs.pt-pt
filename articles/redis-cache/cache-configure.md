<properties 
   pageTitle="Configuración de Caché en Redis de Azure"
   description="Descripción de la configuración predeterminada de Caché en Redis de Azure y más información sobre cómo configurar las instancias de Caché en Redis de Azure"
   services="redis-cache"
   documentationCenter="na"
   authors="steved0x"
   manager="dwrede"
   editor="tysonn" />
<tags 
   ms.service="cache"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="cache-redis"
   ms.workload="tbd"
   ms.date="07/24/2015"
   ms.author="sdanie" />

# Configuración de Caché en Redis de Azure

En este tema se describe cómo revisar y actualizar la configuración de las instancias de Caché en Redis de Azure y se trata la configuración predeterminada del servidor Redis para las instancias de Caché en Redis de Azure.

## Configuración de opciones de la memoria caché en Redis

Se puede tener acceso a las memorias caché en el [Portal de vista previa de Microsoft Azure](https://portal.azure.com) mediante la hoja **Examinar**.

![Caché en Redis de Azure - Hoja Examinar](./media/cache-configure/IC796920.png)

Haga clic en **cachés en Redis** para ver sus cachés.

![Caché en Redis de Azure - Examinar lista en caché](./media/cache-configure/IC796921.png)

Seleccione la memoria caché que quiera para ver las propiedades de dicha memoria.

![Caché en Redis - Todas las configuraciones](./media/cache-configure/IC808312.png)

Haga clic en **Configuración** o en **Todas las configuraciones** para ver y configurar la memoria caché.

![Caché en Redis - Configuración](./media/cache-configure/IC808313.png)

## Propiedades

Haga clic en **Propiedades** para ver información sobre la memoria caché, incluidos los puertos y el extremo de la caché.

![Caché en Redis - Propiedades](./media/cache-configure/IC808314.png)

## Claves de acceso

Haga clic en **Claves de acceso** para ver o volver a generar las claves de acceso de la memoria caché. Estas claves, junto con el nombre de host y los puertos que aparecen en la hoja **Propiedades**, las usan los clientes que se conectan a la memoria caché.

![Caché en Redis - Claves de acceso](./media/cache-configure/IC808315.png)

## Puertos de acceso

El acceso no SSL está deshabilitado de forma predeterminada para las nuevas cachés. Para habilitar el puerto no SSL, haga clic en la hoja **Puertos de acceso** y luego en **No**.

![Caché en Redis - Puertos de acceso](./media/cache-configure/IC808316.png)

## Diagnóstico

Haga clic en **Diagnóstico** para configurar la cuenta de almacenamiento que se usa para almacenar diagnósticos de caché.

![Caché en Redis - Diagnóstico](./media/cache-configure/IC808317.png)

Para obtener más información, vea [Supervisión de Caché en Redis de Azure](cache-how-to-monitor.md).

## Maxmemory-policy y maxmemory-reserved

Haga clic en **Directiva Maxmemory** para configurar las directivas de memoria para la caché. La opción **maxmemory-policy** configura la directiva de expulsión para la memoria caché y **maxmemory-reserved** configura la memoria reservada para los procesos no en caché.

![Caché en Redis - Directiva Maxmemory](./media/cache-configure/IC808318.png)

**Directiva Maxmemory** le permite elegir entre las siguientes directivas de expulsión.

-	volatile-lru: se trata de la directiva predeterminada.
-	allkeys-lru
-	volatile-random
-	allkeys-random
-	volatile-ttl
-	noeviction

Para obtener más información sobre las directivas maxmemory, vea [Directivas de expulsión](http://redis.io/topics/lru-cache#eviction-policies).

La opción **maxmemory-reserved** configura la cantidad de memoria en MB que se reserva para las operaciones no en caché como, por ejemplo, la replicación durante la conmutación por error. También puede usarse cuando se tiene una alta relación de fragmentación. Esta opción le permite tener una experiencia más coherente de servidor Redis cuando varía la carga. Este valor debe establecerse más alto para cargas de trabajo con muchas operaciones de escritura. Cuando la memoria se reserva para dichas operaciones, no está disponible para el almacenamiento de datos en caché.

>[AZURE.IMPORTANT]La opción **maxmemory-reserved** solo está disponible para las memorias caché estándar.

## Notificaciones de espacio de claves (configuración avanzada)

Haga clic en **Configuración avanzada** para configurar las notificaciones de espacio de claves de Redis. Las notificaciones de espacio de claves permiten que los clientes reciban notificaciones cuando se producen determinados eventos.

![Caché en Redis - Configuración avanzada](./media/cache-configure/IC808319.png)

>[AZURE.IMPORTANT]Las notificaciones de espacio de claves y la opción **notify-keyspace-events** solo están disponibles para las memorias caché estándar.

Para obtener más información, vea [Notificaciones de espacio de claves de Redis](http://redis.io/topics/notifications). Para obtener el código de ejemplo, vea el archivo [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) en el ejemplo [Hola a todos](https://github.com/rustd/RedisSamples/tree/master/HelloWorld).

## Usuarios y etiquetas

![Caché en Redis - Usuarios y etiquetas](./media/cache-configure/IC808320.png)

La sección **Usuarios** del Portal ofrece compatibilidad con el control de acceso basado en roles (RBAC) con el fin de que las organizaciones satisfagan sus requisitos de administración de acceso de forma simple y precisa. Para obtener más información, vea [Control de acceso basado en roles en el Portal de vista previa de Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=512803).

La sección **Etiquetas** le ayuda a organizar sus recursos. Para obtener más información, vea [Uso de etiquetas para organizar los recursos de Azure](../resource-group-using-tags.md).

## Configuración predeterminada del servidor Redis

Las nuevas instancias de Caché en Redis de Azure se configuran con los siguientes valores de configuración de Redis predeterminados.

>[AZURE.NOTE]La configuración de esta sección no se puede cambiar con el método `StackExchange.Redis.IServer.ConfigSet`. Si se llama a este método con uno de los comandos de esta sección, se produce una excepción similar a la siguiente:
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Los valores que se pueden configurar como **max-memory-policy**, son configurables a través del portal.

|Configuración|Valor predeterminado|Descripción|
|---|---|---|
|bases de datos|16|La base de datos predeterminada es DB 0. Se puede seleccionar una diferente por conexión mediante connection.GetDataBase(dbid), donde dbid es un número entre 0 y 15.|
|maxclients|10\.000|Se trata del número máximo de clientes conectados que se permiten al mismo tiempo. Una vez alcanzado el límite, Redis cerrará todas las nuevas conexiones y enviará un error de "número máximo de clientes alcanzado".|
|maxmemory-policy|volatile-lru|Directiva Maxmemory es la opción que configura el modo en que Redis seleccionará lo que se debe quitar cuando se alcanza el valor de maxmemory (el tamaño de la oferta de memoria caché que seleccionó al crear la memoria caché). Con Caché en Redis de Azure la opción predeterminada es volatile-lru, que quita las claves con una fecha de expiración definida mediante un algoritmo LRU. Esta opción puede configurarse en el portal. Para obtener más información, vea [Maxmemory-policy y maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-samples|3|Los algoritmos LRU y TTL mínimo no son precisos sino aproximados (con el fin de ahorrar memoria), para que también pueda seleccionar el tamaño de muestra para comprobar. Por ejemplo, Redis comprobará de manera predeterminada tres claves y seleccionará la usada menos recientemente.|
|lua-time-limit|5\.000|Tiempo máximo de ejecución de un script Lua en milisegundos. Si se alcanza el tiempo máximo de ejecución, Redis registrará que un script está aún en ejecución una vez transcurrido el tiempo máximo permitido y empezará a responder a las consultas con un error.|
|lua-event-limit|500|Se trata del tamaño máximo de la cola de eventos de script.|
|client-output-buffer-limit normalclient-output-buffer-limit pubsub|0 0 032mb 8mb 60|Los límites de búfer de salida de cliente pueden usarse para forzar la desconexión de clientes que, por algún motivo (un motivo habitual es que un cliente de Pub/Sub no puede consumir mensajes tan rápidamente como el publicador los crea), no leen datos del servidor con suficiente rapidez. Para obtener más información, vea [http://redis.io/topics/clients](http://redis.io/topics/clients).|

## No se admiten comandos de Redis en Caché en Redis de Azure

>[AZURE.IMPORTANT]Dado que la configuración y administración de instancias de Caché en Redis de Azure se realizan mediante el portal de Azure, se deshabilitan los comandos siguientes. Si intenta invocarlos, recibirá un mensaje de error similar a `"(error) ERR unknown command"`.
>
>-	BGREWRITEAOF
>-	BGSAVE
>-	CONFIG
>-	DEBUG
>-	MIGRATE
>-	GUARDAR
>-	SHUTDOWN
>-	SLAVEOF

Para obtener más información sobre los comandos de Redis, vea [http://redis.io/commands](http://redis.io/commands).

## Consola de Redis

Puede emitir comandos de forma segura para sus instancias de Caché en Redis de Azure con la **consola de Redis** que está disponible para las memorias caché estándar. Para obtener acceso a la consola de Redis, haga clic en **Consola** desde la hoja **Caché en Redis**.

![Consola de Redis](./media/cache-configure/redis-console-menu.png)

>[AZURE.IMPORTANT]La consola de Redis solo está disponible para las memorias caché estándar.

Para emitir comandos con una instancia de la memoria caché, basta con escribir en el comando que desea en la consola.

![Consola de Redis](./media/cache-configure/redis-console.png)

Para obtener una lista de comandos de Redis que están deshabilitados para Caché en Redis de Azure, consulte la sección anterior [No se admiten comandos de Redis en Caché en Redis de Azure](#redis-commands-not-supported-in-azure-redis-cache). Para obtener más información sobre los comandos de Redis, vea [http://redis.io/commands](http://redis.io/commands).

## Pasos siguientes
-	Para obtener más información sobre cómo trabajar con comandos de Redis, vea [Cómo puedo ejecutar comandos de Redis?](cache-faq.md#how-can-i-run-redis-commands).

<!---HONumber=August15_HO6-->