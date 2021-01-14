---
title: Verwaltung mit dem Resource Governor
description: In diesem Artikel erfahren Sie, wie Sie den Resource Governor zum Verwalten der Ressourcenzuordnung der CPU, physischen E/A und des Arbeitsspeichers für Python- und R-Arbeitsauslastungen in SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3e71b33eb08fd386232e992e9b47da7b5057aa32
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170722"
---
# <a name="manage-python-and-r-workloads-with-resource-governor-in-sql-server-machine-learning-services"></a>Verwalten von Python- und R-Arbeitsauslastungen mit dem Resource Governor in SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Artikel erfahren Sie, wie Sie den [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) zum Verwalten der Ressourcenzuordnung der CPU, physischen E/A und des Arbeitsspeichers für Python- und R-Arbeitsauslastungen in SQL Server Machine Learning Services.

Machine Learning-Algorithmen in Python und R sind sehr rechenintensiv. Abhängig von Ihren Arbeitsauslastungsprioritäten müssen Sie die für Machine Learning Services verfügbaren Ressourcen möglicherweise erhöhen oder reduzieren.

Weitere allgemeine Informationen finden Sie unter [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

> [!NOTE] 
> Der Resource Governor ist ein Feature der Enterprise Edition.

## <a name="default-allocations"></a>Standardzuteilungen

Die externen Skriptruntimes für maschinelles Lernen sind standardmäßig auf weniger als 20 % des gesamten Computerarbeitsspeichers beschränkt. Dies hängt zwar von Ihrem System ab, jedoch könnte sich dieser Grenzwert im Allgemeinen für aufwendigere Machine Learning-Aufgaben wie das Trainieren eines Modells oder Vorhersagen vieler Datenzeilen als unzureichend erweisen. 

## <a name="manage-resources-with-resource-governor"></a>Verwalten von Ressourcen mit dem Resource Governor
 
Externe Prozesse nutzen standardmäßig bis zu 20 % des gesamten Hostarbeitsspeichers auf dem lokalen Server. Sie können Änderungen am Standardressourcenpool vornehmen, um serverweite Änderungen zu bewirken, damit R- und Python-Prozesse die Kapazitäten nutzen, die Sie für externe Prozesse zur Verfügung stellen.

Optional können Sie benutzerdefinierte **externe Ressourcenpools** mit zugehörigen Arbeitsauslastungsgruppen und Klassifizierern erstellen, um die Ressourcenzuordnung für Anforderungen von spezifischen Programmen, Hosts oder nach anderen von Ihnen angegebenen Kriterien zu bestimmen. Ein externer Ressourcenpool ist eine Art Ressourcenpool, die in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] eingeführt wurde, um die Verwaltung von externen R- und Python-Prozessen für die Datenbank-Engine zu unterstützen.

1. [Aktivieren Sie den Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md) (er ist standardmäßig deaktiviert).

2. Führen Sie [CREATE EXTERNAL RESOURCE POOL](../../t-sql/statements/create-external-resource-pool-transact-sql.md) aus, um den Ressourcenpool zu erstellen und zu konfigurieren, und führen Sie anschließend [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md) aus, um ihn zu implementieren.

3. Erstellen Sie eine Arbeitsauslastungsgruppe für genaue Zuteilungen, z. B. zwischen Training und Bewertung.

4. Erstellen Sie einen Klassifizierer, um Aufrufe für externe Verarbeitung abzufangen.

5. Führen Sie Abfragen und Prozeduren mithilfe der erstellten Objekte aus.

Eine exemplarische Vorgehensweise mit genauen Anweisungen finden Sie unter [Erstellen eines Ressourcenpools für SQL Server Machine Learning Services](create-external-resource-pool.md).

Eine Einführung in die Terminologie und allgemeinen Konzepte finden Sie unter [Resource Governor-Ressourcenpool](../../relational-databases/resource-governor/resource-governor-resource-pool.md).

## <a name="processes-under-resource-governance"></a>Vom Resource Governor verwaltete Prozesse
  
 Sie können einen *externen Ressourcenpool* verwenden, um die Ressourcen zu verwalten, die von den folgenden ausführbaren Dateien in einer Datenbank-Engine-Instanz verwendet werden:

+ „Rterm.exe“, wenn sie lokal von SQL Server oder remote mit SQL Server als Remotecomputekontext aufgerufen wird
+ „Python.exe“, wenn sie lokal von SQL Server oder remote mit SQL Server als Remotecomputekontext aufgerufen wird
+ BxlServer.exe und Satellitenprozesse
+ Satellitenprozesse, die von Launchpad gestartet werden, z. B. „PythonLauncher.dll“
  
> [!NOTE]
> Die direkte Verwaltung des Launchpad-Diensts mithilfe des Resource Governor wird nicht unterstützt. Launchpad ist ein vertrauenswürdiger Dienst, der nur von Microsoft bereitgestellte Startprogramme hosten kann. Vertrauenswürdige Startprogramme sind explizit dafür konfiguriert, übermäßigen Ressourcenverbrauch zu vermeiden.
  
## <a name="next-steps"></a>Nächste Schritte

+ [Erstellen eines Ressourcenpools für Machine Learning](create-external-resource-pool.md)
+ [Resource Governor-Ressourcenpools](../../relational-databases/resource-governor/resource-governor-resource-pool.md)