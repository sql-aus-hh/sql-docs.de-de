---
title: Asynchrone Programmierung
description: Erfahren Sie mehr über die asynchrone Programmierung im Microsoft SqlClient-Datenanbieter für SQL Server.
ms.date: 12/04/2020
ms.assetid: 85da7447-7125-426e-aa5f-438a290d1f77
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6eec681974499219a1ca649f9a47449a27ff4002
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804305"
---
# <a name="asynchronous-programming"></a>Asynchrone Programmierung

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In diesem Thema wird die Unterstützung für asynchrone Programmierung im Microsoft SqlClient-Datenanbieter für SQL Server (SqlClient) behandelt.

## <a name="legacy-asynchronous-programming"></a>Asynchrone Legacyprogrammierung

Der Microsoft SqlClient-Datenanbieter für SQL Server enthält Methoden aus **System.Data.SqlClient**, um die Abwärtskompatibilität für Anwendungen beizubehalten, die zu <xref:Microsoft.Data.SqlClient> migrieren. Es wird nicht empfohlen, die folgenden Legacymethoden für die asynchrone Programmierung zur Neuentwicklung zu verwenden:

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteReader%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A?displayProperty=nameWithType>

> [!TIP]
> Im Microsoft SqlClient-Datenanbieter für SQL Server ist für diese Legacymethoden das `Asynchronous Processing=true` in der Verbindungszeichenfolge nicht mehr erforderlich.

## <a name="asynchronous-programming-features"></a>Asynchrone Programmierungsfeatures

Diese Features für die asynchrone Programmierung bieten eine einfache Technik, um asynchronen Code zu erstellen.

Weitere Informationen zur asynchronen Programmierung in .NET finden Sie unter:

- [Asynchrone Programmierung in C#](/dotnet/csharp/async)

- [Asynchrone Programmierung mit „Async“ und „Await“ (Visual Basic)](/dotnet/visual-basic/programming-guide/concepts/async/index)

Wenn die Benutzeroberfläche nicht mehr reagiert oder der Server nicht skaliert, ist es wahrscheinlich, dass Sie den Code asynchroner programmieren müssen. Das Schreiben von asynchronem Code umfasste üblicherweise die Installation eines Rückrufs (auch als Fortsetzung bekannt), um die Logik auszudrücken, die nach Ende des asynchronen Vorgangs ausgeführt wird. Dadurch wird die Struktur des asynchronen Codes im Vergleich zu synchronem Code komplizierter.

Sie können asynchrone Methoden aufrufen, ohne Rückrufe zu verwenden und ohne Ihren Code auf mehrere Methoden oder Lambdaausdrücke aufzuteilen.

Der `async`-Modifizierer gibt an, dass eine Methode asynchron ist. Wenn eine `async`-Methode aufgerufen wird, wird eine Aufgabe zurückgegeben. Wenn der `await`-Operator auf eine Aufgabe angewendet wird, wird die aktuelle Methode sofort beendet. Nach Beendigung der Aufgabe wird die Ausführung in derselben Methode fortgesetzt.

> [!TIP]
> Im Microsoft SqlClient Data Provider für SQL Server sind asynchrone Aufrufe nicht erforderlich, um das Schlüsselwort `Context Connection` der Verbindungszeichenfolge festzulegen.

Durch den Aufruf einer `async`-Methode werden keine weiteren Threads zugeordnet. Der vorhandene E/A-Abschlussthread wird möglicherweise kurz am Ende verwendet.

Die folgenden Methoden im Microsoft SqlClient-Datenanbieter für SQL Server unterstützen die asynchrone Programmierung:

- <xref:System.Data.Common.DbConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteDbDataReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.GetFieldValueAsync%2A>

- <xref:System.Data.Common.DbDataReader.IsDBNullAsync%2A>

- <xref:System.Data.Common.DbDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteXmlReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>

Andere asynchrone Member unterstützen die [SqlClient-Streamingunterstützung](sqlclient-streaming-support.md).

> [!TIP]
> Die asynchronen Methoden erfordern kein `Asynchronous Processing=true` in der Verbindungszeichenfolge. Und diese Eigenschaft ist im Microsoft SqlClient-Datenanbieter für SQL Server veraltet.

### <a name="synchronous-to-asynchronous-connection-open"></a>Synchrone zu asynchrone Verbindung geöffnet

Sie können ein Upgrade einer vorhandenen Anwendung durchführen, um das asynchrone Feature zu verwenden. Angenommen, eine Anwendung verfügt über einen synchronen Verbindungsalgorithmus und blockiert den Benutzeroberflächenthread bei jeder Verbindung mit der Datenbank. Außerdem ruft die Anwendung, sobald sie verbunden ist, eine gespeicherte Prozedur auf, die anderen Benutzern den gerade angemeldeten Benutzer signalisiert.

[!code-csharp[SqlCommand_ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery.cs#1)]

Wenn es für die Verwendung der asynchronen Funktionalität konvertiert wird, würde das Programm wie folgt aussehen:

[!code-csharp[SqlCommand_ExecuteNonQueryAsync#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQueryAsync.cs#1)]

### <a name="add-the-asynchronous-feature-in-an-existing-application-mixing-old-and-new-patterns"></a>Hinzufügen des asynchronen Features in eine vorhandene Anwendung (Mischen alter und neuer Muster)

Es ist auch möglich, die asynchrone Funktion (SqlConnection::OpenAsync) hinzuzufügen, ohne die vorhandene asynchrone Logik zu ändern. Wenn eine Anwendung derzeit beispielsweise folgenden Code verwendet...

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#1](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#1)]

Sie können beginnen, das asynchrone Muster zu verwenden, ohne den bestehenden Algorithmus wesentlich zu verändern.

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#2](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#2)]

### <a name="use-the-base-provider-model-and-the-asynchronous-feature"></a>Verwenden des Basisanbietermodells und des asynchronen Features

Möglicherweise müssen Sie ein Tool erstellen, das eine Verbindung mit verschiedenen Datenbanken herstellen und Abfragen ausführen kann. Sie können das Basisanbietermodell und das asynchrone Feature verwenden.

Microsoft Distributed Transaction Controller (MSDTC) muss auf dem Server aktiviert sein, damit verteilte Transaktionen verwendet werden können. Informationen zum Aktivieren von MSDTC finden Sie unter [Aktivieren von MSDTC auf einem Webserver](/previous-versions/commerce-server/dd327979(v=cs.90)).

[!code-csharp[SqlClient_AsyncProgramming_ProviderModel#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_ProviderModel.cs#1)]

### <a name="use-sql-transactions-and-the-new-asynchronous-feature"></a>Verwenden von SQL-Transaktionen und des neuen asynchronen Features

[!code-csharp[SqlClient_AsyncProgramming_SqlTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlTransaction.cs#1)]

### <a name="use-distributed-transactions-and-the-new-asynchronous-feature"></a>Verwenden von verteilten Transaktionen und des neuen asynchronen Features

In einer Unternehmensanwendung müssen Sie in einigen Szenarien möglicherweise **verteilte Transaktionen** hinzufügen, um Transaktionen zwischen mehreren Datenbankservern zu ermöglichen. Sie können den System.Transactions-Namespace verwenden und wie folgt eine verteilte Transaktion eintragen:

[!code-csharp[SqlClient_AsyncProgramming_DistributedTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_DistributedTransaction.cs#1)]

### <a name="cancel-an-asynchronous-operation"></a>Abbrechen eines asynchronen Vorgangs

Sie können eine asynchrone Anforderung mithilfe von <xref:System.Threading.CancellationToken> abbrechen.

[!code-csharp[SqlClient_AsyncProgramming_Cancellation#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_Cancellation.cs#1)]

### <a name="asynchronous-operations-with-sqlbulkcopy"></a>Asynchrone Vorgänge mit SqlBulkCopy

Asynchrone Funktionen sind auch in <xref:Microsoft.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType> (mit <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>).

[!code-csharp[SqlClient_AsyncProgramming_SqlBulkCopy#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlBulkCopy.cs#1)]

## <a name="asynchronously-use-multiple-commands-with-mars"></a>Asynchrones Verwenden mehrerer Befehle mit MARS

Im Beispiel wird eine einzelne Verbindung mit der **AdventureWorks**-Datenbank geöffnet. Wenn Sie ein <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekt verwenden, wird ein <xref:Microsoft.Data.SqlClient.SqlDataReader> erstellt. Bei Verwenden des Readers wird ein zweiter <xref:Microsoft.Data.SqlClient.SqlDataReader> geöffnet, wobei die Daten aus dem ersten <xref:Microsoft.Data.SqlClient.SqlDataReader> als Eingabe in die WHERE-Klausel für den zweiten Reader verwendet werden.

> [!NOTE]
> Im folgenden Beispiel wird die [**AdventureWorks**-Beispieldatenbank](../../samples/adventureworks-install-configure.md) verwendet. Bei der im Beispielcode bereitgestellten Verbindungszeichenfolge wird davon ausgegangen, dass die Datenbank auf dem lokalen Computer installiert und verfügbar ist. Ändern Sie die Verbindungszeichenfolge nach Bedarf für Ihre Umgebung.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommands#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommands.cs#1)]

## <a name="asynchronously-read-and-update-data-with-mars"></a>Asynchrones Lesen und Aktualisieren von Daten mit MARS

MARS ermöglicht die Verwendung einer Verbindung sowohl für Lese- als auch für DML-Vorgänge (Data Manipulation Language, Datenbearbeitungssprache) mit mehr als einem ausstehenden Vorgang. Dieses Feature macht eine Anwendung zur Behandlung von Fehlern im Zusammenhang mit der Verbindungsauslastung überflüssig. Darüber hinaus kann MARS die Verwendung serverseitiger Cursor ersetzen, die im Allgemeinen mehr Ressourcen benötigen. Da außerdem mehrere Vorgänge über eine einzelne Verbindung ausgeführt werden können, kann ein gemeinsamer Transaktionskontext verwendet werden. Dadurch ist die Verwendung der gespeicherten Systemprozeduren **sp_getbindtoken** und **sp_bindsession** nicht mehr erforderlich.

Die folgende Konsolenanwendung veranschaulicht die Verwendung zweier <xref:Microsoft.Data.SqlClient.SqlDataReader>-Objekte mit drei <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekten und einem einzelnen <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekt bei aktiviertem MARS. Das erste Befehlsobjekt ruft eine Liste von Anbietern ab, deren Bonitätsbewertung 5 ist. Das zweite Befehlsobjekt verwendet die von einem <xref:Microsoft.Data.SqlClient.SqlDataReader> bereitgestellte Anbieter-ID, um den zweiten <xref:Microsoft.Data.SqlClient.SqlDataReader> mit allen Produkten für den bestimmten Anbieter zu laden. Jeder Produktdatensatz wird vom zweiten <xref:Microsoft.Data.SqlClient.SqlDataReader> besucht. Es wird eine Berechnung ausgeführt, um den neuen Wert für **OnOrderQty** zu bestimmen. Anschließend wird mithilfe des dritten Befehlsobjekts die **ProductVendor**-Tabelle mit dem neuen Wert aktualisiert. Der gesamte Prozess findet innerhalb einer einzelnen Transaktion statt, für die am Ende ein Rollback erfolgt.

> [!NOTE]
> Im folgenden Beispiel wird die [**AdventureWorks**-Beispieldatenbank](../../samples/adventureworks-install-configure.md) verwendet. Bei der im Beispielcode bereitgestellten Verbindungszeichenfolge wird davon ausgegangen, dass die Datenbank auf dem lokalen Computer installiert und verfügbar ist. Ändern Sie die Verbindungszeichenfolge nach Bedarf für Ihre Umgebung.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommandsEx#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommandsEx.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen und Ändern von Daten in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
