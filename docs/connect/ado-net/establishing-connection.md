---
title: Herstellen einer Verbindung
description: Richtlinie für das Herstellen einer Verbindung mit SQL Server über den Anbieter SqlClient.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: b3d92f49fb4eae6184a673ffc25ff6bbdf521626
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771592"
---
# <a name="establishing-connection"></a>Herstellen einer Verbindung

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Zum Herstellen einer Verbindung mit Microsoft SQL Server verwenden Sie das <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekt des Microsoft SqlClient-Datenanbieters für SQL Server. Informationen zum sicheren Speichern und Abrufen von Verbindungszeichenfolgen finden Sie unter [Schützen von Verbindungsinformationen](protecting-connection-information.md).

## <a name="closing-connections"></a>Schließen von Verbindungen

Es wird empfohlen, die Verbindung nach Verwendung stets zu schließen, damit sie in den Pool zurückgegeben werden kann. Der `Using`-Block in Visual Basic oder C# verwirft automatisch die Verbindung, wenn der Code den Block verlässt, auch im Falle einer unbehandelten Ausnahme. Weitere Informationen finden Sie unter [using-Anweisung](/dotnet/csharp/language-reference/keywords/using-statement) und [Using-Anweisung](/dotnet/visual-basic/language-reference/statements/using-statement).

Sie können auch die Methoden `Close` oder `Dispose` des „Connection“-Objekts verwenden. Verbindungen, die nicht explizit geschlossen werden, werden möglicherweise dem Pool nicht hinzugefügt bzw. nicht an den Pool zurückgegeben. Beispielsweise wird eine Verbindung, die sich nicht mehr im Gültigkeitsbereich befindet, aber nicht explizit geschlossen wurde, nur dann an den Verbindungspool zurückgegeben, wenn die maximale Poolgröße erreicht wurde und die Verbindung immer noch gültig ist.

> [!NOTE]
> Rufen Sie in der `Finalize`-Methode Ihrer Klasse `Close` oder `Dispose` nicht für **Connection**, **DataReader** oder ein anderes verwaltetes Objekt auf. Geben Sie in einer Finalize-Methode nur nicht verwaltete Ressourcen frei, die der Klasse direkt gehören. Wenn die Klasse keine nicht verwalteten Ressourcen besitzt, definieren Sie in der Klasse keine `Finalize`-Methode. Weitere Informationen finden Sie unter [Garbage Collection](/dotnet/standard/garbage-collection/index).

> [!NOTE]
> Wenn eine Verbindung aus dem Verbindungspool abgerufen oder an diesen zurückgegeben wird, werden keine Anmelde- und Abmeldeereignisse auf dem Server ausgelöst, da die Verbindung bei der Rückgabe an den Verbindungspool nicht geschlossen wird. Weitere Informationen finden Sie unter [SQL Server-Verbindungspooling (ADO.NET)](sql-server-connection-pooling.md).

## <a name="connecting-to-sql-server"></a>Herstellen einer Verbindung mit SQL Server

Gültige Zeichenfolgenformatnamen und -werte finden Sie in der Beschreibung der <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>-Eigenschaft des <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekts. Sie können auch die <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>-Klasse verwenden, um zur Laufzeit syntaktisch gültige Verbindungszeichenfolgen zu erstellen. Weitere Informationen finden Sie in [Connection String Builders (Verbindungszeichenfolgengeneratoren)](connection-string-builders.md).

Im folgenden Codebeispiel wird veranschaulicht, wie eine Verbindung mit einer SQL Server-Datenbank erstellt und geöffnet wird.

[!code-csharp[SqlConnection.Open#1](~/../sqlclient/doc/samples/SqlConnection_Open.cs#1)]

### <a name="integrated-security-and-aspnet"></a>Integrierte Sicherheit und ASP.NET

Die integrierte SQL Server-Sicherheit (auch als vertrauenswürdige Verbindungen bekannt) trägt zum Schutz bei der Herstellung einer Verbindung mit SQL Server bei, da sie keine Benutzer-ID und kein Kennwort in der Verbindungszeichenfolge verfügbar macht und die empfohlene Methode zur Authentifizierung einer Verbindung ist. Bei der integrierten Sicherheit wird die aktuelle Sicherheitsidentität oder das aktuelle Sicherheitstoken des ausführenden Prozesses verwendet. Bei Desktopanwendungen ist diese Identität in der Regel die Identität des aktuell angemeldeten Benutzers.

Die Sicherheitsidentität für ASP.NET-Anwendungen kann auf eine von mehreren verschiedenen Optionen festgelegt werden. Informationen zum besseren Verständnis der in ASP.NET-Anwendungen beim Herstellen einer Verbindung mit SQL Server verwendeten Sicherheitsidentität finden Sie unter [ASP.NET-Identitätswechsel](/previous-versions/aspnet/xh507fc5(v=vs.100)), [ASP.NET-Authentifizierung](/previous-versions/aspnet/eeyk640h(v=vs.100)) und [Gewusst wie: Zugreifen auf SQL Server über die integrierte Windows-Sicherheit](/previous-versions/aspnet/bsz5788z(v=vs.100)).

## <a name="see-also"></a>Weitere Informationen

- [Herstellen einer Verbindung mit Datenquellen](connecting-to-data-source.md)
- [Verbindungszeichenfolgen](connection-strings.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
