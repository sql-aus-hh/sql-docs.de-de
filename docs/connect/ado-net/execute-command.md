---
title: Ausführen eines Befehls
description: Beschreibt das `Command`-Objekt des Microsoft SqlClient-Datenanbieters für SQL Server sowie die Verwendung dieses Objekts zum Ausführen von Abfragen und Befehlen für eine Datenquelle.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 40494916-c25a-4cb8-8f7c-fcb8d378464e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 772ecef470b69a34e4bd4b0eba2ca60255657eaf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771422"
---
# <a name="executing-a-command"></a>Ausführen eines Befehls

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Der Microsoft SqlClient-Datenanbieter für SQL Server verfügt über ein <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekt, das Elemente von <xref:System.Data.Common.DbCommand> erbt. Dieses Objekt stellt Methoden zum Ausführen von Befehlen basierend auf dem Befehlstyp und dem gewünschten Rückgabewert zur Verfügung, wie in der folgenden Tabelle beschrieben.

|Get-Help|Rückgabewert|  
|-------------|------------------|  
|`ExecuteReader`|Gibt ein `DataReader`-Objekt zurück.|  
|`ExecuteScalar`|Gibt einen einzelnen Skalarwert zurück.|  
|`ExecuteNonQuery`|Führt einen Befehl aus, der keine Zeilen zurückgibt.|  
|`ExecuteXMLReader`|Gibt einen <xref:System.Xml.XmlReader> zurück. Nur für ein `SqlCommand`-Objekt verfügbar.|

Jedes stark typisierte Befehlsobjekt unterstützt auch eine <xref:System.Data.CommandType>-Enumeration, die angibt, wie eine Befehlszeichenfolge interpretiert wird. Nähere Informationen dazu finden Sie in der folgenden Tabelle.

|CommandType|Beschreibung|
|-----------------|-----------------|  
|`Text`|SQL-Befehl, der die an der Datenquelle auszuführenden Anweisungen definiert|  
|`StoredProcedure`|Name der gespeicherten Prozedur Mit der `Parameters`-Befehlseigenschaft können Sie auf die Eingabe- und Ausgabeparameter sowie auf die Rückgabewerte zugreifen, und dies unabhängig davon, welche `Execute`-Methode aufgerufen wird.|  
|`TableDirect`|Name der Tabelle|

> [!IMPORTANT]
> Wenn Sie die `ExecuteReader`-Methode aufrufen, stehen die Rückgabewerte und die Ausgabeparameter jedoch erst zur Verfügung, wenn das `DataReader`-Objekt geschlossen ist.

## <a name="example"></a>Beispiel

Das folgende Codebeispiel zeigt, wie durch Festlegen seiner Eigenschaften ein <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekt zum Ausführen einer gespeicherten Prozedur erstellt werden kann. Ein <xref:Microsoft.Data.SqlClient.SqlParameter>-Objekt wird zur Angabe des Eingabeparameters für die gespeicherte Prozedur verwendet. Der Befehl wird mit der <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A>-Methode ausgeführt, und die Ausgabe des <xref:Microsoft.Data.SqlClient.SqlDataReader> wird im Konsolenfenster angezeigt.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

### <a name="troubleshooting-commands"></a>Problembehandlung bei Befehlen

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Mit dem Microsoft SqlClient-Datenanbieter für SQL Server werden **Leistungsindikatoren** hinzugefügt, mit denen Sie zeitweilige Probleme aufgrund von Fehlern bei der Befehlsausführung erkennen können. Weitere Informationen finden Sie unter [Leistungsindikatoren in SqlClient](performance-counters.md).

## <a name="see-also"></a>Weitere Informationen:

- [Befehle und Parameter](commands-parameters.md)
- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
