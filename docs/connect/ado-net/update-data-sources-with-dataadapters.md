---
title: Aktualisieren von Datenquellen mit DataAdapters
description: Hier erfahren Sie, wie die Update-Methode der DataAdapter-Klasse in ADO.NET-Anwendungen Änderungen von einer DataSet-Klasse zurück in die Datenquelle auflöst.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6f2feb876d0f232f4d7951de8ee1cc84587e6486
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771355"
---
# <a name="update-data-sources-with-dataadapters"></a>Aktualisieren von Datenquellen mit DataAdapters

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Zum Aktualisieren von Datenquellen mit den Änderungen, die an einem `Update` vorgenommen wurden, wird die <xref:System.Data.Common.DataAdapter>-Methode des <xref:System.Data.DataSet> aufgerufen. Als Argumente akzeptiert die `Update`-Methode, genau wie die `Fill`-Methode, eine Instanz eines `DataSet` sowie ein optionales <xref:System.Data.DataTable>-Objekt oder einen `DataTable`-Namen. Die `DataSet`-Instanz ist das `DataSet`, das die vorgenommenen Änderungen enthält, und der `DataTable`-Wert gibt die Tabelle an, aus der die Änderungen abgerufen werden sollen. Wenn keine `DataTable` angegeben ist, wird die erste `DataTable` im `DataSet` verwendet.

Wenn die `Update`-Methode aufgerufen wird, analysiert der `DataAdapter` die vorgenommenen Änderungen und führt dann den entsprechenden Befehl (INSERT, UPDATE oder DELETE) aus. Wenn der `DataAdapter` eine Änderung an einer <xref:System.Data.DataRow> feststellt, verwendet er zum Verarbeiten der Änderung den <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, den <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> oder den <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A>.

Diese Eigenschaften ermöglichen es Ihnen, die Leistung Ihrer ADO.NET-Anwendung zu maximieren, indem Sie die Befehlssyntax zur Entwurfszeit festlegen und nach Möglichkeit gespeicherte Prozeduren verwenden. Sie müssen die Befehle vor dem Aufrufen von `Update` explizit festlegen. Wenn `Update` aufgerufen wird und der entsprechende Befehl für ein bestimmtes Update nicht vorhanden ist (wenn z. B. `DeleteCommand` für gelöschte Zeilen fehlt), wird eine Ausnahme ausgelöst.

> [!IMPORTANT]
> Wenn Sie gespeicherte SQL Server-Prozeduren verwenden, um Daten mithilfe einer `DataAdapter`-Klasse zu bearbeiten oder zu löschen, stellen Sie sicher, dass Sie in der Definition der gespeicherten Prozedur nicht `SET NOCOUNT ON` verwenden. Anderenfalls ist die zurückgegebene Anzahl der betroffenen Zeilen gleich Null (0), was der `DataAdapter` als Parallelitätskonflikt interpretiert. In diesem Fall wird eine <xref:System.Data.DBConcurrencyException> ausgelöst.

Command-Parameter können verwendet werden, um Ein- und Ausgabewerte für eine SQL-Anweisung oder eine gespeicherte Prozedur für jede geänderte Zeile in einem `DataSet` anzugeben. Weitere Informationen finden Sie unter [DataAdapter-Parameter](dataadapter-parameters.md).

> [!NOTE]
> Wichtig ist dabei, zwischen dem Löschen einer Zeile in einer <xref:System.Data.DataTable> und dem Entfernen der Zeile zu unterscheiden. Wenn Sie die `Remove`-Methode oder die `RemoveAt`-Methode aufrufen, wird die Zeile sofort entfernt. Die entsprechenden Zeilen in der Back-End-Datenquelle **sind nicht betroffen**, wenn Sie anschließend die `DataTable`- oder `DataSet`-Klasse an eine `DataAdapter`-Klasse übergeben und `Update`aufrufen. Wenn Sie die Methode `Delete` verwenden, bleibt die Zeile in der `DataTable` erhalten, wird aber als zu löschen markiert. Wenn Sie dann die `DataTable`- oder `DataSet`-Klasse an eine `DataAdapter`-Klasse übergeben und `Update` aufrufen, wird die entsprechende Zeile in der Back-End-Datenquelle **gelöscht**.

Wenn Ihre `DataTable` einer einzelnen Datenbanktabelle zugeordnet ist oder aus einer einzelnen Datenbanktabelle generiert wurde, können Sie mithilfe des <xref:System.Data.Common.DbCommandBuilder>-Objekts automatisch das `DeleteCommand`-, das `InsertCommand`- und das `UpdateCommand`-Objekt für den `DataAdapter` generieren. Weitere Informationen finden Sie unter [Generieren von Befehlen mit CommandBuilder-Klassen](generate-commands-with-commandbuilders.md).

## <a name="use-updatedrowsource-to-map-values-to-a-dataset"></a>Verwenden von UpdatedRowSource für die Zuordnung von Werten zu einer DataSet-Klasse

Mithilfe der <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A>-Eigenschaft eines <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekts können Sie steuern, wie nach dem Aufrufen der <xref:System.Data.Common.DbDataAdapter.Update%2A>-Methode einer `DataAdapter`-Klasse die von der Datenquelle zurückgegebenen Werte wieder der `DataTable`-Klasse zugeordnet werden. Durch Festlegen eines der `UpdatedRowSource`-Enumerationswerte für die <xref:System.Data.UpdateRowSource>-Eigenschaft kann gesteuert werden, ob die von den `DataAdapter`-Befehlen zurückgegebenen Ausgabeparameter ignoriert oder auf die geänderte Zeile im `DataSet` angewendet werden. Es kann auch festgelegt werden, ob die erste zurückgegebene Zeile (wenn vorhanden) auf die geänderte Zeile in der `DataTable` angewendet wird.

In der folgenden Tabelle werden die verschiedenen Werte der `UpdateRowSource`-Enumeration und deren Auswirkungen auf das Verhalten eines mit einem `DataAdapter` verwendeten Befehls beschrieben.

|"UpdatedRowSource"-Enumeration|BESCHREIBUNG|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|Sowohl die Ausgabeparameter als auch die erste Zeile eines zurückgegebenen Resultset können der geänderten Zeile im `DataSet` zugeordnet werden.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Nur die Daten in der ersten Zeile eines zurückgegebenen Resultset können der geänderten Zeile im `DataSet` zugeordnet werden.|
|<xref:System.Data.UpdateRowSource.None>|Alle Ausgabeparameter oder Zeilen eines zurückgegebenen Resultset werden ignoriert.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Der geänderten Zeile im `DataSet` können nur Ausgabeparameter zugeordnet werden.|

Die `Update`-Methode aktualisiert die Datenquelle mit den vorgenommenen Änderungen. Die Daten in der Datenquelle können aber seit dem letzten Füllen des `DataSet` durch andere Clients geändert worden sein. Wenn Sie das `DataSet` mit den aktuellen Daten aktualisieren möchten, verwenden Sie den `DataAdapter` und die `Fill`-Methode. Der Tabelle werden neue Zeilen hinzugefügt, und aktualisierte Informationen werden in die vorhandenen Zeilen eingefügt.

Die `Fill`-Methode überprüft die Primärschlüsselwerte der Zeilen im `DataSet` und der vom `SelectCommand` zurückgegebenen Zeilen und bestimmt so, ob eine neue Zeile hinzugefügt oder die bestehende Zeile aktualisiert werden soll. Wenn die `Fill`-Methode einen Primärschlüsselwert für eine Zeile im `DataSet` findet, der mit einem Primärschlüsselwert einer Zeile in den vom `SelectCommand` zurückgegebenen Ergebnissen übereinstimmt, aktualisiert sie die vorhandene Zeile mit den Informationen aus der vom `SelectCommand` zurückgegebenen Zeile und legt den <xref:System.Data.DataRow.RowState%2A> der vorhandenen Zeile auf `Unchanged` fest. Wenn der Primärschlüsselwert einer vom `SelectCommand` zurückgegebenen Zeile keinem der Primärschlüsselwerte der Zeilen im `DataSet` entspricht, fügt die `Fill`-Methode eine neue Zeile mit dem `RowState``Unchanged` hinzu.

> [!NOTE]
> Wenn die `SelectCommand`-Eigenschaft die Ergebnisse eines **OUTER JOIN**-Vorgangs zurückgibt, legt die `DataAdapter`-Klasse für die resultierende `DataTable`-Klasse keinen `PrimaryKey`-Wert fest. Sie müssen den `PrimaryKey` selbst definieren, um sicherzustellen, dass doppelte Zeilen ordnungsgemäß aufgelöst werden.

Zur Behandlung von Ausnahmen, die beim Aufrufen der `Update`-Methode auftreten können, können Sie das `RowUpdated`-Ereignis verwenden, um auf beim Aktualisieren von Zeilen auftretende Fehler zu reagieren (siehe [Umgang mit DataAdapter-Ereignissen](handle-dataadapter-events.md)). Alternativ können Sie `true` für <xref:System.Data.Common.DataAdapter.ContinueUpdateOnError%2A> festlegen, bevor Sie `Update` aufrufen, und auf die in der `RowError`-Eigenschaft einer bestimmten Zeile gespeicherten Fehlerinformationen reagieren, wenn das Update abgeschlossen ist.

> [!NOTE]
> Wenn `AcceptChanges` für die `DataSet`-, `DataTable`- oder `DataRow`-Klasse aufgerufen wird, werden alle `Original`-Werte einer `DataRow`-Klasse mit den `Current`-Werten für die `DataRow`-Klasse überschrieben. Wenn die Feldwerte, mit denen die Zeile als eindeutig identifiziert wird, geändert wurden, stimmen die `AcceptChanges`-Werte nicht mehr mit den Werten in der Datenquelle überein, nachdem `Original` aufgerufen wurde. Während eines Aufrufs der `Update`-Methode einer `DataAdapter`-Klasse wird `AcceptChanges` automatisch für jede Zeile aufgerufen. Sie können die Originalwerte während eines Aufrufs der Update-Methode beibehalten, indem Sie zuerst die -Eigenschaft des  auf false setzen oder indem Sie einen Ereignishandler für das -Ereignis erstellen und den  auf  festlegen. Weitere Informationen finden Sie unter [Umgang mit DataAdapter-Ereignissen](handle-dataadapter-events.md).

Die folgenden Beispiele zeigen, wie geänderte Zeilen aktualisiert werden können, indem die `UpdateCommand`-Eigenschaft einer `DataAdapter`-Klasse explizit festgelegt und deren `Update`-Methode aufgerufen wird.

> [!NOTE]
> Der in der `WHERE clause` der `UPDATE statement` angegebene Parameter wird so festgelegt, dass der `Original`-Wert der `SourceColumn`-Eigenschaft verwendet wird. Dies ist wichtig, weil der `Current`-Wert möglicherweise geändert wurde und u. U. nicht mehr mit dem Wert in der Datenquelle übereinstimmt. Beim `Original`-Wert handelt es sich um den Wert, mit dem die `DataTable` aus der Datenquelle aufgefüllt wurde.

[!code-csharp[SqlDataAdapter_Update#1](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#1)]

## <a name="autoincrement-columns"></a>AutoIncrement-Spalten

Wenn die Tabellen aus der Datenquelle automatisch inkrementierende Spalten besitzen, können Sie die Spalten im `DataSet` füllen. Geben Sie dazu die automatisch inkrementierenden Werte als Ausgabeparameter einer gespeicherten Prozedur zurück, und ordnen Sie diesen Parameter einer Spalte in einer Tabelle zu, indem Sie den automatisch inkrementierenden Wert in der ersten Zeile eines von einer gespeicherten Prozedur oder einer SQL-Anweisung zurückgegebenen Resultset zurückgeben oder indem Sie das `RowUpdated`-Ereignis des `DataAdapter` verwenden, um eine weitere SELECT-Anweisung auszuführen. Weitere Informationen und ein Beispiel finden Sie unter [Abrufen von Identity- oder Autonumber-Werten](retrieve-identity-or-autonumber-values.md).

## <a name="ordering-of-inserts-updates-and-deletes"></a>Sortieren von Einfügungen, Updates und Löschungen

In vielen Fällen ist die Reihenfolge, in der die am `DataSet` vorgenommenen Änderungen zur Datenquelle gesendet werden, sehr wichtig. Wenn beispielsweise ein Primärschlüsselwert für eine vorhandene Zeile aktualisiert und eine neue Zeile mit dem neuen Primärschlüsselwert als Fremdschlüssel hinzugefügt wurde, muss das Update vor der Einfügung verarbeitet werden.

Mithilfe der `Select`-Methode der `DataTable` können Sie ein `DataRow`-Array zurückgeben, das nur auf Zeilen mit einem bestimmten `RowState` verweist. Anschließend können Sie das zurückgegebene `DataRow`-Array an die `Update`-Methode des `DataAdapter` übergeben, damit die geänderten Zeilen verarbeitet werden. Wenn Sie eine Teilmenge von Zeilen angeben, die aktualisiert werden sollen, können Sie die Reihenfolge steuern, in der Einfügungen, Updates und Löschvorgänge verarbeitet werden.

## <a name="example"></a>Beispiel

Durch den folgenden Code wird beispielsweise sichergestellt, dass die gelöschten Zeilen der Tabelle zuerst verarbeitet werden, anschließend die aktualisierten Zeilen und dann die eingefügten Zeilen.

[!code-csharp[SqlDataAdapter_Update#2](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#2)]

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>Verwenden einer DataAdapter-Klasse zum Abrufen und Aktualisieren von Daten

Sie können DataAdapter verwenden, um die Daten abzurufen und zu aktualisieren.

- Im Beispiel wird `DataAdapter.AcceptChangesDuringFill` verwendet, um die Daten in der Datenbank zu klonen. Wenn die Eigenschaft auf **false** festgelegt ist, wird **AcceptChanges** beim Ausfüllen der Tabelle nicht aufgerufen, und die neu hinzugefügten Zeilen werden als eingefügte Zeilen behandelt. Daher werden im Beispiel diese Zeilen zum Einfügen der neuen Zeilen in die Datenbank verwendet.

- Im Beispiel wird `DataAdapter.TableMappings` verwendet, um die Zuordnung zwischen der Quelltabelle und der DataTable-Klasse zu definieren.

- Darüber hinaus wird im Beispiel `DataAdapter.FillLoadOption` verwendet, um festzulegen, wie der Adapter die **DataTable**-Klasse aus der **DbDataReader**-Klasse füllt. Wenn Sie eine DataTable-Klasse erstellen, können Sie die Daten aus der Datenbank nur in die aktuelle Version oder die ursprüngliche Version schreiben, indem Sie die Eigenschaft auf **LoadOption.Upsert** oder **LoadOption.PreserveChanges** festlegen.

- Außerdem wird im Beispiel die Tabelle aktualisiert, indem `DbDataAdapter.UpdateBatchSize` zum Ausführen von Batchvorgängen verwendet wird.

Bevor Sie dieses Beispiel kompilieren und ausführen, müssen Sie die Beispieldatenbank erstellen:

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

[!code-csharp[SqlDataAdapter_Properties#1](~/../sqlclient/doc/samples/SqlDataAdapter_Properties.cs#1)]

## <a name="see-also"></a>Weitere Informationen

- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Abrufen von Identity- oder Autonumber-Werten](retrieve-identity-or-autonumber-values.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
