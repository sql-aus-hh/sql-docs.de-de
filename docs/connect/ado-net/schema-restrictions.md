---
title: Schemaeinschränkungen
description: In diesem Artikel werden Schemaeinschränkungen beschrieben, die bei Verwendung des Microsoft-SqlClient-Datenanbieters für SQL Server mit der GetSchema-Methode verwendet werden können.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8d72e2dd020f1345ceb0b68249cb3d3acd1024dc
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051327"
---
# <a name="schema-restrictions"></a>Schemaeinschränkungen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Bei dem zweiten optionalen Parameter der **GetSchema**-Methode handelt es sich um Einschränkungen, die zum Einschränken der Menge der zurückgegebenen Schemainformationen verwendet werden. Dieser Parameter wird als Zeichenfolgenarray an die **GetSchema**-Methode übergeben. Die Position im Array bestimmt die Werte, die zurückgegeben werden können. Dies entspricht der Anzahl der Einschränkungen.  
  
In der folgenden Tabelle werden beispielsweise die Einschränkungen beschrieben, die von der Schemasammlung „Tables“ mit dem Microsoft-SqlClient-Datenanbieter für SQL Server unterstützt werden. Zusätzliche Einschränkungen für SQL Server-Schemaauflistungen werden am Ende dieses Themas aufgeführt.  

|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|TABLE_CATALOG|1|  
|Besitzer|@Owner|TABLE_SCHEMA|2|  
|Tabelle|@Name|table_name|3|  
|TableType|@TableType|TABLE_TYPE|4|  
 
## <a name="specifying-restriction-values"></a>Festlegen von Einschränkungswerten  

Wenn Sie eine der Einschränkungen der Tables-Schemaauflistung verwenden möchten, erstellen Sie ein Zeichenfolgenarray mit vier Elementen und fügen einen Wert in das Element ein, das mit der Einschränkungsnummer übereinstimmt. Legen Sie das zweite Element des Arrays auf „Sales“ fest, bevor Sie es an die **GetSchema**-Methode übergeben, um beispielsweise die Tabellen einzuschränken, die von der **GetSchema**-Methode an die Tabellen im Schema „Sales“ zurückgegeben werden.  
  
> [!NOTE]
> - Die Einschränkungssammlungen für `SqlClient` weisen eine zusätzliche `ParameterName`-Spalte auf. Die Spalte für den Einschränkungsstandard ist zwar aus Gründen der Abwärtskompatibilität vorhanden, wird aber derzeit ignoriert. Verwenden Sie Abfragen mit Parametern statt Zeichenfolgenersetzungen, um das Risiko eines SQL-Injection-Angriffs beim Angeben der Einschränkungswerte zu minimieren.  
> - Die Anzahl der Elementen im Array muss kleiner oder gleich der Anzahl der Einschränkungen sein, die für die angegebene Schemaauflistung unterstützt werden, da sonst eine <xref:System.ArgumentException> ausgelöst wird. Es können weniger Elemente als die maximale Anzahl der Einschränkungen vorhanden sein. Es wird davon ausgegangen, dass die fehlenden Einschränkungen NULL (uneingeschränkt) sind.  
  
Sie können den Microsoft-SqlClient-Datenanbieter für SQL Server abfragen, um die Liste der unterstützten Einschränkungen zu ermitteln, indem Sie die **GetSchema**-Methode mit dem Namen der Schemasammlung für Einschränkungen aufrufen. Diese Name lautet „Restrictions“. Dabei wird eine <xref:System.Data.DataTable> mit einer Liste der Auflistungsnamen, Einschränkungsnamen, Standardeinschränkungswerte und der Anzahl der Einschränkungen zurückgegeben.  
  
### <a name="example"></a>Beispiel  

In den folgenden Beispielen wird veranschaulicht, wie die <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>-Methode der <xref:Microsoft.Data.SqlClient.SqlConnection>-Klasse des SqlClient-Datenanbieters für SQL Server verwendet wird, um Schemainformationen über alle in der Beispieldatenbank **AdventureWorks** enthaltenen Tabellen abzurufen und die zurückgegebenen Informationen auf die Tabellen im Schema „Sales“ einzuschränken:  

[!code-csharp[SqlClient GetSchema with restrictions#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Restriction.cs#1)]  

## <a name="sql-server-schema-restrictions"></a>SQL Server-Schemaeinschränkungen  

 In den folgenden Tabellen sind die Beschränkungen für SQL Server-Schemaauflistungen aufgeführt.  
  
### <a name="users"></a>Benutzer  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>Datenbanken  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Name|@Name|Name|1|  
  
### <a name="tables"></a>Tabellen  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|TABLE_CATALOG|1|  
|Besitzer|@Owner|TABLE_SCHEMA|2|  
|Tabelle|@Name|table_name|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>Spalten  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|TABLE_CATALOG|1|  
|Besitzer|@Owner|TABLE_SCHEMA|2|  
|Tabelle|@Table|table_name|3|  
|Spalte|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|TABLE_CATALOG|1|  
|Besitzer|@Owner|TABLE_SCHEMA|2|  
|Tabelle|@Table|table_name|3|  
|Spalte|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>Sichten  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|TABLE_CATALOG|1|  
|Besitzer|@Owner|TABLE_SCHEMA|2|  
|Tabelle|@Table|table_name|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|VIEW_CATALOG|1|  
|Besitzer|@Owner|VIEW_SCHEMA|2|  
|Tabelle|@Table|VIEW_NAME|3|  
|Spalte|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|SPECIFIC_CATALOG|1|  
|Besitzer|@Owner|SPECIFIC_SCHEMA|2|  
|Name|@Name|SPECIFIC_NAME|3|  
|Parameter|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>Prozeduren  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|SPECIFIC_CATALOG|1|  
|Besitzer|@Owner|SPECIFIC_SCHEMA|2|  
|Name|@Name|SPECIFIC_NAME|3|  
|type|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|db_name()|1|  
|Besitzer|@Owner|user_name()|2|  
|Tabelle|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|Spalte|@Column|c.name|5|  
  
### <a name="indexes"></a>Indizes  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|db_name()|1|  
|Besitzer|@Owner|user_name()|2|  
|Tabelle|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|Einschränkungsname|Parametername|Einschränkungsstandard|Einschränkungsnummer|  
|----------------------|--------------------|-------------------------|------------------------|  
|Katalog|@Catalog|CONSTRAINT_CATALOG|1|  
|Besitzer|@Owner|CONSTRAINT_SCHEMA|2|  
|Tabelle|@Table|table_name|3|  
|Name|@Name|CONSTRAINT_NAME|4|  
  
## <a name="see-also"></a>Weitere Informationen:

- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
