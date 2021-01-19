---
title: Konfigurieren von Verfügbarkeitsgruppen in SSMS
description: Erstellen und Konfigurieren einer Always On-Verfügbarkeitsgruppe mit dem Assistenten für neue Verfügbarkeitsgruppen in SQL Server Management Studio (SSMS)
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.f1
- sql13.swb.newavgroupwiz.f1
helpviewer_keywords:
- New Availability Group Wizard, availability replicas
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], creating
ms.assetid: e1f1dccc-9e65-471d-8fd1-b45085c9484a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 59b0e82558e14e29e4fad69ddc86b76d982a634a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171102"
---
# <a name="use-the-availability-group-wizard-sql-server-management-studio"></a>Verwenden des Assistenten für Verfügbarkeitsgruppen (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie in **mithilfe des** Assistenten für neue Verfügbarkeitsgruppen[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] eine Always On-Verfügbarkeitsgruppe in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] erstellt und konfiguriert wird. Eine *Verfügbarkeitsgruppe* definiert einen Satz von Benutzerdatenbanken, für die als eine einzelne Einheit ein Failover ausgeführt wird, sowie einen Satz von Failoverpartnern, die als *Verfügbarkeitsreplikate* bezeichnet werden, die Failover unterstützen.  
  
> [!NOTE]  
>  Eine Einführung zu Verfügbarkeitsgruppen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)erstellt und konfiguriert wird.  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Es wird dringend empfohlen, dass Sie diesen Abschnitt lesen, bevor Sie versuchen, Ihre erste Verfügbarkeitsgruppe zu erstellen.  
  
###  <a name="prerequisites-restrictions-and-recommendations"></a><a name="Prerequisites"></a> Voraussetzungen, Einschränkungen und Empfehlungen  

In den meisten Fällen können Sie den Assistenten für neue Verfügbarkeitsgruppen verwenden, um alle zum Erstellen und Konfigurieren einer Verfügbarkeitsgruppe erforderlichen Aufgaben auszuführen. Möglicherweise müssen Sie jedoch einige Aufgaben manuell ausführen.  
  
- Wenn Sie einen WSFC-Typ (Windows Server Failover Cluster) verwenden, um eine Verfügbarkeitsgruppe zu hosten, überprüfen Sie, ob sich die Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], die die Verfügbarkeitsreplikate hosten, auf verschiedenen Clusterservern oder -knoten des gleichen WSFC befinden. Stellen Sie außerdem sicher, dass jede der Serverinstanzen alle anderen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]-Voraussetzungen erfüllen. Für weitere Informationen empfehlen wir Ihnen dringend [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
-   Wenn eine Serverinstanz, die Sie zum Hosten eines Verfügbarkeitsreplikats auswählen, unter einem Domänenbenutzerkonto ausgeführt wird und noch keinen Datenbankspiegelungs-Endpunkt aufweist, kann der Assistent den Endpunkt erstellen und dem Dienstkonto der Serverinstanz die CONNECT-Berechtigung erteilen. Wenn der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst jedoch als integriertes Konto, z. B. Lokales System, Lokaler Dienst oder Netzwerkdienst, oder als Nichtdomänenkonto ausgeführt wird, müssen Sie Zertifikate zur Endpunktauthentifizierung verwenden, und der Assistent kann keinen Datenbankspiegelungs-Endpunkt auf der Serverinstanz erstellen. In diesem Fall empfiehlt es sich, dass Sie die Datenbankspiegelungs-Endpunkte manuell erstellen, bevor Sie den Assistenten für neue Verfügbarkeitsgruppen starten.  
  
     **So verwenden Sie Zertifikate für einen Datenbankspiegelungs-Endpunkt:**  
  
     [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
     [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   SQL Server-Failoverclusterinstanzen (FCIs) unterstützen kein automatisches Failover durch Verfügbarkeitsgruppen. Daher können die Verfügbarkeitsreplikate, die von einer FCI gehostet werden, nur für manuelles Failover konfiguriert werden.  
  
-   **Voraussetzungen für die vollständige anfängliche Datensynchronisierung mit dem Assistenten**  
  
    -   Alle Datenbankdateipfade müssen auf allen Serverinstanzen, die ein Replikat für die neue Verfügbarkeitsgruppe hosten, identisch sein.  
  
    -   Primäre Datenbanknamen können nicht auf Serverinstanzen vorhanden sein, die ein sekundäres Replikat hosten. Daher kann noch keine der neuen sekundären Datenbanken vorhanden sein.  
  
    -   Sie müssen eine Netzwerkfreigabe angeben, damit der Assistent Sicherungen erstellen und auf Sicherungen zugreifen kann. Für das primäre Replikat kann das zum Starten von [!INCLUDE[ssDE](../../../includes/ssde-md.md)] verwendete Konto über Lese- und Schreibberechtigungen für das Dateisystem in einer Netzwerkfreigabe verfügen. Bei sekundären Replikaten muss das Konto über eine Leseberechtigung für die Netzwerkfreigabe verfügen.  
  
        > [!IMPORTANT]  
        >  Die Protokollsicherungen sind Teil der Protokollsicherungskette. Speichern Sie die Protokollsicherungsdateien ordnungsgemäß.  
  
     Wenn Sie nicht den Assistenten für eine vollständige anfängliche Datensynchronisierung verwenden können, müssen Sie die sekundären Datenbanken manuell vorbereiten. Dies ist vor oder nach dem Ausführen des Assistenten möglich. Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)erstellt und konfiguriert wird.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.  
  
 Außerdem ist die CONTROL ON ENDPOINT-Berechtigung erforderlich, wenn Sie dem Verfügbarkeitsgruppen-Assistenten die Verwaltung des Datenbankspiegelungs-Endpunkts erlauben möchten.  
  
##  <a name="using-the-new-availability-group-wizard"></a><a name="RunAGwiz"></a> Verwenden des Assistenten für neue Verfügbarkeitsgruppen  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Um den Assistenten für neue Verfügbarkeitsgruppen zu starten, wählen Sie den Befehl **Assistent für neue Verfügbarkeitsgruppen** aus.  
  
4.  Wenn Sie den Assistenten zum ersten Mal ausführen, wird eine **Einführungsseite** angezeigt. Damit diese Seite in Zukunft nicht mehr angezeigt wird, klicken Sie auf **Diese Seite nicht mehr anzeigen**. Nachdem Sie diese Seite gelesen haben, klicken Sie auf **Weiter**.  
  
5.  Geben Sie auf der Seite **Optionen der Verfügbarkeitsgruppe angeben** den Namen der neuen Verfügbarkeitsgruppe im Feld **Name der Verfügbarkeitsgruppe** ein. Dieser Name muss ein gültiger [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Bezeichner sein, der auf dem Cluster und in der Domäne als Ganzes eindeutig ist. Die maximale Länge eines Verfügbarkeitsgruppennamens beträgt 128 Zeichen.

6. Geben Sie als nächstes den Clustertyp an. Die möglichen Clustertypen hängen von der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Version und dem Betriebssystem ab. Wählen Sie entweder **WSFC**, **EXTERNAL** oder **NONE** aus. Weitere Einzelheiten finden Sie auf der [Seite „Namen für Verfügbarkeitsgruppen angeben“](specify-availability-group-name-page.md)
 
6.  Auf der Seite **Datenbanken auswählen** sind im Raster die Benutzerdatenbanken auf der verbundenen Serverinstanz aufgeführt, die *Verfügbarkeitsdatenbanken* werden können. Wählen Sie eine oder mehrere der aufgelisteten Datenbanken aus, um diese in der Verfügbarkeitsgruppe zu verwenden. Diese Datenbanken sind anfänglich die ursprünglichen *primären Datenbanken*.  
  
     Für jede aufgelistete Datenbank zeigt die Spalte **Größe** die Datenbankgröße an (wenn bekannt). Die Spalte **Status** gibt an, ob eine bestimmte Datenbank die [Voraussetzungen](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)für Verfügbarkeitsdatenbanken erfüllt. Wenn die Voraussetzungen nicht erfüllt werden, gibt eine kurze Statusbeschreibung den Grund an, warum die Datenbank nicht geeignet ist, z. B. wenn sie nicht das vollständige Wiederherstellungsmodell verwendet. Klicken Sie auf die Statusbeschreibung, um weitere Informationen zu erhalten.  
  
     Wenn Sie eine Datenbank so ändern, dass sie verwendet werden kann, klicken Sie auf **Aktualisieren** , um das Raster für die Datenbanken zu aktualisieren.  
  
     Wenn die Datenbank einen Datenbank-Hauptschlüssel enthält, geben Sie das Kennwort für den Datenbank-Hauptschlüssel in die Spalte **Kennwort** ein.  
  
7.  Auf der Seite **Replikate angeben** können Sie ein oder mehrere Replikate für die neue Verfügbarkeitsgruppe angeben und konfigurieren. Diese Seite enthält vier Registerkarten. In der folgenden Tabelle werden diese Registerkarten eingeführt. Weitere Informationen finden Sie unter [Seite „Replikate angeben“ &#40;Assistent für neue Verfügbarkeitsgruppen: Assistent zum Hinzufügen von Replikaten&#41;](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md).  
  
    |Registerkarte|Kurzbeschreibung|  
    |---------|-----------------------|  
    |**Replikate**|Geben Sie mit dieser Registerkarte jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] an, die ein sekundäres Replikat hostet. Beachten Sie, dass die Serverinstanz, mit der Sie gerade verbunden sind, das primäre Replikat hosten muss.|  
    |**Endpunkte**|Auf dieser Registerkarte können Sie vorhandene Endpunkte für die Datenbankspiegelung überprüfen und automatisch einen Endpunkt erstellen, falls er auf einer Serverinstanz fehlt, deren Dienstkonten die Windows-Authentifizierung nutzen.<br /><br /> Hinweis: Wenn eine Serverinstanz unter einem Nicht-Domänenbenutzerkonto ausgeführt wird, müssen Sie eine manuelle Änderung an der Serverinstanz vornehmen, bevor Sie den Assistenten fortsetzen können. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen](#Prerequisites).|  
    |**Sicherungseinstellungen**|Geben Sie mit dieser Registerkarte die Sicherungseinstellungen für die Verfügbarkeitsgruppe als Ganzes und die Sicherungsprioritäten für die einzelnen Verfügbarkeitsreplikate an.|  
    |**Listener**|Verwenden Sie diese Registerkarte, um einen Verfügbarkeitsgruppenlistener zu erstellen. Standardmäßig erstellt der Assistent keinen Listener.|  
  
8.  Wählen Sie auf der Seite **Anfängliche Datensynchronisierung auswählen** aus, wie die neuen sekundären Datenbanken erstellt und mit der Verfügbarkeitsgruppe verknüpft werden sollen. Wählen Sie eine der folgenden Optionen aus:  
  
    -   **Automatisches Seeding**  
  
         SQL Server erstellt die sekundären Replikate für jede Datenbank in der Gruppe automatisch. Automatisches Seeding erfordert, dass die Pfade für Daten- und Protokolldateien für jede SQL Server-Instanz der Gruppe identisch sind. Verfügbar auf [!INCLUDE[sssql16-md.md](../../../includes/sssql16-md.md)] und höher. Siehe [Automatically initialize Always On Availability group (Automatisches Initialisieren der Always On-Verfügbarkeitsgruppe)](automatically-initialize-always-on-availability-group.md).
    
    -   **Vollständige Datenbank- und Protokollsicherung**  
  
         Aktivieren Sie diese Option, wenn Ihre Umgebung die Anforderungen zum automatischen Starten der anfänglichen Datensynchronisierung erfüllt. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen, Einschränkungen und Empfehlungen](#Prerequisites).  
  
         Wenn Sie **Vollständig** auswählen, werden vom Assistenten nach der Erstellung der Verfügbarkeitsgruppe alle primären Datenbanken und ihre Transaktionsprotokolle auf einer Netzwerkfreigabe gesichert und die Sicherungen auf allen Serverinstanzen wiederhergestellt, die ein sekundäres Replikat hosten. Der Assistent verknüpft anschließend alle sekundären Datenbanken mit der Verfügbarkeitsgruppe.  
  
         Legen Sie im Feld zum **Angeben eines freigegebenen Netzwerkspeicherorts, auf den von allen Replikaten zugegriffen werden kann** , eine Sicherungsfreigabe fest, für die alle Serverinstanzen, die Replikate hosten, Lese-/Schreibzugriff besitzen. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen](#Prerequisites).  Im Validierungsschritt führt der Assistent Tests durch, um sicherzustellen, dass die angegebene Netzwerkadresse gültig ist. Durch den Test wird eine Datenbank mit dem Namen „BackupLocDb_“ gefolgt von einer GUID auf dem primären Replikat erstellt, und es wird eine Sicherung an der angegebenen Netzwerkadresse durchgeführt. Anschließend wird diese auf dem sekundären Replikat wiederherstellt. Diese Datenbank kann mit dem dazugehörigen Sicherungsverlauf und der Sicherungsdatei gelöscht werden, falls der Assistent dies nicht getan hat.
  
    -   **Nur verknüpfen**  
  
         Wenn Sie sekundäre Datenbanken auf den Serverinstanzen, die die sekundären Replikate hosten, manuell vorbereitet haben, können Sie diese Option aktivieren. Der Assistent verknüpft die vorhandenen sekundären Datenbanken mit der Verfügbarkeitsgruppe.  
  
    -   **Anfängliche Datensynchronisierung überspringen**  
  
         Aktivieren Sie diese Option, wenn Sie eigene Datenbank- und Protokollsicherungen der primären Datenbanken verwenden möchten. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Starten der Datenverschiebung auf einer sekundären Always On-Datenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
9. Auf der Seite **Überprüfung** wird überprüft, ob die in diesem Assistenten angegebenen Werte die Anforderungen des Assistenten für neue Verfügbarkeitsgruppen erfüllen. Um eine Änderung vorzunehmen, klicken Sie auf **Zurück** , um zu einer vorherigen Assistentenseite zurückzukehren und Werte zu ändern. Klicken Sie anschließend auf **Weiter** , um auf die Seite **Überprüfung** zurückzukehren, und klicken Sie auf **Überprüfung erneut ausführen**.  
  
10. Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen für die neue Verfügbarkeitsgruppe. Um eine Änderung vorzunehmen, klicken Sie auf **Zurück** , um zu der relevanten Seite zurückzukehren. Nachdem Sie die Änderung vorgenommen haben, klicken Sie auf **Weiter** , um zur Seite **Zusammenfassung** zurückzukehren.  
  
    > [!IMPORTANT]  
    >  Wenn das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienstkonto einer Serverinstanz, die ein neues Verfügbarkeitsreplikat hostet, noch nicht als Anmeldename vorhanden ist, muss der Anmeldename vom Assistenten für neue Verfügbarkeitsgruppen erstellt werden. Auf der Seite **Zusammenfassung** des Assistenten werden die Informationen für den zu erstellenden Anmeldenamen angezeigt. Wenn Sie auf **Fertig stellen** klicken, erstellt der Assistent diesen Anmeldenamen für das SQL Server-Dienstkonto und erteilt ihm die CONNECT-Berechtigung.  
  
     Wenn Sie mit der Auswahl zufrieden sind, können Sie optional auf **Skript** klicken, um ein Skript der Schritte zu erstellen, die der Assistent ausführt. Klicken Sie dann zum Erstellen und Konfigurieren der neuen Verfügbarkeitsgruppe auf **Fertig stellen**.  
  
11. Auf der Seite **Status** wird der Status der Schritte zum Erstellen der Verfügbarkeitsgruppe angezeigt (Konfigurieren von Endpunkten, Erstellen der Verfügbarkeitsgruppe und Hinzufügen des sekundären Replikats zu der Gruppe).  
  
12. Wenn diese Schritte abgeschlossen sind, wird auf der Seite **Ergebnisse** das Ergebnis der einzelnen Schritte angezeigt. Wenn all diese Schritte erfolgreich sind, ist die neue Verfügbarkeitsgruppe vollständig konfiguriert. Wenn einer der Schritte zu einem Fehler führt, müssen Sie die Konfiguration möglicherweise manuell abschließen oder einen Assistenten für den fehlerhaften Schritt ausführen. Klicken Sie in der Spalte **Ergebnis** auf den zugehörigen Fehlerlink, um weitere Informationen zur Ursache eines bestimmten Fehlers zu erhalten.  
  
     Klicken Sie nach Abschluss des Assistenten auf **Schließen** , um den Assistenten zu beenden.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So schließen Sie die Konfiguration von Verfügbarkeitsgruppen ab**  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Alternative Möglichkeiten zum Erstellen einer Verfügbarkeitsgruppe**  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Erstellen einer Verfügbarkeitsgruppe &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **So aktivieren Sie Always On-Verfügbarkeitsgruppen**  
  
-   [Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **So konfigurieren Sie einen Datenbankspiegelungs-Endpunkt**  
  
-   [Erstellen eines Datenbankspiegelungs-Endpunkts für Always On-Verfügbarkeitsgruppen &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Problembehandlung für die Always On-Verfügbarkeitsgruppenkonfiguration**  
  
-   [Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   **Blogs:**  
  
     [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases (Always On - HADRON-Lernreihe: Nutzung des Arbeitsthreadpools für HADRON-fähige Datenbanken)](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server Always On Team Blogs: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblogs: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server-Technikblogs](/archive/blogs/psssql/)  
  
-   **Videos:**  
  
     [Microsoft SQL Server Codename „Denali“ Always On-Reihe, Teil 1:Introducing the Next Generation High Availability Solution (Einführung in die nächste Generation von Lösungen mit hoher Verfügbarkeit)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Codename „Denali“ Always On-Reihe, Teil 2: Building a Mission-Critical High Availability Solution Using Always On (Erstellen einer unternehmenswichtigen Lösung für hohe Verfügbarkeit mit Always On)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Whitepaper:**  
  
     [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Microsoft-Whitepapers für SQL Server 2012](https://msdn.microsoft.com/library/hh403491.aspx)  
  
     [Whitepapers des SQL Server-Kundenberatungsteams](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="alternate-ways-to-create-availability-groups"></a>Alternative Möglichkeiten zum Erstellen von Verfügbarkeitsgruppen

Als Alternative zur Verwendung des Assistenten für neue Verfügbarkeitsgruppen können Sie [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Cmdlets verwenden. Weitere Informationen finden Sie unter [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md) oder [Erstellen einer Verfügbarkeitsgruppe &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)erstellt und konfiguriert wird.  


## <a name="see-also"></a>Weitere Informationen  
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
