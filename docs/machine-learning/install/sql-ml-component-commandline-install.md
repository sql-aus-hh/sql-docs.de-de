---
title: Installation über die Eingabeaufforderung
description: Führen Sie die SQL Server-Einrichtung über die Befehlszeile aus, um einer SQL Server-Datenbank-Engine-Instanz Machine Learning Services mit Python und R hinzuzufügen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/07/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9b0bf3444ab09cbbbb4aa8eca225dfa9964898c3
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005325"
---
# <a name="install-sql-server-machine-learning-services-with-r-and-python-from-the-command-line"></a>Installieren von SQL Server Machine Learning Services mit R und Python über die Befehlszeile
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Dieser Artikel enthält Anweisungen zum Installieren von [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) mit Python und R über eine Befehlszeile.

Über die Setupbenutzeroberfläche können Sie eine automatische, eine Standard- oder eine vollständige Interaktion festlegen. Dieser Artikel ergänzt [Installieren von SQL Server von der Eingabeaufforderung](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) und beschreibt die Parameter, die für die R- und Python-Komponenten für maschinelles Lernen erforderlich sind.

## <a name="pre-install-checklist"></a>Prüfliste vor der Installation

+ Führen Sie die Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus. 

+ Für datenbankinterne Installationen ist eine Datenbank-Engine-Instanz erforderlich. Sie können nicht nur R- oder Python-Funktionen installieren, aber Sie können diese [einer vorhandenen Instanz inkrementell hinzufügen](#add-existing). Wenn Sie nur R und Python ohne die Datenbank-Engine installieren möchten, installieren Sie den [eigenständigen Server](#shared-feature).

+ Führen Sie die Installation nicht auf einem Failovercluster aus. Der Sicherheitsmechanismus, der zum Isolieren von R- und Python-Prozessen verwendet wird, ist mit einer Windows Server-Failoverclusterumgebung nicht kompatibel.

+ Führen Sie die Installation nicht auf einem Domänencontroller aus. Bei dem Teil des Setups, der sich auf Machine Learning Services bezieht, tritt ein Fehler auf.

+ Vermeiden Sie es, eigenständige und datenbankinterne Instanzen auf demselben Computer zu installieren. Ein eigenständiger Server wird die gleichen Ressourcen nutzen, wodurch die Leistung beider Installationen untergraben wird.

## <a name="command-line-arguments"></a>Befehlszeilenargumente

Das Argument **FEATURES** ist ebenso wie Lizenzierungsbedingungen erforderlich. 

Wenn Sie über die Eingabeaufforderung installieren, unterstützt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des Parameters **/Q** den vollständigen stillen Modus oder mithilfe des Parameters **/QS** den einfachen stillen Modus. Mithilfe des Schalters **/QS** wird nur der Fortschritt angezeigt, es sind jedoch keine Eingaben möglich. Außerdem werden beim Auftreten von Fehlern keine Fehlermeldungen angezeigt. Der Parameter **/QS** wird nur unterstützt, wenn **/Action=install** angegeben wurde.

::: moniker range="=sql-server-2016"
| Argumente | BESCHREIBUNG |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installiert die datenbankinterne Version: SQL Server R Services (datenbankintern).  |
| /FEATURES = SQL_SHARED_MR | Installiert das R-Feature für die eigenständige Version: SQL Server R Server (eigenständig). Ein eigenständiger Server ist ein „freigegebenes Feature“, das nicht an eine Datenbank-Engine-Instanz gekoppelt ist.|
| /IACCEPTROPENLICENSETERMS  | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung der Open-Source-R-Komponenten zugestimmt haben. |
| /IACCEPTPYTHONLICENSETERMS | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung der Python-Komponenten zugestimmt haben. |
| /IACCEPTSQLSERVERLICENSETERMS | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung von SQL Server zugestimmt haben.|
| MRCACHEDIRECTORY | Legt bei Offlinesetups den Ordner fest, der die CAB-Dateien für die R-Komponente enthält. |
::: moniker-end

::: moniker range="=sql-server-2017"
| Argumente | BESCHREIBUNG |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installiert die datenbankinterne Version: SQL Server Machine Learning Services (datenbankintern).  |
| /FEATURES = SQL_INST_MR | Koppeln Sie dieses Argument mit AdvancedAnalytics. Installiert das (datenbankinterne) R-Feature, einschließlich Microsoft R Open und der proprietären R-Pakete. |
| /FEATURES = SQL_INST_MPY | Koppeln Sie dieses Argument mit AdvancedAnalytics. Installiert das (datenbankinterne) Python-Feature, einschließlich Anaconda und der proprietären Python-Pakete. |
| /FEATURES = SQL_SHARED_MR | Installiert das R-Feature für die eigenständige Version: SQL Server Machine Learning Server (eigenständig). Ein eigenständiger Server ist ein „freigegebenes Feature“, das nicht an eine Datenbank-Engine-Instanz gekoppelt ist.|
| /FEATURES = SQL_SHARED_MPY | Installiert das Python-Feature für die eigenständige Version: SQL Server Machine Learning Server (eigenständig). Ein eigenständiger Server ist ein „freigegebenes Feature“, das nicht an eine Datenbank-Engine-Instanz gekoppelt ist.|
| /IACCEPTROPENLICENSETERMS  | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung der Open-Source-R-Komponenten zugestimmt haben. |
| /IACCEPTPYTHONLICENSETERMS | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung der Python-Komponenten zugestimmt haben. |
| /IACCEPTSQLSERVERLICENSETERMS | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung von SQL Server zugestimmt haben.|
| MRCACHEDIRECTORY | Legt bei Offlinesetups den Ordner fest, der die CAB-Dateien für die R-Komponente enthält. |
| MPYCACHEDIRECTORY | Für die zukünftige Verwendung reserviert. Verwenden Sie %TEMP%, um die CAB-Dateien für die Python-Komponente zur Installation auf Computern zu speichern, die nicht über eine Internetverbindung verfügen. |
::: moniker-end

::: moniker range=">=sql-server-ver15"
| Argumente | BESCHREIBUNG |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installiert die datenbankinterne Version: SQL Server Machine Learning Services (datenbankintern).  |
| /FEATURES = SQL_INST_MR | Koppeln Sie dieses Argument mit AdvancedAnalytics. Installiert das (datenbankinterne) R-Feature, einschließlich Microsoft R Open und der proprietären R-Pakete. |
| /FEATURES = SQL_INST_MPY | Koppeln Sie dieses Argument mit AdvancedAnalytics. Installiert das (datenbankinterne) Python-Feature, einschließlich Anaconda und der proprietären Python-Pakete. |
| /FEATURES = SQL_INST_MJAVA | Koppeln Sie dieses Argument mit AdvancedAnalytics. Installiert das (datenbankinterne) Java-Feature, einschließlich Open JRE. Gilt für die [Java-Spracherweiterung für SQL Server](../../language-extensions/install/windows-java.md).|
| /FEATURES = SQL_SHARED_MR | Installiert das R-Feature für die eigenständige Version: SQL Server Machine Learning Server (eigenständig). Ein eigenständiger Server ist ein „freigegebenes Feature“, das nicht an eine Datenbank-Engine-Instanz gekoppelt ist.|
| /FEATURES = SQL_SHARED_MPY | Installiert das Python-Feature für die eigenständige Version: SQL Server Machine Learning Server (eigenständig). Ein eigenständiger Server ist ein „freigegebenes Feature“, das nicht an eine Datenbank-Engine-Instanz gekoppelt ist.|
| /IACCEPTROPENLICENSETERMS  | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung der Open-Source-R-Komponenten zugestimmt haben. |
| /IACCEPTPYTHONLICENSETERMS | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung der Python-Komponenten zugestimmt haben. |
| /IACCEPTSQLSERVERLICENSETERMS | Gibt an, dass Sie den Lizenzbedingungen für die Verwendung von SQL Server zugestimmt haben.|
| MRCACHEDIRECTORY | Legt bei Offlinesetups den Ordner fest, der die CAB-Dateien für die R-Komponente enthält. |
| MPYCACHEDIRECTORY | Für die zukünftige Verwendung reserviert. Verwenden Sie %TEMP%, um die CAB-Dateien für die Python-Komponente zur Installation auf Computern zu speichern, die nicht über eine Internetverbindung verfügen. |
::: moniker-end

## <a name="in-database-instance-installations"></a><a name="indb"></a> Installation von datenbankinternen Instanzen

Datenbankinterne Analysen sind für Datenbank-Engine-Instanzen verfügbar, um das **AdvancedAnalytics**-Feature hinzufügen zu können. Sie können eine Datenbank-Engine-Instanz mit Advanced Analytics installieren oder das Feature [zu einer vorhandenen Instanz hinzufügen](#add-existing). 

Um Statusinformationen ohne interaktive Eingabeaufforderungen anzuzeigen, verwenden Sie das Argument „/qs“.

> [!IMPORTANT]
> Nach der Installation sind zwei zusätzliche Konfigurationsschritte auszuführen. Die Integration ist erst nach Ausführung dieser Aufgaben vollständig. Anweisungen finden Sie unter [Konfiguration nach der Installation](#post-install).

::: moniker range=">=sql-server-2017"
### <a name="sql-server-machine-learning-services-database-engine-advanced-analytics-with-python-and-r"></a>SQL Server Machine Learning Services: Datenbank-Engine, Advanced Analytics mit Python und R

Um eine parallele Installation der Datenbank-Engine-Instanz auszuführen, geben Sie den Instanznamen und eine Administrator-Anmeldung (Windows) an. Schließen Sie Features zum Installieren von Kern- und Sprachkomponenten sowie zur Annahme aller Lizenzbedingungen ein.

```cmd
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Dies ist der gleiche Befehl, aber mit einer SQL Server-Anmeldung in einer Datenbank-Engine mit gemischter Authentifizierung.

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Dieses Beispiel gilt nur für Python und zeigt, dass Sie eine einzige Sprache hinzufügen können, indem Sie ein Feature weglassen.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```
::: moniker-end

::: moniker range="=sql-server-2016"
### <a name="sql-server-r-services-database-engine-and-advanced-analytics-with-r"></a>SQL Server R Services: Datenbank-Engine und Advanced Analytics mit R

Um eine parallele Installation der Datenbank-Engine-Instanz auszuführen, geben Sie den Instanznamen und eine Administrator-Anmeldung (Windows) an. Schließen Sie Features zum Installieren von Kern- und Sprachkomponenten sowie zur Annahme aller Lizenzbedingungen ein.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```
::: moniker-end

## <a name="post-install-configuration-required"></a><a name="post-install"></a> Konfiguration nach der Installation (erforderlich)

Gilt nur für datenbankinterne Installationen.

Nach Abschluss des Setups verfügen Sie über eine Datenbank-Engine-Instanz mit R und Python, den Microsoft R- und Python-Paketen, Microsoft R Open, Anaconda, Tools, Beispielen und Skripts, die zur Distribution gehören. 

Zum Abschließen der Installation müssen zwei weitere Aufgaben ausgeführt werden:


::: moniker range=">=sql-server-2017"
1. Starten Sie den Datenbank-Engine-Dienst neu.

1. SQL Server Machine Learning Services: Aktivieren Sie externe Skripts, um dieses Feature verwenden zu können. Führen Sie als Nächstes die Anweisungen unter [Installieren von SQL Server Machine Learning Services (datenbankintern)](sql-machine-learning-services-windows-install.md) aus. 
::: moniker-end

::: moniker range="=sql-server-2016"
1. Starten Sie den Datenbank-Engine-Dienst neu.

1. SQL Server R Services: Aktivieren Sie externe Skripts, um dieses Feature verwenden zu können. Führen Sie als Nächstes die Anweisungen unter [Installieren von SQL Server R Services (datenbankintern)](sql-r-services-windows-install.md) aus. 
::: moniker-end

## <a name="add-advanced-analytics-to-an-existing-database-engine-instance"></a><a name="add-existing"></a> Hinzufügen von Advanced Analytics zu einer vorhandenen Datenbank-Engine-Instanz

Wenn Sie einer vorhandenen Datenbank-Engine-Instanz datenbankinterne Advanced Analytics-Funktionen hinzufügen, geben Sie den Instanznamen an. Wenn Sie z. B. zuvor eine Datenbank-Engine für SQL Server 2017 oder höher und Python installiert haben, können Sie diesen Befehl verwenden, um R hinzuzufügen.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```

## <a name="silent-install"></a><a name="silent"></a> Automatische Installation

Eine automatische Installation unterdrückt die Suche nach Speicherorten von CAB-Dateien. Aus diesem Grund müssen Sie den Speicherort angeben, an dem CAB-Dateien entpackt werden sollen. Für Python müssen sich CAB-Dateien im Ordner *%TEMP%* befinden. Bei R können Sie den Ordnerpfad im entsprechenden temporären Verzeichnis festlegen.
 
```cmd  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% 
```

## <a name="standalone-server-installations"></a><a name="shared-feature"></a> Eigenständige Serverinstallationen

Ein eigenständiger Server ist ein „freigegebenes Feature“, das nicht an eine Datenbank-Engine-Instanz gekoppelt ist. Das folgende Beispiel zeigt eine gültige Syntax für die Installation des eigenständigen Servers.

::: moniker range=">=sql-server-2017"
SQL Server Machine Learning Server unterstützt Python und R auf einem eigenständigen Server:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end
::: moniker range="=sql-server-2016"
SQL Server R Server unterstützt nur R:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end

Nach Abschluss des Setups verfügen Sie über einen Server, Microsoft-Pakete, Open-Source-Distributionen von R und Python, Tools, Beispiele und Skripts, die zur Distribution gehören. 

Um ein R-Konsolenfenster zu öffnen, wechseln Sie zu `\Program files\Microsoft SQL Server\150 (or 140/130)\R_SERVER\bin\x64`, und doppelklicken Sie auf **RGui.exe**. Haben Sie noch keine Erfahrung mit R? Sehen Sie sich dieses Tutorial an: [Grundlegende R-Befehle und RevoScaleR-Funktionen: 25 allgemeine Beispiele](/machine-learning-server/r/tutorial-r-to-revoscaler).

Um einen Python-Befehl zu öffnen, wechseln Sie zu `\Program files\Microsoft SQL Server\150 (or 140)\PYTHON_SERVER\bin\x64`, und doppelklicken Sie auf **python.exe**.

## <a name="next-steps"></a>Nächste Schritte

Python-Entwickler können in den folgenden Tutorials erfahren, wie Python mit SQL Server verwendet werden kann:

+ [Python-Tutorial: Bereitstellen eines linearen Regressionsmodells in SQL Server Machine Learning Services](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python-Tutorial: Kategorisieren von Kunden mithilfe von K-Means-Clustering mit SQL Server Machine Learning Services](../tutorials/python-clustering-model.md)

R-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von R unter SQL Server kennenlernen. Informationen zu den nächsten Schritten finden Sie unter den folgenden Links:

+ [Schnellstart: Ausführen von R in T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutorial: Datenbankinterne Analysen für R-Entwickler](../tutorials/r-taxi-classification-introduction.md)
