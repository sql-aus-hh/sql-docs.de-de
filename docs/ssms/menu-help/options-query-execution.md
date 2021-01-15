---
title: 'SQL Server-Optionsseite: Abfrageausführung'
description: Optionen (Abfrageausführung)
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
ms.openlocfilehash: 25ac37cdb9095151c90fdf81314d4c32044e6159
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193130"
---
# <a name="query-options-execution-general-page"></a>Abfrageausführung (Seite 'Allgemein')

Auf dieser Seite können Sie die Optionen zum Ausführen von Abfragen in Microsoft SQL Server angeben. Um auf dieses Dialogfeld zuzugreifen, klicken Sie mit der rechten Maustaste auf den Hauptteil des Abfrage-Editor-Fensters, und klicken Sie anschließend auf Abfrageoptionen.

- **SET ROWCOUNT** Der Standardwert 0 zeigt an, dass SQL Server so lange auf die Ergebnisse wartet, bis alle Ergebnisse übermittelt sind. Geben Sie einen Wert größer 0 an, wenn die Abfrage von SQL Server nach Übermittlung einer bestimmten Anzahl von Zeilen abgebrochen werden soll. Geben Sie SET ROWCOUNT 0 an, um diese Option zu deaktivieren (sodass alle Zeilen zurückgegeben werden).

- **SET TEXTSIZE** Der Standardwert von 2.147.483.647 Bytes zeigt an, dass SQL Server ein vollständiges Datenfeld bereitstellt, das maximal der Größe von text-, ntext-, nvarchar(max)- und varchar(max)-Datenfeldern entspricht. Dies hat keine Auswirkungen auf den XML-Datentyp. Geben Sie eine kleinere Zahl an, um Ergebnisse mit großen Werten zu beschränken. Spalten, deren Größe die angegebene Zahl übersteigt, werden abgeschnitten.

- **Ausführungstimeout** Dieses Feld enthält die Zeit (in Sekunden), nach der die Abfrage abgebrochen wird. Der Wert '0' gibt an, dass der Wartevorgang unbegrenzt ist bzw. kein Timeout erfolgt.

- **Batch separator** Geben Sie ein Wort ein, das als Trennzeichen zwischen Transact-SQL-Anweisungen in Batches fungieren soll. Der Standardwert ist GO.

- **Standardmäßig neue Abfragen im SQLCMD-Modus öffnen** Aktivieren Sie dieses Kontrollkästchen, um neue Abfragen im SQLCMD-Modus zu öffnen. Dieses Kontrollkästchen ist nur sichtbar, wenn das Dialogfeld über das Menü Extras geöffnet wird.

    Beachten Sie die folgenden Einschränkungen, wenn Sie diese Option auswählen:

    - IntelliSense im Abfrage-Editor der Datenbank-Engine ist deaktiviert.

    - Da der Abfrage-Editor nicht über die Befehlszeile ausgeführt werden kann, ist es nicht möglich, Befehlszeilenparameter, z. B. Variablen, zu übergeben.

    - Da der Abfrage-Editor nicht auf Anforderungen des Betriebssystems reagieren kann, müssen Sie darauf achten, keine interaktiven Anweisungen auszuführen.

- **Standard wiederherstellen** Setzt alle auf dieser Seite verfügbaren Werte auf die ursprünglichen Standardwerte zurück.