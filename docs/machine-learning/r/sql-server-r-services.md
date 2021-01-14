---
title: Was ist SQL Server 2016 R Services?
titleSuffix: ''
description: R Services ist ein Feature in SQL Server 2016, das die Möglichkeit bietet, R-Skripts mit relationalen Daten auszuführen. Sie können Open-Source-Pakete und -Frameworks und die Microsoft R-Pakete für Predictive Analytics und Machine Learning verwenden. Die Skripts werden in der Datenbank ausgeführt, ohne dass Daten aus SQL Server oder über das Netzwerk verschoben werden. In diesem Artikel werden die Grundlagen von SQL Server R Services erläutert.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016
ms.openlocfilehash: 66e023799f69f35b8e220983b261d9e5ae074f24
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102237"
---
# <a name="what-is-sql-server-2016-r-services"></a>Was ist SQL Server 2016 R Services?

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

R Services ist ein Feature in SQL Server 2016, das die Möglichkeit bietet, R-Skripts mit relationalen Daten auszuführen. Sie können Open-Source-Pakete und -Frameworks und die [Microsoft R-Pakete](#packages) für Predictive Analytics und Machine Learning verwenden. Die Skripts werden in der Datenbank ausgeführt, ohne dass Daten aus SQL Server oder über das Netzwerk verschoben werden. In diesem Artikel werden die Grundlagen von SQL Server R Services erläutert.

> [!Note]
> R Services wurde in SQL Server 2017 und höher in [Machine Learning Services](../sql-server-machine-learning-services.md) umbenannt und dient der Unterstützung von Python und R.

## <a name="what-is-r-services"></a>Was sind R Services?

Mit SQL Server R Services können Sie R-Skripts in der Datenbank ausführen. Sie können das Feature verwenden, um Daten vorzubereiten und zu bereinigen, Features zu entwickeln und Machine Learning-Modelle in einer Datenbank zu trainieren, auszuwerten und bereitzustellen. Mit dem Feature können Sie Skripts ausführen, in denen sich die Daten befinden. Die Übertragung der Daten über das Netzwerk auf einen anderen Server entfällt.

In R Services sind Basisverteilungen von R enthalten. Neben den Microsoft-Paketen [RevoScaleR](../r/ref-r-revoscaler.md), [MicrosoftML](../r/ref-r-microsoftml.md), [olapR]../r/ref-r-olapr.md) und [sqlrutils](../r/ref-r-sqlrutils.md) können Sie für R auch Open-Source-Pakete und -Frameworks verwenden.

Zum Ausführen von R-Skripts in SQL Server wird von R Services ein Erweiterbarkeitsframework verwendet. Weitere Informationen zur Funktionsweise finden Sie unter:

+ [Erweiterbarkeitsframework](../concepts/extensibility-framework.md)
+ [R-Erweiterung](../concepts/extension-r.md)

## <a name="what-can-i-do-with-r-services"></a>Welche Möglichkeiten bietet R Services?

Mit R Services können Sie Machine Learning- und Deep Learning-Modelle in SQL Server erstellen und trainieren. Sie können auch vorhandene Modelle für R Services bereitstellen und relationale Daten für Vorhersagen verwenden.

Hier einige Beispiele für die Art von Vorhersagen, für die Sie SQL Server R Services verwenden können:

|Vorhersagetyp|Beispiel|
|-|-|
|Klassifizierung/Kategorisierung|Automatische Einteilung von Kundenfeedback in positive und negative Kategorien|
|Regression/Vorhersage von kontinuierlichen Werten|Vorhersage des Preises für Häuser basierend auf Größe und Standort|
|Erkennung von Anomalien|Erkennung von betrügerischen Banktransaktionen |
|Empfehlungen|Empfehlung von Produkten anhand bisheriger Anschaffungen, die Onlinekunden gefallen könnten|

### <a name="how-to-execute-r-scripts"></a>Ausführen von R-Skripts

Es gibt zwei Möglichkeiten zum Ausführen von R-Skripts in R Services:

+ Die gängigste Methode ist die Verwendung der gespeicherten T-SQL-Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

+ Sie können auch den von Ihnen bevorzugten R-Client verwenden und Skripts schreiben, mit denen die Ausführung an einen Remotecomputer mit SQL Server gepusht wird. (Dies wird als *Remotecomputekontext* bezeichnet.) Weitere Informationen finden Sie unter [Einrichten eines Data Science-Clients für die Entwicklung in R](../r/set-up-a-data-science-client.md).

<a name="version"></a>

## <a name="r-versions"></a>R-Versionen

Im Folgenden werden die Versionen der R-Runtime aufgeführt, die in den SQL Server 2016 R Services enthalten sind.

SQL Server-Version | R-Runtime-Standardversionen |
|-|-|
| SQL Server 2016 RTM – SP2 CU13 | 3.2.2 |
| SQL Server 2016 SP2 CU14 und höher | 3.2.2 und 3.5.2 |

Cumulative Update (CU) 14 für SQL Server 2016 Service Pack (SP) 2 und höher enthalten neuere R-Runtimes. Weitere Informationen finden Sie unter [Ändern der Language Runtime-Standardversion](../install/change-default-language-runtime-version.md).

Verwenden Sie für andere Versionen von R oder zum Ausführen von Python [Machine Learning Services für SQL Server 2017 und höher](../sql-server-machine-learning-services.md).

<a name="packages"></a>

## <a name="r-packages"></a>R-Pakete

Neben den Enterprise-Paketen von Microsoft können Sie auch Open-Source-Pakete und -Frameworks verwenden. Die gängigsten Open-Source-R-Pakete sind in R Services bereits vorinstalliert. Zudem sind die folgenden R-Pakete von Microsoft bereits enthalten:

| Paket | BESCHREIBUNG |
|-|-|
| [RevoScaleR](../r/ref-r-revoscaler.md) | Das primäre Paket mit skalierbaren R-Funktionen zum Transformieren und Bearbeiten von Daten sowie zum Erstellen von statistischen Übersichten, Visualisierungen und vielen anderen Modellierungsformen. Zudem enthält das Paket zur Parallelverarbeitung Funktionen zum automatischen Verteilen von Workloads auf verfügbare Kerne. |
| [MicrosoftML (R)](../r/ref-r-microsoftml.md) | Stellt Machine Learning-Algorithmen zur Erstellung von benutzerdefinierten Modellen für die Text-, Bild- und Stimmungsanalyse bereit. |
| [olapR](../r/ref-r-olapr.md) | R-Funktionen, die für MDX-Abfragen eines SQL Server Analysis Services-OLAP-Cube verwendet werden können. |
| [sqlrutils](../r/ref-r-sqlrutils.md) | Ein Mechanismus zur Verwendung von R-Skripts in gespeicherten T-SQL-Prozeduren sowie zum Registrieren dieser gespeicherten Prozeduren bei einer Datenbank und zum Ausführen der gespeicherten Prozeduren über eine [R-Entwicklungsumgebung](../r/set-up-a-data-science-client.md). |
| [Microsoft R Open](https://mran.microsoft.com/rro) | Bei Microsoft R Open (MRO) handelt es sich um die erweiterte Verteilung von R von Microsoft. Diese umfassende Open-Source-Plattform wird für statistische Analysen und Data Science verwendet. Sie basiert auf R und ist vollständig kompatibel mit R. Zudem beinhaltet sie Funktionen für eine verbesserte Leistung und Reproduzierbarkeit. |

## <a name="how-do-i-get-started-with-rservices"></a>Erste Schritte mit R Services

1. [Installieren von SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

1. Konfigurieren Sie Ihre Entwicklungstools. Verwenden Sie Folgendes:

    + [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) oder [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md) zur Verwendung von T-SQL sowie die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) zur Ausführung Ihres R-Skripts.
    + R auf Ihrem eigenen Entwicklungslaptop oder Ihrer eigenen Entwicklungsarbeitsstation zum Ausführen von Skripts. Mit [RevoScaleR](../r/ref-r-revoscaler.md) können Sie Daten lokal pullen oder die Ausführung remote auf einen Computer mit SQL Server pushen. Weitere Informationen finden Sie unter [Einrichten eines Data Science-Clients für die Entwicklung in R](../r/set-up-a-data-science-client.md).

1. Schreiben des ersten R-Skripts

    + Schnellstart: [Erstellen und Ausführen einfacher R-Skripts in SQL Server](../tutorials/quickstart-r-create-script.md)
    + Schnellstart: [Erstellen und Trainieren eines Vorhersagemodells in R](../tutorials/quickstart-r-train-score-model.md)
    + Tutorial: [Verwenden von R in T-SQL](../tutorials/r-taxi-classification-introduction.md): Daten durchsuchen, Features entwickeln, Modelle trainieren und bereitstellen und Vorhersagen treffen (fünfteilige Reihe)
    + Tutorial: [Verwenden von R Services in R-Tools](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md): Daten durchsuchen, Graphen und Plots erstellen, Features entwickeln, Modelle trainieren und bereitstellen und Vorhersagen treffen (sechsteilige Reihe)

## <a name="next-steps"></a>Nächste Schritte

+ [Installieren von SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)
+ [Einrichten eines Data Science-Clients für die Entwicklung in R](../r/set-up-a-data-science-client.md)