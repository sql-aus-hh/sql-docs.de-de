---
title: Abrufen von Daten durch einen DataReader
description: In diesem Artikel wird anhand von Beispielcode gezeigt, wie Sie Daten in ADO.NET mit einem DataReader-Objekt abrufen. DataReader bietet einen ungepufferten Datenstrom.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: dfbe41fe29a3dadc69a2a428f1bf8b606a2a7050
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771335"
---
# <a name="retrieve-data-by-a-datareader"></a>Abrufen von Daten durch einen DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Wenn Sie Daten mit einem **DataReader**-Objekt abrufen möchten, müssen Sie zunächst eine Instanz des **Command**-Objekts und anschließend ein **DataReader**-Objekt erstellen, indem Sie **Command.ExecuteReader** aufrufen, um Zeilen aus einer Datenquelle abzurufen. Das **DataReader**-Objekt stellt einen ungepufferten Datenstrom bereit, mit dem prozeduraler Logik eine effektive sequenzielle Verarbeitung der Ergebnisse aus einer Datenquelle ermöglicht wird.

> [!NOTE]
> Das **DataReader**-Objekt ist gut für das Abrufen großer Datenmengen geeignet, da die Daten nicht im Arbeitsspeicher zwischengespeichert werden.

Im folgenden Beispiel wird die Verwendung eines **DataReader**-Objekts gezeigt, wobei `reader` für ein gültiges DataReader-Objekt und `command` für ein gültiges Command-Objekt steht.  

```csharp
reader = command.ExecuteReader();  
```

Verwenden Sie die **DataReader.Read**-Methode, um eine Zeile aus den Abfrageergebnissen abzurufen. Sie können auf jede Spalte der zurückgegebenen Zeile zugreifen, indem Sie dem **DataReader**-Objekt den Namen oder die Ordinalzahl der Spalte übergeben. Die beste Leistung erzielen Sie jedoch, wenn Sie die vom **DataReader**-Objekt bereitgestellten Methoden verwenden, mit denen Sie auf Spaltenwerte in ihren nativen Datentypen zugreifen können (z. B. **GetDateTime**, **GetDouble**, **GetGuid** oder **GetInt32**). Eine Liste mit typisierten Accessormethoden für anbieterspezifische **DataReader**-Objekte finden Sie unter <xref:Microsoft.Data.SqlClient.SqlDataReader>. Mit den typisierten Accessormethoden wird die Menge der beim Abrufen des Spaltenwerts erforderlichen Typkonvertierungen reduziert, wenn der zugrunde liegende Datentyp bekannt ist.  

Im folgenden Beispiel wird ein **DataReader**-Objekt durchlaufen, wobei von jeder Zeile zwei Spalten zurückgegeben werden.  

[!code-csharp[DataWorks SqlClient.HasRows#1](~/../sqlclient/doc/samples/SqlDataReader_HasRows.cs#1)]

## <a name="close-the-datareader"></a>Schließen von DataReader  

Wenn das `DataReader`-Objekt nicht mehr benötigt wird, sollten Sie immer die `Close()`-Methode aufrufen.

> [!NOTE]
> Wenn Ihr **Command**-Objekt Ausgabeparameter oder Rückgabewerte enthält, sind diese Werte erst nach dem Schließen des **DataReader**-Objekts verfügbar.  

> [!IMPORTANT]
> Solange ein **DataReader**-Objekt geöffnet ist, wird das **Connection**-Objekt ausschließlich von diesem **DataReader**-Objekt verwendet. Sie können erst dann Befehle für das **Connection**-Objekt ausführen, z. B. um ein anderes **DataReader**-Objekt zu erstellen, wenn das ursprüngliche **DataReader**-Objekt geschlossen wurde.  

> [!NOTE]
> Rufen Sie **Close** oder **Dispose** nicht für **Connection**- oder **DataReader**-Objekte oder andere verwaltete Objekte in der **Finalize**-Methode Ihrer Klasse auf. Geben Sie in einer Finalize-Methode nur nicht verwaltete Ressourcen frei, die der Klasse direkt gehören. Wenn Ihre Klasse keine nicht verwalteten Ressourcen aufweist, verwenden Sie in Ihrer Klassendefinition keine **Finalize**-Methode. Weitere Informationen finden Sie unter [Garbage Collection](/dotnet/standard/garbage-collection/index).
 
## <a name="retrieve-multiple-result-sets-using-nextresult"></a>Abrufen mehrerer Resultsets mit NextResult

Wenn das **DataReader**-Objekt mehrere Resultsets zurückgibt, rufen Sie die **NextResult**-Methode auf, um die Resultsets nacheinander zu durchlaufen. Im folgenden Beispiel werden die Ergebnisse von zwei SELECT-Anweisungen mit der <xref:Microsoft.Data.SqlClient.SqlDataReader>-Methode von <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A> verarbeitet.  

[!code-csharp[DataWorks SqlClient.NextResult#1](~/../sqlclient/doc/samples/SqlDataReader_NextResult.cs#1)]

## <a name="get-schema-information-from-the-datareader"></a>Abrufen von Schemainformationen aus dem DataReader-Objekt  

Solange ein **DataReader**-Objekt geöffnet ist, können Sie mit der **GetSchemaTable**-Methode Schemainformationen zum aktuellen Resultset abrufen. **GetSchemaTable** gibt ein <xref:System.Data.DataTable>-Objekt zurück, das mit Zeilen und Spalten gefüllt ist, die die Schemainformationen für das aktuelle Resultset enthalten. Das **DataTable**-Objekt enthält eine Zeile für jede Spalte des Resultsets. Jede Spalte der Schematabelle ist einer Eigenschaft der zurückgegebenen Spalten in den Zeilen des Resultsets zugeordnet, wobei **ColumnName** der Name der Eigenschaft und der Spaltenwert der Wert der Eigenschaft ist. Im folgenden Beispiel werden die Schemainformationen für **DataReader** abgerufen.  

[!code-csharp[DataWorks SqlClient.GetSchemaTable#1](~/../sqlclient/doc/samples/SqlDataReader_GetSchemaTable.cs#1)]

## <a name="see-also"></a>Weitere Informationen

- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Befehle und Parameter](commands-parameters.md)
- [Abrufen von Datenbankschemainformationen](retrieving-database-schema-information.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
