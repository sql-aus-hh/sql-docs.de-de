---
title: SQL Server 2017 Release Notes | Microsoft-Dokumentation
description: In diesem Artikel werden Einschränkungen und Probleme in Bezug auf SQL Server 2017 beschrieben und Links zu weiteren Informationen bereitgestellt.
ms.custom: ''
ms.date: 11/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-2017
ms.openlocfilehash: 0c70b2a7202ea7bcd14128a544487ec011c9c7b8
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643319"
---
# <a name="sql-server-2017-release-notes"></a>Versionsanmerkungen zu SQL Server 2017
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
In den folgenden Artikeln werden Einschränkungen und Probleme mit SQL Server 2017 beschrieben. Verwandte Informationen
- [Neues in SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [SQL Server unter Linux: Anmerkungen zu dieser Version](../linux/sql-server-linux-release-notes.md)
- [SQL Server 2017 Cumulative updates (Kumulative Updates für SQL Server 2017)](https://aka.ms/sql2017cu) für Informationen zu den aktuellen kumulativen Updates

**Probieren Sie SQL Server aus!**
- [![Download aus dem Evaluation Center](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) [SQL Server 2017 herunterladen](https://go.microsoft.com/fwlink/?LinkID=829477)
- [![Erstellen eines virtuellen Computers](../includes/media/azure-vm.png)](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) [Starten von virtuellen Computern mit SQL Server 2017](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)

> [!NOTE]
> SQL Server 2019 (Vorschau) ist jetzt verfügbar. Weitere Informationen finden Sie unter [Neues in SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

## <a name="sql-server-2017---general-availability-release-october-2017"></a>SQL Server 2017: allgemein verfügbare Releaseversion (Oktober 2017)
### <a name="database-engine"></a>Datenbank-Engine

- **Problem und Kundenbeeinträchtigung:** Nach dem Upgrade ist die bestehende FILESTREAM-Netzwerkfreigabe möglicherweise nicht mehr verfügbar.

- **Problemumgehung:** Starten Sie zuerst den Computer neu, und prüfen Sie, ob die FILESTREAM-Netzwerkfreigabe verfügbar ist. Führen Sie folgende Schritte aus, wenn die Freigabe danach noch immer nicht verfügbar ist:

    1. Klicken Sie im SQL Server-Konfigurations-Manager erst mit der rechten Maustaste auf die SQL Server-Instanz und anschließend auf **Eigenschaften**. 
    2. Deaktivieren Sie in der Registerkarte **FILESTREAM** die Funktion **FILESTREAM für E/A-Streamingzugriff auf Datei aktivieren**, und klicken Sie anschließend auf **Anwenden**.
    3. Aktivieren Sie dann erneut **FILESTREAM für E/A-Streamingzugriff auf Datei aktivieren** mit dem ursprünglichen Freigabenamen, und klicken Sie auf **Anwenden**.

### <a name="master-data-services-mds"></a>Master Data Services (MDS)
- **Problem und Kundenbeeinträchtigung:**  Auf der Benutzerberechtigungsseite wird Ihnen der folgende Fehler angezeigt, wenn sie in der Strukturansicht auf Stammebene eine Berechtigung erteilen: `"The model permission cannot be saved. The object guid is not valid"`

- **Problemumgehung:** 
  - Grant permission on the sub nodes in the tree view instead of the root level (Erteilen Sie Berechtigung auf den vorhandenen Knoten in der Strukturansicht anstatt auf Stammebene).

### <a name="analysis-services"></a>Analysis Services
- **Problem und Kundenbeeinträchtigung:** Datenconnectors für die folgenden Quellen sind für tabellarische Modelle mit Kompatibilitätsgrad 1400 noch nicht verfügbar.
  - Amazon Redshift
  - IBM Netezza
  - Impala
- **Problemumgehung:** Keine.   

- **Problem und Kundenbeeinträchtigung:** Direkte Abfragemodelle mit Perspektiven und dem Kompatibilitätsgrad 1400 können fehlschlagen, wenn Metadaten abgefragt oder ermittelt werden.
- **Problemumgehung:** Entfernen Sie Perspektiven, und stellen Sie diese erneut bereit.

### <a name="tools"></a>Tools
- **Problem und Kundenbeeinträchtigung:** Fehler bei der Ausführung von *DReplay* mit folgender Meldung: „Fehler bei DReplay. Ein unerwarteter Fehler ist aufgetreten!“.
- **Problemumgehung:** Keine.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc2---august-2017"></a>SQL Server 2017 Release Candidate (RC2: August 2017)
Es gibt keine Anmerkungen zu dieser Version von SQL Server für Windows. Siehe: [SQL Server on Linux Release notes (Versionsanmerkungen zu SQL Server unter Linux)](../linux/sql-server-linux-release-notes.md).


![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc1---july-2017"></a>SQL Server 2017 Release Candidate (RC1 – Juli 2017)
### <a name="sql-server-integration-services-ssis-rc1---july-2017"></a>SQL Server Integration Services (SSIS) (RC1 – Juli 2017)
- **Problem und Kundenbeeinträchtigung:** Der Parameter *runincluster* der gespeicherten Prozedur **[catalog].[create_execution]** wird hinsichtlich Konsistenz und Lesbarkeit in *runinscaleout* umbenannt.
- **Problemumgehung:** Wenn Skripts zum Ausführen von Paketen in Scale Out vorhanden sind, müssen Sie den Parameternamen von *runincluster* in *runinscaleout* ändern, damit die Skripts in RC1 funktionieren.

- **Problem und Kundenbeeinträchtigung:** SQL Server Management Studio (SSMS) 17.1 und frühere Versionen können die Ausführung des Pakets in Scale Out in RC1 nicht auslösen. Die Fehlermeldung lautet: " *\@runincluster* is not a parameter for procedure **create_execution**." (*@runincluster* ist kein Parameter für die Prozedur create_execution). Dieses Problem wurde in der nächsten Version von SSMS, (Version 17.2) behoben. Die Version 17.2 und spätere SSMS-Versionen unterstützen den neuen Parameternamen und die Paketausführung in Scale Out. 
- **Problemumgehung:** Bis Version 17.2 von SSMS verfügbar ist:
  1. Verwenden Sie Ihre bestehende Version von SSMS, um das Paketausführungsskript zu generieren.
  2. Ändern Sie den Namen des Parameters *runincluster* im Skript zu *runinscaleout*.
  3. Führen Sie das Skript aus.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-21-may--2017"></a>SQL Server 2017 CTP 2.1 (Mai 2017)
### <a name="documentation-ctp-21"></a>Dokumentation (CTP 2.1)
- **Problem und Kundenbeeinträchtigung:** Die Dokumentation für [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] ist eingeschränkt, und die Inhalte sind in der [!INCLUDE[ssSQL15_md](../includes/sssql15-md.md)]-Dokumentation enthalten.  Inhalte in Artikeln, die für [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] spezifisch sind, sind mit **Gilt für:** gekennzeichnet. 
- **Problem und Kundenbeeinträchtigung:** Für [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] sind keine Offlineinhalte verfügbar.

### <a name="sql-server-reporting-services-ctp-21"></a>SQL Server Reporting Services (CTP 2.1)

- **Problem und Kundenbeeinträchtigung:** Wenn Sie sowohl SQL Server Reporting Services als auch Power BI-Berichtsserver auf demselben Computer installiert haben und eine der beiden Anwendungen deinstallieren, können Sie mit dem Berichtsserver-Konfigurations-Manager keine Verbindung mit dem verbleibenden Berichtsserver herstellen.
- **Problemumgehung:** Um dieses Problem zu umgehen, müssen Sie nach der Deinstallation einer der Server die folgenden Vorgänge ausführen.

    1. Starten Sie eine Eingabeaufforderung im Administratormodus.
    2. Wechseln Sie zu dem Verzeichnis, in dem der verbleibenden Berichtsserver installiert ist.

        *Standardspeicherort von Power BI-Berichtsserver: C:\Program Files\Microsoft Power BI Report Server*

        *Standardspeicherort von SQL Server Reporting Services: C:\Program Files\Microsoft SQL Server Reporting Services*

    3. Wechseln Sie dann zum nächsten Ordner. Dabei handelt es sich entweder um *SSRS* oder um *PBIRS*, je nachdem, was verbleibt.
    4. Wechseln Sie zum WMI-Ordner.
    5. Führen Sie den folgenden Befehl aus:

        ```console
        regsvr32 /i ReportingServicesWMIProvider.dll
        ```

        Wenn die folgende Fehlermeldung angezeigt wird, ignorieren Sie diese.

        ```
        The module "ReportingServicesWMIProvider.dll" was loaded but the entry-point DLLInstall was not found. Make sure that "ReportingServicesWMIProvider.dll" is a valid DLL or OCX file and then try again.
        ```

### <a name="tsqllanguageservicemsi-ctp-21"></a>TSqlLanguageService.msi (CTP 2.1)

- **Problem und Kundenbeeinträchtigung:** Nach der Installation auf einem Computer mit einer 2016er-Version von *TSqlLanguageService.msi* (entweder über SQL-Setup oder als eigenständiges Redistributable) werden die v13.* (SQL 2016)-Versionen von *Microsoft.SqlServer.Management.SqlParser.dll* und *Microsoft.SqlServer.Management.SystemMetadataProvider.dll* entfernt. Jede Anwendung, die eine Abhängigkeit von den 2016er-Versionen dieser Assemblys hat, funktioniert nicht mehr und erzeugt einen ähnlichen Fehler wie: *Fehler: Could not load file or assembly 'Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' or one of its dependencies. (Die Datei oder Assembly 'Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' oder eine Abhängigkeit davon konnte nicht geladen werden.) The system cannot find the file specified. („csc. exe“ wurde mit dem Fehlercode 1 beendet -- eine Instanz von Analyzer AAAA kann nicht aus „C:\BBBB.dll“ erstellt werden: Die Datei oder Assembly „Microsoft.CodeAnalysis, Version=X.X.X.X, Culture=neutral, PublicKeyToken=31bf3856ad364e35“ oder eine Abhängigkeit davon wurde nicht gefunden. Das System kann die angegebene Datei nicht finden.)*

   Darüber hinaus wird beim Versuch, eine 2016er-Version von „TSqlLanguageService.msi“ neu zu installieren, die folgende Fehlermeldung angezeigt: *Fehler bei der Installation von Microsoft SQL Server 2016 T-SQL-Sprachdienst, da eine neuere Version bereits auf dem Computer vorhanden ist*.

- **Problemumgehung:** Um dieses Problem zu umgehen und eine Anwendung zu korrigieren, die von der v13-Version des Assemblys abhängt, gehen Sie folgendermaßen vor:

   1. Wechseln Sie zu **Programme hinzufügen/entfernen**.
   2. Suchen Sie nach *Microsoft SQL Server 2019 T-SQL-Sprachdienst CTP2.1*, klicken Sie mit der rechten Maustaste darauf, und klicken Sie dann auf **Deinstallieren**.
   3. Nachdem die Komponente entfernt wurde, reparieren Sie die kaputte Anwendung oder installieren Sie die entsprechende Version von *TSqlLanguageService.MSI* neu.

   Diese Problemumgehung entfernt die v14-Version dieser Assemblys, sodass alle Anwendungen, die von den v14-Versionen abhängen, nicht mehr funktionieren. Wenn diese Assemblys erforderlich sind, ist eine separate Installation ohne parallele 2016er-Installationen erforderlich.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-20-april--2017"></a>SQL Server 2017 CTP 2.0 (April 2017)
### <a name="documentation-ctp-20"></a>Dokumentation (CTP 2.0)
- **Problem und Kundenbeeinträchtigung:** Die Dokumentation für [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] ist eingeschränkt, und die Inhalte sind in der [!INCLUDE[ssSQL15_md](../includes/sssql15-md.md)]-Dokumentation enthalten.  Inhalte in Artikeln, die für [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] spezifisch sind, sind mit **Gilt für:** gekennzeichnet. 
- **Problem und Kundenbeeinträchtigung:** Für [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] sind keine Offlineinhalte verfügbar.

### <a name="always-on-availability-groups"></a>Always On-Verfügbarkeitsgruppen

- **Problem und Kundenbeeinträchtigung:** Eine SQL Server-Instanz, die ein sekundäres Replikat einer Verfügbarkeitsgruppe hostet, stürzt ab, wenn die Hauptversion von SQL Server älter ist als die Instanz, die das primäre Replikat hostet. Hat Auswirkungen auf Upgrades von allen unterstützten Versionen von SQL Server, die Verfügbarkeitsgruppen für SQL Server [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] CTP 2.0 hosten. Dieses Problem tritt unter den folgenden Bedingungen auf. 

> 1. Ein Benutzer aktualisiert die SQL Server-Instanz, die ein sekundäres Replikat hostet, gemäß den [bewährten Methoden](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).
> 2. Nach dem Upgrade tritt ein Failover auf, und ein neu aktualisiertes sekundäres Replikat wird zum primären Replikat, bevor das Upgrade für alle sekundären Replikate in der Verfügbarkeitsgruppe abgeschlossen ist. Das frühere primäre Replikat ist nun ein sekundäres Replikat, d.h. mit niedrigerer Version als das primäre Replikat.
> 3. Die Verfügbarkeitsgruppe befindet sich in einer nicht unterstützten Konfiguration, und für die verbleibenden sekundären Replikate könnte die Gefahr bestehen, dass sie abstürzen. 

- **Problemumgehung** Stellen Sie eine Verbindung mit der SQL Server-Instanz her, die das neue primäre Replikat hostet, und entfernen Sie das fehlerhafte sekundäre Replikat aus der Konfiguration.

   ```sql
   ALTER AVAILABILITY GROUP agName REMOVE REPLICA ON NODE instanceName;
   ```

   Die SQL Server-Instanz, die das sekundäre Replikat hostet, wird wiederhergestellt.

## <a name="more-information"></a>Weitere Informationen
- [SQL Server Reporting Services release notes (Versionshinweise für SQL Server Reporting Services)](../reporting-services/release-notes-reporting-services.md)beschrieben.
- [Known Issues for Machine Learning Services (Bekannte Probleme bei Machine Learning-Diensten)](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md)
- [SQL Server-Update Center – Links und Informationen zu allen unterstützten Versionen](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)