---
description: Partitioned Tables and Indexes
title: Partitionierte Tabellen und Indizes | Microsoft-Dokumentation
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- partitioned tables [SQL Server], about partitioned tables
- partitioned indexes [SQL Server], architecture
- partitioned tables [SQL Server], architecture
- partitioned indexes [SQL Server], about partitioned indexes
- index partitions
ms.assetid: cc5bf181-18a0-44d5-8bd7-8060d227c927
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8471695cfde49d36ba107264fa23654757d8c2ab
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241875"
---
# <a name="partitioned-tables-and-indexes"></a>Partitioned Tables and Indexes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt die Tabellen- und Indexpartitionierung. Die Daten partitionierter Tabellen und Indizes werden in Einheiten aufgeteilt, die optional über mehrere Dateigruppen in einer Datenbank verteilt sein können. Die Daten werden horizontal partitioniert, sodass Gruppen von Zeilen einzelnen Partitionen zugeordnet werden. Alle Partitionen eines einzelnen Indexes oder einer Tabelle müssen sich in der gleichen Datenbank befinden. Die Tabelle oder der Index wird als einzelne logische Entität behandelt, wenn Abfragen oder Aktualisierungen für die Daten ausgeführt werden. Vor [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1 waren partitionierte Tabellen und Indizes nicht in jeder Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Editionen und unterstütze Funktionen für den SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
> [!IMPORTANT]  
> [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] unterstützt standardmäßig bis zu 15.000 Partitionen. In früheren Versionen als [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wurde die Anzahl der Partitionen standardmäßig auf 1.000 beschränkt.  
  
## <a name="benefits-of-partitioning"></a>Vorteile der Partitionierung  
 Das Partitionieren großer Tabellen oder Indizes kann die folgenden Vorteile bei der Verwaltung und Leistung haben.  
  
-   Sie können Teilmengen von Daten schnell und effizient übertragen und darauf zugreifen, während die Integrität der Datensammlung erhalten bleibt. So dauert beispielsweise ein Vorgang wie das Laden von Daten von einem OLTP-System in ein OLAP-System nur Sekunden, statt Minuten und Stunden, wenn die Daten nicht partitioniert sind.  
  
-   Sie können Wartungsvorgänge für eine oder mehrere Partitionen schneller ausführen. Die Vorgänge sind effizienter, da sie auf nur diese Datenteilmengen abzielen, statt auf die ganze Tabelle. Sie können z. B. wählen, Daten in einer oder mehreren Partitionen zu komprimieren oder eine oder mehrere Partitionen eines Indexes neu zu erstellen.  
  
-   Die Abfrageleistung lässt sich, abhängig von den am häufigsten ausgeführten Abfragetypen sowie der Hardwarekonfiguration, möglicherweise verbessern. So kann der Abfrageoptimierer zum Beispiel Gleichheitsjoin-Abfragen zwischen zwei oder mehreren partitionierten Tabellen schneller verarbeiten, wenn die Partitionierungsspalten mit den Spalten identisch sind, über die die Tabellen verknüpft werden. Weitere Informationen finden Sie weiter unten unter [Abfragen](#queries).
  
Beim Sortieren von Daten nach E-A-Operationen geht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zunächst nach Partitionen vor. Um die Leistung beim Sortieren von Daten zu verbessern, verteilen Sie die Datendateien der Partitionen über mehrere Datenträger, d. h. durch das Einrichten eines RAID (Redundant Array of Independent Disks). Auf diese Weise kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , obwohl die Daten dabei weiterhin nach Partitionen sortiert werden, auf alle Laufwerke der einzelnen Partitionen gleichzeitig zugreifen.  
  
Darüber hinaus kann die Leistung verbessert werden, indem eine Sperrenausweitung auf Partitionsebene statt auf die gesamte Tabelle angewendet wird. Dies kann Sperrenkonflikte für die Tabelle reduzieren. Setzen Sie die `LOCK_ESCALATION`-Option der `ALTER TABLE`-Anweisung auf AUTO, um eine Sperrenausweitung auf die Partition zuzulassen und damit Sperrenkonflikte zu verringern. 

> [!TIP]
> Partitionen einer Tabelle oder eines Index können in einer Dateigruppe, beispielsweise der Dateigruppe `PRIMARY`, oder in mehreren Dateigruppen platziert werden. Bei der Verwendung von mehrstufigem Speicher ermöglicht die Verwendung mehrerer Dateigruppen das Zuweisen bestimmter Partitionen zu bestimmten Speicherebenen. Alle anderen Partitionierungsvorteile gelten unabhängig von der Anzahl der verwendeten Dateigruppen oder der Platzierung von Partitionen in bestimmten Dateigruppen.
  
## <a name="components-and-concepts"></a>Komponenten und Konzepte  
Die folgenden Begriffe beziehen sich auf die Tabellen- und Indexpartitionierung.  
  
### <a name="partition-function"></a>Partitionsfunktion  
Hierbei handelt es sich um ein Datenbankobjekt, das definiert, wie die Zeilen einer Tabelle oder eines Index basierend auf den Werten einer bestimmten Spalte, einer sogenannten Partitionierungsspalte, mehreren Partitionen zugeordnet werden. Bei den einzelnen Werten in der Partitionierungsspalte handelt es sich um eine Eingabe für die Partitionsfunktion, die einen Partitionswert zurückgibt. Die Partitionsfunktion definiert die Anzahl von Partitionen sowie die Begrenzungen der Partitionen, über die die Tabelle verfügt. Angenommen, Sie verfügen über eine Tabelle, die Verkaufsauftragsdaten enthält, und möchten die Tabelle möglicherweise in zwölf (monatliche) Partitionen auf Grundlage einer **datetime** -Spalte (z.B. Verkaufsdatum) partitionieren.  
  
### <a name="partition-scheme"></a>Partitionsschema 
Ein Datenbankobjekt, das die Partitionen einer Partitionsfunktion Dateigruppen zuordnet. Der wichtigste Grund dafür, dass Partitionen in separaten Dateigruppen platziert werden, besteht darin, sicherzustellen, dass Sie Sicherungsvorgänge unabhängig für Partitionen ausführen können. Dies liegt daran, dass Sie Sicherungen für einzelne Dateigruppen ausführen können.  

> [!NOTE]
> In Azure SQL-Datenbank werden nur primäre Dateigruppen unterstützt.  
  
### <a name="partitioning-column"></a>Partitionierungsspalte  
Die Spalte einer Tabelle oder eines Indexes, die von einer Partitionsfunktion zum Partitionieren der Tabelle oder des Indexes verwendet wird. Berechnete Spalten, die in eine Partitionsfunktion einbezogen werden, müssen explizit als PERSISTED gekennzeichnet sein. Alle Datentypen, die zum Verwenden als Indexspalten zulässig sind, können als Partitionsspalte verwenden werden, mit Ausnahme des Datentyps **timestamp**. Die Datentypen **ntext**-, **text**-, **image**-, **xml**-, **varchar(max)** -, **nvarchar(max)** oder **varbinary(max)** können nicht angegeben werden. Zudem können der benutzerdefinierte CLR-Typ (Common Language Runtime) von Microsoft .NET Framework und die Spalten mit dem Aliasdatentyp nicht angegeben werden.  
  
### <a name="aligned-index"></a>Ausgerichteter Index  
Ein Index, der auf dem gleichen Partitionsschema wie die zugehörige Tabelle aufbaut. Wenn eine Tabelle und ihre Indizes aneinander ausgerichtet sind, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schnell zwischen Partitionen wechseln und gleichzeitig die Partitionsstruktur sowohl der Tabelle als auch ihrer Indizes beibehalten. Ein Index muss nicht an derselben benannten Partitionsfunktion beteiligt sein, um an ihrer Basistabelle ausgerichtet zu sein. Allerdings müssen die Partitionsfunktionen des Indexes und der Basistabelle im Wesentlichen identisch sein, d.h.:
 1. Die Argumente der Partitionsfunktionen müssen denselben Datentyp besitzen.
 2. Sie definieren dieselbe Anzahl an Partitionen.
 3. Sie definieren dieselben Begrenzungswerte für Partitionen.  

#### <a name="partitioning-clustered-indexes"></a>Partitionieren gruppierter Indizes
Beim Partitionieren eines gruppierten Index muss der Gruppierungsschlüssel die Partitionierungsspalte enthalten. Wenn beim Partitionieren eines nicht eindeutigen gruppierten Index die Partitionierungsspalte nicht explizit im Gruppierungsschlüssel angegeben ist, fügt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Partitionierungsspalte standardmäßig zur Liste der gruppierten Indexschlüssel hinzu. Wenn der gruppierte Index eindeutig ist, müssen Sie explizit angeben, dass der gruppierte Indexschlüssel die Partitionierungsspalte enthält. Weitere Informationen zu gruppierten Indizes und zur Indexarchitektur finden Sie unter [Richtlinien für den Entwurf gruppierter Indizes](../../relational-databases/sql-server-index-design-guide.md#Clustered).       

#### <a name="partitioning-nonclustered-indexes"></a>Partitionieren nicht gruppierter Indizes
Beim Partitionieren eines eindeutigen nicht gruppierten Index muss der Indexschlüssel die Partitionierungsspalte enthalten. Beim Partitionieren eines nicht eindeutigen, nicht gruppierten Index fügt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Partitionierungsspalte standardmäßig als eine Nichtschlüsselspalte (eingeschlossene Spalte) des Indexes hinzu, um sicherzustellen, dass der Index an der Basistabelle ausgerichtet ist. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fügt die Partitionierungsspalte nicht zum Index hinzu, wenn sie bereits im Index vorhanden ist. Weitere Informationen zu nicht gruppierten Indizes und zur Indexarchitektur finden Sie unter [Entwurfsrichtlinien für einen nicht gruppierten Index](../../relational-databases/sql-server-index-design-guide.md#Nonclustered).

### <a name="non-aligned-index"></a>Nicht ausgerichteter Index  
Ein Index, der unabhängig von der zugehörigen Tabelle partitioniert ist. Das heißt, der Index hat ein anderes Partitionsschema oder wird in einer anderen Dateigruppe als die Basistabelle platziert. Das Entwerfen eines nicht ausgerichteten partitionierten Indexes kann in den folgenden Fällen nützlich sein:  
-   Die Basistabelle ist nicht partitioniert.  
-   Der Indexschlüssel ist eindeutig und enthält nicht die Partitionierungsspalte der Tabelle.  
-   Sie möchten die Basistabelle an angeordneten Joins mit weiteren Tabellen beteiligen, die unterschiedliche Joinspalten verwenden.  

### <a name="partition-elimination"></a>Partitionsentfernung
Der Prozess, durch den der Abfrageoptimierer nur auf relevante Partitionen zugreift, um die Filterkriterien der Abfrage zu erfüllen.  

## <a name="performance-guidelines"></a>Leistungsrichtlinien  
 Die neue, höhere Grenze von 15.000 Partitionen wirkt sich auf den Arbeitsspeicher, Vorgänge mit partitionierten Indizes, DBCC-Befehle und Abfragen aus. In diesem Abschnitt werden die Auswirkungen auf die Leistung beschrieben, wenn die Anzahl der Partitionen mehr als 1.000 beträgt, und es werden mögliche Problemumgehungen bereitgestellt. Dadurch, dass die maximale Anzahl von Partitionen auf 15.000 erhöht wurde, können Sie Daten über einen längeren Zeitraum speichern. Sie sollten Daten jedoch nur so lange beibehalten, wie sie benötigt werden, und darauf achten, dass die Leistung und die Anzahl der Partitionen ausgewogen ist.  
  
### <a name="processor-cores-and-number-of-partitions-guidelines"></a>Richtlinien für Prozessorkerne und die Anzahl der Partitionen  
 Zur Maximierung der Leistung paralleler Vorgänge wird empfohlen, die gleiche Anzahl von Partitionen und Prozessorkernen und bis zu einem Maximum von 64 zu verwenden (was die maximal zulässige Anzahl paralleler Prozessoren ist, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nutzen kann).  
  
### <a name="memory-usage-and-guidelines"></a>Speicherauslastung und Richtlinien  
 Es empfiehlt sich, mindestens 16 GB Arbeitsspeicher zu verwenden, wenn eine große Anzahl von Partitionen verwendet wird. Wenn das System nicht über ausreichend Arbeitsspeicher verfügt, kann es bei DML-Anweisungen (Datenbearbeitungssprache), DDL-Anweisungen (Datendefinitionssprache) und anderen Vorgängen aufgrund ungenügenden Arbeitsspeichers zu Fehlern kommen. Bei Systemen mit 16 GB Arbeitsspeicher, die zahlreiche speicherintensive Prozesse ausführen, kann es bei Vorgängen, die für eine große Anzahl von Partitionen ausgeführt werden, zu Fehlern aufgrund von Speicherauslastung kommen. Je mehr Arbeitsspeicher Sie über die empfohlenen 16 GB hinaus verwenden, desto geringer ist die Wahrscheinlichkeit, dass Probleme mit der Leistung und Speicherauslastung auftreten.  
  
 Arbeitsspeichereinschränkungen können sich negativ auf die Leistung oder auf die Möglichkeit von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Erstellen eines partitionierten Index auswirken. Das gilt insbesondere für den Fall, wenn der Index nicht an seiner Basistabelle oder an deren gruppierten Index ausgerichtet ist, sofern für die Tabelle bereits ein gruppierter Index erstellt wurde. In diesem Fall ist es möglicherweise sinnvoll, die Serverkonfigurationsoption „Speicher für Indexerstellung“ heraufzusetzen. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Speicher für Indexerstellung](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md). 
  
### <a name="partitioned-index-operations"></a>Vorgänge für partitionierte Indizes  
Das Erstellen bzw. Neuerstellen von nicht ausgerichteten Indizes für eine Tabelle mit mehr als 1.000 Partitionen ist möglich, wird aber nicht unterstützt. Dies hätte Leistungseinbußen oder eine zu hohe Speicherauslastung während der Vorgänge zur Folge.  
  
Das Erstellen und Neuerstellen von ausgrichteten Indizes kann um so länger dauern, je mehr Partitionen hinzugefügt werden. Es empfiehlt sich, nicht mehrere Befehle zum Erstellen und Neuerstellen von Befehlen gleichzeitig auszuführen, da es zu Leistungs- und Arbeitsspeicherproblemen kommen kann.  
  
 Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sortiervorgänge zum Erstellen partitionierter Indizes durchführt, erstellt es zuerst eine Sortiertabelle für jede Partition. Anschließend werden die Sortiertabellen entweder in der jeweiligen Dateigruppe jeder Partition oder in **tempdb** erstellt, wenn die SORT_IN_TEMPDB-Indexoption angegeben wurde. Jede Sortiertabelle setzt für ihre Erstellung eine Mindestmenge an Arbeitsspeicher voraus. Wenn Sie einen partitionierten Index erstellen, der an seiner Basistabelle ausgerichtet ist, werden alle Sortiertabellen nacheinander erstellt, was weniger Arbeitsspeicher in Anspruch nimmt. Wenn Sie allerdings einen nicht gruppierten partitionierten Index erstellen, werden alle Sortiertabellen gleichzeitig erstellt. Das heißt, es muss ausreichend Arbeitsspeicher verfügbar sein, um diese gleichzeitigen Sortiervorgänge zu verarbeiten. Je größer die Anzahl der Partitionen, desto mehr Arbeitsspeicher wird benötigt. Die Mindestgröße für jede Sortiertabelle beträgt 40 Seiten für jede Partition mit 8 Kilobyte pro Seite. So beansprucht z.&nbsp;B. ein nicht ausgerichteter partitionierter Index mit 100 Partitionen ausreichend Arbeitsspeicher, um 4.000 (40 * 100) Seiten gleichzeitig seriell sortieren zu können. Wenn dieser Arbeitsspeicher verfügbar ist, ist die Erstellung zwar erfolgreich, jedoch kann die Leistung darunter leiden. Wenn dieser Arbeitsspeicher nicht verfügbar ist, schlägt die Erstellung fehl. Alternativ erfordert ein ausgerichteter partitionierter Index mit 100 Partitionen nur ausreichend Arbeitsspeicher, um 40 Seiten zu sortieren, da die Sortiervorgänge nicht gleichzeitig durchgeführt werden.  
  
 Sowohl bei ausgerichteten als auch bei nicht ausgerichteten Indizes kann der Arbeitsspeicherbedarf noch höher sein, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bei einem Computer mit mehreren Prozessoren Grade der Parallelität beim Erstellungsvorgang verwendet. Denn je höher die Grade der Parallelität sind, desto größer ist auch der Arbeitsspeicherbedarf. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] z. B. die Grade der Parallelität auf 4 festlegt, benötigt ein nicht ausgerichteter partitionierter Index mit 100 Partitionen ausreichend Arbeitsspeicher, damit vier Prozessoren gleichzeitig jeweils 4.000 Seiten sortieren können – also 6.000 Seiten gleichzeitig. Wenn der partitionierte Index ausgerichtet ist, verringert sich der Arbeitsspeicherbedarf auf vier Prozessoren, die jeweils 40 Seiten sortieren – also 160 (4 * 40) Seiten. Sie können die MAXDOP-Indexoption verwenden, um die Grade der Parallelität manuell zu reduzieren.  

### <a name="dbcc-commands"></a>DBCC-Befehle  
Bei einer größeren Anzahl von Partitionen können DBCC-Befehle mehr Zeit für die Ausführung in Anspruch nehmen, während sich die Anzahl von Partitionen erhöht.  
  
### <a name="queries"></a>Abfragen  
Abfragen, die Partitionsentfernung verwenden, weisen eine vergleichbare oder verbesserte Leistung bei einer größeren Anzahl von Partitionen auf. Abfragen, die keine Partitionsentfernung verwenden, nehmen mehr Zeit für die Ausführung in Anspruch, wenn sich die Anzahl der Partitionen erhöht.  
  
Nehmen Sie beispielsweise an, eine Tabelle hat 100 Millionen Zeilen und Spalten `A`, `B`und `C`. 
-  In Szenario 1 ist die Tabelle in 1000 Partitionen der Spalte `A`unterteilt. 
-  In Szenario 2 ist die Tabelle in 10.000 Partitionen der Spalte `A`unterteilt. Eine Abfrage der Tabelle, die über eine `WHERE`-Klausel verfügt, die nach Spalte `A` filtert, führt die Partitionsentfernung aus und scannt eine Partition. Die gleiche Abfrage wird in Szenario 2 möglicherweise schneller ausgeführt, da es weniger zu scannende Zeilen in einer Partition gibt. Eine Abfrage, die über eine `WHERE`-Klausel verfügt, die nach Spalte B filtert, scannt alle Partitionen. Die Abfrage wird möglicherweise in Szenario 1 schneller als in Szenario 2 ausgeführt, da weniger Partitionen gescannt werden müssen.  

Abfragen, die Operatoren wie TOP oder MAX/MIN für andere Spalten als die Partitionierungsspalte verwenden, erzielen aufgrund der Partitionierung möglicherweise eine geringere Leistung, da alle Partitionen ausgewertet werden müssen.  

Wenn Sie häufig Abfragen ausführen, die Gleichheitsjoins zwischen mindestens zwei partitionierten Tabellen voraussetzen, sollten deren Partitionsspalten dieselben sein wie die Spalten, an denen die Tabellen verknüpft sind. Außerdem sollten die Tabellen oder deren Indizes angeordnet sein. Dies bedeutet, dass sie entweder dieselbe benannte Partitionsfunktion verwenden oder aber verschiedene Partitionsfunktionen, die sich in folgenden wesentlichen Punkten entsprechen:
-  Sie besitzen dieselbe Anzahl an Parametern für die Partitionierung, und die entsprechenden Parameter sind vom selben Datentyp.
-  Sie definieren dieselbe Anzahl an Partitionen.
-  Sie definieren dieselben Begrenzungswerte für Partitionen.
Dies ermöglicht dem Abfrageoptimierer, den Join schneller zu verarbeiten, da die Partitionen selbst verknüpft werden können. Wenn eine Abfrage zwei Tabellen verknüpft, die nicht angeordnet oder nicht am Joinsfeld miteinander verknüpft sind, kann das Vorhandensein von Partitionen die Abfrageverarbeitung womöglich sogar verlangsamen, anstatt zu beschleunigen.

Weitere Informationen zur Verarbeitung von Partitionen in der Abfrageverarbeitung finden Sie unter [Verbesserte Abfrageverarbeitung bei partitionierten Tabellen und Indizes](../../relational-databases/query-processing-architecture-guide.md#query-processing-enhancements-on-partitioned-tables-and-indexes).

## <a name="behavior-changes-in-statistics-computation-during-partitioned-index-operations"></a>Das Verhalten ändert sich beim Berechnen von Statistiken, während Vorgänge für partitionierte Indizes durchgeführt werden  
 Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]werden Statistiken nicht durch das Scannen aller Zeilen in der Tabelle erstellt, wenn ein partitionierter Index erstellt oder neu erstellt wird. Der Abfrageoptimierer generiert stattdessen Statistiken mithilfe des Standardalgorithmus zur Stichprobenentnahme. Nachdem eine Datenbank mit partitionierten Indizes aktualisiert wurde, bemerken Sie möglicherweise einen Unterschied in den Histogrammdaten für diese Indizes. Diese Änderung des Verhaltens beeinträchtigt die Abfrageleistung möglicherweise nicht. Um Statistiken zu partitionierten Indizes durch das Scannen aller Zeilen in der Tabelle abzurufen, verwenden Sie `CREATE STATISTICS` oder `UPDATE STATISTICS` mit der `FULLSCAN`-Klausel.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Aufgaben|Thema|  
|-|-|   
|Beschreibt das Erstellen von Partitionsfunktionen und Partitionsschemas sowie deren Anwendung auf eine Tabelle und einen Index.|[Erstellen partitionierter Tabellen und Indizes](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)|  
|||  
  
## <a name="related-content"></a>Verwandte Inhalte  
 Die folgenden Whitepaper zu partitionierten Tabellen und Indexstrategien sowie Implementierungen sind möglicherweise für Sie interessant.  
-   [Partitionierte Tabellen- und Indexstrategien für SQL Server 2008](https://msdn.microsoft.com/library/dd578580\(SQL.100\).aspx)    
-   [So implementieren Sie ein automatisch gleitendes Fenster](https://msdn.microsoft.com/library/aa964122\(SQL.90\).aspx)    
-   [Massenladen in eine partitionierte Tabelle](/previous-versions/sql/sql-server-2005/administrator/cc966380(v=technet.10))    
-   [Project REAL: Data Lifecycle -- Partitioning (Project REAL: Lebenszyklus von Daten, Partitionierung)](/previous-versions/sql/sql-server-2005/administrator/cc966424(v=technet.10))    
-   [Verbesserte Abfrageverarbeitung bei partitionierten Tabellen und Indizes](/previous-versions/sql/sql-server-2008-r2/ms345599(v=sql.105))    
-   [Top 10 Best Practices for Building a Large Scale Relational Data Warehouse (Top 10 Best Practices zum Erstellen eines umfassenden relationalen Data Warehouse)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20Relational%20Engine.pdf) in _SQLCATs Handbuch zu Relational Engineering_
  
