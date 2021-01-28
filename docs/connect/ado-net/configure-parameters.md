---
title: Konfigurieren von Parametern
description: Command-Objekte übergeben Werte mithilfe von Parametern an SQL-Anweisungen oder gespeicherte Prozeduren. Dabei bieten sie eine Typüberprüfung und -validierung in ADO.NET.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12dfcb26484b468bed9f9262403806ea88829385
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595775"
---
# <a name="configuring-parameters"></a>Konfigurieren von Parametern

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Befehlsobjekte verwenden Parameter, um Werte an SQL-Anweisungen oder gespeicherte Prozeduren zu übergeben, und ermöglichen so Typüberprüfungen und Validierungen. Im Unterschied zu Befehlstext wird die Parametereingabe als Literalwert und nicht als ausführbarer Code behandelt. Dies hilft beim Schutz vor SQL Injection-Angriffen, bei denen ein Angreifer einen SQL-Befehl, der die Sicherheit auf dem Server gefährdet, in eine SQL-Anweisung einschleust.

Parametrisierte Befehle können außerdem die Leistung bei der Abfrageausführung verbessern, da sie den Datenbankserver dabei unterstützen, den eingehenden Befehl mit dem richtigen zwischengespeicherten Abfrageplan abzugleichen. Weitere Informationen finden Sie unter [Zwischenspeichern und Wiederverwenden von Ausführungsplänen](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse) und [Parameter und Wiederverwendung von Ausführungsplänen](../../relational-databases/query-processing-architecture-guide.md#PlanReuse). Parametrisierte Befehle sind aber nicht nur aus Sicherheits- und Leistungsgründen vorteilhaft, sondern sie stellen auch eine bequeme Methode zum Organisieren von Werten dar, die an eine Datenquelle übergeben werden.

Ein <xref:System.Data.Common.DbParameter> -Objekt kann mithilfe des zugehörigen Konstruktors erstellt werden, oder es wird durch Aufrufen der <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> -Methode der `Add` -Auflistung zur <xref:System.Data.Common.DbParameterCollection> hinzugefügt. Die `Add` -Methode verwendet als Eingabe je nach Datenanbieter Konstruktorargumente oder ein vorhandenes Parameterobjekt.

## <a name="supply-the-parameterdirection-property"></a>Bereitstellen der ParameterDirection-Eigenschaft

Beim Hinzufügen von Parametern müssen Sie für alle Parameter, die keine Eingabeparameter sind, die <xref:System.Data.ParameterDirection> -Eigenschaft bereitstellen. Die folgende Tabelle zeigt die `ParameterDirection` -Werte, die Sie mit der <xref:System.Data.ParameterDirection> -Enumeration verwenden können.

|Membername|Beschreibung|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|Der Parameter ist ein Eingabeparameter. Dies ist die Standardoption.|
|<xref:System.Data.ParameterDirection.InputOutput>|Der Parameter kann sowohl für die Eingabe als auch für die Ausgabe verwendet werden.|
|<xref:System.Data.ParameterDirection.Output>|Der Parameter ist ein Ausgabeparameter.|
|<xref:System.Data.ParameterDirection.ReturnValue>|Der Parameter steht für einen Eingabewert aus einem Vorgang, wie z. B. einer gespeicherten Prozedur, einer integrierten Funktion oder einer benutzerdefinierten Funktion.|

## <a name="work-with-parameter-placeholders"></a>Arbeiten mit Parameterplatzhaltern

Die Syntax für Parameterplatzhalter ist abhängig von der jeweiligen Datenquelle. Beim Microsoft SqlClient-Datenanbieter für SQL Server werden die Benennung und Angabe von Parametern und Parameterplatzhaltern unterschiedlich behandelt. Der SqlClient-Datenanbieter verwendet benannte Parameter im Format `@`*Parametername*.

## <a name="specify-parameter-data-types"></a>Angeben von Parameterdatentypen

Der Microsoft SqlClient-Datenanbieter für SQL Server verwendet für Parameter einen spezifischen Datentyp. Beim Angeben des Typs wird der Wert des `Parameter`-Objekts in den Typ des Microsoft SqlClient-Datenanbieters für SQL Server konvertiert, bevor er an die Datenquelle übergeben wird. Der Typ eines `Parameter` kann auch in generischer Form angegeben werden, indem die `DbType` -Eigenschaft des `Parameter` -Objekts auf einen bestimmten <xref:System.Data.DbType>festgelegt wird.

Der Typ eines `Parameter`-Objekts des Microsoft SqlClient-Datenanbieters für SQL Server wird vom .NET Framework-Typ des `Value` des `Parameter`-Objekts oder vom `DbType` des `Parameter`-Objekts abgeleitet. Die folgende Tabelle zeigt den abgeleiteten `Parameter` -Typ basierend auf dem als `Parameter` -Wert übergebenen Objekt oder dem angegebenen `DbType`.

|.NET-Typ|DbType|SqlDbType|
|-------------------------|------------|---------------|
|<xref:System.Boolean>|Boolean|bit|
|<xref:System.Byte>|Byte|TinyInt|
|byte[]|Binary|VarBinary. Bei dieser impliziten Konvertierung tritt ein Fehler auf, wenn das Bytearray größer als die maximal zulässige Größe eines VarBinary (8.000 Byte) ist. Legen Sie für Bytearrays mit mehr als 8.000 Byte explizit den <xref:System.Data.SqlDbType> fest.|
|<xref:System.Char>| |Das Ableiten von <xref:System.Data.SqlDbType> aus char wird nicht unterstützt.|
|<xref:System.DateTime>|Datetime|Datetime|
|<xref:System.DateTimeOffset>|DateTimeOffset|"DateTimeOffset" in SQL Server 2008. Das Ableiten von <xref:System.Data.SqlDbType> aus DateTimeOffset wird erst ab SQL Server 2008 unterstützt.|
|<xref:System.Decimal>|Decimal|Decimal|
|<xref:System.Double>|Double|Float|
|<xref:System.Single>|Single|Real|
|<xref:System.Guid>|Guid|UniqueIdentifier|
|<xref:System.Int16>|Int16|SmallInt|
|<xref:System.Int32>|Int32|Int|
|<xref:System.Int64>|Int64|BigInt|
|<xref:System.Object>|Objekt|Variant|
|<xref:System.String>|String|NVarChar. Diese implizite Konvertierung kann nicht ausgeführt werden, wenn die Zeichenfolge die maximale NVarChar-Länge (4000 Zeichen) überschreitet. Legen Sie für Zeichenfolgen mit mehr als 4000 Zeichen explizit <xref:System.Data.SqlDbType>fest.|
|<xref:System.TimeSpan>|Time|"Time" in SQL Server 2008. Das Ableiten von <xref:System.Data.SqlDbType> aus TimeSpan wird erst ab SQL Server 2008 unterstützt.|
|<xref:System.UInt16>|UInt16|Das Ableiten von <xref:System.Data.SqlDbType> aus UInt16 wird nicht unterstützt.|
|<xref:System.UInt32>|UInt32|Das Ableiten von <xref:System.Data.SqlDbType> aus UInt32 wird nicht unterstützt.|
|<xref:System.UInt64>|UInt64|Das Ableiten von <xref:System.Data.SqlDbType> aus UInt64 wird nicht unterstützt.|
||AnsiString|VarChar|
||AnsiStringFixedLength|Char|
||Währung|Money|
||Date|"Date" in SQL Server 2008. Das Ableiten von <xref:System.Data.SqlDbType> aus Date wird erst ab SQL Server 2008 unterstützt.|
||SByte|Das Ableiten von <xref:System.Data.SqlDbType> aus SByte wird nicht unterstützt.|
||StringFixedLength|NChar|
||Time|"Time" in SQL Server 2008. Das Ableiten von <xref:System.Data.SqlDbType> aus Time wird erst ab SQL Server 2008 unterstützt.|
||VarNumeric|Das Ableiten von <xref:System.Data.SqlDbType> aus VarNumeric wird nicht unterstützt.|
|Benutzerdefinierter Typ (ein Objekt mit <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>)|SqlClient gibt immer ein Objekt zurück|`SqlDbType.Udt`, wenn <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute> vorhanden ist. Anderenfalls `Variant`|

> [!NOTE]
> Beim Konvertieren von "decimal" in einen anderen Typ erhalten Sie nur eine annähernde Entsprechung, da der Wert auf die nächste Ganzzahl abgerundet wird. Wenn das Ergebnis der Konvertierung im Zieltyp nicht darstellbar ist, wird eine <xref:System.OverflowException> ausgelöst.

> [!NOTE]
> Wenn Sie einen NULL-Parameterwert an den Server senden, müssen Sie <xref:System.DBNull> und nicht `null` (in Visual Basic `Nothing`)+++ angeben. Der NULL-Wert im System ist ein leeres Objekt, das über keinen Wert verfügt. <xref:System.DBNull> wird zur Darstellung von NULL-Werten verwendet.

## <a name="derive-parameter-information"></a>Ableiten von Parameterinformationen

Parameter können auch mit der `DbCommandBuilder` -Klasse aus einer gespeicherten Prozedur abgeleitet werden. Die `SqlCommandBuilder`-Klasse stellt eine statische Methode (`DeriveParameters`) bereit, die die Parameterauflistung eines Befehlsobjekts, das Parameterinformationen aus einer gespeicherten Prozedur verwendet, automatisch füllt. Beachten Sie, dass `DeriveParameters` alle vorhandenen Parameterinformationen für den Befehl überschreibt.

> [!NOTE]
> Das Ableiten von Parameterinformationen geht mit einem Leistungsverlust einher, weil zum Abrufen der Informationen ein zusätzlicher Roundtrip durch die Datenquelle erforderlich ist. Wenn die Parameterinformationen zur Entwurfszeit bekannt sind, können Sie die Leistung der Anwendung verbessern, indem Sie die Parameter explizit festlegen.

Weitere Informationen finden Sie unter [Generieren von Befehlen mit CommandBuilder-Objekten](generate-commands-with-commandbuilders.md).

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>Verwenden von Parametern mit einem SqlCommand und einer gespeicherten Prozedur

Gespeicherte Prozeduren bieten zahlreiche Vorteile in datengesteuerten Anwendungen. Mit gespeicherten Prozeduren können Datenbankoperationen in einem einzelnen Befehl zusammengefasst, für die beste Leistung optimiert und mit zusätzlicher Sicherheit ausgestattet werden. Während eine gespeicherte Prozedur problemlos aufgerufen werden kann, indem der Name der gespeicherten Prozedur gefolgt von Parameterargumenten als SQL-Anweisung übergeben wird, ermöglicht die Verwendung der <xref:System.Data.Common.DbCommand.Parameters%2A>-Auflistung des ADO.NET-Objekts <xref:System.Data.Common.DbCommand> eine genauere Definition der Parameter der gespeicherten Prozedur sowie den Zugriff auf Ausgabeparameter und Rückgabewerte.

> [!NOTE]
> Parametrisierte Anweisungen werden auf dem Server mit `sp_executesql,` ausgeführt, sodass die Wiederverwendung von Abfrageplänen möglich ist. Lokale Cursor oder Variablen im `sp_executesql` -Batch sind für den Batch, der `sp_executesql`aufruft, nicht sichtbar. Änderungen am Datenbankkontext sind nur bis zum Ende der `sp_executesql` -Anweisung gültig. Weitere Informationen finden Sie unter [sp_executesql (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md).

Wenn Sie Parameter mit einem <xref:Microsoft.Data.SqlClient.SqlCommand> verwenden, um eine gespeicherte SQL Server-Prozedur auszuführen, müssen die der <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> -Auflistung hinzugefügten Parameternamen mit den Namen der Parametermarkierungen in der gespeicherten Prozedur übereinstimmen. Der Microsoft SqlClient-Datenanbieter für SQL Server unterstützt keine Fragezeichenplatzhalter (?) für die Übergabe von Parametern an eine SQL-Anweisung oder gespeicherte Prozedur. Er behandelt die Parameter in der gespeicherten Prozedur als benannte Parameter und sucht nach den entsprechenden Parametermarkierungen. Nehmen wir z. B. an, die gespeicherte Prozedur `CustOrderHist` ist mit einem Parameter mit dem Namen `@CustomerID`definiert. Wenn Ihr Code die gespeicherte Prozedur ausführt, muss er ebenfalls einen Parameter mit dem Namen `@CustomerID`verwenden.

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>Beispiel

Dieses Beispiel zeigt, wie Sie eine gespeicherte SQL Server-Prozedur in der `Northwind` -Beispieldatenbank aufrufen können. Der Name der gespeicherten Prozedur ist `dbo.SalesByCategory` , und die Prozedur besitzt einen Eingabeparameter mit dem Namen `@CategoryName` und dem Datentyp `nvarchar(15)`. Der Code erstellt eine neue <xref:Microsoft.Data.SqlClient.SqlConnection> innerhalb eines verwendeten Blocks, sodass die Verbindung nach dem Ende der Prozedur verworfen wird. Es werden die Objekte <xref:Microsoft.Data.SqlClient.SqlCommand> und <xref:Microsoft.Data.SqlClient.SqlParameter> erstellt, und deren Eigenschaften werden festgelegt. Ein <xref:Microsoft.Data.SqlClient.SqlDataReader> führt den `SqlCommand` aus und gibt den Resultset aus der gespeicherten Prozedur zurück, wobei die Ausgabe im Konsolenfenster angezeigt wird.

> [!NOTE]
> Statt die Objekte `SqlCommand` und `SqlParameter` zu erstellen und dann die Eigenschaften in separaten Anweisungen festzulegen, können Sie auch mit einem der überladenen Konstruktoren mehrere Eigenschaften in einer einzigen Anweisung festlegen.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Befehle und Parameter](commands-parameters.md)
- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)