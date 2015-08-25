<properties 
	pageTitle="Creación de claves de contenido con .NET" 
	description="Aprenda a crear claves de contenido que proporcionen un acceso seguro a los recursos." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/15/2015" 
	ms.author="juliako"/>


#Creación de claves de contenido con .NET

Este artículo forma parte de la serie [Flujo de trabajo de vídeo bajo demanda de Servicios multimedia](media-services-video-on-demand-workflow.md) y [Flujo de trabajo de streaming en directo de Servicios multimedia](media-services-live-streaming-workflow.md).

Servicios multimedia permite crear nuevos recursos y entregar recursos cifrados. Una **ContentKey** proporciona acceso seguro a los **recursos**.

Al crear un nuevo recurso (por ejemplo, antes de [cargar archivos](media-services-dotnet-upload-files.md)), puede especificar las siguientes opciones de cifrado: **StorageEncrypted**, **CommonEncryptionProtected** o **EnvelopeEncryptionProtected**.

Al enviar recursos a los clientes, puede [configurar que los recursos se cifren de forma dinámica](media-services-dotnet-configure-asset-delivery-policy.md) con uno de los dos cifrados siguientes: **DynamicEnvelopeEncryption** o **DynamicCommonEncryption**.

Los recursos cifrados tienen que estar asociados con **ContentKey**. En este artículo se describe cómo crear una clave de contenido.

>[AZURE.NOTE]Al crear un nuevo recurso **StorageEncrypted** con el SDK de Servicios multimedia para .NET, la **ContentKey** se crea automáticamente y se vincula al recurso.

##ContentKeyType

Uno de los valores que debe configurar al crear una clave de contenido es el tipo de clave de contenido. Elija uno de los valores siguientes.

    /// <summary>
    /// Specifies the type of a content key.
    /// </summary>
    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for encrypting encoding configuration data that may contain sensitive preset information. 
        /// </summary>
        ConfigurationEncryption = 2,
    }

##<a id="envelope_contentkey"></a>Crear ContentKey de tipo de sobre

El siguiente fragmento de código crea una clave de contenido del tipo de cifrado de sobre. A continuación, asocia la clave con el recurso especificado.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

llamada

	IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Crear ContentKey de tipo común    

El fragmento de código siguiente crea una clave de contenido del tipo de cifrado común. A continuación, asocia la clave con el recurso especificado.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
llamada

	IContentKey key = CreateCommonTypeContentKey(encryptedsset); 

<!---HONumber=August15_HO6-->