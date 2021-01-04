---
title: Optimistische Parallelität
description: Beschreibt die vollständige und die eingeschränkte Parallelität und wie Sie auf Parallelitätsverletzungen testen können.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12894591f4ee7b1db5e514b7218337d92382842b
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051306"
---
# <a name="optimistic-concurrency"></a>Optimistische Parallelität

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In einer Umgebung mit mehreren Benutzern gibt es zwei Modelle für das Update von Daten in einer Datenbank: das Modell der vollständigen Parallelität und das Modell der eingeschränkten Parallelität. Das <xref:System.Data.DataSet>-Objekt unterstützt die Verwendung der vollständigen Parallelität für lange Aktivitäten, wie bei der Datenfernverarbeitung und der Interaktion mit Daten.  
  
Die eingeschränkte Parallelität umfasst das Sperren von Zeilen an der Datenquelle, damit Benutzer Daten, die andere Benutzer betreffen, nicht ändern können. Wenn ein Benutzer bei einem eingeschränkten Modell eine Aktion ausführt, durch die eine Sperre angewendet wird, können andere Benutzer so lange keine mit der Sperre in Konflikt stehenden Aktionen ausführen, bis der Eigentümer der Sperre diese aufgehoben hat. Dieses Modell wird hauptsächlich in Umgebungen verwendet, in denen häufig Datenkonflikte auftreten, oder in denen der Aufwand von Datensperren geringer ist als der Aufwand für Rollbacks von Transaktionen, die aufgrund von Konflikten durch eine Parallelbearbeitung ausgeführt werden müssen.  
  
Daher erstellt ein Benutzer, der eine Zeile aktualisiert, bei einem Modell für pessimistische Parallelität eine Sperre. Die Zeile kann erst wieder von einem anderen Benutzer geändert werden, wenn der Benutzer den Updatevorgang für die Zeile abgeschlossen und die Sperre aufgehoben hat. Aus diesem Grund eignet sich die eingeschränkte Parallelität am besten bei kurzen Sperrfristen wie bei der programmgesteuerten Verarbeitung von Datensätzen. Die eingeschränkte Parallelität stellt keine flexible Option dar, wenn Benutzer mit Daten interagieren und Datensätze dadurch für einen relativ langen Zeitraum sperren.

> [!NOTE]
> Wenn Sie mehrere Zeilen auf einmal aktualisieren müssen, ermöglicht das Erstellen einer Transaktion ein höheres Maß an Skalierung als das Sperren bei eingeschränkter Parallelität.

Im Gegensatz dazu sperren Benutzer bei der vollständigen Parallelität keine Zeile, während sie sie lesen. Wenn ein Benutzer eine Zeile aktualisieren möchte, muss durch die Anwendung festgestellt werden, ob ein anderer Benutzer die Zeile seit dem Abruf geändert hat. Die vollständige Parallelität wird im Allgemeinen in Umgebungen mit wenigen Datenkonflikten verwendet. Vollständige Parallelität führt zu einer Leistungssteigerung, weil keine Datensätze gesperrt werden müssen, denn für das Sperren von Datensätzen sind zusätzliche Serverressourcen erforderlich. Außerdem ist für die Verwaltung von Datensatzsperren eine ständige Verbindung zum Datenbankserver notwendig. Da dies beim Modell der vollständigen Parallelität nicht der Fall ist, können Verbindungen zum Server innerhalb eines kürzeren Zeitraums eine Vielzahl von Clients bedienen.

Beim Modell der vollständigen Parallelität wird dann von einer Verletzung ausgegangen, wenn, nachdem ein Benutzer einen Wert aus der Datenbank empfangen hat, ein anderer Benutzer den Wert ändert, bevor der erste Benutzer den Wert zu ändern versucht hat. Wie der Server eine Parallelitätsverletzung auflöst, kann am Besten anhand des folgenden Beispiels erläutert werden.

Die folgenden Tabellen stellen ein Beispiel für eine vollständige Parallelität dar.  
  
Um 13:00 Uhr ruft User1 eine Zeile mit den folgenden Werten aus der Datenbank auf:  
  
**CustID     LastName     FirstName**  
  
101          Smith             Bob  
  
|Spaltenname|Ursprünglicher Wert|Aktueller Wert|Wert in Datenbank|  
|-----------------|--------------------|-------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bernd|Bernd|Bernd|  
  
Um 13:01 Uhr ruft User2 dieselbe Zeile ab.  
  
User2 ändert **FirstName** um 13:03 Uhr von „Bob“ in „Robert“ und aktualisiert die Datenbank.  
  
|Spaltenname|Ursprünglicher Wert|Aktueller Wert|Wert in Datenbank|  
|-----------------|--------------------|-------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bernd|Robert|Bernd|  
  
Das Update ist erfolgreich, weil die Werte in der Datenbank zur Zeit des Updates mit den ursprünglichen Werten übereinstimmen, die User2 vorliegen.  
  
Um 13:05 Uhr ändert User1 FirstName von Bob in James und versucht, die Zeile zu aktualisieren.  
  
|Spaltenname|Ursprünglicher Wert|Aktueller Wert|Wert in Datenbank|  
|-----------------|--------------------|-------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bernd|James|Robert|  
  
Zu diesem Zeitpunkt stellt User1 eine Verletzung der optimistischen Parallelität fest, weil der Wert in der Datenbank ("Robert") nicht mit dem ursprünglichen Wert übereinstimmt, den User1 erwartet hat ("Bob"). Die Parallelitätsverletzung sagt Ihnen einfach nur, dass das Update fehlgeschlagen ist. Nun muss entschieden werden, ob die von User2 vorgenommenen Änderungen mit den Änderungen von User1 überschrieben oder die Änderungen von User1 verworfen werden sollen.

## <a name="testing-for-optimistic-concurrency-violations"></a>Testen auf Verstöße gegen die optimistische Nebenläufigkeit

Es gibt mehrere Testverfahren, mit denen eine Verletzung der vollständigen Parallelität festgestellt werden kann. Eine Methode besteht im Einfügen einer Timestamp-Spalte in die Tabelle.

Datenbanken stellen im Allgemeinen Timestamp-Funktionen zur Verfügung, mit denen der Updatezeitpunkt (Datum und Uhrzeit) des Datensatzes gekennzeichnet werden kann. Bei dieser Methode wird eine Timestamp-Spalte in die Tabellendefinition eingefügt. Bei jedem Update des Datensatzes wird der Timestamp aktualisiert, sodass er den aktuellen Zeitpunkt (Datum und Uhrzeit) angibt.

Bei einem Test auf eine Verletzung der vollständigen Parallelität wird die Timestamp-Spalte bei jeder Abfrage des Tabelleninhalts zurückgegeben. Bei einem Updateversuch wird der Timestamp-Wert in der Datenbank mit dem ursprünglichen Timestamp-Wert verglichen, der in der geänderten Zeile enthalten ist. Wenn die Werte identisch sind, wird die Timestamp-Spalte mit der aktuellen Uhrzeit aktualisiert, um das Update zu kennzeichnen. Wenn die Werte nicht identisch sind, wurde die vollständige Parallelität verletzt.

Beim zweiten Testverfahren, mit dem eine Verletzung der vollständigen Parallelität festgestellt werden kann, wird überprüft, ob alle ursprünglichen Spaltenwerte einer Zeile mit denen in der Datenbank übereinstimmen. Betrachten Sie beispielsweise die folgende Abfrage:

```sql
SELECT Col1, Col2, Col3 FROM Table1  
```  
  
Zum Testen auf einen Verstoß gegen die optimistische Nebenläufigkeit beim Aktualisieren einer Zeile in **Table1** verwenden Sie die folgende UPDATE-Anweisung:  
  
```sql
UPDATE Table1 Set Col1 = @NewCol1Value,  
              Set Col2 = @NewCol2Value,  
              Set Col3 = @NewCol3Value  
WHERE Col1 = @OldCol1Value AND  
      Col2 = @OldCol2Value AND  
      Col3 = @OldCol3Value  
```
Wenn die ursprünglichen Werte mit den Werten in der Datenbank übereinstimmen, wird das Update durchgeführt. Wenn ein Wert geändert wurde, wird die Zeile von der UPDATE-Anweisung nicht aktualisiert, weil die WHERE-Klausel keine Übereinstimmung findet.  
  
Es empfiehlt sich, in einer Abfrage stets einen eindeutigen Primärschlüsselwert zurückzugeben. Andernfalls könnte die vorhergehende UPDATE-Anweisung mehr als eine Zeile aktualisieren, was möglicherweise nicht von Ihnen beabsichtigt ist.  
  
Wenn in einer Spalte Ihrer Datenquelle NULL-Werte zulässig sind, müssen Sie Ihre WHERE-Klausel erweitern, um nach einem entsprechenden NULL-Verweis in Ihrer lokalen Tabelle und in der Datenquelle zu suchen. Die folgende UPDATE-Anweisung prüft z. B., ob ein NULL-Verweis in der lokalen Zeile noch mit einem NULL-Verweis in der Datenquelle übereinstimmt oder ob der Wert in der lokalen Zeile noch mit dem Wert in der Datenquelle übereinstimmt.  
  
```sql
UPDATE Table1 Set Col1 = @NewVal1  
  WHERE (@OldVal1 IS NULL AND Col1 IS NULL) OR Col1 = @OldVal1  
```  
  
Sie können bei einem Modell der vollständigen Parallelität auch weniger strenge Kriterien anwenden. Wenn Sie z. B. nur die Primärschlüsselspalten in der WHERE-Klausel verwenden, werden die Daten unabhängig davon überschrieben, ob die anderen Spalten seit der letzten Abfrage aktualisiert wurden oder nicht. Zudem besteht die Möglichkeit, eine WHERE-Klausel auf spezifische Spalten anzuwenden, sodass die Daten überschrieben werden, sofern nicht bestimmte Felder seit deren letzten Abfrage aktualisiert wurden.

### <a name="the-dataadapterrowupdated-event"></a>DataAdapter.RowUpdated-Ereignis

Das **RowUpdated**-Ereignis des <xref:System.Data.Common.DataAdapter>-Objekts kann in Verbindung mit den oben beschriebenen Methoden eingesetzt werden, um Ihre Anwendung über Verstöße gegen die optimistische Nebenläufigkeit zu benachrichtigen. Das **RowUpdated**-Ereignis tritt nach jedem Versuch auf, eine **Modified**-Zeile eines **DataSets** zu aktualisieren. Damit können Sie spezifischen Behandlungscode hinzufügen, einschließlich Verarbeitung bei Ausnahmen, Einfügen von benutzerdefinierten Fehlerinformationen, Hinzufügen einer Wiederholungslogik usw.

Das <xref:System.Data.Common.RowUpdatedEventArgs>-Objekt gibt eine **RecordsAffected**-Eigenschaft mit der Anzahl der Zeilen zurück, die von einem bestimmten Updatebefehl für eine bestimmte geänderte Zeile in einer Tabelle betroffen sind. Wenn Sie den Updatebefehl auf einen Test auf optimistische Nebenläufigkeit festlegen, gibt die **RecordsAffected**-Eigenschaft bei einem Verstoß der optimistischen Nebenläufigkeit den Wert „0“ (null) als Ergebnis zurück, weil keine Datensätze aktualisiert wurden. Wenn dies der Fall ist, wird eine Ausnahme ausgelöst.

Mit dem **RowUpdated**-Ereignis können Sie dieser Situation begegnen und die Ausnahme verhindern, indem Sie einen entsprechenden **RowUpdatedEventArgs.Status**-Wert festlegen, z. B. **UpdateStatus.SkipCurrentRow**. Weitere Informationen zum **RowUpdated**-Ereignis finden Sie unter [Umgang mit DataAdapter-Ereignissen](handle-dataadapter-events.md).

Optional können Sie **DataAdapter.ContinueUpdateOnError** auf **TRUE** festlegen, bevor Sie **Update** aufrufen, und auf die in der **RowError**-Eigenschaft einer bestimmten Zeile gespeicherten Fehlerinformationen reagieren, wenn die **Update**-Operation abgeschlossen ist. Weitere Informationen finden Sie unter [Informationen zu Zeilenfehlern](/dotnet/framework/data/adonet/dataset-datatable-dataview/row-error-information).

## <a name="optimistic-concurrency-example"></a>Beispiel für die optimistische Nebenläufigkeit

Im folgenden einfachen Beispiel wird die **UpdateCommand**-Eigenschaft einer **DataAdapter**-Klasse darauf festgelegt, einen Test für die optimistische Nebenläufigkeit auszuführen. Anschließend wird das **RowUpdated**-Ereignis verwendet, um auf Verstöße gegen die optimistische Nebenläufigkeit zu prüfen. Wenn ein Verstoß gegen die optimistische Nebenläufigkeit ermittelt wird, legt die Anwendung die **RowError**-Eigenschaft der Zeile fest, für die das Update ausgeführt wurde, um einen Verstoß gegen die optimistische Nebenläufigkeit anzugeben.

Beachten Sie, dass die an die WHERE-Klausel des UPDATE-Befehls übergebenen Parameterwerte den **Original**-Werten der jeweiligen Spalten zugeordnet werden.

[!code-csharp[SqlDataAdapter_Concurrency#1](~/../sqlclient/doc/samples/SqlDataAdapter_Concurrency.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen und Ändern von Daten in ADO.NET](retrieving-modifying-data.md)
- [Aktualisieren von Datenquellen mit "DataAdapters"](update-data-sources-with-dataadapters.md)
- [Transaktionen und Parallelität](transactions-and-concurrency.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
