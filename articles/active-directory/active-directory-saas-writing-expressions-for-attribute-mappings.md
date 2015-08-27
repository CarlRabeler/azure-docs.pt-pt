<properties
	pageTitle="Escritura de expresiones para la asignación de atributos en Azure Active Directory"
	description="Obtenga información sobre cómo usar asignaciones de expresiones para transformar valores de atributos en un formato aceptable durante el aprovisionamiento automático de objetos de aplicaciones SaaS en Azure Active Directory."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/27/2015"
	ms.author="markusvi"/>


# Escritura de expresiones para la asignación de atributos en Azure Active Directory

Al configurar el aprovisionamiento para una aplicación SaaS, uno de los tipos de asignaciones de atributos que puede especificar es una asignación de expresiones. En estos casos, debe escribir una expresión similar a un script que permite transformar los datos de los usuarios en formatos más aceptables para la aplicación SaaS.





## Información general sobre la sintaxis

La sintaxis de expresiones para asignaciones de atributos recuerda a las funciones de Visual Basic para Aplicaciones (VBA).

- Toda la expresión se debe definir en términos de funciones, que constan de un nombre seguido de argumentos entre paréntesis: <br> *NombreFunción(<<argument 1>>,<<argument N>>)*


- Es posible anidar funciones dentro de otras. Por ejemplo: <br> *FunciónUna(FunciónDos(<<argument1>>))*


- Puede transformar tres tipos diferentes de argumentos en funciones:

   1. Atributos, que deben ir entre corchetes. Por ejemplo: [NombreAtributo]

   2. Constantes de cadena, que deben ir entre comillas. Por ejemplo: "Estados Unidos"

   3. Otras funciones. Por ejemplo: FunciónUna(<<argument1>>, FunciónDos(<<argument2>>))


- Para las constantes de cadena, si necesita una barra diagonal inversa (\\) o comillas dobles (") en la cadena, se deben convertirse con el símbolo de barra diagonal inversa (\\). Por ejemplo: "Nombre de empresa: "Contoso""



## Lista de funciones

[Append](#append) &nbsp;&nbsp;&nbsp;&nbsp; [Coalesce](#coalesce) &nbsp;&nbsp;&nbsp;&nbsp; [FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [MatchRegex](#matchregex) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp;&nbsp;&nbsp; [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [ObsoleteReplace](#obsoletereplace) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [ReplaceRegex](#replaceregex) &nbsp;&nbsp;&nbsp;&nbsp; [StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)





----------
### Append

**Función:**<br> Append(source, suffix)

**Descripción:**<br> adopta un valor de la cadena de origen y anexa el sufijo al final de la misma.
 
**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena | Normalmente el nombre del atributo del objeto de origen |
| **suffix** | Obligatorio | Cadena | La cadena que se va a anexar al final del valor de origen. |


----------
### Coalesce

**Función:**<br> Coalesce(source1, source2, …)

**Descripción:**<br> devuelve el primer valor no vacío de la lista de parámetros de origen.
 
**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| ****source1 .. sourceN ** | Obligatorio, número variable de veces | Cadena | valores **source** de entre los que elegir |



----------
### FormatDateTime

**Función:**<br> FormatDateTime(source, inputFormat, outputFormat)

**Descripción:**<br> adopta una cadena de fecha en un formato y la convierte a un formato distinto.
 
**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena | Normalmente el nombre del atributo del objeto de origen. |
| **inputFormat** | Obligatorio | Cadena | Formato esperado del valor de origen. Para saber los formatos admitidos, vea [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** | Obligatorio | Cadena | Formato de la fecha de salida. |



----------
### Join

**Función:**<br> Join(separator, source1, source2, …)

**Descripción:**<br> Join() es similar a Append(), excepto en que puede combinar varios valores de cadena de **origen** en una sola cadena, y cada valor estará separado por una cadena de **separador**.

Si uno de los valores de origen es un atributo multivalor, cada valor de ese atributo se une, separado del valor del separador.

 
**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **separator** | Obligatorio | Cadena | Cadena utilizada para separar los valores de origen cuando se concatenan en una sola cadena. Puede ser "" si no es necesario ningún separador. |
| ****source1 … sourceN ** | Obligatorio, número variable de veces | Cadena | Valores de cadena que se van a agrupar. |





----------
### MatchRegex

**Función:**<br> MatchRegex(source, find, group)

**Descripción:**<br> devuelve la subcadena dentro del valor de origen que coincide con el patrón de expresión regular especificado en el parámetro find. Si se especifica el grupo, devuelve solo el valor de dicho grupo RegEx


**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena | Valor **source** en el que buscar. |
| **find** | Obligatorio | Cadena | Expresión regular que coincida en el valor **source**. |
| **group** | Opcional | String | Nombre del grupo dentro de la coincidencia de expresión regular cuyo valor se desea utilizar. |



----------
### Mid

**Función:**<br> Mid(source, start, length)

**Descripción:**<br> devuelve una subcadena del valor de origen. Una subcadena es una cadena que contiene sólo algunos de los caracteres de la cadena de origen.


**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | String | Normalmente el nombre del atributo. |
| **start** | Obligatorio | integer | Índice en la cadena **source** donde debe empezar la subcadena. El primer carácter de la cadena tendrá el índice de 1, el segundo carácter tendrá el índice de 2, y así sucesivamente. |
| **length** | Obligatorio | integer | Longitud de la subcadena. Si la longitud acaba fuera de la cadena de **source**, la función devolverá la subcadena desde el índice **start** hasta el final de la cadena **source**. |




----------
### Not

**Función:**<br> Not(source)

**Descripción:**<br> invierte el valor booleano del valor **source**. Si el valor **source** es "*True*", devuelve "*False*". De lo contrario, devuelve "*True*".


**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena booleana | Los valores **source** esperados son "True" o "False". |



----------
### ObsoleteReplace

**Función:**<br> ObsoleteReplace(source, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, template)

**Descripción:**<br>
> [AZURE.NOTE]Esta función quedará en desuso en un futuro próximo y se reemplazará por versiones más sencillas

Reemplaza los valores dentro de una cadena. Funciona de forma diferente dependiendo de los parámetros proporcionados:

- Cuando se proporcionan **oldValue** y **replacementValue**:

   - Reemplaza todas las ocurrencias de oldValue en el origen con replacementValue

- Cuando se proporcionan **oldValue** y **template**:

   - Reemplaza todas las repeticiones de **oldValue** en **template** con el valor **source**

- Cuando se proporcionan **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementValue**:

   - Reemplaza todos los valores que coinciden en oldValueRegexPattern en la cadena de origen con replacementValue

- Cuando se proporcionan **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementPropertyName**:

   - Si **source** tiene algún valor, se devuelve **source**

- Si **source** no tiene ningún valor, usa **oldValueRegexPattern** y **oldValueRegexGroupName** para extraer el valor de reemplazo de la propiedad con **replacementPropertyName**. El valor de reemplazo se devuelve como resultado


**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena | Normalmente el nombre del atributo del objeto de origen. |
| **oldValue** | Opcional | String | Valor que se debe reemplazar en **source** o **template**. |
| **regexPattern** | Opcional | Cadena | Patrón Regex del valor que se va a reemplazar en **source**. O bien, cuando se utiliza replacementPropertyName, patrón para extraer el valor de la propiedad de reemplazo. |
| **regexGroupName** | Opcional | Cadena | Nombre del grupo dentro de **regexPattern**. Sólo cuando se utiliza replacementPropertyName, se extrae el valor de este grupo como replacementValue de la propiedad de reemplazo. |
| **replacementValue** | Opcional | Cadena | Nuevo valor para reemplazar uno anterior. |
| **replacementAttributeName** | Opcional | String | Nombre del atributo que se utilizará para el valor de reemplazo, cuando el origen no tiene ningún valor. |
| **template** | Opcional | Cadena | Cuando se proporcione el valor **template**, buscaremos **oldValue** dentro de la plantilla y lo reemplazaremos por el valor de origen. |



----------
### Sustituya

**Función:**<br> Replace(source, find, replace)

**Descripción:**<br> reemplaza todas las repeticiones del valor **find** en la cadena **source** con el valor del parámetro **replace**.

**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena | Valor **source** en el que buscar. |
| **find** | Obligatorio | String | Valor que hay que buscar. |
| **replace** | Obligatorio | String | Valor por el que reemplazar. |



----------
### ReplaceRegex

**Función:**<br> ReplaceRegex(source, find, replace, group)

**Descripción:**<br> dentro de la cadena **source**, reemplaza todas las subcadenas que coinciden con la expresión regular **find** por el valor **replace**. Si se especifica un parámetro **grupo**, solo reemplaza el valor de dicho grupo RegEx.

**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | Cadena | Valor **source** en el que buscar. |
| **find** | Obligatorio | Cadena | Expresión regular que coincida en el valor **source**. |
| **replace** | Obligatorio | String | Valor por el que reemplazar. |
| **group** | Opcional | String | Nombre del grupo dentro de la coincidencia de expresión regular cuyo valor se desea utilizar. |




----------
### StripSpaces

**Función:**<br> StripSpaces(source)

**Descripción:**<br> quita todos los caracteres de espacio (" ") de la cadena de origen.

**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | String | Valor **source** que se va a actualizar. |



----------
### Switch

**Función:**<br> Switch(source, defaultValue, key1, value1, key2, value2, …)

**Descripción:**<br> si el valor **source** coincide con un parámetro **key**, devuelve el parámetro **value** de dicho **key**. Si el valor de **source** no coincide con ninguna clave, devuelve **defaultValue**. Los parámetros **key** y **value** siempre deben estar emparejados. La función espera siempre un número par de parámetros.

**Parámetros:**<br>

|Nombre| Obligatorio/Repetición | Tipo | Notas |
|--- | ---                 | ---  | ---   |
| **de origen** | Obligatorio | String | Valor **source** que se va a actualizar. |
| **defaultValue** | Opcional | Cadena | Valor predeterminado que se usará si el origen no coincide con ninguna clave. Puede tratarse de una cadena vacía (""). |
| **key** | Obligatorio | Cadena | **Clave** con la que se compara el valor **source**. |
| **value** | Obligatorio | Cadena | Valor de reemplazo para el valor **source** que coincide con la clave. |



## Ejemplos

### Seccionar un nombre de dominio conocido

Debe seccionar un nombre de dominio conocido de correo electrónico de un usuario para obtener un nombre de usuario. <br> Por ejemplo, si el dominio es "contoso.com", puede usar la expresión siguiente:


**Expresión:** <br> `Replace([mail], "@contoso.com", "")`

**Entrada/salida de muestra:** <br>

- **ENTRADA** (mail): "john.doe@contoso.com"

- **SALIDA**: "john.doe"



### Anexar sufijos constantes a nombres de usuario

Si está utilizando un espacio aislado de Salesforce, deberá anexar un sufijo adicional a los nombres de usuario antes de sincronizarlas.




**Expresión:** <br> `Append([userPrincipalName], ".test"))`

**Entrada/salida de muestra:** <br>

- **ENTRADA**: (userPrincipalName): "John.Doe@contoso.com"


- **SALIDA**: "John.Doe@contoso.com.test"





### Generar el alias de usuario concatenando partes de nombre y apellidos

Debe generar un alias de usuario con las tres primeras letras del nombre del usuario y las cinco primeras letras del apellido del usuario.


**Expresión:** <br> `Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Entrada/salida de muestra:** <br>

- **ENTRADA** (givenName): "John"

- **ENTRADA** (surname): "Doe"

- **SALIDA**: "JohDoe"




### Fecha de resultado como una cadena en un formato determinado

Desea enviar las fechas a una aplicación SaaS con un formato determinado. <br> Por ejemplo, desea formatear fechas para ServiceNow.



**Expresión:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Entrada/salida de ejemplo:**

- **ENTRADA** (extensionAttribute1): "20150123105347.1Z"

- **SALIDA**: "2015-01-23"





### Reemplazar un valor basado en un conjunto predefinido de opciones

Debe definir la zona horaria del usuario según el código de estado almacenado en Azure AD. <br> Si el código de estado no coincide con ninguna de las opciones predefinidas, use el valor predeterminado de "Australia/Sídney".


**Expresión:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Entrada/salida de ejemplo:**

- **ENTRADA** (state): "QLD"

- **SALIDA**: "Australia/Brisbane"


[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!---HONumber=August15_HO6-->