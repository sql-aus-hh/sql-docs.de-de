---
title: Was sind Big Data-Cluster?
titleSuffix: SQL Server Big Data Clusters
description: Hier erfahren Sie mehr über Big Data-Cluster für SQL Server, die auf Kubernetes ausgeführt werden, und stellen Optionen für das horizontale Skalieren für relationale Daten und HDFS-Daten bereit.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 01/07/2020
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ced001726b7a8306f2064c1436ddb41cf65a6eb0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091808"
---
# <a name="what-are-big-data-clusters-2019"></a>Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ab [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] können Sie mit [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] skalierbare Cluster von Containern für SQL Server, Spark und HDFS bereitstellen, die auf Kubernetes ausgeführt werden. Diese Komponenten werden nebeneinander ausgeführt, sodass Sie Big Data von Transact-SQL oder Spark lesen, schreiben und verarbeiten können, während Sie Ihre wichtigen relationalen Daten mit einem hohen Big-Data-Volumen problemlos kombinieren und analysieren können.

Verwenden Sie [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] für Folgendes:

- [Bereitstellen skalierbarer Cluster](./deploy-get-started.md) aus SQL Server-, Spark- und HDFS-Containern, die auf Kubernetes ausgeführt werden 
- Lesen, Schreiben und Verarbeiten von Big Data von Transact-SQL oder Spark
- Einfaches Kombinieren und Analysieren hochwertiger relationaler Daten mit hohen Volumen von Big Data
- Abfragen externer Datenquellen
- Speichern von Big Data in von SQL Server verwaltetem HDFS
- Abfragen von Daten aus mehreren externen Datenquellen über den Cluster
- Verwenden der Daten für künstliche Intelligenz, maschinelles Lernen und andere Analyseaufgaben
- [Bereitstellen und Ausführen von Anwendungen](./concept-application-deployment.md) in [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]
- Virtualisieren von Daten mit [PolyBase](../relational-databases/polybase/polybase-guide.md). Abfragen von Daten aus externen Datenquellen in SQL Server, Oracle, Teradata, MongoDB und ODBC mit externen Tabellen.
- Bereitstellen der Hochverfügbarkeit für die SQL Server-Masterinstanz und alle Datenbanken mithilfe von Always On-Verfügbarkeitsgruppen

Weitere Informationen zu neuen Features und bekannten Problemen der neuesten Version finden Sie in den dazugehörigen [Versionshinweisen](release-notes-big-data-cluster.md).

## <a name="scenarios"></a>Szenarien

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] bieten Flexibilität bei der Interaktion mit Big Data. Sie können externe Datenquellen abfragen, Big Data in dem von SQL Server verwalteten HDFS speichern oder Daten aus mehreren externen Datenquellen über den Cluster abfragen. Die Daten können Sie dann für KI, Machine Learning und andere Analyseaufgaben verwenden. In den folgenden Abschnitten finden Sie weitere Informationen zu diesen Szenarios.

### <a name="data-virtualization"></a>Datenvirtualisierung

Mit [PolyBase](../relational-databases/polybase/polybase-guide.md) können [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] externe Datenquellen abfragen, ohne die Daten zu verschieben oder zu kopieren. [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] führt neue Connectors für Datenquellen ein.

![Datenvirtualisierung](media/big-data-cluster-overview/data-virtualization.png)

### <a name="data-lake"></a>Data Lake

Ein Big-Data-Cluster für SQL Server enthält einen skalierbaren HDFS-*Speicherpool*. Dieser kann verwendet werden, um Big Data zu speichern, die möglicherweise aus mehreren externen Quellen erfasst wird. Sobald die Big Data im HDFS im Big-Data-Cluster gespeichert wurden, können Sie die Daten analysieren und abfragen und mit ihren relationalen Daten kombinieren.

![Data Lake](media/big-data-cluster-overview/data-lake.png)

### <a name="scale-out-data-mart"></a>Data Mart mit horizontaler Skalierung

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] bieten Rechen- und Speicherkapazitäten für die horizontale Skalierung, um die Leistung bei der Analyse beliebiger Daten zu verbessern. Daten aus einer Vielzahl von Quellen können erfasst und auf *Datenpool*-Knoten als Cache zur weiteren Analyse verteilt werden.

![Data Mart](media/big-data-cluster-overview/data-mart.png)

### <a name="integrated-ai-and-machine-learning"></a>Integrierte KI und Machine Learning

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ermöglichen KI- und Machine Learning-Aufgaben für die Daten, die in HDFS-Speicherpools und den Datenpools gespeichert werden. Mithilfe von R, Python, Scala oder Java können Sie sowohl Spark als auch integrierte KI-Tools in SQL Server verwenden.

![KI und Machine Learning](media/big-data-cluster-overview/ai-ml-spark.png)

### <a name="management-and-monitoring"></a>Verwaltung und Überwachung

Verwaltung und Überwachung werden durch eine Kombination von Befehlszeilentools, APIs, Portalen und dynamischen Verwaltungssichten bereitgestellt.

Sie können [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) dazu verwenden, eine Vielzahl von Aufgaben im Big-Data-Cluster auszuführen:
- Integrierte Codeausschnitte für allgemeine Verwaltungsaufgaben.
- Möglichkeit zum Durchsuchen von HDFS, zum Hochladen von Dateien, zur Vorschau von Dateien und zum Erstellen von Verzeichnissen.
- Möglichkeit zum Erstellen, Öffnen und Ausführen von Jupyter-kompatiblen Notebooks.
- Datenvirtualisierungsassistent für eine vereinfachte Erstellung externer Datenquellen (aktiviert durch die **Datenvirtualisierungserweiterung**)

## <a name="architecture"></a><a id="architecture"></a> Architektur

Ein Big-Data-Cluster für SQL Server ist ein Cluster von Linux-Containern, die von [Kubernetes](https://kubernetes.io/docs/concepts/) orchestriert werden.

### <a name="kubernetes-concepts"></a>Kubernetes-Konzepte

Kubernetes ist ein Open-Source-Containerorchestrator, mit dem Sie Containerbereitstellungen nach Bedarf skalieren können. In der folgenden Tabelle sind einige wichtige Kubernetes-Termini definiert:

|Begriff|BESCHREIBUNG|
|:--|:--|
| **Cluster** | Ein Kubernetes-Cluster ist eine Gruppe von Computern, die als Knoten bezeichnet werden. Ein Knoten steuert den Cluster und wird als Masterknoten bezeichnet. Die übrigen Knoten sind Workerknoten. Der Kubernetes-Master ist für die Verteilung der Arbeit auf die Worker und für die Überwachung der Clusterintegrität verantwortlich. |
| **Node** | Ein Knoten führt Containeranwendungen aus. Dabei kann es sich entweder um einen physischen oder einen virtuellen Computer handeln. Ein Kubernetes-Cluster kann eine Mischung aus Knoten von physischen und virtuellen Computern enthalten. |
| **Pod** | Ein Pod ist die unteilbare Bereitstellungseinheit von Kubernetes. Ein Pod ist eine logische Gruppe von einem oder mehreren Containern und zugeordneter Ressourcen, die zum Ausführen einer Anwendung erforderlich sind. Jeder Pod läuft auf einem Knoten. Ein Knoten kann einen oder mehrere Pods ausführen. Der Kubernetes-Master weist den Knoten im Cluster automatisch Pods zu. |
| &nbsp; ||

In [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ist Kubernetes für den Zustand der [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] verantwortlich. Dabei erstellt und konfiguriert Kubernetes die Clusterknoten, weist den Knoten Pods zu und überwacht die Integrität des Clusters.

### <a name="big-data-clusters-architecture"></a>Architektur von Big-Data-Clustern

Das folgende Diagramm zeigt die Komponenten eines SQL Server-Big-Data-Clusters.

![Übersicht über die Architektur](media/big-data-cluster-overview/architecture-diagram-overview.png)

### <a name="controller"></a><a id="controlplane"></a> Controller

Der Controller bietet Verwaltungs-und Sicherheitsfunktionen für den Cluster. Er enthält den Verwaltungsdienst, den Konfigurationsspeicher und andere Dienste auf Clusterebene wie Kibana, Grafana und Elasticsearch.

### <a name="compute-pool"></a><a id="computeplane"></a> Computepool

Der Computepool stellt Rechenressourcen für den Cluster bereit. Er enthält Knoten, auf denen Pods für SQL Server für Linux laufen. Die Pods im Computepool werden für bestimmte Verarbeitungsaufgaben in *SQL-Computeinstanzen* unterteilt. 

### <a name="data-pool"></a><a id="dataplane"></a> Datenpool

Der Datenpool wird für Datenpersistenz und zum Zwischenspeichern verwendet. Der Datenpool besteht aus mindestens einem Pod, auf dem SQL Server für Linux ausgeführt wird. Er wird zum Erfassen von Daten aus SQL-Abfragen oder Spark-Aufträgen verwendet. Data Marts für SQL Server-Big-Data-Cluster werden im Datenpool persistent gespeichert. 

### <a name="storage-pool"></a>Speicherpool

Der Speicherpool besteht aus den Speicherpoolpods, bestehend aus SQL Server für Linux, Spark und HDFS. Alle Speicherknoten in einem Big-Data-Cluster für SQL Server sind Mitglieder eines HDFS-Clusters.

> [!TIP]
> Einen detaillierten Einblick in die Architektur und Installation von Big-Data-Clustern erhalten Sie unter [Workshop: Microsoft-Architektur für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/microsoft/sqlworkshops-bdc).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Erste Schritte mit [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-get-started.md).