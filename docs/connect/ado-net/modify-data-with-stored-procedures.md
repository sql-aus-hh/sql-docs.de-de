---
title: Ändern von Daten mit gespeicherten Prozeduren
description: Beschreibt, wie mit Eingabe- und Ausgabeparametern von gespeicherten Prozeduren eine Zeile in eine Datenbank eingefügt wird, wodurch ein neuer Identitätswert zurückgegeben wird.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f04884302bb1f13852097182d6ebc8e06570c66b
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744371"
---
# <a name="modify-data-with-stored-procedures"></a>Ändern von Daten mit gespeicherten Prozeduren

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Gespeicherte Prozeduren können Daten als Eingabeparameter akzeptieren und Daten als Ausgabeparameter, Resultsets oder Rückgabewerte zurückgeben. Das folgende Beispiel veranschaulicht, wie Microsoft SqlClient-Datenanbieter für SQL Server die Eingabeparameter, Ausgabeparameter und Rückgabewerte sendet und empfängt. Im Beispiel wird ein neuer Datensatz in eine Tabelle eingefügt, in der die Spalte für den Primärschlüssel eine Identitätsspalte ist.

> [!NOTE]
> Wenn Sie gespeicherte Prozeduren verwenden, um Daten mithilfe einer <xref:Microsoft.Data.SqlClient.SqlDataAdapter>-Klasse zu bearbeiten oder zu löschen, stellen Sie sicher, dass Sie in der Definition der gespeicherten Prozedur nicht **SET NOCOUNT ON** verwenden. Anderenfalls ist die zurückgegebene Anzahl der betroffenen Zeilen gleich Null (0), was der `DataAdapter` als Parallelitätskonflikt interpretiert. In diesem Fall wird eine <xref:System.Data.DBConcurrencyException> ausgelöst.

## <a name="example"></a>Beispiel

In diesem Beispiel wird die folgende gespeicherte Prozedur verwendet, um in die **Categories**-Tabelle der **Northwind**-Beispieldatenbank eine neue Kategorie einzufügen. Die gespeicherte Prozedur nimmt den Wert in der **CategoryName**-Spalte als Eingabeparameter, verwendet die **SCOPE_IDENTITY()** -Funktion, um den neuen Wert des Identitätsfelds **CategoryID** abzurufen, und gibt ihn als Ausgabeparameter zurück. Die RETURN-Anweisung gibt über die **\@\@ROWCOUNT**-Funktion die Anzahl der eingefügten Zeilen zurück.

```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  

Im folgenden Codebeispiel wird die oben beschriebene gespeicherte Prozedur `InsertCategory` als Quelle für den <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> des <xref:Microsoft.Data.SqlClient.SqlDataAdapter> verwendet. Der `@Identity`-Ausgabeparameter erscheint im <xref:System.Data.DataSet>, nachdem der Datensatz in die Datenbank eingefügt und die `Update`-Methode des <xref:Microsoft.Data.SqlClient.SqlDataAdapter> aufgerufen wurde. Der Code ruft auch den Rückgabewert ab.

[!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](~/../sqlclient/doc/samples/SqlDataAdapter_SPIdentityReturn.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen und Ändern von Daten in ADO.NET](retrieving-modifying-data.md)
- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Ausführen eines Befehls](execute-command.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
