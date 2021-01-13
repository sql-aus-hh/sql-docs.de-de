---
title: DataAdapter-Parameter
description: Hier erfahren Sie mehr über die Eigenschaften der DbDataAdapter-Klasse, die Daten aus einer Datenquelle zurückgeben und Änderungen an der Datenquelle verwalten.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 90985f155392a78da408ffbda3e1d2c0b12d4648
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771602"
---
# <a name="dataadapter-parameters"></a>DataAdapter-Parameter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Der <xref:System.Data.Common.DbDataAdapter> verfügt über vier Eigenschaften, die zum Abrufen von Daten aus einer Datenquelle und zum Aktualisieren der Daten in der Datenquelle verwendet werden: die <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A>-Eigenschaft gibt die Daten aus der Datenquelle zurück, und die Eigenschaften <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> und <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> dienen zum Verwalten von Änderungen in der Datenquelle.

> [!NOTE]
> Die `SelectCommand`-Eigenschaft muss vor dem Aufrufen der `Fill`-Methode des `DataAdapter` festgelegt werden. Die Eigenschaft `InsertCommand`, `UpdateCommand` oder `DeleteCommand` (abhängig von der Art der Änderungen in der `Update`) muss vor dem Aufruf der `DataAdapter`-Methode des <xref:System.Data.DataTable> festgelegt werden. Wenn beispielsweise Zeilen hinzugefügt wurden, muss vor dem Aufrufen von `InsertCommand` das `Update` festgelegt werden. Wenn `Update` eine eingefügte, aktualisierte oder gelöschte Zeile verarbeitet, verwendet der `DataAdapter` die entsprechende `Command`-Eigenschaft, um die Aktion zu verarbeiten. Aktuelle Informationen zur geänderten Zeile werden über die `Command`-Auflistung an das `Parameters`-Objekt übergeben.

Wenn Sie eine Zeile in der Datenquelle aktualisieren, rufen Sie die UPDATE-Anweisung auf, die einen eindeutigen Bezeichner verwendet, um die Zeile in der zu aktualisierenden Tabelle zu identifizieren. Der eindeutige Bezeichner ist im Allgemeinen der Wert eines Primärschlüsselfelds. Die UPDATE-Anweisung verwendet Parameter, die sowohl den eindeutigen Bezeichner als auch die Spalten und Werte enthalten, die aktualisiert werden sollen, wie in der folgenden Transact-SQL-Anweisung dargestellt.

```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  

> [!NOTE]
> Die Syntax für Parameterplatzhalter ist abhängig von der jeweiligen Datenquelle. Dieses Beispiel zeigt Platzhalter für eine SQL Server-Datenquelle.

In diesem Beispiel wird das `CompanyName`-Feld mit dem Wert des `@CompanyName`-Parameters für die Zeile aktualisiert, in der `CustomerID` dem Wert des `@CustomerID`-Parameters entspricht. Die Parameter rufen mithilfe der <xref:Microsoft.Data.SqlClient.SqlParameter.SourceColumn%2A>-Eigenschaft des <xref:Microsoft.Data.SqlClient.SqlParameter>-Objekts Informationen aus der geänderten Zeile ab. Im Folgenden finden Sie die Parameter für das vorherige Beispiel einer UPDATE-Anweisung. Der Code geht davon aus, dass die `adapter`-Variable für ein gültiges <xref:Microsoft.Data.SqlClient.SqlDataAdapter>-Objekt steht.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#2](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#2)]

Die `Add`-Methode der `Parameters`-Auflistung nimmt den Namen des Parameters, den Datentyp, die Größe (wenn für den Typ zutreffend) und den Namen der <xref:System.Data.Common.DbParameter.SourceColumn%2A> aus der `DataTable` an. Beachten Sie, dass der <xref:System.Data.Common.DbParameter.SourceVersion%2A>-Wert des `@CustomerID`-Parameters `Original` lautet. Dadurch wird sichergestellt, dass die vorhandene Zeile in der Datenquelle aktualisiert wird, wenn sich der Wert der ID-Spalte oder -Spalten in der geänderten <xref:System.Data.DataRow> geändert hat. In diesem Fall stimmt der `Original`-Zeilenwert mit dem aktuellen Wert in der Datenquelle überein, und der `Current`-Zeilenwert enthält den aktualisierten Wert. Die `SourceVersion` für den `@CompanyName`-Parameter ist nicht festgelegt, und es wird der Standardwert, nämlich der `Current`-Zeilenwert, verwendet.

> [!NOTE]
> Sowohl für die `Fill`-Vorgänge der `DataAdapter`-Klasse als auch für die `Get`-Methoden von `DataReader` wird der .NET-Typ von dem Typ abgeleitet, der vom Microsoft SqlClient-Datenanbieter für SQL Server zurückgegeben wird. Die abgeleiteten .NET-Typen und Accessormethoden für Microsoft SQL Server-Datentypen werden unter [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md) erläutert.

## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn, Parameter.SourceVersion

Die `SourceColumn` und die `SourceVersion` können als Argumente an den `Parameter`-Konstruktor übergeben oder als Eigenschaften eines vorhandenen `Parameter` festgelegt werden. Die `SourceColumn` ist der Name der <xref:System.Data.DataColumn> aus der <xref:System.Data.DataRow>, aus der der Wert des `Parameter` abgerufen wird. Die `SourceVersion` gibt die `DataRow`-Version an, die der `DataAdapter` zum Abrufen des Werts verwendet.

Die folgende Tabelle zeigt die <xref:System.Data.DataRowVersion>-Enumerationswerte an, die für die Verwendung mit `SourceVersion` zur Verfügung stehen.

|DataRowVersion-Enumeration|BESCHREIBUNG|  
|--------------------------------|-----------------|  
|`Current`|Der Parameter verwendet den aktuellen Wert der Spalte. Dies ist die Standardoption.|  
|`Default`|Der Parameter verwendet den `DefaultValue` der Spalte.|  
|`Original`|Der Parameter verwendet den ursprünglichen Wert der Spalte.|  
|`Proposed`|Der Parameter verwendet einen vorgeschlagenen Wert.|  

Das `SqlClient`-Codebeispiel im nächsten Abschnitt definiert einen Parameter für einen <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A>, bei dem die `CustomerID`-Spalte als `SourceColumn` für die folgenden beiden Parameter verwendet wird: `@CustomerID` (`SET CustomerID = @CustomerID`) und `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`). Der `@CustomerID`-Parameter wird zum Aktualisieren der **CustomerID**-Spalte auf den aktuellen Wert in der `DataRow`-Klasse verwendet. Folglich wird die `SourceColumn`-Eigenschaft für `CustomerID` mit `Current` für `SourceVersion` verwendet. Der `@OldCustomerID`-Parameter wird dafür verwendet, die aktuelle Zeile in der Datenquelle anzugeben. Da sich der passende Spaltenwert in der `Original`-Version der Zeile befindet, wird die gleiche `SourceColumn` (`CustomerID`) mit einer `SourceVersion` von `Original` verwendet.

## <a name="work-with-sqlclient-parameters"></a>Arbeiten mit SqlClient-Parametern

Im folgenden Beispiel wird gezeigt, wie Sie einen <xref:Microsoft.Data.SqlClient.SqlDataAdapter> erstellen und für die <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A><xref:System.Data.MissingSchemaAction.AddWithKey> festlegen können, um zusätzliche Schemainformationen aus der Datenbank abzurufen. Die Eigenschaften <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> und <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> werden festgelegt, und die zugehörigen <xref:Microsoft.Data.SqlClient.SqlParameter>-Objekte werden der <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A>-Auflistung hinzugefügt. Die Methode gibt ein `SqlDataAdapter`-Objekt zurück.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#1](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#1)]

## <a name="see-also"></a>Weitere Informationen

- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Befehle und Parameter](commands-parameters.md)
- [Aktualisieren von Datenquellen mit DataAdapters](update-data-sources-with-dataadapters.md)
- [Ändern von Daten mit gespeicherten Prozeduren](modify-data-with-stored-procedures.md)
- [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
