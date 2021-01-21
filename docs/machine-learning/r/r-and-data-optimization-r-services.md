---
title: Leistungsoptimierung für Daten
description: In diesem Artikel werden Leistungsoptimierungen für R- oder Python-Skripts erläutert, die in SQL Server ausgeführt werden. Außerdem werden Methoden beschrieben, mit denen Sie den R-Code aktualisieren können, um die Leistung zu steigern und bekannte Probleme zu vermeiden.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 71c8852efd538ef0a702c8f96435facf4882b7d2
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596665"
---
# <a name="performance-tuning-and-data-optimization-for-r"></a>Leistungs- und Datenoptimierung für R
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Artikel werden Leistungsoptimierungen für R- oder Python-Skripts erläutert, die in SQL Server ausgeführt werden. Mit diesen Methoden können Sie Ihren R-Code aktualisieren, um die Leistung zu steigern und bekannte Probleme zu vermeiden.

## <a name="choosing-a-compute-context"></a>Auswählen eines Computekontexts

In SQL Server können Sie beim Ausführen von R-oder Python-Skripts entweder den Computekontext **lokal** oder **SQL** verwenden.

Wenn Sie den **lokalen** Computekontext verwenden, wird die Analyse auf dem Computer und nicht auf dem Server ausgeführt. Wenn Sie also in Ihrem Code verwendete Daten aus SQL Server abrufen, müssen die Daten über das Netzwerk abgerufen werden. Die Leistungseinbußen für diesen Netzwerkübertragung hängen von der Größe der übertragenen Daten, dem Geschwindigkeit des Netzwerks und anderen Netzwerkübertragungen ab, die zur gleichen Zeit auftreten.

Bei Verwendung des **SQL Server-Computekontexts** wird der Code auf dem Server ausgeführt. Wenn Sie Daten aus SQL Server erhalten, sollten die Daten für den Server, auf dem die Analyse ausgeführt wird, lokal sein, wodurch keine Netzwerkbelastung entsteht. Wenn Sie Daten aus anderen Quellen importieren müssen, sollten Sie die Einrichtung von ETL in Betracht ziehen.

Bei der Arbeit mit großen Datenmengen sollten Sie immer den SQL Compute Context verwenden.

## <a name="factors"></a>Faktoren

Die R-Sprache verfügt über das Konzept der *Faktoren*, bei denen es sich um spezielle Variablen für kategorische Daten handelt. Datenanalysten verwenden in ihren Formeln häufig Faktorvariablen, da durch die Verwendung von kategorischen Variablen als Faktoren sichergestellt wird, dass die Daten von Machine Learning-Funktionen ordnungsgemäß verarbeitet werden.

Entwurfsbedingt können Faktorvariablen von Zeichenfolgen in ganze Zahlen umgewandelt und für die Speicherung oder Verarbeitung wieder zurückkonvertiert werden. Die R-Funktion `data.frame` verarbeitet alle Zeichenfolgen als Faktorvariablen, es sei denn, das Argument *stringsAsFactors* ist auf **false** festgelegt. Dies bedeutet, dass Zeichenfolgen für die Verarbeitung automatisch in eine ganze Zahl konvertiert und dann der ursprünglichen Zeichenfolge wieder zugeordnet werden.

Wenn die Quelldaten für Faktoren als ganze Zahl gespeichert werden, kann die Leistung beeinträchtigt werden, da R die Faktorganzzahlen zur Laufzeit in Zeichenfolgen konvertiert und dann eine eigene interne Konvertierung von Zeichenfolgen in ganze Zahlen ausführt.

Um solche Laufzeitkonvertierungen zu vermeiden, sollten Sie die Werte als ganze Zahlen in der SQL Server-Tabelle speichern und mit dem Argument _colInfo_ die Ebenen für die als Faktor verwendete Spalte angeben. Die meisten Datenquellenobjekte in RevoScaleR verwenden den Parameter _colInfo_. Verwenden Sie diesen Parameter, um die von der Datenquelle verwendeten Variablen zu benennen, ihren Typ anzugeben und die Variablenebenen oder Transformationen für die Spaltenwerte zu definieren.

Der folgende R-Funktionsaufruf ruft beispielsweise die Ganzzahlen 1, 2 und 3 aus einer Tabelle ab, ordnet die Werte jedoch einem Faktor mit den Ebenen „Apple“, „Orange“ und „Banane“ zu.

```R
c("fruit" = c(type = "factor", levels=as.character(c(1:3)), newLevels=c("apple", "orange", "banana")))
```

Wenn die Quellspalte Zeichenfolgen enthält, ist es immer effizienter, die Ebenen vorab mit dem Parameter _colInfo_ anzugeben. Der folgende R-Code behandelt beispielsweise die Zeichenfolgen beim Lesen als Faktoren.

```R
c("fruit" = c(type = "factor", levels= c("apple", "orange", "banana")))
```

Wenn es keinen semantischen Unterschied in der Erstellung der Modelle gibt, kann der letztgenannte Ansatz die Leistung verbessern.

## <a name="data-transformations"></a>Datentransformationen

Datenanalysten verwenden häufig Transformationsfunktionen, die als Teil der Analyse in R geschrieben werden. Die Transformationsfunktion wird auf jede Zeile, die aus der Tabelle abgerufen wird, angewendet. In SQL Server werden solche Transformationen auf alle in einem Batch abgerufenen Zeilen angewendet, für die eine Kommunikation zwischen dem R-Interpreter und der Analyse-Engine erforderlich ist. Zum Ausführen der Umwandlungen werden die Daten von SQL an die Datenanalyse-Engine und dann an den Prozess der R-Interpreter verschoben und zurück.

Aus diesem Grund können Transformationen als Teil des R-Codes eine erhebliche negative Auswirkung auf die Leistung des Algorithmus, abhängig von der Datenmenge, haben.

Es ist effizienter, vor der Analyse alle erforderlichen Spalten in der Tabelle oder Ansicht zu haben, um Transformationen während der Berechnung zu vermeiden. Wenn es nicht möglich ist, zusätzliche Spalten zu vorhandenen Tabellen hinzuzufügen, können Sie eine andere Tabelle oder Ansicht mit den transformierten Spalten erstellen und eine entsprechende Abfrage zum Abrufen der Daten verwenden.

## <a name="batch-row-reads"></a>Lesen von Zeilen in Batches

Wenn Sie im Code eine SQL Server-Datenquelle (`RxSqlServerData`) verwenden, wird empfohlen, den Parameter _rowsPerRead_ zur Angabe der Batchgröße zu verwenden. Dieser Parameter definiert die Anzahl der Zeilen, die abgefragt und dann zur Verarbeitung an das externe Skript gesendet werden. Zur Laufzeit sieht der Algorithmus nur die angegebene Anzahl an Zeilen in jedem Batch.

Die Möglichkeit, die Menge der gleichzeitig zu verarbeitenden Daten zu steuern, kann Ihnen helfen, kann Ihnen bei der Lösung oder Vermeidung von Problemen helfen. Wenn das Eingabedataset beispielsweise sehr breit ist (viele Spalten aufweist) oder das Dataset über einige große Spalten (wie freien Text) verfügt, können Sie die Batchgröße verringern, um das Auslagern von Daten aus dem Arbeitsspeicher zu vermeiden.

Standardmäßig ist der Wert dieses Parameters auf 50000 festgelegt, um eine gute Leistung auch auf Computern mit wenig Arbeitsspeicher zu gewährleisten. Wenn der Server über genügend Arbeitsspeicher verfügt, kann eine bessere Leistung, insbesondere bei großen Tabellen, durch Erhöhen dieses Wertes auf 500.000 oder selbst auf eine Million erzielt werden.

Die Vorteile der Vergrößerung der Batchgröße werden deutlich bei großen Datasets und in Aufgaben, die in mehreren Prozessen ausgeführt werden können. Wenn Sie diesen Wert erhöhen, erzielen Sie jedoch nicht immer die besten Ergebnisse. Es wird empfohlen, dass Sie mit Ihren Daten und dem Algorithmus experimentieren, um den optimalen Wert zu ermitteln.

## <a name="parallel-processing"></a>Parallelverarbeitung

Um die Leistung von **RX**-Analysefunktionen zu verbessern, können Sie die Fähigkeit von SQL Server nutzen, Aufgaben parallel mithilfe der verfügbaren Kerne auf dem Servercomputer auszuführen.

Es gibt zwei Möglichkeiten zum Erreichen der Parallelisierung mit R in SQL Server:

+ **Verwenden Sie \@parallel.** Bei Verwendung der `sp_execute_external_script` gespeicherten Prozedur zur Ausführung eines R-Skripts legen Sie den `@parallel`-Parameter auf `1` fest. Dies ist die beste Methode, wenn Ihr R-Skript **keine** RevoScaleR-Funktionen verwendet, die andere Mechanismen für die Verarbeitung aufweisen. Wenn Ihr Skript RevoScaleR-Funktionen verwendet – die in der Regel mit dem Präfix „RX“ versehen sind–, wird die parallele Verarbeitung automatisch ausgeführt, und Sie müssen `@parallel` nicht explizit auf `1` festlegen.

  Wenn das R-Skript und die SQL-Abfrage parallelisiert werden können, dann erstellt die Datenbank-Engine mehrere parallele Prozesse. Die maximale Anzahl von Prozessen, die erstellt werden kann, entspricht der Einstellung für die **max. Grad an Parallelität** (MAXDOP) für die Instanz. Alle Prozesse führen dann dasselbe Skript aus, empfangen aber nur einen Teil der Daten.
  
  Diese Methode ist also nicht nützlich für Skripts, die alle Daten sehen müssen, wie z. B. beim Trainieren eines Modells. Es ist jedoch nützlich, wenn Tasks wie die Batchvorhersage parallel ausgeführt werden. Weitere Informationen zur Verwendung von Parallelität mit `sp_execute_external_script` finden Sie im Abschnitt **Tipps für Fortgeschrittene: parallele Verarbeitung** in [Verwenden von R-Code in Transact-SQL](../tutorials/quickstart-r-create-script.md).

+ **Verwenden Sie numTasks =1.** Wenn Sie **RX**-Funktionen in einem SQL Server-Computekontext verwenden, legen Sie den Wert des Parameters _numTasks_ auf die Anzahl der Prozesse fest, die Sie erstellen möchten. Die Anzahl der erstellten Prozesse kann nie mehr als **MAXDOP** sein. Die tatsächliche Anzahl der erstellten Prozesse wird jedoch von der Datenbank-Engine bestimmt und ist möglicherweise kleiner als Sie angefordert haben.

  Wenn das R-Skript und die SQL-Abfrage parallelisiert werden können, dann erstellt SQL Server mehrere parallele Prozesse, wenn die Rx-Funktionen ausgeführt werden. Die tatsächliche Anzahl der erzeugten Prozesse hängt von verschiedenen Faktoren ab. Dazu zählen Ressourcenkontrolle, aktuelle Verwendung von Ressourcen, andere Sitzungen und der Abfrageausführungsplan für die mit dem R-Skript verwendete Abfrage.

## <a name="query-parallelization"></a>Parallelisierung der Abfragen

Sie können in Microsoft R Sie mit SQL Server-Datenquellen arbeiten, indem Sie die Daten als RxSqlServerData-Datenquellenobjekt definieren.

Erstellt eine Datenquelle auf der Grundlage einer gesamten Tabelle oder Ansicht:

```R
RxSqlServerData(table= "airline", connectionString = sqlConnString)
```

Erstellt eine Datenquelle auf der Grundlage einer SQL-Abfrage:

```R
RxSqlServerData(sqlQuery= "SELECT [ArrDelay],[CRSDepTime],[DayOfWeek] FROM  airlineWithIndex WHERE rowNum <= 100000", connectionString = sqlConnString)
```

> [!NOTE]
> Wenn anstatt einer Abfrage eine Tabelle in der Datenquelle angegeben ist, verwendet R Services interne Heuristik, um die erforderlichen Spalten zum Abrufen aus der Tabelle zu ermitteln. Dieser Ansatz wird jedoch nicht in einer parallelen Ausführung enden.

Um sicherzustellen, dass die Daten parallel analysiert werden können, sollte die Abfrage zum Abrufen der Daten so eingeschlossen werden, dass die Datenbank-Engine einen Plan für die parallele Abfrage erstellen kann. Wenn der Code oder Algorithmus große Datenmengen verwendet, stellen Sie sicher, dass die für `RxSqlServerData` angegebene Abfrage für die parallele Ausführung optimiert ist. Eine Abfrage, die nicht zu einem parallelen Ausführungsplan führt, kann zu einem einzelnen Prozess für die Berechnung führen.

Wenn Sie mit großen Datasets arbeiten müssen, verwenden Sie Management Studio oder einen anderen SQL Query Analyzer, bevor Sie den R-Code zur Analyse des Ausführungsplans ausführen. Führen Sie dann die empfohlenen Schritte aus, um die Leistung der Abfrage zu verbessern. Ein fehlender Index für eine Tabelle kann z.B. die Zeit zum Ausführen einer Abfrage beeinträchtigen. Weitere Informationen finden Sie unter [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md).

Ein weiterer häufiger Fehler, die die Leistung beeinträchtigen könnte, besteht darin, wenn die Abfrage mehr Spalten als erforderlich aufruft. Wenn eine Formel beispielsweise auf nur drei Spalten basiert, die Quelltabelle jedoch 30 Spalten enthält, verschieben Sie die Daten unnötig.

+ Vermeiden Sie, `SELECT *`zu verwenden.
+ Nehmen Sie sich etwas Zeit, um die Spalten im Dataset zu überprüfen und nur die für die Analyse benötigten Elemente zu identifizieren.
+ Entfernen Sie aus den Abfragen alle Spalten, die Datentypen enthalten, die mit R-Code nicht kompatibel sind, z. B. GUIDs und rowguids.
+ Überprüfen auf nicht unterstützte Datums- und Uhrzeitformate
+ Anstatt eine Tabelle zu laden, erstellen Sie eine Ansicht, mit der bestimmte Werte ausgewählt oder Spalten umgewandelt werden, um Konvertierungsfehler zu vermeiden.

## <a name="optimizing-the-machine-learning-algorithm"></a>Optimieren des Machine Learning-Algorithmus

Dieser Abschnitt enthält verschiedene Tipps und Ressourcen, die für RevoScaleR und andere Optionen in Microsoft R spezifisch sind.

> [!TIP]
> Eine allgemeine Erläuterung der R-Optimierung ist nicht Teil dieses Artikels. Wenn Sie Ihren Code jedoch schneller machen müssen, empfiehlt es sich, den Artikel [Das R-Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf) zu lesen. Er behandelt Programmierkonstrukte in R, beschreibt häufige Fehler in anschaulicher Sprache und im Detail und bietet viele spezifische Beispiele für R-Programmiertechniken.

### <a name="optimizations-for-revoscaler"></a>Optimierungen für RevoScaleR

Viele RevoScaleR-Algorithmen unterstützen Parameter, um zu steuern, wie das Trainingsmodell generiert wird. Zwar sind die Genauigkeit und Richtigkeit des Modells wichtig, die Leistung des Algorithmus ist möglicherweise jedoch genauso wichtig. Um das richtige Gleichgewicht zwischen der Genauigkeit und der Trainingszeit zu erzielen, können Sie Parameter zum Beschleunigen der Berechnung ändern, und in vielen Fällen können Sie die Leistung verbessern, ohne die Genauigkeit und Richtigkeit zu reduzieren.

+ [rxDTree](/r-server/r-reference/revoscaler/rxdtree)

  `rxDTree` unterstützt den `maxDepth`-Parameter, der die Tiefe der Entscheidungsstruktur steuert. Wenn `maxDepth` erhöht wird, kann die Leistung beeinträchtigt werden. Daher ist es wichtig, die Vorteile einer größeren Tiefe im Vergleich zu den Auswirkungen auf die Leistung zu analysieren.

  Sie können ebenfalls das Gleichgewicht zwischen Zeitkomplexität und Vorhersagegenauigkeit steuern, indem Sie Parameter wie `maxNumBins`, `maxDepth`, `maxComplete` und `maxSurrogate` anpassen. Das Erhöhen der Tiefe auf mehr als 10 oder 15 kann die Berechnung sehr teuer machen.

+ [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)

  Verwenden Sie das `cube`-Argument, wenn die erste abhängige Variable der Formel eine Faktorvariable ist.
  
  Wenn der `cube`-Wert auf `TRUE` festgelegt ist, erfolgt die Regression mithilfe einer partitionierten Inverse, die möglicherweise schneller ist und weniger Arbeitsspeicher beansprucht als die Berechnung durch Standardregression. Wenn die Formel über eine große Anzahl von Variablen verfügt, kann die Leistungssteigerung erheblich sein.

+ [rxLogit](/r-server/r-reference/revoscaler/rxlogit)

  Verwenden Sie das `cube`-Argument, wenn die erste abhängige Variable eine Faktorvariable ist.
  
  Wenn `cube` auf `TRUE` festgelegt ist, verwendet der Algorithmus eine partitionierte Inverse, die möglicherweise schneller ist und weniger Arbeitsspeicher beansprucht. Wenn die Formel über eine große Anzahl von Variablen verfügt, kann die Leistungssteigerung erheblich sein.

Weitere Informationen zur Optimierung von RevoScaleR finden Sie in den folgenden Artikeln:

+ Supportartikel: [Optionen für die Leistungsoptimierung für rxDForest und rxDTree](https://support.microsoft.com/kb/3104235)

+ Methoden zum Steuern der Modellanpassung in einem verstärktem Strukturmodell: [Schätzen von Modellen mit Stochastic Gradient Boosting](/r-server/r/how-to-revoscaler-boosting)

+ Übersicht über die Verschiebung und Verarbeitung von Daten durch RevoScaleR: [Schreiben von benutzerdefinierten Segmentierungsalgorithmen in ScaleR](/r-server/r/how-to-developer-write-chunking-algorithms)

+ Programmiermodell für RevoScaleR: [Verwalten von Threads in RevoScaleR](/r-server/r/how-to-developer-manage-threads)

+ Funktionsreferenz für [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

+ Funktionsreferenz für [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)

### <a name="use-microsoftml"></a>Verwenden von MicrosoftML

Außerdem wird empfohlen, dass Sie sich das neue **MicrosoftML**-Paket ansehen, das skalierbare Machine Learning-Algorithmen bereitstellt, die die von RevoScaleR bereitgestellten Computekontexte und Transformationen verwenden können.

+ [Erste Schritte mit MicrosoftML](/r-server/r/concept-what-is-the-microsoftml-package)

+ [Auswählen eines MicrosoftML-Algorithmus](/r-server/r/how-to-choose-microsoftml-algorithms-cheatsheet)

## <a name="next-steps"></a>Nächste Schritte

+ Informationen zu R-Funktionen, mit denen Sie die Leistung Ihres R-Codes verbessern können, finden Sie unter [Verwenden von R-Code-Profilerstellungsfunktionen](using-r-code-profiling-functions.md).

+ Ausführlichere Informationen zur Leistungsoptimierung in SQL Server finden Sie unter [Leistungscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).