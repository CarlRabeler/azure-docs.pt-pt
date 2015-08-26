<properties
   pageTitle="Autenticación de una entidad de servicio con el Administrador de recursos de Azure"
   description="Describe cómo conceder acceso a una entidad de servicio a través del control de acceso basado en rol y autenticarla. Muestra cómo realizar estas tareas con PowerShell y la CLI de Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="07/24/2015"
   ms.author="tomfitz"/>

# Autenticación de una entidad de servicio con el Administrador de recursos de Azure

En este tema se muestra cómo permitir que una entidad de servicio (por ejemplo, un proceso, una aplicación o un servicio automatizado) tenga acceso a otros recursos de la suscripción. Con el Administrador de recursos de Azure, puede usar el control de acceso basado en rol para conceder las acciones permitidas a una entidad de servicio y autenticar a esa entidad de servicio. En este tema se muestra cómo usar PowerShell y la CLI de Azure para asignar un rol a una entidad de servicio y autenticar la entidad de servicio.


## Conceptos
1. Azure Active Directory (AAD): un servicio de administración de identidades y acceso para la nube. Para obtener más información, consulte [¿Qué es Azure Active Directory?](active-directory/active-directory-whatis.md)
2. Entidad de servicio: una instancia de una aplicación en un directorio que necesita acceder a otros recursos.
3. Aplicación de AD: un registro de directorio que identifica una aplicación en AAD. Para obtener más información, consulte [Conceptos básicos sobre autenticación en Azure AD](https://msdn.microsoft.com/library/azure/874839d9-6de6-43aa-9a5c-613b0c93247e#BKMK_Auth).

## Concesión de acceso a una entidad de servicio y su autenticación con PowerShell

Si no tiene instalado Azure PowerShell, consulte [Instalación y configuración de Azure PowerShell](./powershell-install-configure.md).

Comenzará creando una entidad de servicio. Para ello, debemos crear una aplicación en el directorio. Esta sección le guiará a través de la creación de una nueva aplicación en el directorio.

1. Cree una nueva aplicación de AAD ejecutando el comando **New-AzureADApplication**. Proporcione un nombre para mostrar para la aplicación, el URI para una página que describe la aplicación (no se comprueba el vínculo), los URI que identifican la aplicación y la contraseña para la identidad de aplicación.

        PS C:\> $azureAdApplication = New-AzureADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password "<Your_Password>"

     Se devuelve a la aplicación de Azure AD. La propiedad **ApplicationId** es necesaria para la creación de entidades de servicio, las asignaciones de roles y la adquisición de tokens JWT. Guarde la salida o la captura en una variable.

        Type                    : Application
        ApplicationId           : a41acfda-d588-47c9-8166-d659a335a865
        ApplicationObjectId     : a26aaa48-bd52-4a7f-b29f-1bebf74c91e3
        AvailableToOtherTenants : False
        AppPermissions          : {{
                            "claimValue": "user_impersonation",
                            "description": "Allow the application to access My
                              Application on behalf of the signed-in user.",
                            "directAccessGrantTypes": [],
                            "displayName": "Access <<Your Application Display Name>>",
                            "impersonationAccessGrantTypes": [
                              {
                                "impersonated": "User",
                                "impersonator": "Application"
                              }
                            ],
                            "isDisabled": false,
                            "origin": "Application",
                            "permissionId":
                            "b866ef28-9abb-4698-8c8f-eb4328533831",
                            "resourceScopeType": "Personal",
                            "userConsentDescription": "Allow the application
                             to access <<Your Application Display Name>> on your behalf.",
                            "userConsentDisplayName": "Access <<Your Application Display Name>>",
                            "lang": null
                          }}


2. Cree a una entidad de servicio para la aplicación.

        PS C:\> New-AzureADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

     Ahora ha creado una entidad de servicio en el directorio, pero el servicio no tiene asignado ningún permiso o ámbito. Debe conceder explícitamente permisos a la entidad de servicio a fin de realizar operaciones en cierto ámbito.

3. Conceda los permisos de la entidad de servicio en su suscripción. En este ejemplo se concederá a la entidad de servicio el permiso de lectura para todos los recursos de la suscripción. Para el parámetro **ServicePrincipalName**, proporcione el valor de **ApplicationId** o **IdentifierUris** que utilizó al crear la aplicación. Para obtener más información sobre el control de acceso basado en rol, consulte [Administración y auditoría de acceso a recursos](azure-portal/resource-group-rbac.md).

        PS C:\> New-AzureRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $azureAdApplication.ApplicationId

4. Recupere la suscripción en la que se creó la asignación de roles. Esta suscripción se utilizará más adelante para obtener el valor de **TenantId** del inquilino en el que reside la asignación de rol de la entidad de servicio.

        PS C:\> $subscription = Get-AzureSubscription | where { $_.IsCurrent }

     Si ha creado la asignación de rol en una suscripción que no sea la suscripción seleccionada actualmente, puede especificar los parámetros **SubscriptoinId** o **SubscriptionName** para recuperar una suscripción diferente.

5. Cree un nuevo objeto **PSCredential** que contiene las credenciales mediante la ejecución del comando **Get-Credential**.

        PS C:\> $creds = Get-Credential

     Se le pedirá que escriba sus credenciales.

     ![][1]

     Para el nombre de usuario, utilice el valor de **ApplicationId** o **IdentifierUris** que utilizó al crear la aplicación. Para la contraseña, use la que especificó al crear la cuenta.

6. Utilice las credenciales que especificó para el cmdlet **Add-AzureAccount**, que firmará la entidad de servicio en:

        PS C:\> Add-AzureAccount -Credential $creds -ServicePrincipal -Tenant $subscription.TenantId

     Ahora debe autenticarse como la entidad de servicio para la aplicación de AAD que ha creado.


## Concesión de acceso a una entidad de servicio y su autenticación con la CLI de Azure

Si no tiene la CLI de Azure para Mac, Linux y Windows instalada, consulte [Instalación y configuración de la interfaz de la línea de comandos de Azure](xplat-cli-install.md).

1. Cree una nueva aplicación de AAD ejecutando el comando **azure ad app create**. Proporcione un nombre para mostrar para la aplicación, el URI para una página que describe la aplicación (no se comprueba el vínculo), los URI que identifican la aplicación y la contraseña para la identidad de aplicación.

        azure ad app create --name "<Your Application Display Name>" --home-page "<https://YourApplicationHomePage>" --identifier-uris "<https://YouApplicationUri>" --password <Your_Password>
        
    Se devuelve a la aplicación de Azure AD. La propiedad ApplicationId es necesaria para la creación de entidades de servicio, las asignaciones de roles y la adquisición de tokens JWT.

        info:    Executing command ad app create
        + Creating application exampleapp                                                
        data:    Application Id:          b57dd71d-036c-4840-865e-23b71d8098ec
        data:    Application Object Id:   d5c519e2-6149-447e-b323-88d2c4ea27de
        data:    Application Permissions:  
        data:                             claimValue:  user_impersonation
        data:                             description:  Allow the application to access exampleapp on behalf of the signed-in user.
        ...
        info:    ad app create command OK

2. Cree a una entidad de servicio para la aplicación. Proporcione el identificador de la aplicación que se devolvió en el paso anterior.

        azure ad sp create b57dd71d-036c-4840-865e-23b71d8098ec
        
    Se devuelve la nueva entidad de servicio. Cuando se conceden permisos, es necesario el identificador de objeto.
    
        info:    Executing command ad sp create
        + Creating service principal for application b57dd71d-036c-4840-865e-23b71d8098ec
        data:    Object Id:               47193a0a-63e4-46bd-9bee-6a9f6f9c03cb
        data:    Display Name:            exampleapp
        ...
        info:    ad sp create command OK

    Ahora ha creado una entidad de servicio en el directorio, pero el servicio no tiene asignado ningún permiso o ámbito. Debe conceder explícitamente permisos a la entidad de servicio a fin de realizar operaciones en cierto ámbito.

3. Conceda los permisos de la entidad de servicio en su suscripción. En este ejemplo se concederá a la entidad de servicio el permiso de lectura para todos los recursos de la suscripción. Para el parámetro **ServicePrincipalName**, proporcione el valor de **ApplicationId** o **IdentifierUris** que utilizó al crear la aplicación. Para obtener más información sobre el control de acceso basado en rol, consulte [Administración y auditoría de acceso a recursos](azure-portal/resource-group-rbac.md).

        azure role assignment create --objectId 47193a0a-63e4-46bd-9bee-6a9f6f9c03cb -o Reader -c /subscriptions/{subscriptionId}/

4. Determine el valor **TenantId** del inquilino en el que reside la asignación del rol de la entidad de servicio mostrando las cuentas y buscando la propiedad **TenantId** en la salida.

        azure account list

5. Inicie sesión utilizando la entidad de servicio como su identidad. Para el nombre de usuario, utilice el valor de **ApplicationId** que utilizó al crear la aplicación. Para la contraseña, use la que especificó al crear la cuenta.

        azure login -u "<ApplicationId>" -p "<password>" --service-principal --tenant "<TenantId>"

    Ahora debe autenticarse como la entidad de servicio para la aplicación de AAD que ha creado.

## Pasos siguientes
  
- Para obtener más información sobre el control de acceso basado en rol, consulte [Administración y auditoría de acceso a recursos](azure-portal/resource-group-rbac.md)  
- Para obtener información sobre cómo usar el portal con entidades de servicio, consulte [Creación de una nueva entidad de servicio de Azure mediante el Portal de Azure](./resource-group-create-service-principal-portal.md)  
- Para obtener instrucciones sobre cómo implementar la seguridad con el Administrador de recursos de Azure, consulte [Consideraciones de seguridad para el Administrador de recursos de Azure](best-practices-resource-manager-security.md).


<!-- Images. -->
[1]: ./media/resource-group-authenticate-service-principal/arm-get-credential.png

<!---HONumber=August15_HO6-->