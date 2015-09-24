<properties
	pageTitle="Copia de seguridad de máquinas virtuales de Azure: copia de seguridad | Microsoft Azure"
	description="Aprenda cómo hacer una copia de seguridad de una máquina virtual de Azure después del registro"
	services="backup"
	documentationCenter=""
	authors="aashishr"
	manager="shreeshd"
	editor=""/>

<tags ms.service="backup" ms.workload="storage-backup-recovery" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="hero-article" ms.date="07/30/2015" ms.author="aashishr"; "jimpark"/>


# Copia de seguridad de máquinas virtuales de Azure
Este artículo es la guía esencial para realizar copias de seguridad de máquinas virtuales de Azure. Antes de continuar, asegúrese de que todos los [requisitos previos](backup-azure-vms-introduction.md#prerequisites) se han cumplido.

La realización de copias de seguridad de máquinas virtuales de Azure consta tres pasos principales:

![Tres pasos para hacer una copia de seguridad de una máquina virtual de Azure](./media/backup-azure-vms/3-steps-for-backup.png)

## 1\. Detección de máquinas virtuales de Azure
El proceso de detección envía consultas a Azure para obtener la lista de máquinas virtuales incluidas en la suscripción, junto con información adicional, por ejemplo, el nombre del servicio en la nube y la región.

> [AZURE.NOTE]Siempre se debe ejecutar el proceso de detección como el primer paso. De este modo se asegura de que se identifican las máquinas virtuales nuevas que se agregan a la suscripción.

### Para desencadenar el proceso de detección

1. Vaya al almacén de credenciales de copia de seguridad, que puede encontrarse en **Servicios de recuperación** en el Portal de Azure y haga clic en la pestaña **Elementos registrados**.

2. Elija el tipo de carga de trabajo en el menú desplegable como **Máquina virtual de Azure** y haga clic en el botón **Seleccionar**.

    ![seleccionar carga de trabajo](./media/backup-azure-vms/discovery-select-workload.png)

3. Haga clic en el botón **DETECTAR** que se encuentra en la parte inferior de la página. ![botón Detectar](./media/backup-azure-vms/discover-button-only.png)

4. El proceso de detección puede ejecutarse durante varios minutos mientras se tabulan las máquinas virtuales. Aparece una notificación del sistema en la parte inferior de la pantalla mientras se ejecuta el proceso de detección.

    ![detectar máquinas virtuales](./media/backup-azure-vms/discovering-vms.png)

5. Una vez completado el proceso de detección, aparece una notificación del sistema.

    ![detectar-completado](./media/backup-azure-vms/discovery-complete.png)

##  2\. Registro de máquinas virtuales de Azure
Para que se pueda proteger una máquina virtual, esta debe estar registrada en el servicio Copia de seguridad de Azure. El proceso de registro tiene dos objetivos principales:

1. Conectar la extensión de copia de seguridad en el agente de VM en la máquina virtual

2. Para asociar la máquina virtual con el servicio Copia de seguridad de Azure

El registro suele ser una actividad de una vez. El servicio Copia de seguridad de Azure controla perfectamente la actualización y revisión de la extensión de copia de seguridad sin necesidad de intervención incómoda del usuario. Esto libera al usuario de la "sobrecarga de administración de agente" que normalmente se asocia con productos de copia de seguridad.

### Para registrar las máquinas virtuales

1. Vaya al almacén de credenciales de copia de seguridad, que puede encontrarse en **Servicios de recuperación** en el Portal de Azure y haga clic en la pestaña **Elementos registrados**

2. Elija el tipo de carga de trabajo en el menú desplegable como **Máquina virtual de Azure** y haga clic en el botón Seleccionar.

    ![seleccionar carga de trabajo](./media/backup-azure-vms/discovery-select-workload.png)

3. Haga clic en el botón **REGISTRAR** que se encuentra en la parte inferior de la página. ![botón Registrar](./media/backup-azure-vms/register-button-only.png)

4. En la ventana emergente **Elementos registrados**, elija las máquinas virtuales que desea registrar. Si hay dos o más máquinas virtuales con el mismo nombre, use el servicio en la nube para distinguir entre las máquinas virtuales.

    La operación **Registrar** puede realizarse a escala, lo que significa que se pueden seleccionar a la vez varias máquinas virtuales para registrar. Esto reduce en gran medida el esfuerzo único realizado para preparar la máquina virtual para la copia de seguridad.

    >[AZURE.NOTE]Se mostrarán solo las máquinas virtuales que no estén registradas y pertenezcan a la misma región que el almacén de copia de seguridad.

5. Se crea un trabajo para cada máquina virtual que se debe registrar. La notificación del sistema muestra el estado de esta actividad. Haga clic en **Ver trabajo** para ir a la página **Trabajos**.

    ![registrar trabajo](./media/backup-azure-vms/register-create-job.png)

6. La máquina virtual también aparece en la lista de elementos registrados y se muestra el estado de la operación de registro

    ![Registrando estado 1](./media/backup-azure-vms/register-status01.png)

7. Una vez completada la operación, se cambiará el estado en el portal para reflejar el estado registrado.

    ![Registrando estado 2](./media/backup-azure-vms/register-status02.png)

## 3\. Protección: copia de seguridad de máquinas virtuales de Azure
Este paso implica configurar una directiva de copia de seguridad y retención para la máquina virtual. Para proteger una máquina virtual, siga estos pasos:

### Para realizar una copia de seguridad de máquinas virtuales de Azure
1. Vaya al almacén de credenciales de copia de seguridad, que puede encontrarse en **Servicios de recuperación** en el Portal de Azure y haga clic en la pestaña **Elementos registrados**.
2. Elija el tipo de carga de trabajo en el menú desplegable como **Máquina virtual de Azure** y haga clic en el botón **Seleccionar**.

    ![Seleccionar carga de trabajo en el portal](./media/backup-azure-vms/select-workload.png)

3. Haga clic en el botón **PROTEGER** que se encuentra en la parte inferior de la página.

4. Se abrirá el asistente **Proteger elementos**, donde puede seleccionar las máquinas virtuales que se protegerán. Si hay dos o más máquinas virtuales con el mismo nombre, use el servicio en la nube para distinguir entre las máquinas virtuales.

    La operación **Proteger** puede realizarse a escala, lo que significa que se pueden seleccionar a la vez varias máquinas virtuales para proteger. Esto reduce en gran medida el esfuerzo único realizado para proteger la máquina virtual.

    >[AZURE.NOTE]Aquí solo se mostrarán las máquinas virtuales que se han registrado correctamente con el servicio Copia de seguridad de Azure y que están en la misma región que el almacén de copia de seguridad.

5. En la segunda pantalla del asistente **Proteger elementos**, elija una directiva de copia de seguridad y retención para hacer una copia de seguridad de las máquinas virtuales seleccionadas. Elija una directa del conjunto de directivas existentes o defina una nueva.

    >[AZURE.NOTE]Para la vista previa, se admiten hasta 30 días de retención y un máximo de una copia de seguridad una vez al día.

    En cada almacén de copia de seguridad, puede tener varias directivas de copia de seguridad. Las directivas reflejan los detalles acerca de cómo se debe programar y retener la copia de seguridad. Por ejemplo, una directiva de copia de seguridad podría ser para copia de seguridad diaria a las 22:00 h, mientras que otra directiva de copia de seguridad podría ser para copia de seguridad semanal a las 6:00 h. Varias directivas de copia de seguridad proporcionan flexibilidad en la programación de copias de seguridad de su infraestructura de máquinas virtuales.

    Cada directiva de copia de seguridad puede tener varias máquinas virtuales asociadas. La máquina virtual puede asociarse solo con una directiva en cualquier momento.

6. Se crea un trabajo para cada máquina virtual para configurar la directiva de protección y asociar las máquinas virtuales a la directiva. Haga clic en la pestaña **Trabajos** y elija el filtro adecuado para ver la lista de trabajos de **Configurar protección**.

    ![Configurar trabajo de protección](./media/backup-azure-vms/protect-configureprotection.png)

7. Una vez completada la acción, las máquinas virtuales se protegen con una directiva y se debe esperar a que se complete el tiempo de copia de seguridad programado para la copia de seguridad inicial. La máquina virtual aparecerá ahora en la pestaña **Elementos protegidos** y tendrá un estado protegido de *Protegido* (copia de seguridad inicial pendiente).
    >[AZURE.NOTE]De momento, el inicio de la copia de seguridad inicial inmediatamente después de configurar la protección no está disponible.

8. En el tiempo programado, el servicio Copia de seguridad de Azure crea un trabajo de copia de seguridad para cada máquina virtual de la que se debe hacer una copia de seguridad. Haga clic en la pestaña **Trabajos** para ver la lista de los trabajos de **Copia de seguridad**. Como parte de la operación de copia de seguridad, el servicio Copia de seguridad de Azure emite un comando a la extensión de copia de seguridad en cada máquina virtual para vaciar toda la escritura y tomar una instantánea coherente.

    ![Copia de seguridad en curso](./media/backup-azure-vms/protect-inprogress.png)

9. Una vez completada la acción, el estado de protección de la máquina virtual en la pestaña **Elementos protegidos** se mostrará como *Protegido*.

    ![Se realiza una copia de seguridad de la máquina virtual con punto de recuperación](./media/backup-azure-vms/protect-backedupvm.png)

## Visualización de los detalles y el estado de la copia de seguridad
Una vez protegidas, el recuento de máquinas virtuales también aumenta en el resumen de la página **Panel**. Además, la página Panel muestra el número de trabajos de las últimas 24 horas que se realizaron correctamente, que han producido un error y que siguen en curso. Al hacer clic en una categoría, esta se desglosará en la página **Trabajos**.

![Estado de la copia de seguridad en la página Panel](./media/backup-azure-vms/dashboard-protectedvms.png)

## Solución de errores
Puede solucionar los errores detectados al usar Copia de seguridad de Azure con la información incluida en la tabla siguiente.

| Operación de copia de seguridad | Detalles del error | Solución alternativa |
| -------- | -------- | -------|
| Detección | Se ha producido un error al detectar elementos nuevos: Copia de seguridad de Microsoft Azure ha detectado un error interno. Espere unos minutos y vuelva a intentar la operación. | Después de 15 minutos, vuelva a intentar el proceso de detección.
| Detección | Se ha producido un error al detectar elementos nuevos: ya hay otra operación de detección en curso. Espere hasta que se haya completado la operación de detección. | None |
| Registro | El rol de VM de Azure no está en un estado para instalar la extensión: compruebe si la máquina virtual está en estado de ejecución. La extensión de Servicios de recuperación de Azure requiere que la máquina virtual se esté ejecutando. | Inicie la máquina virtual y cuando esté en estado en ejecución, vuelva a intentar la operación de registro.|
| Registro | El número de discos de datos asociados a la máquina virtual sobrepasa el límite admitido. Desconecte algunos discos de datos de esta máquina virtual y vuelva a intentarlo. Copia de seguridad de Azure admite hasta 5 discos de datos conectados a una máquina virtual de Azure para copia de seguridad. | None |
| Registro | Copia de seguridad de Microsoft Azure encontró un error interno. Espere unos minutos y vuelva a intentarlo. Si el problema persiste, póngase en contacto con el Soporte técnico de Microsoft. | Este error puede deberse a una de las siguientes configuraciones no admitidas: <ol><li>LRS Premium <li>Varias NIC <li>Equilibrador de carga </ol> |
| Registro | No se encuentra el certificado del agente invitado de la máquina virtual. | Siga estas instrucciones para resolver el error: <ol><li>Descargue la versión más reciente del agente de la máquina virtual desde [aquí](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Asegúrese de que la versión del agente descargado sea la 2.6.1198.718 o una posterior. <li>Instale el agente de la máquina virtual en la máquina virtual.</ol> [Aprenda](#validating-vm-agent-installation) a comprobar la versión del agente de la máquina virtual. |
| Registro | Se ha producido un error en el registro porque se ha agotado el tiempo de espera de la operación de instalación del agente. | Compruebe si se admite la versión del sistema operativo de la máquina virtual. |
| Registro | Error en la ejecución de comando: hay otra operación en curso en este elemento. Espere hasta que se complete la operación anterior. | None |
| Copia de seguridad | Al copiar discos duros virtuales desde el almacén de copia de seguridad se agotó el tiempo de espera. Vuelva a intentar la operación en unos minutos. Si el persiste el problema, póngase en contacto con el Soporte técnico de Microsoft. | Esto ocurre cuando hay demasiados datos que copiar. Compruebe si tiene menos de 6 discos de datos. |
| Copia de seguridad | Se agotó el tiempo de espera de la subtarea de la máquina virtual para realizar la instantánea: vuelva a intentar la operación en unos minutos. Si el problema persiste, póngase en contacto con el Servicio técnico de Microsoft. | Este error se produce si hay un problema con el agente de la máquina virtual o cuando el acceso de red a la infraestructura de Azure está bloqueado por algún motivo. <ul><li>Obtenga información sobre cómo [depurar los problemas del agente de la máquina virtual.](#Troubleshooting-vm-agent-related-issues) <li>Obtenga información sobre cómo [depurar los problemas relacionados con las redes.](#troubleshooting-networking-issues) </ul> |
| Copia de seguridad | Error interno en la copia de seguridad. Vuelva a intentar la operación en unos minutos. Si el problema persiste, póngase en contacto con el Servicio técnico de Microsoft. | Este error puede aparecer por 2 razones: <ol><li> Hay demasiados datos que copiar. Compruebe si tiene menos de 6 discos. <li>Se ha eliminado la máquina virtual original y, por tanto, no se puede realizar la copia de seguridad. Para mantener los datos de copia de seguridad de una máquina virtual eliminada y, a la vez, detener los errores de copia de seguridad, desproteja la máquina virtual y elija la opción para conservar los datos. Este comando detiene la programación de la copia de seguridad y también los mensajes de error recurrentes. |
| Copia de seguridad | No se pudo instalar la extensión de los Servicios de recuperación de Azure en el elemento seleccionado. El agente de la máquina virtual es un requisito previo para la extensión de los servicios de recuperación de Azure. Instale el agente de la máquina virtual de Azure y reinicie el funcionamiento del registro. | <ol> <li>Compruebe si el agente de la máquina virtual se ha instalado correctamente. <li>Asegúrese de que la marca de configuración de la máquina virtual se haya establecido correctamente.</ol> [Obtenga más información](#validating-vm-agent-installation) acerca del agente de la máquina virtual y sobre cómo validar su instalación. |
| Copia de seguridad | Error en la ejecución del comando: otra operación está actualmente en curso en este elemento. Espere hasta que se complete la operación anterior y vuelva a intentarlo. | Se está ejecutando una copia de seguridad existente o un trabajo de restauración para la máquina virtual. No se puede iniciar un nuevo trabajo mientras se está ejecutando otro. <br><br>Si desea que esté disponible la opción para cancelar un trabajo en curso, especifique su voto en el [foro de comentarios de Azure](http://feedback.azure.com/forums/258995-azure-backup-and-scdpm/suggestions/7941501-add-feature-to-allow-cancellation-of-backup-restor). |

### Resolución de problemas relacionados con el agente de la máquina virtual

#### Configuración del agente de la máquina virtual
Normalmente, el agente de la máquina virtual ya está presente en las máquinas virtuales que se crean desde la Galería de Azure. Sin embargo, las máquinas virtuales que se migran desde los centros de datos locales no tienen instalado el agente de la máquina virtual. Para dichas máquinas virtuales, el agente de la máquina virtual debe instalarse explícitamente. Lea más sobre cómo [instalar el agente de la máquina virtual en una máquina virtual existente](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Para las máquinas virtuales de Windows:

- Descargue e instale el [agente MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Necesitará privilegios de administrador para efectuar la instalación.
- [Actualice la propiedad de la máquina virtual](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) para indicar que el agente está instalado.


#### Actualización del agente de la máquina virtual
Actualizar el agente de la máquina virtual es tan sencillo como volver a instalar los [archivos binarios del agente de la máquina virtual](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Sin embargo, deberá asegurarse de que no se está ejecutando ninguna operación de copia de seguridad mientras se actualiza el agente de la máquina virtual.


#### Validación de la instalación del agente de la máquina virtual
Cómo comprobar la versión del agente de la máquina virtual en máquinas virtuales de Windows:

1. Inicie sesión en la máquina virtual de Azure y vaya a la carpeta *C:\\WindowsAzure\\Packages*. El archivo WaAppAgent.exe debe estar ahí.
2. Haga clic con el botón derecho en el archivo, vaya a **Propiedades** y seleccione la pestaña **Detalles**. En el campo de versión del producto, debe aparecer el valor 2.6.1198.718 o uno superior.

### Resolución de problemas de red
Al igual que todas las extensiones, la de copia de seguridad necesita tener acceso a Internet para funcionar. Si no tiene acceso a Internet, se pueden producir estos problemas:

- Se puede producir un error en la instalación de la extensión.
- Se puede producir un error en las operaciones de copia de seguridad (como al realizar la instantánea de disco).
- Se puede producir un error en la operación para mostrar el estado de la operación de copia de seguridad.

La necesidad de resolver direcciones públicas de Internet se describe [aquí](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Deberá comprobar las configuraciones de DNS para la red virtual y asegurarse de que se puedan resolver los URI de Azure.

Una vez que la resolución de nombres se haya realizado correctamente, también hay que proporcionar acceso a las direcciones IP de Azure. Para desbloquear el acceso a la infraestructura de Azure, siga estos pasos:

1. Obtenga la lista de [IP del centro de datos de Azure](https://msdn.microsoft.com/library/azure/dn175718.aspx) que van a formar parte de la lista de direcciones IP aprobadas.
2. Desbloquee las direcciones IP usando el commandlet [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx). Ejecute este commandlet en la máquina virtual de Azure, en una ventana de PowerShell con privilegios elevados (realice la ejecución como administrador).


## Coherencia de los puntos de recuperación
Cuando se trabaja con datos de copia de seguridad, los clientes se preocupan por el comportamiento de la máquina virtual después de haberla restaurado. Las preguntas típicas que plantean los clientes son:

- ¿Arrancará la máquina virtual?
- ¿Los datos estarán disponibles en el disco o se producirá una pérdida de datos?
- ¿La aplicación podrá leer los datos o se dañarán los datos?
- ¿Los datos tendrán sentido para la aplicación o serán coherentes cuando la aplicación los lea?

La siguiente tabla explica los tipos de coherencia que se detectan durante la restauración y la copia de seguridad de máquinas virtuales de Azure:

| Coherencia | Basada en el VSS | Explicación y detalles |
|-------------|-----------|---------|
| Coherencia de las aplicaciones | Sí | Este es el lugar ideal para las cargas de trabajo de Microsoft, ya que garantiza:<ol><li> que la máquina virtual *arranque*, <li>no *se produzca ningún daño*, <li>no se produzca ninguna *pérdida de datos* y<li> los datos sean coherentes con la aplicación que los usa, implicando la aplicación en el momento de la copia de seguridad mediante el uso del VSS.</ol> El servicio de instantáneas de volumen (VSS) garantiza que los datos se escriban correctamente en el almacenamiento. La mayoría de cargas de trabajo de Microsoft tienen escritores VSS que realizan acciones específicas de carga de trabajo relacionadas con la coherencia de los datos. Por ejemplo, Microsoft SQL Server tiene un escritor VSS que garantiza que las escrituras en el archivo de registro de transacciones y en la base de datos se realizan correctamente.<br><br> En la copia de seguridad de máquina virtual de Azure, obtener un punto de recuperación coherente con la aplicación significa que la extensión de copia de seguridad pudo invocar el flujo de trabajo VSS y completarlo *correctamente* antes de que se tomase la instantánea de la máquina virtual. Naturalmente, esto significa que los escritores VSS de todas las aplicaciones de la máquina virtual de Azure también se han invocado.<br><br>Obtenga [Aprenda los conceptos básicos de VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) y profundice en los detalles de su [funcionamiento](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx). |
| Coherencia del sistema de archivos | Sí, para máquinas de Windows | Hay dos escenarios donde el punto de recuperación puede ser coherente con el sistema de archivos:<ul><li>copia de seguridad de máquinas virtuales Linux en Azure, ya que Linux no tiene una plataforma equivalente a VSS.<li>Error de VSS durante la copia de seguridad de máquinas virtuales de Windows en Azure.</li></ul> En ambos casos, lo mejor que puede hacer es asegurarse de que: <ol><li> la máquina virtual *arranque*, <li> *no se produzca ningún daño* y <li>no se produzca *ninguna pérdida de datos*.</ol> Las aplicaciones deben implementar su propio mecanismo de "reparación" en los datos restaurados.|
| Coherencia de bloqueos | No | Esta situación es equivalente a aquellos casos en que una máquina experimenta un "bloqueo" (a través de un restablecimiento parcial o completo). Esto suele ocurrir cuando la máquina virtual de Azure se apaga en el momento de realizar la copia de seguridad. Para la copia de seguridad de la máquina virtual de Azure, obtener un punto de recuperación coherente con el bloqueo significa que Copia de seguridad de Azure no ofrece ninguna garantía sobre la coherencia de los datos en el medio de almacenamiento, ni desde la perspectiva del sistema operativo ni desde la perspectiva de la aplicación. Solamente se capturan y se hace una copia de seguridad de los datos que ya existen en el disco en el momento de la copia de seguridad. <br/> <br/> Aunque no hay ninguna garantía, en la mayoría de los casos, se iniciará el sistema operativo. Normalmente, esto va seguido de un procedimiento de comprobación de disco como chkdsk para corregir los errores por daños. Se perderán los datos o las escrituras en memoria que no se hayan vaciado completamente en el disco. Normalmente, la aplicación sigue con su propio mecanismo de comprobación en caso de que se deba realizar una reversión de datos. En la copia de seguridad de una máquina virtual de Azure, obtener un punto de recuperación coherente con el bloqueo significa que Copia de seguridad de Azure no ofrece ninguna garantía sobre la coherencia de los datos en el almacenamiento, ya sea desde el punto de vista del sistema operativo o desde el punto de vista de la aplicación. Esto ocurre normalmente cuando la máquina virtual de Azure se apaga en el momento de la copia de seguridad.<br><br>Por ejemplo, si el registro de transacciones tiene entradas que no están presentes en la base de datos, el software de la base de datos realiza una reversión hasta que los datos sean coherentes. Cuando se trabaja con datos repartidos en varios discos virtuales (por ejemplo, los volúmenes distribuidos), un punto de recuperación consistente para fallas proporciona garantías para la corrección de los datos.|

## Pasos siguientes
Para obtener más información acerca de cómo empezar a usar Copia de seguridad de Azure, consulte:

- [Restauración de máquinas virtuales](backup-azure-restore-vms.md)
- [Administración de máquinas virtuales](backup-azure-manage-vms.md)

<!---HONumber=August15_HO6-->