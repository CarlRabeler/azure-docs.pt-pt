<properties
   pageTitle="SQL dinámico en Almacenamiento de datos SQL | Microsoft Azure"
   description="Sugerencias para usar SQL dinámico en Almacenamiento de datos SQL Azure para desarrollar soluciones."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/26/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# SQL dinámico en Almacenamiento de datos SQL
Al desarrollar código de aplicación para Almacenamiento de datos SQL, puede que se encuentre con la necesidad de usar SQL dinámico con el fin de proporcionar soluciones flexibles, genéricas y modulares. Sin embargo, Almacenamiento de datos SQL no admite por el momento tipos de datos blob. Esto puede limitar el tamaño de las cadenas ya que los tipos de blob incluyen tipos varchar (max) y nvarchar (max). Puede encontrarse con que ha usado estos tipos en el código de aplicación al crear cadenas muy grandes de código SQL dinámico que es necesario ejecutar.

En estas situaciones, deberá dividir el código en fragmentos y usar en su lugar la instrucción EXEC.

A continuación se muestra un ejemplo simplificado:

```
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Si la cadena no es especialmente larga, puede usar entonces [sp\_executesql][] como es habitual.


## Pasos siguientes
Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo][].

<!--Image references-->

<!--Article references-->
[información general sobre desarrollo]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp\_executesql]: https://msdn.microsoft.com/es-es/library/ms188001.aspx

<!--Other Web references-->

<!---HONumber=August15_HO6-->