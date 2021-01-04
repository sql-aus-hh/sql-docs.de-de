---
title: GetSchema und Schemasammlungen
description: In diesem Artikel wird die Verwendung der GetSchema-Methode zum Abrufen und Einschränken von Schemainformationen einer Datenbank beschrieben.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051334"
---
# <a name="get-schema-and-schema-collections"></a>GetSchema und Schemasammlungen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Die **SqlConnection**-Klasse im SqlClient-Datenanbieter für SQL Server von Microsoft implementiert eine **GetSchema**-Methode, die zum Abrufen von Schemainformationen über die Datenbank verwendet wird, die derzeit verbunden ist. Die von der **GetSchema**-Methode zurückgegebenen Schemainformationen weisen das Format <xref:System.Data.DataTable> auf. Bei der **GetSchema**-Methode handelt es sich um eine überladene Methode, die optionale Parameter zum Angeben der zurückzugebenden Schemasammlung und zum Einschränken der zurückzugebenden Informationsmenge bereitstellt.

## <a name="specifying-the-schema-collections"></a>Festlegen der Schemasammlungen

Beim ersten optionalen Parameter der **GetSchema**-Methode handelt es sich um den Namen der Sammlung, der als Zeichenfolge angegeben wird. Es gibt zwei Arten von Schemaauflistungen: allgemeine Schemaauflistungen, die von allen Anbietern verwendet werden, und besondere Schemaauflistungen, die für jeden Anbieter spezifisch sind.  

Sie können den SqlClient-Datenanbieter für SQL Server von Microsoft abfragen, um die Liste der unterstützten Schemasammlungen zu ermitteln. Rufen Sie hierzu die **GetSchema**-Methode ohne Argumente oder mit dem Schemasammlungsnamen „MetaDataCollections“ auf. Dadurch wird <xref:System.Data.DataTable> mit einer Liste der unterstützten Schemaauflistungen, der Anzahl der von diesen Schemaauflistungen unterstützten Einschränkungen und der Anzahl der von diesen Schemaauflistungen verwendeten Bezeichnerteilen zurückgegeben.  

### <a name="retrieving-schema-collections-example"></a>Beispiel zum Abrufen von Schemasammlungen

In den folgenden Beispielen wird veranschaulicht, wie Sie die <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>-Methode der <xref:Microsoft.Data.SqlClient.SqlConnection>-Klasse des SqlClient-Datenanbieters für SQL Server von Microsoft verwenden, um Schemainformationen zu allen in der Beispieldatenbank **AdventureWorks** enthaltenen Tabellen abzurufen:  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen von Datenbankschemainformationen](retrieving-database-schema-information.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
