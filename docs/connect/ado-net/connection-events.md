---
title: Verbindungsereignisse
description: Die Verbindungsereignisse zum Abrufen von Informationsmeldungen aus einer Datenquelle und zur Bestimmung, ob ihr Zustand geändert wurde.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 5a29de74-acfc-4134-8616-829dd7ce0710
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8151915dc6c16c6225fec9ab90cb5a88e86b992f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771442"
---
# <a name="connection-events"></a>Verbindungsereignisse

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Der Microsoft SqlClient-Datenanbieter für SQL Server verfügt über **Connection**-Objekte mit zwei Ereignissen, mit denen Sie Informationsmeldungen aus einer Datenquelle abrufen oder eine Zustandsänderung eines **Connection**-Objekts feststellen können. In der folgenden Tabelle werden die Ereignisse des **Connection**-Objekts beschrieben.

|Ereignis|BESCHREIBUNG|  
|-----------|-----------------|  
|**InfoMessage**|Dieses Ereignis tritt auf, wenn eine Informationsmeldung aus einer Datenquelle zurückgegeben wird. Bei Informationsmeldungen handelt es sich um Meldungen aus einer Datenquelle, die keine Ausnahme auslösen.|  
|**StateChange**|Tritt auf, wenn sich der Zustand des **Connection**-Objekts ändert.|  

## <a name="work-with-the-infomessage-event"></a>Arbeiten mit dem InfoMessage-Ereignis

Mit dem <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis des <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekts können Warnungen und Informationsmeldungen aus einer SQL Server-Datenquelle abgerufen werden. Wenn von einer Datenquelle Fehler mit einem Schweregrad zwischen 11 und 16 zurückgegeben werden, wird eine Ausnahme ausgelöst. Mit dem <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis können jedoch Meldungen aus der Datenquelle abgerufen werden, die keinem Fehler zugewiesen sind. Bei Microsoft SQL Server werden alle Meldungen mit einem Schweregrad von 10 oder weniger als Informationsmeldungen betrachtet und mit dem <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis aufgezeichnet. Weitere Informationen finden Sie im Artikel [Schweregrade von Datenbank-Engine-Fehlern](/sql/relational-databases/errors-events/database-engine-error-severities).

Das <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis empfängt ein <xref:Microsoft.Data.SqlClient.SqlInfoMessageEventArgs>-Objekt, das in der **Errors**-Eigenschaft eine Sammlung der Meldungen aus einer Datenquelle enthält. Die **Error**-Objekte in dieser Sammlung können auf Fehlernummer und Meldungstext sowie Quelle des Fehlers abgefragt werden. Der Microsoft SqlClient-Datenanbieter für SQL Server enthält auch Details zur Datenbank, gespeicherten Prozedur und Nummer der Zeile, aus der die Meldung stammt.

### <a name="example"></a>Beispiel

Im folgenden Codebeispiel wird veranschaulicht, wie für das <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis ein Ereignishandler hinzugefügt wird.

[!code-csharp[SqlConnection_._InfoMessage#1](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#1)]

## <a name="handle-errors-as-infomessages"></a>Behandeln von Fehlern als InfoMessages

Das <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis wird i. d. R. nur für Informations- und Warnmeldungen ausgelöst, die vom Server gesendet werden. Bei einem tatsächlichen Fehler wird die Ausführung der **ExecuteNonQuery**-Methode oder **ExecuteReader**-Methode, die den Servervorgang eingeleitet hat, angehalten und eine Ausnahme ausgelöst.

Wenn Sie die Verarbeitung der restlichen Anweisungen in einem Befehl unabhängig von den vom Server erzeugten Fehlern fortsetzen möchten, legen sie die <xref:Microsoft.Data.SqlClient.SqlConnection.FireInfoMessageEventOnUserErrors%2A>-Eigenschaft der <xref:Microsoft.Data.SqlClient.SqlConnection> auf `true` fest. Bei dieser Vorgehensweise wird bei Fehlern von der Verbindung das <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis ausgelöst, anstatt eine Ausnahme auszulösen und die Verarbeitung zu unterbrechen. Die Clientanwendung kann dann dieses Ereignis behandeln und auf Fehlerbedingungen reagieren.

> [!NOTE]
> Wenn aufgrund eines Fehlers mit einem Schweregrad von 17 oder höher der Server die Verarbeitung des Befehls abbricht, muss dieser Fehler als Ausnahme behandelt werden. In diesem Fall wird unabhängig davon, wie der Fehler im <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>-Ereignis behandelt wird, eine Ausnahme ausgelöst.

## <a name="work-with-the-statechange-event"></a>Arbeiten mit dem StateChange-Ereignis

Das **StateChange**-Ereignis tritt auf, wenn sich der Zustand eines **Connection**-Objekts ändert. Das **StateChange**-Ereignis erhält <xref:System.Data.StateChangeEventArgs>, mit denen Sie anhand der **OriginalState**-Eigenschaft und **CurrentState**-Eigenschaft Zustandsänderungen des **Connection**-Objekts ermitteln können. Die **OriginalState**-Eigenschaft ist eine <xref:System.Data.ConnectionState>-Enumeration, die den Zustand des **Connection**-Objekts vor der Änderung angibt. **CurrentState** ist eine <xref:System.Data.ConnectionState>-Enumeration, die den Zustand des **Connection**-Objekts nach der Änderung angibt.

Im folgenden Codebeispiel wird mit dem **StateChange**-Ereignis eine Meldung in die Konsole geschrieben, sobald sich der Zustand des **Connection**-Objekts ändert.

[!code-csharp[SqlConnection_._StateChange#2](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#2)]

## <a name="see-also"></a>Weitere Informationen

- [Herstellen einer Verbindung mit Datenquellen](connecting-to-data-source.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
