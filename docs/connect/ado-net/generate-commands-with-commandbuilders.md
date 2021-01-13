---
title: Generieren von Befehlen mit CommandBuilder-Objekten
description: Erläutert, wie Sie CommandBuilder-Objekte verwenden, um automatisch INSERT-, UPDATE- und DELETE-Befehle für einen `DataAdapter` zu generieren, der über einen SELECT-Befehl für eine einzelne Tabelle verfügt.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 6e3fb8b5-373b-4f9e-ab03-a22693df8e91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d738650db9dbb7b0b2ed3f9411945173f73f748f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771305"
---
# <a name="generating-commands-with-commandbuilders"></a>Generieren von Befehlen mit CommandBuilder-Objekten

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Wenn die `SelectCommand`-Eigenschaft des <xref:System.Data.Common.DbDataAdapter>-Objekts zur Laufzeit dynamisch angegeben wird (z. B. über ein Abfragetool, das einen Textbefehl des Benutzers ausführt), können Sie die jeweiligen `InsertCommand`-, `UpdateCommand`- oder `DeleteCommand`-Befehle möglicherweise nicht zur Entwurfszeit angeben. Wenn Ihre <xref:System.Data.DataTable> einer einzelnen Datenbanktabelle zugeordnet ist oder aus einer solchen generiert wurde, können Sie mithilfe des <xref:System.Data.Common.DbCommandBuilder>-Objekts automatisch den `DeleteCommand`, den `InsertCommand` und den `UpdateCommand` des <xref:System.Data.Common.DbDataAdapter> generieren.

> [!NOTE]
> Im Microsoft SqlClient-Datenanbieter für SQL Server wird die Klasse <xref:Microsoft.Data.SqlClient.SqlDataAdapter> von der Klasse <xref:System.Data.Common.DbDataAdapter> und die Klasse <xref:Microsoft.Data.SqlClient.SqlCommandBuilder> von der Klasse <xref:System.Data.Common.DbCommandBuilder> abgeleitet.

Damit die automatische Befehlsgenerierung funktioniert, muss mindestens die `SelectCommand`-Eigenschaft festgelegt werden. Das durch die `SelectCommand`-Eigenschaft abgerufene Tabellenschema bestimmt die Syntax der automatisch generierten INSERT-, UPDATE- und DELETE-Anweisungen.

Der <xref:System.Data.Common.DbCommandBuilder> muss `SelectCommand` ausführen, damit die zum Konstruieren der SQL-Befehle INSERT, UPDATE und DELETE erforderlichen Metadaten zurückgegeben werden. Daher ist ein erneutes Lesen der Datenquelle erforderlich, was die Leistung herabsetzen kann. Eine optimale Leistung lässt sich erzielen, indem Sie die Befehle direkt angeben, anstatt den <xref:System.Data.Common.DbCommandBuilder> zu verwenden.

> [!NOTE]
> Der `SelectCommand` muss außerdem mindestens einen Primärschlüssel oder eine eindeutige Spalte zurückgeben. Falls kein Schlüssel bzw. keine Spalte vorhanden ist, wird anstelle der Befehle eine `InvalidOperation`-Ausnahme generiert.

Bei einer Zuordnung zu einem `DataAdapter` generiert der <xref:System.Data.Common.DbCommandBuilder> automatisch die Eigenschaften `InsertCommand`, `UpdateCommand` und `DeleteCommand` des `DataAdapter`, sofern es sich um NULL-Verweise handelt. Wenn für eine Eigenschaft bereits ein `Command` vorhanden ist, wird der vorhandene `Command` verwendet.

Datenbankansichten, die durch das Verknüpfen von zwei oder mehr Datenbanken erstellt wurden, werden nicht als eine einzelne Datenbanktabelle betrachtet. Bei dieser Instanz kann <xref:System.Data.Common.DbCommandBuilder> nicht verwendet werden, um Befehle automatisch zu generieren. Die Befehle müssen explizit angegeben werden. Informationen, wie Sie explizit Befehle zum Aktualisieren der Datenquelle mit `DataSet`-Änderungen festlegen können, finden Sie unter [Aktualisieren von Datenquellen mit DataAdapters](update-data-sources-with-dataadapters.md).

Unter Umständen möchten Sie der aktualisierten Zeile eines `DataSet` erneut Ausgabeparameter zuordnen. Eine allgemeine Aufgabe wäre das Abrufen des Werts eines automatisch generierten Identitätsfelds oder Timestamps aus der Datenquelle. Standardmäßig werden den Spalten in einer aktualisierten Zeile vom <xref:System.Data.Common.DbCommandBuilder> keine Ausgabeparameter zugeordnet. Bei dieser Instanz muss der Befehl explizit angegeben werden. Ein Beispiel für das erneute Zuordnen eines automatisch generierten Identitätsfelds zu einer Spalte einer eingefügten Zeile finden Sie unter [Abrufen von Identity- oder Autonumber-Werten](retrieve-identity-or-autonumber-values.md).

## <a name="rules-for-automatically-generated-commands"></a>Regeln für automatisch generierte Befehle

Die folgende Tabelle enthält die Regeln für das Generieren von automatisch generierten Befehlen.

|Get-Help|Regel|  
|-------------|----------|  
|`InsertCommand`|Fügt in der Datenquelle für alle Zeilen in der Tabelle, die den <xref:System.Data.DataRow.RowState%2A><xref:System.Data.DataRowState.Added> aufweisen, eine Zeile ein. Fügt Werte für alle Spalten ein, die aktualisiert werden können (jedoch nicht für Spalten wie Identitäten, Ausdrücke oder Timestamps).|  
|`UpdateCommand`|Aktualisiert Zeilen in der Datenquelle für alle Zeilen in der Tabelle, die den `RowState`-Wert <xref:System.Data.DataRowState.Modified> aufweisen. Aktualisiert die Werte aller Spalten mit Ausnahme von Spalten, die nicht aktualisiert werden können, z. B. Identitäten oder Ausdrücke. Aktualisiert alle Zeilen, deren Spaltenwerte in der Datenquelle den Primärschlüssel-Spaltenwerten der Zeile entsprechen und bei denen die übrigen Spalten in der Datenquelle den ursprünglichen Werten der Zeile entsprechen. Weitere Informationen finden Sie unter "Vollständiges Parallelitätsmodell für Updates und Löschvorgänge" weiter unten in diesem Thema.|  
|`DeleteCommand`|Löscht Zeilen in der Datenquelle für alle Zeilen in der Tabelle, die den `RowState`-Wert <xref:System.Data.DataRowState.Deleted> aufweisen. Löscht alle Zeilen, deren Spaltenwerte den Primärschlüssel-Spaltenwerten der Zeile entsprechen und bei denen die übrigen Spalten in der Datenquelle den ursprünglichen Werten der Zeile entsprechen. Weitere Informationen finden Sie unter [Optimistisches Parallelitätsmodell für Updates und Löschvorgänge](#optimistic-concurrency-model-for-updates-and-deletes) weiter unten in diesem Thema.|

## <a name="optimistic-concurrency-model-for-updates-and-deletes"></a>Optimistisches Parallelitätsmodell für Updates und Löschvorgänge

Die Logik beim automatischen Generieren von Befehlen für UPDATE- und DELETE-Anweisungen basiert auf der *optimistischen Parallelität*, d. h., die Datensätze werden für die Bearbeitung nicht gesperrt und können von anderen Benutzern oder Prozessen jederzeit geändert werden. Da ein Datensatz nach der Rückgabe aus der SELECT-Anweisung und vor der Ausführung der UPDATE- oder DELETE-Anweisung ggf. geändert wurde, enthält die automatisch generierte UPDATE- oder DELETE-Anweisung eine WHERE-Klausel, die angibt, dass eine Zeile nur dann aktualisiert wird, wenn sie alle ursprünglichen Werte enthält und nicht aus der Datenquelle gelöscht wurde. Hierdurch wird vermieden, dass neue Daten überschrieben werden.
 
> [!NOTE]
> In Fällen, in denen ein automatisch generierter Updatebefehl versucht, eine Zeile zu aktualisieren, die gelöscht wurde oder nicht die ursprünglichen Werte im <xref:System.Data.DataSet> enthält, hat der Befehl keine Auswirkungen auf Datensätze, und es wird eine <xref:System.Data.DBConcurrencyException> ausgelöst.

Wenn UPDATE oder DELETE unabhängig von den ursprünglichen Werten ausgeführt werden sollen, muss der `UpdateCommand` für den `DataAdapter` explizit festgelegt und die automatische Befehlsgenerierung damit außer Kraft gesetzt werden.

## <a name="limitations-of-automatic-command-generation-logic"></a>Einschränkungen der Logik für das automatische Generieren von Befehlen

Folgende Einschränkungen gelten für die automatische Generierung von Befehlen.

### <a name="unrelated-tables-only"></a>Nur nicht verknüpfte Tabellen

Die Logik für die automatische Generierung von Befehlen generiert INSERT-, UPDATE- oder DELETE-Befehle für eigenständige Tabellen, wobei Verknüpfungen mit anderen Tabellen in der Datenquelle unberücksichtigt bleiben. Daher kann ein Fehler auftreten, wenn Sie `Update` aufrufen, um Änderungen an einer Spalte zu übermitteln, die in der Datenbank Fremdschlüsseleinschränkungen unterworfen ist. Sie können diese Ausnahme vermeiden, indem Sie zum Aktualisieren von Spalten, die einer Einschränkung eines Fremdschlüssels unterworfen sind, nicht den <xref:System.Data.Common.DbCommandBuilder> verwenden, sondern die Anweisungen zur Durchführung der Operation explizit angeben.

### <a name="table-and-column-names"></a>Tabellen- und Spaltennamen

Die Logik für die automatische Befehlsgenerierung kann fehlschlagen, wenn Spalten- oder Tabellennamen Sonderzeichen wie Leerzeichen, Punkte, Anführungszeichen oder andere nicht alphanumerische Zeichen enthalten, selbst wenn diese durch Klammern getrennt sind. Leerzeichen können je nach Anbieter von der Generierungslogik durch Festlegen des QuotePrefix-Parameters und QuoteSuffix-Parameters möglicherweise verarbeitet werden, Sonderzeichen können jedoch nicht mit Escapezeichen versehen werden. Vollqualifizierte Tabellennamen der Form *catalog.schema.table* werden unterstützt.

## <a name="use-the-commandbuilder-to-automatically-generate-an-sql-statement"></a>Verwenden der CommandBuilder-Objekte zum automatischen Generieren einer SQL-Anweisung

Legen Sie zum automatischen Generieren von SQL-Anweisungen für einen `DataAdapter` zuerst die `SelectCommand`-Eigenschaft des `DataAdapter` fest. Erstellen Sie dann ein `CommandBuilder`-Objekt, und geben Sie als Argument den `DataAdapter` an, für den der `CommandBuilder` automatisch SQL-Anweisungen generieren soll.

[!code-csharp[SqlCommandBuilder_Create#1](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#1)]

## <a name="modify-the-selectcommand"></a>Ändern von SelectCommand

Wenn Sie den `CommandText` von `SelectCommand` ändern, nachdem die Befehle INSERT, UPDATE oder DELETE automatisch generiert wurden, kann eine Ausnahme auftreten. Wenn der geänderte `SelectCommand.CommandText` Schemainformationen enthält, die nicht mit dem `SelectCommand.CommandText` zum Zeitpunkt der automatischen Generierung der Befehle INSERT, UPDATE oder DELETE konsistent sind, wird bei späteren Aufrufen der `DataAdapter.Update`-Methode möglicherweise versucht, auf Spalten zuzugreifen, die nicht mehr in der aktuellen Tabelle vorhanden sind, auf die `SelectCommand` verweist. In diesem Fall wird eine Ausnahme ausgelöst.

Sie können die vom `CommandBuilder` zum automatischen Generieren von Befehlen verwendeten Schemainformationen aktualisieren, indem Sie die `RefreshSchema`-Methode des `CommandBuilder` aufrufen.

Wenn Sie wissen möchten, welcher Befehl automatisch generiert wurde, können Sie mit den Methoden `GetInsertCommand`, `GetUpdateCommand` und `GetDeleteCommand` des `CommandBuilder`-Objekts Verweise auf die Befehle abrufen und die `CommandText`-Eigenschaft des zugeordneten Befehls überprüfen.

Im folgenden Codebeispiel wird der automatisch generierte UPDATE-Befehl in die Konsole geschrieben.

[!code-csharp[SqlCommandBuilder_Create#2](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#2)]

Im folgenden Beispiel wird die Tabelle im Dataset neu erstellt. Die **RefreshSchema**-Methode wird zum Aktualisieren der automatisch generierten Befehle mit den neuen Spalteninformationen aufgerufen.

[!code-csharp[SqlCommandBuilder_Create#3](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#3)]

## <a name="see-also"></a>Weitere Informationen:

- [Befehle und Parameter](commands-parameters.md)
- [Ausführen eines Befehls](execute-command.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
