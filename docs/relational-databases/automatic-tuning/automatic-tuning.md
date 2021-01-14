---
title: Automatische Optimierung | Microsoft-Dokumentation
description: Erfahren Sie mehr über die automatische Optimierung in SQL Server und Azure SQL-Datenbank
ms.custom: fasttrack-edit
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- performance tuning [SQL Server]
- automatic tuning
- aprc
- automatic plan regression correction
- last known good plan
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9c01a9e02576d666c39df13dc6e7e01f6d622a7d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169872"
---
# <a name="automatic-tuning"></a>Automatische Optimierung
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

Die automatische Datenbankoptimierung bietet einen Einblick in die potentiellen Abfrageleistungsprobleme, empfiehlt Lösungen und kann identifizierte Probleme automatisch beheben.

Bei der automatischen Optimierung, die in eingeführt wurde, werden [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Sie benachrichtigt, sobald ein mögliches Leistungsproblem erkannt wird, und Sie können Korrekturmaßnahmen anwenden, oder die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] Leistungsprobleme können von automatisch behoben werden. Die automatische Optimierung [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht es Ihnen, Leistungsprobleme zu identifizieren und zu beheben, die durch eine **Abfrage Ausführungsplan Auswahl Regressionen** verursacht werden. Bei der automatischen Optimierung in werden [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] auch erforderliche Indizes erstellt und nicht verwendete Indizes gelöscht. Weitere Informationen zu Abfrage Ausführungsplänen finden Sie unter [Ausführungspläne](../../relational-databases/performance/execution-plans.md).

[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Überwacht die Abfragen, die für die Datenbank ausgeführt werden, und verbessert die Leistung der Arbeitsauslastung automatisch. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]Verfügt über einen integrierten Intelligence-Mechanismus, mit dem die Leistung Ihrer Abfragen automatisch optimiert und verbessert werden kann, indem die Datenbank dynamisch an ihre Arbeitsauslastung angepasst wird. Es stehen zwei Features für die automatische Optimierung zur Verfügung:

-   Die **Automatische Plan Korrektur** identifiziert problematische Abfrage Ausführungspläne, wie z. b. eine [Parameter Sensitivität oder Parametersniffing](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) -Probleme, und korrigiert Leistungsprobleme im Zusammenhang mit dem Abfrage Ausführungsplan, indem Sie den letzten bekannten guten Plan vor der Regression erzwingt. **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

-   Die **Automatische Index Verwaltung** identifiziert Indizes, die in der Datenbank hinzugefügt werden sollen, und Indizes, die entfernt werden sollen. **Gilt für:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

## <a name="why-automatic-tuning"></a>Gründe für die automatische Optimierung

Zu den drei Hauptaufgaben bei der klassischen Datenbankverwaltung gehören die Überwachung der Arbeitsauslastung, das Identifizieren kritischer [!INCLUDE[tsql_md](../../includes/tsql-md.md)] Abfragen und das Identifizieren von Indizes, die hinzugefügt werden sollen, um die Leistung zu verbessern, oder Indizes, die selten verwendet werden und zur Verbesserung der Leistung entfernt werden. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Bietet detaillierte Einblicke in die Abfragen und Indizes, die Sie überwachen müssen. Eine kontinuierliche Überwachung der Datenbank ist jedoch eine schwierige und aufwendige Aufgabe, insbesondere bei sehr vielen Datenbanken. Die Verwaltung einer großen Anzahl von Datenbanken ist möglicherweise nicht effizient. Anstatt die Datenbank manuell zu überwachen und zu optimieren, empfiehlt es sich ggf., einige der Überwachungs-und Optimierungs Aktionen an die Funktion zur automatischen Optimierung zu delegieren [!INCLUDE[ssde_md](../../includes/ssde_md.md)] .

### <a name="how-does-automatic-tuning-work"></a>Wie funktioniert die automatische Optimierung?

Die automatische Optimierung ist ein kontinuierlicher Überwachungs-und Analyseprozess, der ständig die Merkmale Ihrer Arbeitsauslastung erfährt und potenzielle Probleme und Verbesserungen identifiziert.

![Automatischer Optimierungsprozess](./media/tuning-process.png)

Dadurch kann die Datenbank dynamisch an ihre Arbeitsauslastung angepasst werden, indem ermittelt wird, welche Indizes und Pläne die Leistung Ihrer Workloads verbessern können und welche Indizes ihre Workloads beeinflussen. Basierend auf diesen Ergebnissen wendet die automatische Optimierung Optimierungs Aktionen an, die die Leistung Ihrer Arbeitsauslastung verbessern. Außerdem überwacht die automatische Optimierung kontinuierlich die Leistung der Datenbank nach der Implementierung von Änderungen, um sicherzustellen, dass Sie die Leistung Ihrer Arbeitsauslastung verbessert. Jede Aktion, die die Leistung nicht verbessert hat, wird automatisch rückgängig gemacht. Dieser Überprüfungs Vorgang ist ein wichtiges Feature, das sicherstellt, dass alle Änderungen, die durch die automatische Optimierung vorgenommen werden, die Gesamtleistung der Arbeitsauslastung nicht verringern.

## <a name="automatic-plan-correction"></a>Automatische Plankorrektur

Die automatische Plan Korrektur ist eine Funktion zur automatischen Optimierung, die die **Regression der Ausführungsplan Auswahl** identifiziert und das Problem automatisch durch Erzwingen des letzten bekannten guten Plans korrigiert. Weitere Informationen zu Abfrage Ausführungsplänen und zum Abfrageoptimierer finden Sie im [Handbuch zur Architektur der Abfrage Verarbeitung](../../relational-databases/query-processing-architecture-guide.md).

> [!IMPORTANT]
> Die automatische Plan Korrektur hängt davon ab, Abfragespeicher in der Datenbank für die workloadverfolgung aktiviert wird. 

### <a name="what-is-execution-plan-choice-regression"></a>Was ist eine Regression der Ausführungsplan Auswahl?

Der [!INCLUDE[ssdenoversion_md](../../includes/ssdenoversion_md.md)] kann verschiedene Ausführungspläne zum Ausführen der [!INCLUDE[tsql_md](../../includes/tsql-md.md)] Abfragen verwenden. Abfrage Pläne sind abhängig von den Statistiken, Indizes und anderen Faktoren. Der optimale Plan, der zum Ausführen einer Abfrage verwendet werden sollte, kann sich im [!INCLUDE[tsql_md](../../includes/tsql-md.md)] Laufe der Zeit je nach den Änderungen dieser Faktoren ändern. In einigen Fällen ist der neue Plan möglicherweise nicht besser als der vorherige Plan, und der neue Plan kann zu einer Leistungs Regression führen, wie z. b. ein Problem mit der [Parameter Sensitivität oder einer Parameter](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) Ermittlung. 

 ![Regression der Abfrage Ausführungsplan Auswahl](media/plan-choice-regression.png "Regression der Abfrage Ausführungsplan Auswahl") 

Wenn Sie feststellen, dass eine Plan Auswahl Regression aufgetreten ist, sollten Sie einen vorherigen guten Plan finden und erzwingen, dass er anstelle der aktuellen verwendet wird. Dies kann mithilfe des `sp_query_store_force_plan` Verfahrens erfolgen. Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] in [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] enthält Informationen zu zurück gestellten Plänen und empfohlenen Korrekturmaßnahmen. Außerdem [!INCLUDE[ssde_md](../../includes/ssde_md.md)] ermöglicht es Ihnen, diesen Prozess vollständig zu automatisieren und das [!INCLUDE[ssde_md](../../includes/ssde_md.md)] Problem zu beheben, das im Zusammenhang mit der Planänderung zu finden ist.

> [!IMPORTANT]
> Die automatische Plan Korrektur sollte im Rahmen eines Upgrades des Datenbank-Kompatibilitäts Niveaus verwendet werden, nachdem eine Baseline aufgezeichnet wurde, um die Risiken bei der Arbeitsauslastung automatisch zu verringern. Weitere Informationen zu diesem Anwendungsfall finden Sie unter [aufrechterhalten einer stabilen Leistung während des Upgrades auf neuere SQL Server](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). 

### <a name="automatic-plan-choice-correction"></a>Automatische Korrektur der Planauswahl

Der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] kann automatisch zum letzten bekannten guten Plan wechseln, wenn eine Plan Auswahl Regression erkannt wird.

![Auswahl Korrektur für Abfrage Ausführungsplan](media/force-last-good-plan.png "Auswahl Korrektur für Abfrage Ausführungsplan") 

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]Erkennt automatisch jede mögliche Regression der Plan Auswahl, einschließlich des Plans, der anstelle des falschen Plans verwendet werden sollte. Wenn das den [!INCLUDE[ssde_md](../../includes/ssde_md.md)] letzten bekannten guten Plan anwendet, bevor die Regression aufgetreten ist, wird automatisch die Leistung des erzwungenen Plans überwacht. Wenn der erzwungene Plan nicht besser als der zurück gestellte Plan ist, wird der neue Plan nicht erzwungen, und der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] kompiliert einen neuen Plan. Wenn überprüft, ob [!INCLUDE[ssde_md](../../includes/ssde_md.md)] der erzwungene Plan besser als der zurück gestellte Plan ist, wird der erzwungene Plan beibehalten. Sie wird bis zur erneuten Kompilierung beibehalten (z. b. bei der nächsten Statistik Aktualisierung oder Schema Änderung). Weitere Informationen zum Erzwingen von Plänen und Typen von Plänen, die erzwungen werden können, finden Sie unter [Plan Erzwingung von Einschränkungen](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md#plan-forcing-limitations).

> [!NOTE]
> Wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Instanz neu gestartet wird, bevor eine Plan Erzwingungs Aktion überprüft wird, wird der Plan automatisch nicht erzwungen. Andernfalls wird die Plan [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Erzwingung bei Neustarts beibehalten.

### <a name="enabling-automatic-plan-choice-correction"></a>Aktivieren der automatischen Korrektur der Planauswahl

Sie können die automatische Optimierung pro Datenbank aktivieren und angeben, dass der letzte geeignete Plan immer dann erzwungen werden soll, wenn eine Plan Änderungs Regression erkannt wird. Die automatische Optimierung wird durch folgenden Befehl aktiviert:

```sql   
ALTER DATABASE <yourDatabase>
SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON ); 
```

Nachdem Sie diese Option aktiviert haben, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] wird von automatisch jede Empfehlung erzwungen, bei der der geschätzte CPU-Zuwachs mehr als 10 Sekunden beträgt, oder die Anzahl der Fehler im neuen Plan ist höher als die Anzahl der Fehler im empfohlenen Plan und überprüft, ob der erzwungene Plan besser als der aktuelle ist.

### <a name="alternative---manual-plan-choice-correction"></a>Alternative – manuelle Korrektur der Planauswahl

Ohne automatische Optimierung müssen Benutzer das System in regelmäßigen Abständen überwachen und nach Abfragen suchen, die zurückgeführt haben. Wenn ein Plan rückgängig gemacht wurde, sollte der Benutzer einen vorherigen guten Plan suchen und ihn anstelle des aktuellen Plans mithilfe von `sp_query_store_force_plan` Procedure erzwingen. Die bewährte Vorgehensweise besteht darin, den letzten als funktionierend bekannten Plan zu erzwingen, da ältere Pläne aufgrund von Statistik-oder Indexänderungen möglicherweise ungültig sind. Der Benutzer, der den letzten bekannten guten Plan erzwingt, sollte die Leistung der Abfrage überwachen, die mit dem erzwungenen Plan ausgeführt wird, und überprüfen, ob der erzwungene Plan erwartungsgemäß funktioniert. Abhängig von den Ergebnissen der Überwachung und Analyse sollte der Plan erzwungen werden, oder der Benutzer sollte eine andere Möglichkeit finden, um die Abfrage zu optimieren, z. b. das erneute Schreiben. Manuell erzwungene Pläne sollten nicht immer erzwungen werden, da der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] in der Lage sein sollte, optimale Pläne anzuwenden. Der Benutzer oder DBA sollte die Erzwingungs Planung des Plans schließlich mithilfe der `sp_query_store_unforce_plan` Prozedur erzwingen und den [!INCLUDE[ssde_md](../../includes/ssde_md.md)] optimalen Plan suchen lassen. 

> [!TIP]
> Verwenden Sie alternativ die **Abfragen mit erzwungenen Plänen** Abfragespeicher Sicht, um Pläne zu suchen und deren erzwungene aufgängig zu machen.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet alle notwendigen Sichten und Prozeduren, die zur Überwachung der Leistung und Behebung von Problemen in Abfragespeicher erforderlich sind.

In [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] können Sie mit Abfragespeicher System Sichten Plan Auswahl Regressionen suchen. Ab [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] [!INCLUDE[ssde_md](../../includes/ssde_md.md)] erkennt und zeigt mögliche Regressionen der Plan Auswahl und die empfohlenen Aktionen an, die im [sys.dm_db_tuning_recommendations &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) -DMV angewendet werden sollten. Die DMV zeigt Informationen über das Problem, die Wichtigkeit des Problems und Details wie z. b. die identifizierte Abfrage, die ID des zurück gestellten Plans, die ID des Plans, der als Baseline für den Vergleich verwendet wurde, und die Anweisung, die [!INCLUDE[tsql_md](../../includes/tsql-md.md)] ausgeführt werden kann, um das Problem zu beheben.

| Typ | Beschreibung | datetime | Ergebnis Ihrer App | Details | ... |
| --- | --- | --- | --- | --- | --- |
| `FORCE_LAST_GOOD_PLAN` | Die CPU-Zeit wurde von 4 ms auf 14 ms geändert. | 3/17/2017 | 83 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |
| `FORCE_LAST_GOOD_PLAN` | Die CPU-Zeit wurde von 37 MS auf 84 MS geändert. | 16.3.2017 | 26 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |

Einige Spalten aus dieser Sicht werden in der folgenden Liste beschrieben:
 - Der Typ der empfohlenen Aktion `FORCE_LAST_GOOD_PLAN` .
 - Beschreibung, die Informationen enthält, weshalb der der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] Meinung ist, dass diese Planänderung eine potenzielle Leistungs Regression ist.
 - DateTime, wenn die potenzielle Regression erkannt wird.
 - Bewertung dieser Empfehlung.
 - Details zu den Problemen, wie z. b. die ID des erkannten Plans, die ID des zurück gestellten Plans, die ID des Plans, der zur Behebung des Problems gezwungen werden soll, das Skript, das ggf [!INCLUDE[tsql_md](../../includes/tsql-md.md)] . zur Behebung des Problems verwendet werden kann, usw. Details werden im [JSON-Format](../json/json-data-sql-server.md)gespeichert.

Verwenden Sie die folgende Abfrage zum Abrufen eines Skripts, das das Problem behebt, und zusätzliche Informationen zum geschätzten Gewinn:

```sql   
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]     

| reason | Ergebnis Ihrer App | Skript | Abfrage- \_ ID | ID des aktuellen Plans \_ | Empfohlene Plan- \_ ID | Geschätzter \_ Gewinn | Fehler \_ anfällig
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Die CPU-Zeit wurde von 3 MS in 46 MS geändert. | 36 | Exec SP \_ Query \_ Store \_ Force \_ Plan 12, 17; | 12 | 28 | 17 | 11,59 | 0

Die-Spalte `estimated_gain` stellt die geschätzte Anzahl von Sekunden dar, die gespeichert werden, wenn der empfohlene Plan für die Abfrage Ausführung anstelle des aktuellen Plans verwendet würde. Der empfohlene Plan sollte anstelle des aktuellen Plans erzwungen werden, wenn der Gewinn größer als 10 Sekunden ist. Wenn im aktuellen Plan mehr Fehler (z. b. Timeouts oder abgebrochene Ausführungen) als im empfohlenen Plan vorhanden sind, wird die Spalte `error_prone` auf den Wert festgelegt `YES` . Ein fehleranfälliger Plan ist ein weiterer Grund, warum der empfohlene Plan anstelle des aktuellen Plans erzwungen werden sollte.

Obwohl die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] alle Informationen bereitstellt, die zum Identifizieren von Plan Auswahl Regressionen erforderlich sind, kann die kontinuierliche Überwachung und das Beheben von Leistungsproblemen zu einem mühsamen Prozess werden. Durch die automatische Optimierung wird dieser Prozess erheblich vereinfacht.

> [!NOTE]
> Die Daten in der `sys.dm_db_tuning_recommendations` DMV werden zwischen den Neustarts der- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Instanz nicht beibehalten.

## <a name="automatic-index-management"></a>Automatische Indexverwaltung

In [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] ist die Index Verwaltung einfach, da [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] ihre Arbeitsauslastung erfährt und sicherstellt, dass Ihre Daten immer optimal indiziert werden. Das richtige Indexdesign ist von entscheidender Bedeutung für die optimale Leistung Ihrer Workload, und die automatische Indexverwaltung kann Ihnen bei der Optimierung Ihrer Indizes behilflich sein. Mit der automatischen Indexverwaltung können entweder Leistungsprobleme in falsch indizierten Datenbanken behoben werden, oder es können Indizes im vorhandenen Datenbankschema verwaltet und verbessert werden. Die automatische Optimierung in [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] führt die folgenden Aktionen aus:

 - Identifiziert Indizes, die die Leistung von Abfragen verbessern können [!INCLUDE[tsql_md](../../includes/tsql-md.md)] , die Daten aus den Tabellen lesen.
 - Identifiziert redundante Indizes oder Indizes, die nicht in längerer Zeit verwendet wurden, die entfernt werden konnten. Durch das Entfernen unnötiger Indizes wird die Leistung von Abfragen verbessert, die Daten in Tabellen aktualisieren.

### <a name="why-do-you-need-index-management"></a>Gründe für die Nutzung der Indexverwaltung

Indizes beschleunigen einige Ihrer Abfragen, die Daten aus den Tabellen lesen, aber Sie können die Abfragen, mit denen Daten aktualisiert werden, verlangsamen. Sie müssen sorgfältig analysieren, wann Sie einen Index erstellen und welche Spalten Sie in den Index einbinden müssen. Einige Indizes werden nach einiger Zeit unter Umständen nicht mehr benötigt. Daher müssen Sie diese Indizes in regelmäßigen Abständen identifizieren und löschen, die keine Vorteile bieten. Wenn Sie die nicht verwendeten Indizes ignorieren, würde die Leistung der Abfragen, mit denen Daten aktualisiert werden, ohne jeglichen Vorteil der Abfragen, die Daten lesen, verringert werden. Nicht verwendete Indizes wirken sich außerdem auf die Gesamtleistung des Systems aus, da für zusätzliche Updates eine unnötige Protokollierung erforderlich ist.

Zur Ermittlung des optimalen Satzes mit Indizes, mit denen die Leistung der Abfragen zum Lesen von Daten aus Ihren Tabellen verbessert wird und die eine minimale Auswirkung auf Updates hat, ist ggf. eine fortlaufende und komplexe Analyse erforderlich.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] verwendet integrierte und erweiterte Regeln, mit denen Ihre Abfragen analysiert, Indizes identifiziert werden, die für Ihre aktuellen Workloads optimal geeignet sind, und die Indizes identifizieren, die möglicherweise entfernt werden müssen. Azure SQL-Datenbank stellt sicher, dass Sie über einen minimalen erforderlichen Satz von Indizes verfügen, die die Abfragen optimieren, die Daten lesen, mit minimierter Auswirkung auf die anderen Abfragen.

### <a name="automatic-index-management"></a>Automatische Indexverwaltung

Zusätzlich zur Erkennung [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] können identifizierte Empfehlungen von automatisch angewendet werden. Wenn Sie feststellen, dass die integrierten Regeln die Leistung Ihrer Datenbank verbessern, können Sie die [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] Indizes automatisch verwalten.

Informationen zum Aktivieren der automatischen Optimierung in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und zur vollständigen Verwaltung der Arbeitsauslastung durch die Funktion zur automatischen Optimierung finden Sie unter [Aktivieren der automatischen Optimierung in Azure SQL-Datenbank mithilfe von Azure-Portal](/azure/sql-database/sql-database-automatic-tuning-enable).

Wenn [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] eine CREATE Index-oder Drop Index-Empfehlung anwendet, wird automatisch die Leistung der Abfragen überwacht, die vom Index betroffen sind. Der neue Index wird nur beibehalten, wenn die Leistung der betroffenen Abfragen verbessert wird. Der gelöschte Index wird automatisch neu erstellt, wenn einige Abfragen aufgrund der Abwesenheit des Indexes langsamer ausgeführt werden.

### <a name="automatic-index-management-considerations"></a>Aspekte der automatischen Indexverwaltung

Aktionen, die erforderlich sind, um erforderliche Indizes in zu erstellen, [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] können Ressourcen beanspruchen und die workloadleistung temporell Um die Auswirkungen der Indexerstellung auf die workloadleistung zu minimieren, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  sucht ein geeignetes Zeitfenster für jeden Index Verwaltungsvorgang. Die Optimierungs Aktion wird verschoben, wenn die Datenbank Ressourcen zum Ausführen der Arbeitsauslastung benötigt und neu gestartet wird, wenn die Datenbank über ausreichend nicht verwendete Ressourcen verfügt, die für den Wartungs Task verwendet werden können. Ein wichtiges Feature der automatischen Indexverwaltung ist die Überprüfung der Aktionen. Wenn [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  einen Index erstellt oder löscht, analysiert ein Überwachungsprozess die Leistung der Arbeitsauslastung, um zu überprüfen, ob die Gesamtleistung durch die Aktion verbessert wurde. Wenn dies nicht zu einer erheblichen Verbesserung geführt hat, wird die Aktion sofort wieder hergestellt. Auf diese Weise wird [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  sichergestellt, dass sich automatische Optimierungs Aktionen nicht negativ auf die Leistung der Arbeitsauslastung auswirken. Indizes, die mit der automatischen Optimierung erstellt werden, sind für den Wartungsvorgang des zugrunde liegenden Schemas transparent. Schemaänderungen, wie das Verwerfen oder Umbenennen von Spalten, werden durch das Vorhandensein von automatisch erstellten Indizes nicht blockiert. Indizes, die automatisch von erstellt werden, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  werden sofort gelöscht, wenn die verknüpften Tabellen gelöscht werden.

### <a name="alternative---manual-index-management"></a>Alternative: manuelle Index Verwaltung

Ohne die automatische Index Verwaltung muss ein Benutzer oder DBA die [sys.dm_db_missing_index_details &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) anzeigen oder den Leistungs Dashboard-Bericht in verwenden, [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] um Indizes zu suchen, die möglicherweise die Leistung verbessern, Indizes mithilfe der in dieser Ansicht bereitgestellten Details erstellen und die Leistung der Abfrage manuell überwachen. Um die Indizes zu ermitteln, die gelöscht werden sollen, sollten die Benutzer die Betriebs Verwendungs Statistik der Indizes überwachen, um selten verwendete Indizes zu finden.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] vereinfacht diesen Prozess. [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] analysiert ihre Arbeitsauslastung, identifiziert die Abfragen, die mit einem neuen Index schneller ausgeführt werden können, und identifiziert nicht verwendete oder duplizierte Indizes. Weitere Informationen zur Identifikation von Indizes, die geändert werden sollten, finden Sie unter [Azure SQL Database Advisor im Azure-Portal](/azure/sql-database/sql-database-advisor-portal).

## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_automatic_tuning_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)  
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sp_query_store_force_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)     
 [sp_query_store_unforce_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)           
 [sys.database_query_store_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [JSON-Funktionen](../json/json-data-sql-server.md)    
 [Ausführungspläne](../../relational-databases/performance/execution-plans.md)    
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Tools für die Leistungsüberwachung und -optimierung](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
 [Abfrageoptimierungs-Assistent](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)