<properties
   pageTitle="Conector de HDInsight"
   description="Uso del conector de HDInsight en el Servicio de aplicaciones de Azure"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/19/2015"
   ms.author="sameerch"/>


# Conector de Microsoft HDInsight #

Los conectores pueden utilizarse en aplicaciones lógicas para capturar, procesar o insertar datos como parte de un flujo. El conector de HDInsight le permite crear clústeres de Hadoop en Azure y enviar trabajos de Hadoop diferentes como Hive, Pig, MapReduce y MapReduce de streaming. El servicio de Azure HDInsight implementa y aprovisiona clústeres de Apache Hadoop en la nube con el fin de proporcionar un marco de software diseñado para realizar tareas de administración, análisis y generación de informes en relación con grandes volúmenes de datos. El núcleo de Hadoop proporciona almacenamiento de datos confiable con el sistema de archivos distribuido Hadoop (HDFS, Hadoop Distributed File System) y un sencillo modelo de programación MapReduce para procesar y analizar, en paralelo, los datos almacenados en este sistema distribuido. Mediante el conector de HDInsight puede crear o eliminar un clúster, enviar un trabajo y esperar a que el trabajo finalice.

### Acciones básicas

- Crear clúster
- Esperar a que se cree el clúster
- Enviar trabajo de Pig
- Enviar trabajo de Hive
- Enviar trabajo de MapReduce
- Esperar la finalización del trabajo
- Eliminar clúster


## Creación de la aplicación de API del conector de HDInsight ##

Un conector puede crearse dentro de una aplicación lógica o directamente desde Azure Marketplace. Para crear un conector desde Marketplace:

1. En el panel de inicio de Azure, seleccione **Marketplace**.
2. Busque "Conector de HDInsight", selecciónelo y seleccione **Crear**.
3. Escriba el nombre, el plan del Servicio de aplicaciones y otras propiedades.
4. En la configuración del paquete, especifique el nombre de usuario y la contraseña del clúster de HDInsight. Seleccione **Aceptar**.
5. Seleccione **Crear**: ![][1]  

## Configuración del certificado (opcional) ##

> [AZURE.NOTE]Este paso solo se necesita si desea realizar operaciones de administración (creación y eliminación de clústeres) en la aplicación lógica.

Vaya a la aplicación de API del conector de HDInsight que acaba de crear y observará que en el componente “Seguridad” aparece 0, lo que significa que no hay ningún certificado de administración cargado: ![][2]

Para cargar el certificado de administración en la aplicación de API:

1. Seleccione el componente “Seguridad”.
2. En la hoja “Seguridad”, seleccione **CARGAR CERTIFICADO**.
3. Busque y seleccione el archivo de certificado en la hoja siguiente.
4. Una vez seleccionado el certificado, seleccione **Aceptar**.

Cuando el certificado se ha cargado correctamente, se muestran los detalles de este: ![][3]

> [AZURE.NOTE]Si desea cambiar el certificado, puede cargar otro para reemplazar el existente.

## Uso del conector en una aplicación lógica ##

El conector de HDInsight puede usarse solo como acción de la aplicación lógica. Empecemos por una aplicación lógica sencilla que crea un clúster, ejecuta un trabajo “Hive” y elimina el clúster al final de la finalización del trabajo.


1. En la ficha "Iniciar lógica", haga clic en "Ejecutar esta lógica manualmente".
2. Seleccione la aplicación de API del conector de HDInsight creada desde la galería. Se enumeran las acciones disponibles: ![][5]

3. Seleccione “Crear clúster”, especifique todos los parámetros del clúster necesarios y seleccione el signo ✓: ![][6]

4. La acción aparece ahora como configurada en la aplicación lógica. Se muestran las salidas de la acción, que se pueden usar como entradas en un paso posterior: ![][7]

5. Seleccione el mismo conector de HDInsight de la Galería como una acción. Seleccione la acción “Esperar la creación de clústeres”, especifique todos los parámetros necesarios y seleccione el signo ✓: ![][8]

6. Seleccione el mismo conector de HDInsight de la Galería como una acción. Seleccione la acción “Enviar trabajo de Hive”, especifique todos los parámetros necesarios y seleccione el signo ✓: ![][9]

7. Seleccione el mismo conector de HDInsight de la Galería como una acción. Seleccione la acción “Esperar la finalización del trabajo”, especifique todos los parámetros necesarios y seleccione el signo ✓: ![][10]

8. Seleccione el mismo conector de HDInsight de la Galería como una acción. Seleccione la acción “Eliminar clúster”, especifique todos los parámetros necesarios y seleccione el signo ✓: ![][11]

9. Guarde la aplicación lógica con el comando habilitado para ello en la parte superior del diseñador.

Para probar el escenario, seleccione **Ejecutar ahora** para iniciar la aplicación lógica manualmente.

## Aplicaciones adicionales del conector
Una vez creado el conector, puede agregarlo a un flujo de trabajo empresarial mediante una aplicación lógica. Consulte [¿Qué son las aplicaciones lógicas?](app-service-logic-what-are-logic-apps.md)

Consulte la referencia de API de REST de Swagger en [Referencia de conectores y aplicaciones de API](http://go.microsoft.com/fwlink/p/?LinkId=529766).

También puede consultar las estadísticas de rendimiento y la seguridad de control para el conector. Consulte [Administración y supervisión de las aplicaciones de API y los conectores integrados](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-hdinsight/Create.jpg
[2]: ./media/app-service-logic-connector-hdinsight/CertNotConfigured.jpg
[3]: ./media/app-service-logic-connector-hdinsight/CertConfigured.jpg
[5]: ./media/app-service-logic-connector-hdinsight/LogicApp1.jpg
[6]: ./media/app-service-logic-connector-hdinsight/LogicApp2.jpg
[7]: ./media/app-service-logic-connector-hdinsight/LogicApp3.jpg
[8]: ./media/app-service-logic-connector-hdinsight/LogicApp4.jpg
[9]: ./media/app-service-logic-connector-hdinsight/LogicApp5.jpg
[10]: ./media/app-service-logic-connector-hdinsight/LogicApp6.jpg
[11]: ./media/app-service-logic-connector-hdinsight/LogicApp7.jpg

<!---HONumber=August15_HO8-->