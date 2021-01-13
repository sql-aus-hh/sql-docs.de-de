---
title: SqlClient-Streamingunterstützung
description: Erläutert, wie Sie Anwendungen schreiben, die Daten aus SQL Server streamen, ohne sie vollständig in den Arbeitsspeicher zu laden.
ms.date: 12/04/2020
ms.assetid: c449365b-470b-4edb-9d61-8353149f5531
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c5ae05856f3f1e01d5831a6e80338f19e3994966
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744361"
---
# <a name="sqlclient-streaming-support"></a>SqlClient-Streamingunterstützung

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Die Streamingunterstützung zwischen SQL Server und einer Anwendung unterstützt unstrukturierte Daten auf dem Server (Dokumente, Bilder und Mediendateien). Eine SQL Server-Datenbank kann BLOBs (Binary Large Objects) speichern, beim Abrufen von BLOBs kann jedoch viel Arbeitsspeicher beansprucht werden.

Durch die Unterstützung des Streamings an und von SQL Server wird das Schreiben von Anwendungen vereinfacht, die Daten streamen. Daten müssen nicht vollständig in den Arbeitsspeicher geladen werden, was zu weniger Ausnahmefehlern aufgrund Arbeitsspeicherüberlaufs führt.

Die Unterstützung des Streamings wird auch eine bessere Skalierung von Middle-Tier-Anwendungen ermöglichen, insbesondere in Szenarien, in denen Geschäftsobjekte Verbindungen mit Azure SQL herstellen, um große BLOBs zu senden, abzurufen und zu bearbeiten.

> [!WARNING]
> Die Member, die Streaming unterstützen, werden zum Abrufen von Daten aus Abfragen und zur Übergabe von Parametern an Abfragen und gespeicherte Prozeduren verwendet. Das Streamingfeature behandelt grundlegende OLTP- und Datenmigrationsszenarien und ist für lokale und externe Datenmigrationsumgebungen geeignet.

## <a name="streaming-support-from-sql-server"></a>Streamingunterstützung von SQL Server

Mit der Unterstützung des Streamings von SQL Server werden neue Funktionen in den Klassen <xref:System.Data.Common.DbDataReader> und <xref:Microsoft.Data.SqlClient.SqlDataReader> eingeführt, um <xref:System.IO.Stream>-, <xref:System.Xml.XmlReader>- und <xref:System.IO.TextReader>-Objekte abzurufen und darauf zu reagieren. Diese Klassen werden verwendet, um Daten aus Abfragen abzurufen. Daher betrifft das unterstützte Streaming von SQL Server OLTP-Szenarien und ist für lokale und externe Umgebungen geeignet.

Die folgenden Member wurden <xref:Microsoft.Data.SqlClient.SqlDataReader> hinzugefügt, um die Streamingunterstützung von SQL Server zu ermöglichen:

- <xref:Microsoft.Data.SqlClient.SqlDataReader.IsDBNullAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValue%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValueAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetStream%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetTextReader%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetXmlReader%2A>

Die folgenden Member wurden <xref:System.Data.Common.DbDataReader> hinzugefügt, um die Streamingunterstützung von SQL Server zu ermöglichen:

- <xref:System.Data.Common.DbDataReader.GetFieldValue%2A>

- <xref:System.Data.Common.DbDataReader.GetStream%2A>

- <xref:System.Data.Common.DbDataReader.GetTextReader%2A>

## <a name="streaming-support-to-sql-server"></a>Streamingunterstützung zu SQL Server

Die Streamingunterstützung zu SQL Server befindet sich in der Klasse <xref:Microsoft.Data.SqlClient.SqlParameter>, damit sie <xref:System.Xml.XmlReader>-, <xref:System.IO.Stream>- und <xref:System.IO.TextReader>-Objekte akzeptieren und auf diese reagieren kann. <xref:Microsoft.Data.SqlClient.SqlParameter> wird verwendet, um Parameter an Abfragen und gespeicherte Prozeduren zu übergeben.

> [!NOTE]
> Durch das Verwerfen eines <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekts oder das Aufrufen von <xref:Microsoft.Data.SqlClient.SqlCommand.Cancel%2A> muss jeglicher Streamingvorgang abgebrochen werden. Wenn eine Anwendung <xref:System.Threading.CancellationToken> sendet, wird der Abbruch nicht sichergestellt.

Die folgenden <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A>-Typen akzeptieren den <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A>-Wert <xref:System.IO.Stream>:

- **Binär (Binary)**

- **VarBinary**

Die folgenden <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A>-Typen akzeptieren den <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A>-Wert <xref:System.IO.TextReader>:

- **Char**

- **NChar**

- **NVarChar**

- **Xml**

Der **Xml**<xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A>-Typ akzeptiert ein <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> von <xref:System.Xml.XmlReader>.

<xref:Microsoft.Data.SqlClient.SqlParameter.SqlValue%2A> kann Werte des Typs <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> und <xref:System.IO.Stream> akzeptieren.

Das <xref:System.Xml.XmlReader>-, <xref:System.IO.TextReader>- und <xref:System.IO.Stream>-Objekt wird bis zu dem von <xref:Microsoft.Data.SqlClient.SqlParameter.Size%2A> definierten Wert übertragen.

## <a name="sample----streaming-from-sql-server"></a>Beispiel: Streaming von SQL Server

Verwenden Sie das folgende Transact-SQL, um die Beispieldatenbank zu erstellen:

```sql
CREATE DATABASE [Demo]
GO
USE [Demo]
GO
CREATE TABLE [Streams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX),
[bindata] VARBINARY(MAX),
[xmldata] XML)
GO
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'This is a test', 0x48656C6C6F, N'<test>value</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Hello, World!', 0x54657374696E67, N'<test>value2</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Another row', 0x666F6F626172, N'<fff>bbb</fff><fff>bbc</fff>')
GO
```

Das Beispiel erläutert die folgenden Aufgaben:

- Vermeiden, dass ein Benutzeroberflächenthread blockiert wird, indem eine asynchrone Methode zum Abrufen großer Dateien bereitgestellt wird.

- Übertragen einer großen Textdatei aus SQL Server in .NET.

- Übertragen einer großen XML-Datei aus SQL Server in .NET.

- Abrufen von Daten aus SQL Server.

- Übertragen großer Dateien (BLOB)s aus einer SQL Server-Datenbank in eine andere, ohne dass der Arbeitsspeicher knapp wird.

[!code-csharp[SqlClient_Streaming_FromServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_FromServer.cs#1)]

## <a name="sample----streaming-to-sql-server"></a>Beispiel: Streaming zu SQL Server

Verwenden Sie das folgende Transact-SQL, um die Beispieldatenbank zu erstellen:

```sql
CREATE DATABASE [Demo2]
GO
USE [Demo2]
GO
CREATE TABLE [BinaryStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
CREATE TABLE [TextStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX))
GO
CREATE TABLE [BinaryStreamsCopy] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
```

Das Beispiel erläutert die folgenden Aufgaben:

- Übertragen eines großen BLOBs zu SQL Server in .NET.

- Übertragen einer großen Textdatei zu SQL Server in .NET.

- Verwenden der neuen asynchronen Funktion zur Übertragung eines großen BLOBs.

- Verwenden der neuen asynchronen Funktion und des await-Schlüsselworts zur Übertragung eines großen BLOBs.

- Abbrechen der Übertragung eines großen BLOBs.

- Streamen von einer SQL Server-Instanz zu einer anderen mithilfe des neuen asynchronen Features.

[!code-csharp[SqlClient_Streaming_ToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ToServer.cs#1)]

## <a name="sample----streaming-from-one-sql-server-to-another-sql-server"></a>Beispiel: Streaming von einer SQL Server-Instanz zu einer anderen SQL Server-Instanz

Dieses Beispiel zeigt, wie ein großes BLOB asynchron von einer SQL Server-Instanz zu einer anderen gestreamt wird, wobei der Abbruch unterstützt wird.

> [!NOTE]
> Stellen Sie vor dem Ausführen des folgenden Beispiels sicher, dass die Datenbanken Demo und Demo2 erstellt wurden.

[!code-csharp[SqlClient_Streaming_ServerToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ServerToServer.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen und Ändern von Daten in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
