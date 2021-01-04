---
title: Abrufen von Datenbankschemainformationen
description: In diesem Artikel erfahren Sie mehr über die Verwendung des SqlClient-Datenanbieters für SQL Server von Microsoft zum Abrufen von Informationen zum Datenbankschema.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051328"
---
# <a name="retrieving-database-schema-information"></a>Abrufen von Datenbankschemainformationen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Die Schemainformationen aus einer Datenbank werden mithilfe der Schemasuche abgerufen. Mithilfe der Schemaermittlung können Anwendungen anfordern, dass verwaltete Anbieter Informationen zum Datenbankschema einer jeweiligen Datenbank suchen und zurückgeben sollen. Diese Informationen werden auch als *Metadaten* bezeichnet. Verschiedene Schemaelemente von Datenbanken (z. B. Tabellen, Spalten und gespeicherte Prozeduren) werden über Schemaauflistungen verfügbar gemacht. Jede Schemaauflistung enthält eine Vielzahl von Schemainformationen, die für den verwendeten Anbieter spezifisch sind.

Der Microsoft-SqlClient-Datenanbieter für SQL Server implementiert die Methode **GetSchema** in der Klasse **SqlConnection**, und die von der Methode **GetSchema** zurückgegebenen Informationen weisen das Format <xref:System.Data.DataTable> auf. Bei der **GetSchema**-Methode handelt es sich um eine überladene Methode, die optionale Parameter zum Angeben der zurückzugebenden Schemasammlung und zum Einschränken der zurückzugebenden Informationsmenge bereitstellt. Der SqlClient-Datenanbieter stellt außerdem die Methode **GetSchemaTable** bereit, die eine DataTable-Klasse zurückgibt, die die Spaltenmetadaten von **SqlDataReader** beschreibt.

## <a name="in-this-section"></a>In diesem Abschnitt

[„GetSchema“ und Schemaauflistungen](getschema-and-schema-collections.md)  
In diesem Artikel wird die Methode **GetSchema** und ihre Verwendung zum Abrufen und Einschränken von Schemainformationen einer Datenbank beschrieben.

[Schemaeinschränkungen](schema-restrictions.md)  
In diesem Artikel werden Schemaeinschränkungen beschrieben, die mit der Methode **GetSchema** verwendet werden können. 

[Allgemeine Schemaauflistungen](common-schema-collections.md)  
In diesem Artikel werden alle allgemeinen Schemasammlungen beschrieben, die von allen von .NET verwalteten Anbietern unterstützt werden.  
  
[SQL Server-Schemaauflistungen](sql-server-schema-collections.md)  
In diesem Artikel werden die zusätzlichen vom Microsoft-SqlClient-Datenanbieter unterstützten Schemasammlungen für SQL Server beschrieben. 

## <a name="reference"></a>Verweis

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
In diesem Artikel wird die **GetSchema**-Methode der <xref:System.Data.Common.DbConnection>-Klasse beschrieben.

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
In diesem Artikel wird die **GetSchema**-Methode der <xref:Microsoft.Data.SqlClient.SqlConnection>-Klasse beschrieben.

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
In diesem Artikel wird die **GetSchemaTable**-Methode der <xref:System.Data.Common.DbDataReader>-Klasse beschrieben. 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
In diesem Artikel wird die **GetSchemaTable**-Methode der <xref:Microsoft.Data.SqlClient.SqlDataReader>-Klasse beschrieben.

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen und Ändern von Daten in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
