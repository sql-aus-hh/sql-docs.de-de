---
title: Auffüllen eines Datasets aus einem DataAdapter
description: Hier erfahren Sie, wie Sie eine DataSet-Klasse aus einer DataAdapter-Klasse in ADO.NET auffüllen, wo ein von der Datenquelle unabhängiges, konsistentes relationales Programmiermodell bereitgestellt wird.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e6c50bf7255dc77edfd0b93e03dedeec83ed4c4d
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771502"
---
# <a name="populate-a-dataset-from-a-dataadapter"></a>Auffüllen eines Datasets aus einem DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Das ADO.NET-<xref:System.Data.DataSet> ist eine speicherresidente Datendarstellung, das ein konsistentes relationales und von der Datenquelle unabhängiges Programmiermodell bereitstellt. Das `DataSet` stellt eine vollständige Datengruppe einschließlich Tabellen, Einschränkungen und Beziehungen zwischen Tabellen dar. Da das `DataSet` von der Datenquelle unabhängig ist, kann ein `DataSet` sowohl lokale Daten einer Anwendung als auch Daten aus mehreren Datenquellen enthalten. Die Interaktion mit vorhandenen Datenquellen wird über den `DataAdapter`gesteuert.

Die `SelectCommand` -Eigenschaft des `DataAdapter` ist ein `Command` -Objekt, das Daten aus der Datenquelle abruft. Die `InsertCommand`-Eigenschaft, die `UpdateCommand`-Eigenschaft und die `DeleteCommand` -Eigenschaft des `DataAdapter` sind `Command` -Objekte, die Updates an den Daten in der Datenquelle entsprechend den Modifikationen an den Daten im `DataSet`verwalten. Diese Eigenschaften werden unter [Aktualisieren von Datenquellen mit DataAdapter-Klassen](update-data-sources-with-dataadapters.md) ausführlicher behandelt.

Mithilfe der `Fill` -Methode des `DataAdapter` wird ein `DataSet` mit den Ergebnissen vom `SelectCommand` des `DataAdapter`aufgefüllt. `Fill` verwendet als Argumente ein aufzufüllendes `DataSet` sowie ein `DataTable` -Objekt bzw. den Namen der `DataTable` , die mit den Zeilen gefüllt werden soll, die vom `SelectCommand`zurückgegeben werden.

> [!NOTE]
> Die Verwendung des `DataAdapter` zum Abrufen einer gesamten Tabelle kann einige Zeit in Anspruch nehmen, insbesondere, wenn die Tabelle viele Zeilen enthält. Der Grund hierfür ist, dass das Zugreifen auf die Datenbank und das Auffinden, Verarbeiten und Übertragen der Daten an den Client zeitaufwändig ist. Durch das Abrufen der gesamten Tabelle seitens des Clients werden außerdem alle ihre Zeilen auf dem Server gesperrt. Zur Verbesserung der Leistung können Sie die `WHERE` -Klausel verwenden, um die Anzahl der an den Client zurückgegebenen Zeilen deutlich zu reduzieren. Die Anzahl der an den Client zurückgegebenen Daten kann auch verringert werden, indem Sie in der `SELECT` -Anweisung nur die erforderlichen Spalten explizit auflisten. Eine weitere gute Möglichkeit, dieses Problem zu umgehen, besteht darin, die Zeilen in Stapeln (z. B. immer einige hundert Zeilen auf einmal) abzurufen, wobei der nächste Stapel erst dann abgerufen wird, wenn der Client mit dem aktuellen Stapel fertig ist.

Die `Fill` -Methode verwendet das `DataReader` -Objekt implizit, um die Spaltennamen und Spaltentypen zurückzugeben, mit denen die Tabellen im `DataSet`erstellt werden, sowie die Daten zum Füllen der Tabellenzeilen im `DataSet`. Tabellen und Spalten werden nur erstellt, wenn sie noch nicht vorhanden sind. Andernfalls verwendet die `Fill` -Methode das vorhandene `DataSet` -Schema. Spaltentypen werden als .NET Framework-Typen gemäß den Tabellen unter [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md) erstellt. Primärschlüssel werden nur erstellt, wenn diese in der Datenquelle vorhanden sind und `DataAdapter` **.** `MissingSchemaAction` auf `MissingSchemaAction` **.** `AddWithKey` festgelegt ist. Wenn die `Fill` -Methode ermittelt, dass ein Primärschlüssel für eine Tabelle vorhanden ist, werden Daten im `DataSet` mit den Daten aus der Datenquelle überschrieben. Dies gilt für Zeilen, deren Werte in der Primärschlüsselspalte mit denen der Zeile übereinstimmen, die von der Datenquelle zurückgegeben wurde. Wurde kein Primärschlüssel gefunden, so werden die Daten an die Tabellen im `DataSet`angehängt. `Fill` verwendet alle Zuordnungen, die möglicherweise vorhanden sind, wenn Sie die `DataSet`-Klasse auffüllen (siehe [DataAdapter-, DataTable- und DataColumn-Zuordnungen](dataadapter-datatable-datacolumn-mappings.md)).

> [!NOTE]
> Wenn vom `SelectCommand` die Ergebnisse eines OUTER JOIN zurückgegeben werden, wird vom `DataAdapter` kein `PrimaryKey` -Wert für die resultierende `DataTable`festgelegt. Sie müssen den `PrimaryKey` selbst definieren, um sicherzustellen, dass doppelte Zeilen ordnungsgemäß aufgelöst werden.

Im folgenden Codebeispiel wird eine Instanz eines <xref:Microsoft.Data.SqlClient.SqlDataAdapter> erstellt, die eine <xref:Microsoft.Data.SqlClient.SqlConnection> zur Microsoft SQL Server-Datenbank `Northwind` verwendet und eine <xref:System.Data.DataTable> in einem `DataSet` mit der Kundenliste füllt. Mit der SQL-Anweisung und den <xref:Microsoft.Data.SqlClient.SqlConnection> -Argumenten, die an den <xref:Microsoft.Data.SqlClient.SqlDataAdapter> -Konstruktor übergeben werden, wird die <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> -Eigenschaft des <xref:Microsoft.Data.SqlClient.SqlDataAdapter>erstellt.

## <a name="example"></a>Beispiel

[!code-csharp[SqlDataAdapter_FillDataSet#1](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#1)]

> [!NOTE]
> Der in diesem Beispiel dargestellte Code öffnet und schließt die `Connection`nicht explizit. Die `Fill` -Methode öffnet implizit die `Connection` , die vom `DataAdapter` verwendet wird, wenn die Verbindung nicht bereits geöffnet ist. Wenn die Verbindung durch `Fill` geöffnet wurde, wird sie nach dem `Fill` -Vorgang auch wieder geschlossen. Bei einzelnen Vorgängen wie `Fill` oder `Update`kann der Code auf diese Weise vereinfacht werden. Wenn Sie jedoch mehrere Vorgänge durchführen, für die eine geöffnete Verbindung erforderlich ist, können Sie die Leistung der Anwendung verbessern, indem Sie explizit die `Open` -Methode der `Connection`aufrufen, die Vorgänge für die Datenquelle durchführen und anschließend die `Close` -Methode der `Connection`aufrufen. Sie sollten die Verbindungen zur Datenquelle so kurz wie möglich geöffnet lassen, um die Ressourcen freizugeben, die von anderen Clientanwendungen verwendet werden.

## <a name="multiple-result-sets"></a>Mehrere Resultsets

Wenn der `DataAdapter` mehrere Resultsets ermittelt, werden mehrere Tabellen im `DataSet`erstellt. Diesen Tabellen werden standardmäßig Namen nach dem Schema Table *N*, beginnend mit "Table" für Table0, zugewiesen, die jeweils um eins erhöht werden. Wenn der Tabellenname als Argument an die `Fill` -Methode übergeben wird, erhalten die Tabellen standardmäßig Namen nach dem Schema TableName *N*, beginnend mit "TableName" für TableName0, die jeweils um eins erhöht werden.  
  
## <a name="populate-a-dataset-from-multiple-dataadapters"></a>Auffüllen einer DataSet-Klasse aus mehreren DataAdapter-Klassen  

Eine `DataSet`-Klasse kann mit einer beliebigen Anzahl von `DataAdapter`-Objekten verwendet werden. Mit jedem `DataAdapter` -Objekt können ein oder mehrere `DataTable` -Objekte gefüllt und Updates in die zugehörige Datenquelle übernommen werden. Das`DataRelation` -Objekt und das `Constraint` -Objekt können dem `DataSet` lokal hinzugefügt werden. Dadurch haben Sie die Möglichkeit, Daten aus mehreren unähnlichen Datenquellen zu verbinden. Ein `DataSet` kann beispielsweise Daten aus einer Microsoft SQL Server-Datenbank, einer über OLE DB verfügbar gemachten IBM DB2-Datenbank und einer XML-Datenquelle enthalten. Für die Kommunikation mit den einzelnen Datenquellen sind ein oder mehrere `DataAdapter` -Objekte zuständig.  
  
### <a name="example"></a>Beispiel  

Im folgenden Codebeispiel wird eine Kundenliste von der `Northwind`-Datenbank unter Microsoft SQL Server sowie eine Auftragsliste von der in Microsoft Access gespeicherten `Northwind`-Datenbank gefüllt. Die gefüllten Tabellen werden mit einer `DataRelation`verbunden, und die Kundenliste wird anschließend mit Aufträgen für den jeweiligen Kunden angezeigt.

[!code-csharp[SqlDataAdapter_FillDataSet#2](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#2)]

## <a name="sql-server-decimal-type"></a>SQL Server-Typ decimal

Standardmäßig speichert die `DataSet`-Klasse Daten mithilfe von .NET-Datentypen. Für die meisten Anwendungen sind diese sehr gut zur Darstellung der Informationen aus der Datenquelle geeignet. Diese Darstellung kann jedoch problematisch werden, wenn es sich bei dem Datentyp in der Datenquelle um den SQL Server-Datentyp „decimal“ oder um einen numerischen Datentyp handelt. Der .NET-Datentyp `decimal` lässt maximal 28 signifikante Stellen zu, während der SQL Server-Datentyp `decimal` 38 signifikante Stellen zulässt. Wenn der `SqlDataAdapter` während eines `Fill` -Vorgangs feststellt, dass die Präzision eines SQL Server-Felds mit dem Datentyp `decimal` mehr als 28 Zeichen beträgt, wird der `DataTable`die aktuelle Zeile nicht hinzugefügt. Stattdessen tritt das `FillError` -Ereignis auf. Dadurch können Sie überprüfen, ob ein Präzisionsverlust eintritt, und Sie können entsprechend reagieren. Weitere Informationen zum `FillError`-Ereignis finden Sie unter [Umgang mit DataAdapter-Ereignissen](handle-dataadapter-events.md). Den SQL Server-Wert `decimal` erhalten Sie auch, indem Sie ein <xref:Microsoft.Data.SqlClient.SqlDataReader> -Objekt verwenden und die <xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A> -Methode aufrufen.

ADO.NET enthält auch erweiterte Unterstützung für <xref:System.Data.SqlTypes> in der `DataSet`-Klasse. Weitere Informationen finden Sie unter [SqlTypes and the DataSet](./sql/sqltypes-dataset.md).

## <a name="see-also"></a>Weitere Informationen

- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md)
- [Mehrere aktive Resultsets (MARS)](./sql/multiple-active-result-sets-mars.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
