---
title: Aktualisieren von Daten in einer Datenquelle
description: Beschreibt das Ausführen von Befehlen oder gespeicherten Prozeduren, mit denen Daten in einer Datenbank geändert werden.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 55c545e5-dcd5-4323-a5b9-3825c2157462
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: aca7b1adb8ce91a12832bccfd6cbd27b07229d22
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771482"
---
# <a name="updating-data-in-a-data-source"></a>Aktualisieren von Daten in einer Datenquelle

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

SQL-Anweisungen, mit denen Daten geändert werden (z. B. INSERT, UPDATE oder DELETE), geben keine Zeilen zurück. Ähnlich verhält es sich mit vielen gespeicherten Prozeduren, die zwar eine Aktion durchführen, jedoch keine Zeilen zurückgeben. Zum Ausführen von Befehlen, die keine Zeilen zurückgeben, erstellen Sie ein **Command**-Objekt mit dem entsprechenden SQL-Befehl und ein **Connection**-Objekt, einschließlich der erforderlichen **Parameter**. Führen Sie den Befehl mit der <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A>-Methode des <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekts aus.

> [!NOTE]
> Die **ExecuteNonQuery**-Methode gibt eine ganze Zahl zurück, die der Anzahl von Zeilen entspricht, auf die die ausgeführte Anweisung oder gespeicherte Prozedur angewendet wurde. Wenn mehrere Anweisungen ausgeführt werden, entspricht der zurückgegebene Wert der Summe der Datensätze, die von allen ausgeführten Anweisungen betroffen sind.

## <a name="example"></a>Beispiel

Im folgenden Codebeispiel wird eine INSERT-Anweisung ausgeführt, um einen Datensatz mithilfe von **ExecuteNonQuery** in einer Datenbank einzufügen.
  
[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#1)]

Im folgenden Codebeispiel wird die gespeicherte Prozedur ausgeführt, die mit dem Beispielcode unter [Ausführen von Katalogoperationen](perform-catalog-operations.md) erstellt wurde. Da von der gespeicherten Prozedur keine Zeilen zurückgegeben werden, wird die **ExecuteNonQuery**-Methode verwendet. Die gespeicherte Prozedur empfängt jedoch einen Eingabeparameter und gibt einen Ausgabeparameter und einen Rückgabewert zurück.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#2](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#2)]

## <a name="see-also"></a>Weitere Informationen:

- [Verwenden von Befehlen zum Ändern von Daten](use-commands-to-modify-data.md)
- [Aktualisieren von Datenquellen mit DataAdapters](update-data-sources-with-dataadapters.md)
- [Befehle und Parameter](commands-parameters.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
