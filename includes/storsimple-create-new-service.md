
#### Para crear un nuevo servicio

1. Con sus credenciales de cuenta de Microsoft, inicie sesión en el Portal de administración en esta dirección URL: [http://azure.microsoft.com/](http://azure.microsoft.com/).

2. En el Portal de administración, haga clic en **Nuevo** > **Servicios de datos** > **Administrador de StorSimple** > **Creación rápida**.

3. En el formulario que aparece, haga lo siguiente:
  1. Proporcione un **Nombre** único para el servicio. Se trata de un nombre descriptivo que sirve para identificar el servicio. El nombre puede tener entre 2 y 50 caracteres que pueden ser letras, números y guiones. El nombre debe empezar y terminar con una letra o un número.
  2. Proporcione una **Ubicación** para el servicio. En general, elige una ubicación más cercana a la región geográfica donde quieras implementar el dispositivo. También puedes querer tener en cuenta lo siguiente: 
	 
		- If you have existing workloads in Azure that you also intend to deploy with your StorSimple device, you should use that datacenter.
		- Your StorSimple Manager service and Azure storage can be in two separate locations. In such a case, you are required to create the StorSimple Manager and Azure storage account separately. To create an Azure storage account, go to the Azure Storage service in the Management Portal and follow the steps in [Create an Azure Storage account](storage-create-storage-account.md#create-a-storage-account). After you create this account, add it to the StorSimple Manager service by following the steps in [Configure a new storage account for the service](storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service).
		 
  3. Elija una **Suscripción** en la lista desplegable. La suscripción está vinculada a la cuenta de facturación. Este campo no está presente si tiene una sola suscripción.
  4. Seleccione **Crear una cuenta de almacenamiento nueva** para crear automáticamente una cuenta de almacenamiento con el servicio. Esta cuenta de almacenamiento tendrá un nombre especial, como "storsimplebwv8c6dcnf." Si necesitas tener tus datos en una ubicación diferente, desactiva esta casilla. 
  5. Haga clic en **Crear Administrador de StorSimple** para crear el servicio.

   ![crear un servicio](./media/storsimple-create-new-service/HCS_CreateAService-include.png)

  Se le dirigirá a la página de aterrizaje del **Servicio**. La creación del servicio tardará unos minutos. Después de que el servicio se cree correctamente, se le notificará de forma adecuada y el estado del servicio cambiará a **Activo**.
 
   ![creación de servicios](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)

<!---HONumber=August15_HO8-->