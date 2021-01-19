---
description: Erstellen von Fremdschlüssel-Beziehungen
title: Erstellen von Fremdschlüssel-Beziehungen | Microsoft Dokumentation
ms.custom: ''
ms.date: 06/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], creating
ms.assetid: 867a54b8-5be4-46e6-9702-49ae6dabf67c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 933532a516f18aab01e1a32584590d65940c75b6
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170202"
---
# <a name="create-foreign-key-relationships"></a>Erstellen von Fremdschlüssel-Beziehungen


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

In diesem Artikel wird beschrieben, wie Fremdschlüsselbeziehungen in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]erstellt werden. Sie erstellen eine Beziehung zwischen zwei Tabellen, wenn Sie die Zeilen der einen Tabelle mit den Zeilen der anderen Tabelle verknüpfen möchten.

## <a name="permissions"></a>Berechtigungen

Zum Erstellen einer neuen Tabelle mit einem Fremdschlüssel sind die [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)-Berechtigung für die Datenbank und die [ALTER](../../t-sql/statements/alter-schema-transact-sql.md)-Berechtigung für das Schema erforderlich, in dem die Tabelle erstellt wird.

Zum Erstellen eines Fremdschlüssels für eine vorhandene Tabelle ist die [ALTER](../../t-sql/statements/alter-table-transact-sql.md)-Berechtigung für die Tabelle erforderlich.

## <a name="limits-and-restrictions"></a><a name="BeforeYouBegin"></a> Einschränkungen

- Eine FOREIGN KEY-Einschränkung muss nicht nur mit einer PRIMARY KEY-Einschränkung einer anderen Tabelle verknüpft sein. Fremdschlüssel können auch so definiert werden, dass sie auf die Spalten einer UNIQUE-Einschränkung in einer anderen Tabelle verweisen.
- Wenn ein anderer Wert als NULL in die Spalte einer FOREIGN KEY-Einschränkung eingegeben wird, muss der Wert in der Spalte vorhanden sein, auf die verwiesen wird. Andernfalls wird eine Fehlermeldung zu einem Fremdschlüsselverstoß zurückgegeben. Um sicherzustellen, dass alle Werte einer zusammengesetzten FOREIGN KEY-Einschränkung überprüft werden, legen Sie NOT NULL für alle betroffenen Spalten fest.
- FOREIGN KEY-Einschränkungen können nur auf Tabellen verweisen, die sich innerhalb derselben Datenbank auf demselben Server befinden. Datenbankübergreifende referenzielle Integrität muss durch Trigger implementiert werden. Weitere Informationen finden Sie unter [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- FOREIGN KEY-Einschränkungen können auf eine andere Spalte in derselben Tabelle verweisen. Dies wird als Eigenverweis bezeichnet.
- Eine auf Spaltenebene angegebene FOREIGN KEY-Einschränkung kann nur eine Verweisspalte auflisten. Diese Spalte muss denselben Datentyp aufweisen wie die Spalte, für die die Einschränkung definiert wurde.
- Eine auf Tabellenebene angegebene FOREIGN KEY-Einschränkung muss ebenso viele Verweisspalten haben, wie sich Spalten in der Einschränkungsspaltenliste befinden. Der Datentyp jeder Verweisspalte muss ebenfalls mit dem der entsprechenden Spalte in der Spaltenliste übereinstimmen.
- Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] weist keine vordefinierte Begrenzung für die Anzahl von FOREIGN KEY-Einschränkungen auf, die eine Tabelle enthalten kann, die auf andere Tabellen verweist. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] begrenzt außerdem auch nicht die Anzahl von FOREIGN KEY-Einschränkungen im Besitz anderer Tabellen, die auf eine bestimmte Tabelle verweisen. Die tatsächlich verwendete Anzahl von FOREIGN KEY-Einschränkungen wird jedoch durch die Hardwarekonfiguration und den Entwurf der Datenbank und der Anwendung begrenzt. Eine Tabelle kann auf maximal 253 andere Tabellen und Spalten als Fremdschlüssel (ausgehende Referenzen) verweisen. In [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher wird der Grenzwert für die Anzahl der anderen Tabellen und Spalten, die auf Spalten in einer einzelnen Tabelle verweisen können (eingehende Verweise), von 253 auf 10.000 erhöht. (Kompatibilitätsgrad 130 oder höher erforderlich.) Für die Erhöhung gelten folgende Einschränkungen:

  - Mehr als 253 Fremdschlüsselverweise werden nur für DELETE- und UPDATE- DML-Vorgänge unterstützt. MERGE-Vorgänge werden nicht unterstützt.
  - Auch eine Tabelle mit einem Fremdschlüsselverweis auf sich selbst ist auf 253 Fremdschlüsselverweise beschränkt.
  - Für Columnstore-Indizes, speicheroptimierte Tabellen oder Stretch Database sind derzeit nicht mehr als 253 Fremdschlüsselverweise möglich.

- FOREIGN KEY-Einschränkungen werden nicht für temporäre Tabellen erzwungen.
- Wenn ein Fremdschlüssel für eine Spalte eines CLR-benutzerdefinierten Typs definiert wird, muss die Implementierung des Typs eine binäre Sortierreihenfolge unterstützen. Weitere Informationen finden Sie unter [Benutzerdefinierte CLR-Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Eine Spalte vom Typ **varchar(max)** kann nur dann in eine FOREIGN KEY-Einschränkung einbezogen werden, wenn der Primärschlüssel, auf den sie verweist, ebenfalls als Typ **varchar(max)** definiert ist.

## <a name="create-a-foreign-key-relationship-in-table-designer"></a>Erstellen einer Fremdschlüsselbeziehung im Tabellen-Designer

### <a name="using-sql-server-management-studio"></a>Verwendung von SQL Server Management Studio

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Tabelle, die sich auf der Fremdschlüsselseite der Beziehung befinden soll, und klicken Sie auf **Entwerfen**.

   Die Tabelle wird im [**Tabellen-Designer**](../../ssms/visual-db-tools/design-tables-visual-database-tools.md) geöffnet.
2. Klicken Sie im Menü **Tabellen-Designer** auf **Beziehungen**.
3. Klicken Sie im Dialogfeld **Fremdschlüsselbeziehungen** auf **Hinzufügen**.

   Die Beziehung wird in der Liste **Selected Relationship** (Ausgewählte Beziehung) mit einem vom System bereitgestellten Namen im Format FK_\<*tablename*>_\<*tablename*> angezeigt, wobei *tablename* dem Namen der Fremdschlüsseltabelle entspricht.
4. Klicken Sie in der Liste **Ausgewählte Beziehung** auf die Beziehung.
5. Klicken Sie im Datenblatt rechts auf **Tabellen- und Spaltenspezifikation**, und klicken Sie anschließend auf die rechts neben der Eigenschaft angezeigten Auslassungspunkte ( **...** ).
6. Wählen Sie im Dialogfeld **Tabellen und Spalten** in der Dropdownliste **Primärschlüssel** die Tabelle aus, die sich auf der Primärschlüsselseite der Beziehung befinden soll.
7. Wählen Sie im darunter angezeigten Datenblatt die Spalten aus, die für den Primärschlüssel der Tabelle verwendet werden sollen. Geben Sie in die jeweils rechts neben den einzelnen Spalten angrenzende Rasterzelle die entsprechende Fremdschlüsselspalte aus der Fremdschlüsseltabelle ein.

   Der **Tabellen-Designer** schlägt einen Namen für die Beziehung vor. Wenn Sie diesen Namen ändern möchten, bearbeiten Sie den Inhalt des Textfelds **Beziehungsname** .
8. Klicken Sie auf **OK** , um die Beziehung zu erstellen.
9. Schließen Sie das Fenster mit dem Tabellen-Designer, und **speichern Sie** Ihre Änderungen, damit die Änderung der Fremdschlüsselbeziehung wirksam wird.

## <a name="create-a-foreign-key-in-a-new-table"></a>Erstellen eines Fremdschlüssels in einer neuen Tabelle

### <a name="using-transact-sql"></a>Verwenden von Transact-SQL

Im folgenden Beispiel wird eine Tabelle erstellt und eine Fremdschlüsseleinschränkung für die Spalte `TempID` definiert, die auf die Spalte `SalesReasonID` in der Tabelle `Sales.SalesReason` in der AdventureWorks-Datenbank verweist. Die Klauseln ON DELETE CASCADE und ON UPDATE CASCADE werden verwendet, um sicherzustellen, dass an der `Sales.SalesReason` -Tabelle vorgenommene Änderungen automatisch an die Tabelle `Sales.TempSalesReason` weitergegeben werden.    

```sql
CREATE TABLE Sales.TempSalesReason 
   (
      TempID int NOT NULL, Name nvarchar(50)
      , CONSTRAINT PK_TempSales PRIMARY KEY NONCLUSTERED (TempID)
      , CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
        REFERENCES Sales.SalesReason (SalesReasonID)
        ON DELETE CASCADE
        ON UPDATE CASCADE
   )
;
```

## <a name="create-a-foreign-key-in-an-existing-table"></a>Erstellen eines Fremdschlüssels in einer vorhandenen Tabelle

### <a name="using-transact-sql"></a>Verwenden von Transact-SQL
Im folgenden Beispiel wird ein Fremdschlüssel für die Spalte `TempID` erstellt und auf die Spalte `SalesReasonID` in der Tabelle `Sales.SalesReason` in der AdventureWorks-Datenbank verwiesen.

```sql
ALTER TABLE Sales.TempSalesReason
   ADD CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
      REFERENCES Sales.SalesReason (SalesReasonID)
      ON DELETE CASCADE
      ON UPDATE CASCADE
;
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

- [Primärschlüssel- und Fremdschlüsseleinschränkungen](primary-and-foreign-key-constraints.md)
- [GRANT – Datenbankberechtigungen](../../t-sql/statements/grant-database-permissions-transact-sql.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md).
