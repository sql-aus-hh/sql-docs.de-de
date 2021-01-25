---
title: Verwenden von Transact-SQL-Assertionen in SQL Server-Komponententests
description: In diesem Artikel erhalten Sie Informationen zu Transact-SQL-Assertionen. Sie erfahren außerdem, wann Assertionen in SQL Server-Komponententests verwendet werden und wann Testbedingungen verwendet werden. Zusätzlich können Sie sich Beispiele der Verwendung von Assertionen ansehen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 55d8be9c-9282-47d3-be7f-e2c26f00c95e
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f7b6eaf18166bd546e8eb3e89ee2176c39dfb49a
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564418"
---
# <a name="using-transact-sql-assertions-in-sql-server-unit-tests"></a>Verwenden von Transact-SQL-Assertionen in SQL Server-Komponententests

In einem SQL Server-Komponententest wird ein Transact\-SQL-Testskript ausgeführt, von dem ein Ergebnis zurückgegeben wird. In einigen Fällen werden die Ergebnisse als Resultset zurückgegeben. Sie können die Ergebnisse mithilfe von Testbedingungen überprüfen. Beispielsweise können Sie eine Testbedingung verwenden, um zu überprüfen, wie viele Zeilen in einem bestimmten Resultset zurückgegeben wurden, bzw. um zu ermitteln, wie lange ein bestimmter Test gedauert hat. Weitere Informationen zu Testbedingungen finden Sie unter [Verwenden von Testbedingungen in SQL Server-Komponententests](../ssdt/using-test-conditions-in-sql-server-unit-tests.md).  
  
Anstelle von Testbedingungen können Sie auch Transact\-SQL-Assertionen verwenden. Dabei handelt es sich um THROW- oder RAISERROR-Anweisungen in einem Transact\-SQL-Skript. Unter bestimmten Bedingungen ist eine Transact\-SQL-Assertion einer Testbedingung vorzuziehen.  
  
## <a name="using-transact-sql-assertions"></a>Verwenden von Transact-SQL-Assertionen  
Bevor Sie entscheiden, ob Sie Daten mithilfe von Transact\-SQL -Assertionen oder mithilfe von Testbedingungen überprüfen möchten, sollten Sie die folgenden Punkte überdenken.  
  
-   **Leistung**: Sie sparen Zeit, wenn Sie eine Transact\-SQL-Assertion auf dem Server ausführen, anstatt Daten zuerst auf einen Clientcomputer zu verschieben und lokal zu bearbeiten.  
  
-   **Vertraute Programmiersprache**: Abhängig von Ihrem Kenntnisstand ziehen Sie möglicherweise eine bestimmte Programmiersprache vor und geben deshalb Transact\-SQL-Assertionen bzw. Visual C\#- oder Visual Basic-Testbedingungen den Vorzug.  
  
-   **Komplexe Überprüfung**: In einigen Fällen können Sie in Visual C\# oder Visual Basic komplexere Tests erstellen und die Tests auf dem Client überprüfen.  
  
-   **Einfachheit**: Häufig ist es einfacher, eine vordefinierte Testbedingung zu verwenden, anstatt ein gleichwertiges Skript in Transact\-SQL zu schreiben.  
  
-   **Ältere Überprüfungsbibliotheken**: Wenn Sie bereits über Code verfügen, durch den eine Überprüfung ausgeführt wird, können Sie ihn in einen SQL Server-Komponententest übernehmen, anstatt Testbedingungen zu verwenden.  
  
## <a name="mark-unit-test-methods-with-the-expected-exception"></a>Kennzeichen von Komponententestmethoden mit der erwarteten Ausnahme  
Um eine SQL Server-Komponententestmethode mit erwarteten Ausnahmen zu kennzeichnen, fügen Sie das folgende Attribut hinzu:  
  
```vb  
<ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)> _  
```  
  
```csharp  
[ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)]  
```  
  
Hierbei gilt:  
  
-   *nnnnn* ist die Nummer der erwarteten Meldung, z. B. 14025.  
  
-   *x* ist der Schweregrad der erwarteten Ausnahme.  
  
-   *y* ist der Zustand der erwarteten Ausnahme.  
  
Nicht festgelegte Parameter werden ignoriert. Sie übergeben diese Parameter an die RAISERROR-Anweisung im Datenbankcode. Wenn Sie MatchFirstError = true angeben, entspricht das Attribut jedem SqlError in der Ausnahme. Das Standardverhalten („MatchFirstError = true“) besteht darin, dass nur der erste aufgetretene Fehler vom Attribut verglichen wird.  
  
Ein Beispiel dafür, wie erwartete Ausnahmen und ein negativer SQL Server-Komponententest verwendet werden, finden Sie unter [Exemplarische Vorgehensweise: Erstellen und Ausführen eines SQL Server-Komponententests](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="the-raiserror-statement"></a>Die RAISERROR-Anweisung  
  
> [!NOTE]  
> Verwenden Sie THROW anstelle von RAISERROR. RAISERROR ist mittlerweile veraltet.  
  
Sie können Transact\-SQL-Assertionen direkt auf dem Server verwenden, indem Sie die RAISERROR-Anweisung im Transact\-SQL-Skript ausführen. Die Syntax sieht wie folgt aus:  
  
**RAISERROR (\@ErrorMessage, \@ErrorSeverity, \@ErrorState)**  
  
Dabei gilt:  
  
@ErrorMessage steht für eine beliebige benutzerdefinierte Fehlermeldung. Sie können diese Meldungszeichenfolge ähnlich wie die printf_s-Funktion formatieren.  
  
@ErrorSeverity ist ein benutzerdefinierter Schweregrad zwischen 0 und 18.  
  
> [!NOTE]  
> Ein Schweregrad vom Wert „0“ und „10“ führt nicht dazu, dass beim SQL Server-Komponententest ein Fehler auftritt. Sie können einen beliebigen anderen Wert im Bereich von 0 bis 18 eingeben, der dazu führt, dass der Test fehlschlägt.  
  
@ErrorState ist eine willkürliche ganze Zahl zwischen 1 und 127. Sie können diese ganze Zahl verwenden, um Vorkommen eines einzelnen Fehlers zu unterscheiden, die an verschiedenen Stellen im Code vorkommen.  
  
Weitere Informationen finden Sie unter [RAISERROR (Transact-SQL)](../t-sql/language-elements/raiserror-transact-sql.md). Ein Beispiel zur Verwendung von RAISERROR in einem SQL Server-Komponententest finden Sie im Thema [Gewusst wie: Schreiben eines SQL Server-Komponententests, der im Gültigkeitsbereich einer einzelnen Transaktion ausgeführt wird](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Verwenden von Testbedingungen in SQL Server-Komponententests](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[Überprüfen des Datenbankcodes mithilfe von SQL Server-Komponententests](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Vorgehensweise: Öffnen eines SQL Server-Komponententests zur Bearbeitung](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)  
