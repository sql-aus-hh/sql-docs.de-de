---
title: CAB-Datei für den kumulativen Updates für SQL Server-downloads | Microsoft-Dokumentation
description: CAB-Downloads für SQL Server 2017-Machine Learning Services und SQL Server 2016 R Services.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/01/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 25568dc5a76283b18affd10ef0419f83515f6403
ms.sourcegitcommit: 615f8b5063aed679495d92a04ffbe00451d34a11
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48232604"
---
# <a name="cab-downloads-for-cumulative-updates-of-sql-server-in-database-analytics-instances"></a>CAB-downloads für kumulative Updates Analysefunktionen von SQL Server in der Datenbank-Instanzen

SQL Server-Instanzen, die für in-Database-Analyse konfiguriert sind, enthalten R- und Python-Funktionen. Diese Features ausliefern in CAB-Dateien installiert und über SQL Server-Setup verarbeitet. Auf dem Internet verbundene Geräte werden die CAB-Updates über Windows Update in der Regel angewendet. Auf getrennten Servern müssen die CAB-Dateien werden heruntergeladen und manuell angewendet werden. 

Dieser Artikel enthält Links zum Herunterladen der CAB-Dateien für die kumulativen Updates. Links werden für sowohl SQL Server 2017 Machine Learning Services (R- und Python) als auch SQL Server 2016 R Services bereitgestellt. Weitere Informationen zu Offlineinstallationen, finden Sie unter [Installieren von SQL Server-Machine learning-Komponenten ohne Internetzugang](sql-ml-component-install-without-internet-access.md#apply-cu).

## <a name="prerequisites"></a>Erforderliche Komponenten

Beginnen Sie mit einer Basisinstallation.

+ Auf SQL Server 2017 Machine Learning Services ist die erste Version die Baseline-Installation. 
+ Auf SQL Server 2016 R Services können Sie mit der ersten Version, SP1 oder SP2 starten. 

Sie können auch kumulative Updates auf einem eigenständigen Server anwenden.

## <a name="sql-server-2017-cabs"></a>SQL Server 2017 CAB-Dateien

CAB-Dateien werden in umgekehrter chronologischer Reihenfolge aufgeführt. Wenn Sie die CAB-Dateien herunterladen und auf den Zielcomputer übertragen, sie in einem geeigneten Ordner speichern wie z. B. **Downloads** oder des Setup-Benutzers % temp %-Ordner.

Release  |Downloadlink  | Die Probleme | 
---------|---------------|-------|
**[SQL Server 2017 CU10](https://support.microsoft.com/help/4342123)-[CU11](https://support.microsoft.com/help/4462262)** |  |  |
Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Keine Änderung gegenüber früheren Versionen. |
R Server      |[SRS_9.2.0.1000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2006287&clcid=1033)| Einige kleinere Korrekturen.|
Öffnen Sie Microsoft-Python     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Keine Änderung gegenüber früheren Versionen. |
Python-Server    |[SPS_9.2.0.1000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2006805&clcid=1033)| Python-Rx_data_step verliert Reihenfolge der Zeilen an, wenn Duplikate entfernt werden. <br/>SPEE schlägt fehl, Erkennung von Datatype für gruppierte columnstore-Index. <br/>Gibt eine leere Tabelle zurück, wenn Spalten alle null-Werte enthalten. |
**[SQL Server 2017 CU8](https://support.microsoft.com/help/4338363)-[CU9](https://support.microsoft.com/help/4341265)** |  |  |
Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Keine Änderung gegenüber früheren Versionen. |
R Server      |[SRS_9.2.0.800_1033.cab](https://go.microsoft.com/fwlink/?LinkId=874708&clcid=1033)|
Öffnen Sie Microsoft-Python     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Keine Änderung gegenüber früheren Versionen. |
Python-Server    |[SPS_9.2.0.800_1033.cab](https://go.microsoft.com/fwlink/?LinkId=874707&clcid=1033)|
**[SQL Server 2017 CU6](https://support.microsoft.com/help/4101464)-[CU7](https://support.microsoft.com/help/4229789)** |  |  |
Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Keine Änderung gegenüber früheren Versionen. |
R Server      |[SRS_9.2.0.600_1033.cab](https://go.microsoft.com/fwlink/?LinkId=871074&clcid=1033)|
Öffnen Sie Microsoft-Python     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Keine Änderung gegenüber früheren Versionen. |
Python-Server    |[SPS_9.2.0.600_1033.cab](https://go.microsoft.com/fwlink/?LinkId=871073&clcid=1033)| DateTime-Datentypen in SPEES Abfrage.<br/>Fehlermeldungen in Microsoftml verbessert, wenn vorab trainierte Modelle fehlen.<br/> Korrekturen für von Revoscalepy transformieren, Funktionen und Variablen.|
**[SQL Server 2017 CU5](https://support.microsoft.com/help/4092643)** |  |  |
Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Keine Änderung gegenüber früheren Versionen. |
R Server      |[SRS_9.2.0.500_1033.cab](https://go.microsoft.com/fwlink/?LinkId=869052&clcid=1033)| Lange pfadbezogene Fehler im RxInstallPackages.<br/>Verbindungen in einen Loopback für RxExec.
Öffnen Sie Microsoft-Python    | Keine Änderung gegenüber früheren Versionen. |
Python-Server    |[SPS_9.2.0.500_1033.cab](https://go.microsoft.com/fwlink/?LinkId=869053&clcid=1033)| <br/>Verbindungen in einen Loopback für Rx_exec.
**[SQL Server 2017 CU4](https://support.microsoft.com/help/4056498)** |  |   |
Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Keine Änderung gegenüber früheren Versionen. |
R Server      |[SRS_9.2.0.400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866212&clcid=1033)|
Öffnen Sie Microsoft-Python     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Keine Änderung gegenüber früheren Versionen. |
 Python-Server    |[SPS_9.2.0.400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866213&clcid=1033)|
**[SQL Server 2017 CU3](https://support.microsoft.com/help/4052987)** |  |  |
Microsoft R Open     |[SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)|
R Server      |[SRS_9.2.0.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863893)|
Öffnen Sie Microsoft-Python     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Keine Änderung gegenüber früheren Versionen. |
Python-Server    |[SPS_9.2.0.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863892)| Serialisierung in Revoscalepy, Python-Modell mithilfe der [Rx_serialize_model Funktion](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-serialize-model).<br/>[Native Bewertung](../sql-native-scoring.md) zudem die Verwendung von Erweiterungen zu [in Echtzeit bewerten](../real-time-scoring.md). 
**SQLServer 2017 [CU1](https://support.microsoft.com/help/4038634)-[CU2](https://support.microsoft.com/help/4052574)** |  |  |
Microsoft R Open     | [SRO_3.3.3.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851496)| Keine Änderung gegenüber früheren Versionen. |
R Server      |[SRS_9.2.0.100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851501)|
Öffnen Sie Microsoft-Python     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Keine Änderung gegenüber früheren Versionen. | 
Python-Server    |[SPS_9.2.0.100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851500) | Fügt Rx_create_col_info für die Rückgabe von Schemainformationen an. <br/>Verbesserungen bei der [Rx_exec](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-exec) zur Unterstützung der paralleler Szenarien für die Verwendung der `RxLocalParallel` compute Context verwenden.|
**Erste Version** |  |  |
Microsoft R Open     |[SRO_3.3.3.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851496)|
R Server      |[SRS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851507)|
Öffnen Sie Microsoft-Python     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502) |
Python-Server    |[SPS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851508) |


<a name="bkmk_2016Installers"></a>

## <a name="sql-server-2016-cabs"></a>SQL Server 2016 CAB-Dateien

Sind für SQL Server 2016 R Services Baseline-Releases, entweder die RTM-Version oder ein Service Pack-Version.

Release  |Downloadlink  |
---------|---------------|
**SQL Server 2016 SP2 CU1-CU2**     |
Microsoft R Open     |[SRO_3.2.2.20000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866039)|
Microsoft R Server    |[SRS_8.0.3.20000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866038)|
**SQL Server 2016 SP1 CU4-CU10**     |
Microsoft R Open     |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836819)|
Microsoft R Server    |[SRS_8.0.3.17000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=850317)
**SQL Server 2016 SP1 CU1-CU3**     |
Microsoft R Open     |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836819)|
Microsoft R Server    |[SRS_8.0.3.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836818)|
**SQL Server 2016 SP1**     |
Microsoft R Open     |[SRO_3.2.2.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824879)
Microsoft R Server     |[SRS_8.0.3.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824881)
**SQL Server 2016 CU4-CU9**     |
Microsoft R Open     |[SRO_3.2.2.13000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=831785)|
Microsoft R Server     |[SRS_8.0.3.13000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=831676)|
**SQL Server 2016 CU2-CU3**     |
Microsoft R Open     |[SRO_3.2.2.12000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=827398)
Microsoft R Server     |[SRS_8.0.3.12000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=827399)
**SQL Server 2016 CU1**     |
Microsoft R Open     |[SRO_3.2.2.10000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=808803)
Microsoft R Server     |[SRS_8.0.3.10000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=808805)
**SQL Server 2016 RTM**     |
Microsoft R Open     |[SRO_3.2.2.803_1033.cab](https://go.microsoft.com/fwlink/?LinkId=761266)
Microsoft R Server     |[SRS_8.0.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=735051)

> [!NOTE]
> 
> Wenn Sie SQL Server 2016 SP1 CU4 oder SP1 CU5 offline zu installieren, laden Sie SRO_3.2.2.16000_1033.cab herunter. Wenn Sie SRO_3.2.2.13000_1033.cab von FWLINK 831785 heruntergeladen, gemäß der einrichten (Dialogfeld), benennen Sie die Datei als SRO_3.2.2.16000_1033.cab vor der Installation des kumulativen Updates.

Wenn Sie den Quellcode für Microsoft R anzeigen möchten, es ist zum Download zur Verfügung als eines Archivs in TAR-Datei vor: [Installationsprogramme für R Server herunterladen](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows#download)

## <a name="see-also"></a>Siehe auch

[Anwenden von kumulativen Updates auf Computern ohne Internetzugriff](sql-ml-component-install-without-internet-access.md#apply-cu)

[Anwenden von kumulativen Updates auf Computern mit Internetverbindung](sql-ml-component-install-without-internet-access.md#apply-cu)

[Kumulative Updates auf einem eigenständigen Server anwenden.](sql-machine-learning-standalone-windows-install.md#apply-cu)
