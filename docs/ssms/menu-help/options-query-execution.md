---
title: 'SSMS-Optionsseite: Abfrageausführung'
description: SSMS-Optionen (Abfrageausführung)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Query_Execution.Sql_Server.General
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/13/2021
ms.openlocfilehash: 29ee1a365031eedae80abcffdb1147053d56f069
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241771"
---
# <a name="options-query-execution---general"></a>Optionen (Abfrageausführung allgemein)

Auf dieser Seite können Sie die Optionen zum Ausführen von Abfragen in Microsoft SQL Server angeben. Um auf dieses Dialogfeld zuzugreifen, klicken Sie mit der rechten Maustaste in den Text eines Abfrage-Editor-Fensters, und wählen Sie **Abfrageoptionen** aus. Sie können auch auf der oberen Menüleiste **Tools > Optionen > Abfrageausführung** auswählen.

- **SET ROWCOUNT** Der Standardwert 0 zeigt an, dass SQL Server so lange auf die Ergebnisse wartet, bis alle Ergebnisse übermittelt sind. Geben Sie einen Wert größer 0 an, wenn die Abfrage von SQL Server nach Übermittlung einer bestimmten Anzahl von Zeilen abgebrochen werden soll. Um diese Option zu deaktivieren (sodass alle Zeilen zurückgegeben werden), geben Sie „SET ROWCOUNT 0“ an.

- **SET TEXTSIZE** Der Standardwert von 2.147.483.647 Bytes zeigt an, dass SQL Server ein vollständiges Datenfeld bereitstellt, das maximal der Größe von text-, ntext-, nvarchar(max)- und varchar(max)-Datenfeldern entspricht. Dies hat keine Auswirkungen auf den XML-Datentyp. Geben Sie eine kleinere Zahl an, um bei großen Werten die Anzahl der Ergebnisse zu beschränken. Spalten, deren Größe die angegebene Zahl übersteigt, werden abgeschnitten.

- **Ausführungstimeout** Dieses Feld enthält die Zeit (in Sekunden), nach der die Abfrage abgebrochen wird. Der Wert '0' gibt an, dass der Wartevorgang unbegrenzt ist bzw. kein Timeout erfolgt.

- **Batch separator** Geben Sie ein Wort ein, das als Trennzeichen zwischen Transact-SQL-Anweisungen in Batches fungieren soll. Der Standardwert ist GO.

- **Standardmäßig neue Abfragen im SQLCMD-Modus öffnen** Aktivieren Sie dieses Kontrollkästchen, um neue Abfragen im SQLCMD-Modus zu öffnen. Dieses Kontrollkästchen ist nur sichtbar, wenn das Dialogfeld über das Menü Extras geöffnet wird.

    Beachten Sie die folgenden Einschränkungen, wenn Sie diese Option auswählen:

    - IntelliSense im Abfrage-Editor der Datenbank-Engine ist deaktiviert.

    - Da der Abfrage-Editor nicht über die Befehlszeile ausgeführt werden kann, können Sie keine Befehlszeilenparameter wie etwa Variablen übergeben.

    - Da der Abfrage-Editor nicht auf Aufforderungen des Betriebssystems reagieren kann, müssen Sie darauf achten, dass keine interaktiven Anweisungen ausgeführt werden.

- **Standard wiederherstellen** Setzt alle auf dieser Seite verfügbaren Werte auf die ursprünglichen Standardwerte zurück.