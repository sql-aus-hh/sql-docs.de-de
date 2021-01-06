---
title: Editionen und unterstützte Funktionen
titleSuffix: SQL Server 2016
description: In diesem Artikel werden die von den verschiedenen Editionen von SQL Server 2016 unterstützten Features beschrieben, die unterschiedliche Leistungs-, Laufzeit- und Preisanforderungen erfüllen.
ms.custom:
- seo-lt-2019
- sqlfreshmay19
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
- CPSA
ms.assetid: e5186f02-dd91-47d0-8fa4-de3f41c76903
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8e1544a46e84b77ce290dded936f86b75a251f13
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642923"
---
# <a name="editions-and-supported-features-of-sql-server-2016"></a>Editionen und unterstützte Funktionen von SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

Dieses Thema bietet detaillierte Informationen zu den von verschiedenen SQL Server-Versionen unterstützten Funktionen.

Informationen zu anderen Versionen finden Sie unter:

* [SQL Server 2019](editions-and-components-of-sql-server-version-15.md).  
* [SQL Server 2017](editions-and-components-of-sql-server-2017.md).  
* [SQL Server 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true).

Die Installationsanforderungen variieren je nach den benötigten Anwendungen. Die verschiedenen Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tragen den individuellen Leistungs-, Laufzeit- und Preisanforderungen von Organisationen und Einzelpersonen Rechnung. Welche [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Komponenten Sie installieren, hängt auch von den individuellen Anforderungen ab. In den folgenden Abschnitten erfahren Sie, wie Sie die bestmögliche Auswahl unter den in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]verfügbaren Editionen und Komponenten treffen.  

Die SQL Server Evaluation Edition steht für einen Testzeitraum von 180 Tagen zur Verfügung.  
  
Die neuesten Versionsanmerkungen und Informationen zu Neuerungen finden Sie über die folgenden Links:
- [SQL Server 2017 release notes (Versionsanmerkungen zu SQL Server 2017)](../sql-server/sql-server-2017-release-notes.md)
- [Versionsanmerkungen zu SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)
- [Neues in SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Neuerungen in SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)

### <a name="try-sql-server"></a>Testen Sie SQL Server!    
    
> [![Download aus dem Evaluation Center](https://docs.microsoft.com/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) **[Laden Sie SQL Server 2016 aus dem Evaluation Center herunter.](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**    
> 
> ![Virtueller Azure-Computer (klein)](/analysis-services/analysis-services/media/azure-virtual-machine-small.png) **[Starten eines virtuellen Computers, auf dem SQL Server 2016 bereits installiert ist](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)**   
  
## <a name="ssnoversion-editions"></a>[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Editionen  
 In der folgenden Tabelle werden die Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]beschrieben. 
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Edition|Definition|  
|---------------------------------------|----------------|  
|Enterprise|Das Premium-Angebot, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition, übermittelt umfassende hochwertige Datencenterfunktionen mit blitzschneller Performance, unbegrenzter Virtualisierung und End-to-End Business Intelligence – und bietet so hohe Servicelevel für unternehmenswichtige Workloads und Endbenutzerzugriff auf Dateneinblicke.|  
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition stellt eine grundlegende Datenverwaltungs- und Business Intelligence-Datenbank bereit, auf der Abteilungen und kleinere Unternehmen ihre Anwendungen ausführen. Diese unterstützt allgemeine Entwicklungstools für lokale und Cloudverwendung und ermöglicht eine effektive Datenbankverwaltung mit minimalen IT-Ressourcen.|  
|Web|Die [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition ist eine mit geringen Anschaffungs- und Betriebskosten verbundene Option für Webhoster und Web-VAPs, die kostengünstige Skalierbarkeit und Verwaltungsfunktionen für Webpräsenzen jeder Größe bietet.|  
|Entwickler|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition ermöglicht Entwicklern das Erstellen beliebiger Anwendungen auf der Basis von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Sämtliche Funktionen der Enterprise Edition stehen zur Verfügung. Die Lizenz bezieht sich jedoch auf die Verwendung als Entwicklungs- und Testsystem und nicht als Produktionsserver. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer ist eine ideale Option zum Erstellen von [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] und Testen von Anwendungen.|  
|Express-Editionen|Die Express Edition ist eine kostenlose Datenbank auf Einstiegsebene und eignet sich ideal zum Üben und zum Erstellen von datengesteuerten Anwendungen für Desktopcomputer und kleine Server. Dies ist die beste Wahl für unabhängige Softwareanbieter, Entwickler und Tüftler, die Clientanwendungen erstellen. Wenn Sie erweiterte Datenbankfunktionen benötigen, können Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express nahtlos auf höhere Endversionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]aktualisieren. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB, eine neue Light-Version von Express, die sämtliche Programmierbarkeitsfunktionen besitzt, wird noch im Benutzermodus ausgeführt und bietet eine schnelle, konfigurationsfreie Installation und eine kurze Liste an Voraussetzungen.|  
  
## <a name="using-ssnoversion-with-an-internet-server"></a>Verwenden von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mit einem Internetserver  
 Die Clienttools von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] werden normalerweise auf einem Internetserver, z. B. einem Server mit Microsoft Internetinformationsdienste (Internet Information Services, IIS), installiert. Die Clienttools enthalten die Clientkonnektivitätskomponenten, mit denen eine Anwendung eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]herstellt.  
  
> [!NOTE]
> Die Installation einer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz auf einem Computer mit IIS ist zwar möglich, in der Praxis aber nur bei kleinen Websites üblich, die über einen einzigen Servercomputer verfügen. Bei den meisten Websites befindet sich das IIS-System mittlerer Ebene auf einem Server oder Servercluster, während die Datenbanken auf einem separaten Server oder einem vereinten System gespeichert werden.  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Verwenden von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mit Client/Server-Anwendungen  
 Sie können auch nur die Clientkomponenten von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Computer mit Client-/Serveranwendungen installieren und so eine direkte Verbindung mit einer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz herstellen. Die Installation der Clientkomponenten ist auch dann eine gute Wahl, wenn Sie eine Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Datenbankserver verwalten, oder wenn Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anwendungen entwickeln möchten.  
  
 Die Clienttools-Option installiert die folgenden [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Funktionen: Abwärtskompatibilitätskomponenten, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], Konnektivitätskomponenten, Verwaltungstools, Software Development Kit und [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Onlinedokumentations-Komponenten. Weitere Informationen finden Sie unter [Install SQL Server (Installieren von SQL Server)](../database-engine/install-windows/install-sql-server.md).  
  
## <a name="deciding-among-ssnoversion-components"></a>Auswählen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Komponenten  
 Auf der Seite Funktionsauswahl des Installationsassistenten für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] können Sie die Komponenten auswählen, die mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]installiert werden sollen. Standardmäßig sind in der Struktur keine Funktionen ausgewählt.  
  
 Anhand der Informationen aus den folgenden Tabellen können Sie ermitteln, welche Gruppe von Funktionen für Ihre Anforderungen am besten geeignet ist.  
  
|Serverkomponenten|BESCHREIBUNG|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] enthält die [!INCLUDE[ssDE](../includes/ssde-md.md)], den Kerndienst zum Speichern, Verarbeiten und Sichern von Daten sowie für Replikation, Volltextsuche, Verwaltungstools für relationale und XML-Daten, Integration in die Datenbankanalyse und PolyBase-Integration für den Zugriff auf Hadoop und andere heterogene Datenquellen sowie den [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)]-Server (DQS).|  
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] enthält Tools zum Erstellen und Verwalten der analytischen Onlineverarbeitung (OLAP, Online Analytical Processing) sowie Data Mining-Anwendungen.|  
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] enthält Server- und Clientkomponenten, mit denen Berichte in Form einer Tabelle, Matrix, Grafik oder Freiform erstellt, verwaltet und bereitgestellt werden können. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] ist gleichzeitig eine erweiterbare Plattform, die Sie zum Entwickeln von Berichtsanwendungen verwenden können.|  
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] stellt eine Gruppe grafischer Tools und programmierbarer Objekte zum Verschieben, Kopieren und Transformieren von Daten bereit. Es beinhaltet außerdem die [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS)-Komponente für [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) ist die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Lösung für die Masterdatenverwaltung. MDS kann konfiguriert werden, um jede Domäne (Produkte, Kunden, Konten) zu verwalten und umfasst Hierarchien, präzise Sicherheit, Transaktionen, Datenversionsverwaltung und Geschäftsregeln sowie einen [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] , der verwendet werden kann, um Daten zu verwalten.|  
|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)]|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)] unterstützt verteilte, skalierbare R-Lösungen auf mehreren Plattformen und verwendet mehrere Enterprise-Datenquellen einschließlich Linux, Hadoop und Teradata.|  
  
|Verwaltungstools|Beschreibung|  
|----------------------|-----------------|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] bietet eine integrierte Umgebung, in der Sie auf [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Komponenten zugreifen sowie diese konfigurieren, verwalten und entwickeln können. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] ermöglicht Entwicklern und Administratoren mit unterschiedlichen Fähigkeiten die Verwendung von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> Herunterladen und Installieren <br />                [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] hier:  [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Konfigurations-Manager|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Konfigurations-Manager stellt eine einfache Konfigurationsverwaltung für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Dienste, Server- und Clientprotokolle sowie Clientaliase bereit.|  
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] stellt eine grafische Benutzeroberfläche zum Überwachen einer Instanz von [!INCLUDE[ssDE](../includes/ssde-md.md)] oder [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]bereit.|  
|[!INCLUDE[ssDE](../includes/ssde-md.md)] -Optimierungsratgeber|Der Optimierungsratgeber für[!INCLUDE[ssDE](../includes/ssde-md.md)] unterstützt Sie beim Erstellen einer optimalen Menge von Indizes, indizierten Sichten und Partitionen.|  
|Data Quality Client|Stellt eine sehr einfache und intuitive grafische Benutzeroberfläche bereit, um eine Verbindung mit dem DQS-Server herzustellen und Datenbereinigungsvorgänge auszuführen. Diese Komponente ermöglicht außerdem die zentrale Überwachung verschiedener Aktivitäten, die während des Datenbereinigungsvorgangs ausgeführt werden.|  
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] bietet eine IDE zum Erstellen von Lösungen für die Business Intelligence-Komponenten: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]und [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Früher Business Intelligence Development Studio genannt).<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] schließt auch "Datenbankprojekte" ein, die eine integrierte Umgebung für Datenbankentwickler bereitstellen, die ihre ganze Datenbankentwurfsarbeit für eine beliebige [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Plattform (sowohl vor Ort als auch remote) in Visual Studio ausführen möchten. Datenbankentwickler können den verbesserten Server-Explorer in Visual Studio verwenden, um Datenbankobjekte und Daten einfach zu erstellen oder zu bearbeiten und Abfragen auszuführen.|  
|Konnektivitätskomponenten|Installiert Komponenten für die Kommunikation zwischen Clients und Servern, einschließlich Netzwerkbibliotheken für die DB-Library, ODBC und OLE DB.|  
  
|Dokumentation|BESCHREIBUNG|  
|-------------------|-----------------|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Onlinedokumentation|Kerndokumentation für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].| 

**Developer und Evaluation-Editionen**  
Von den Editionen Developer und Evaluation unterstützte Funktionen finden Sie in den Funktionen der Enterprise Edition von SQL Server in den folgenden Tabellen.


Die Developer Edition unterstützt weiterhin nur einen Client für [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Scale Limits  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|
|Maximale von einer einzelnen Instanz verwendete Rechenkapazität ( [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum des Betriebssystems|Beschränkt auf weniger als 4 Sockets oder 24 Kerne|Beschränkt auf weniger als 4 Sockets oder 16 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne| 
|Maximale von einer einzelnen Instanz verwendete Rechenkapazität ( [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] oder [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)])|Maximum des Betriebssystems|Beschränkt auf weniger als 4 Sockets oder 24 Kerne|Beschränkt auf weniger als 4 Sockets oder 16 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|  
|Maximaler Arbeitsspeicher für den Pufferpool pro Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum des Betriebssystems|128 GB|64 GB|1410 MB|1410 MB|
|Maximaler Arbeitsspeicher für Columnstore-Segmentcache pro Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Unbegrenzter Arbeitsspeicher| 32 GB<sup>2</sup>| 16 GB<sup>2</sup>| 352 MB<sup>2</sup>| 352 MB<sup>2</sup>|  
|Maximale speicheroptimierte Datengröße pro Datenbank in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Unbegrenzter Arbeitsspeicher| 32 GB<sup>2</sup>| 16 GB<sup>2</sup>| 352 MB<sup>2</sup>| 352 MB<sup>2</sup>|  
|Maximaler genutzter Arbeitsspeicher pro Instanz von [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Maximum des Betriebssystems|Tabellarisch: 16 GB<br /><br /> MOLAP: 64 GB|N/V|–|N/V|  
|Maximaler genutzter Arbeitsspeicher pro Instanz von [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum des Betriebssystems|64 GB|64 GB|4 GB|N/V|
|Maximale relationale Datenbankgröße|524 PB|524 PB|524 PB|10 GB|10 GB|  
  
<sup>1</sup> Die Enterprise Edition mit einer Lizenzierung auf der Grundlage von Serverlizenz + Clientzugriffslizenz (CAL) (für neue Verträge nicht verfügbar) ist auf maximal 20 Kerne pro SQL Server-Instanz beschränkt. Für das auf Prozessorkernen basierende Serverlizenzierungsmodell gelten keine Beschränkungen. Weitere Informationen finden Sie unter [Compute Capacity Limits by Edition of SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
<sup>2</sup> Gilt für [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (CPSA) für alle Editionen. 

##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS High Availability  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Server Core-Unterstützung <sup>1</sup>|Ja|Ja|Ja|Ja|Ja|  
|Protokollversand|Ja|Ja|Ja|Nein|Nein|  
|Datenbankspiegelung|Ja|Ja<br /><br /> Nur vollständige Sicherheit|Nur WITNESS|Nur WITNESS|Nur WITNESS| 
|Sicherungskomprimierung|Ja|Ja|Nein|Nein|Nein | 
|Datenbankmomentaufnahme|Ja|Ja <sup>3</sup>|Ja <sup>3</sup>|Ja <sup>3</sup>|Ja <sup>3</sup>|
|Always On-Failoverclusterinstanzen|Ja<br /><br /> 16|Ja<br /><br /> Unterstützung für 2 Knoten|Nein|Nein|Nein|  
|Always On-Verfügbarkeitsgruppen|Ja<br /><br /> Bis zu 8 sekundäre Replikate, einschließlich 2 synchronen sekundären Replikaten|Nein|Nein|Nein|Nein|
|Basis-Verfügbarkeitsgruppen <sup>2</sup>|Nein|Ja<br /><br /> Unterstützung für 2 Knoten|Nein|Nein|Nein |
|Onlineseiten- und Onlinedateiwiederherstellung|Ja|Nein|Nein|Nein|Nein |
|Online-Indizierung|Ja|Nein|Nein|Nein|Nein |
|Onlineschemaänderung|Ja|Nein|Nein|Nein|Nein |
|Schnelle Wiederherstellung|Ja|Nein|Nein|Nein|Nein |
|Gespiegelte Sicherungen|Ja|Nein|Nein|Nein|Nein |
|Hinzufügen von Speicher im laufenden Systembetrieb und CPU|Ja|Nein|Nein|Nein|Nein|
|Datenbankwiederherstellungsberater|Ja|Ja|Ja|Ja|Ja|
|Verschlüsselte Sicherung|Ja|Ja|Nein|Nein|Nein |
|Hybridsicherung in Azure (Sicherung über URL)|Ja|Ja|Nein|Nein|Nein|  
  
 <sup>1</sup> Weitere Informationen zum Installieren von SQL Server unter Server Core finden Sie unter [Install SQL Server on Server Core (Installieren von SQL unter Server Core)](../database-engine/install-windows/install-sql-server-on-server-core.md). 

<sup>2</sup> Weitere Informationen über Basis-Verfügbarkeitsgruppen finden Sie unter [Basis-Verfügbarkeitsgruppen](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).  

<sup>3</sup> Gilt für [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 2016 SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (CPSA) für alle Editionen. 
  
##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS Scalability and Performance  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Columnstore <sup>1</sup>|Ja|Ja <sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|  
|In-Memory-OLTP <sup>1</sup>|Ja|Ja <sup>2</sup>|Ja<sup>2</sup>|Ja <sup>2</sup>, <sup>3</sup>|Ja <sup>2</sup>|
|Stretch Database|Ja|Ja|Ja|Ja|Ja|
|Persistenter Hauptspeicher|Ja|Ja|Ja|Ja|Ja|
|Unterstützung mehrerer Instanzen|50|50|50|50|50|
|Tabellen- und Indexpartitionierung|Ja|Ja <sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|  
|Datenkomprimierung|Ja|Ja <sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|
|Resource Governor|Ja|Nein|Nein|Nein|Nein |  
|Parallelverarbeitung für partitionierte Tabellen|Ja|Nein|Nein|Nein|Nein|
|Mehrere FILESTREAM-Container|Ja|Ja <sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|
|NUMA-basierter Arbeitsspeicher für umfangreiche Seiten und Zuordnung von Pufferarrays|Ja|Nein <sup>4</sup>|Nein|Nein|Nein|
|Pufferpoolerweiterung|Ja|Ja|Nein|Nein|Nein|
|Ressourcengovernance für E/A-Vorgänge|Ja|Nein|Nein|Nein|Nein|
|Vorauslesen (Read-Ahead)|Ja|Nein|Nein|Nein|Nein|
|Erweitertes Scannen|Ja|Nein|Nein|Nein|Nein |
|Verzögerte Dauerhaftigkeit|Ja|Ja|Ja|Ja|Ja|

<sup>1</sup> Die Größe der In-Memory OLTP-Daten und des Columnstore-Segmentcaches sind auf die Größe des Arbeitsspeichers beschränkt, die von der Edition im Bereich Kapazitätsgrenzen festgelegt wird. Den maximale Grad an Parallelität ist beschränkt. Der Grad an Prozessparallelität (Degree of Parallelism, DOP) für eine Indexerstellung ist auf 2 DOP für die Standard Edition und auf 1 DOP für die Web und die Express Edition beschränkt. Dies gilt für Columnstore-Indizes, die über datenträgerbasierte Tabellen und speicheroptimierte Tabellen erstellt wurden.

<sup>2</sup> Gilt für [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (Common Programmability Surface Area, CPSA) für alle Editionen. Aggregatweitergabe, Zeichenfolgenprädikatweitergabe und SIMD-Optimierungen sind Erweiterungen der Skalierbarkeit von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition. Weitere Einzelheiten finden Sie unter [Columnstore-Indizes – Neuigkeiten](../relational-databases/indexes/columnstore-indexes-what-s-new.md). 

<sup>3</sup> Dieses Feature ist in der LocalDB-Installationsoption nicht enthalten.

<sup>4</sup> Standard Edition und SQL Server + CAL-basierte Lizenzierung kann die Anzahl von Prozessoren einschränken, die von SQL Server Standard verwendet werden können, aber SQL Server Standard ist NUMA-fähig. 
##  <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS Security  
  
|Funktion|Enterprise|Standard|Web|Express|Express mit Advanced Services|  
|-------------|----------------|--------------|---------|-------------|------------------------------------| 
|Sicherheit auf Zeilenebene|Ja|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|  
|Always Encrypted|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>| 
|Dynamische Datenmaskierung|Ja|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|   
|Serverüberwachung|Ja|Ja|Ja|Ja|Ja| 
|Datenbanküberwachung|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>| 
|Transparente Datenbankverschlüsselung|Ja|Nein|Nein|Nein|Nein|   
|Erweiterbare Schlüsselverwaltung|Ja|Nein|Nein|Nein|Nein | 
|Benutzerdefinierte Rollen|Ja|Ja|Ja|Ja|Ja| 
|Eigenständige Datenbanken|Ja|Ja|Ja|Ja|Ja| 
|Verschlüsselung von Sicherungen|Ja|Ja|Nein|Nein|Nein|  

<sup>1</sup> Gilt für [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (CPSA) für alle Editionen.      

##  <a name="replication"></a><a name="Replication"></a> Replikation  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Heterogene Abonnenten|Ja|Ja|Nein|Nein|Nein|  
|Mergereplikation|Ja|Ja|Ja (Nur Abonnent)|Ja (Nur Abonnent)|Ja (Nur Abonnent)|   
|Veröffentlichungen mit Oracle|Ja|Nein|Nein|Nein|Nein| 
|Peer-zu-Peer-Transaktionsreplikation|Ja|Nein|Nein|Nein|Nein|   
|Momentaufnahmereplikation|Ja|Ja|Ja (Nur Abonnent)|Ja (Nur Abonnent)|Ja (Nur Abonnent)|   
|SQL Server-Änderungsnachverfolgung|Ja|Ja|Ja|Ja|Ja| 
|Transaktionsreplikation|Ja|Ja|Ja (Nur Abonnent)|Ja (Nur Abonnent)|Ja (Nur Abonnent)|   
|Transaktionsreplikation zu Azure|Ja|Ja|Nein|Nein|Nein|   
|Aktualisierbares Abonnement für Transaktionsreplikation|Ja|Ja|Nein|Nein|Nein|  
  
##  <a name="management-tools"></a><a name="SSMS"></a> Management Tools  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|SQL Management Objects (SMO)|Ja|Ja|Ja|Ja|Ja|  
|SQL-Konfigurations-Manager|Ja|Ja|Ja|Ja|Ja|   
|SQL CMD (Command Prompt Tool – Eingabeaufforderungstool)|Ja|Ja|Ja|Ja|Ja|      
|Distributed Replay: Administratortool|Ja|Ja|Ja|Ja|Nein|  
|Distributed Replay – Client|Ja|Ja|Ja|Nein|Nein|  
|Distributed Replay - Controller|Ja (bis zu 16 Clients)|Ja (1 Client)|Ja (1 Client)|Nein|Nein|   
|SQL Profiler|Ja|Ja|Nein <sup>1</sup>|Nein <sup>1</sup>|Nein <sup>1</sup>|  
|SQL Server-Agent|Ja|Ja|Ja|Nein|Nein| 
|Microsoft System Center Operations Manager Management Pack|Ja|Ja|Ja|Nein|Nein|  
|Datenbankoptimierungsratgeber (DTA)|Ja|Ja <sup>2</sup>|Ja<sup>2</sup>|Nein|Nein|      
  
 <sup>1</sup> SQL Server Web, SQL Server Express, SQL Server Express mit Tools und SQL Server Express mit Advanced Services können mit der SQL Server Standard- und SQL Server Enterprise-Edition profiliert werden.  
  
 <sup>2</sup> Optimierung ist nur in Funktionen der Standard Edition aktiviert.  
  
##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS Manageability  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Benutzerinstanzen|Nein|Nein|Nein|Ja|Ja| 
|LocalDB|Nein|Nein|Nein|Ja|Nein| 
|Dedizierte Administratorverbindung|Ja|Ja|Ja|Ja, mit Ablaufverfolgungsflag|Ja, mit Ablaufverfolgungsflag|   
|PowerShell-Skriptunterstützung|Ja|Ja|Ja|Ja|Ja| 
|SysPrep-Unterstützung <sup>1</sup>|Ja|Ja|Ja|Ja|Ja| 
|Unterstützung für Komponentenvorgänge der Datenschichtanwendung: Extrahieren, Bereitstellen, Aktualisieren, Löschen|Ja|Ja|Ja|Ja|Ja| 
|Richtlinienautomatisierung (Überprüfung nach Zeitplan und Änderungen)|Ja|Ja|Ja|Nein|Nein |   
|Sammler von Leistungsdaten|Ja|Ja|Ja|Nein|Nein| 
|Fähigkeit zur Registrierung als verwaltete Instanz in einer Mehrfachinstanzverwaltung|Ja|Ja|Ja|Nein|Nein |   
|Standardleistungsberichte|Ja|Ja|Ja|Nein|Nein | 
|Planhinweislisten und Planeinfrierung für Planhinweislisten|Ja|Ja|Ja|Nein|Nein |   
|Direkte Abfrage von indizierten Sichten (mittels NOEXPAND-Hinweis)|Ja|Ja|Ja|Ja|Ja| 
|Automatische Wartung für indizierte Sichten|Ja|Ja|Ja|Nein|Nein | 
|Verteilte partitionierte Sichten|Ja|Nein|Nein|Nein|Nein | 
|Parallele Indexvorgänge|Ja|Nein|Nein|Nein|Nein |  
|Automatische Verwendung indizierter Sichten mittels Abfrageoptimierer|Ja|Nein|Nein|Nein|Nein | 
|Parallele Konsistenzprüfung|Ja|Nein|Nein|Nein|Nein | 
|SQL Server-Steuerungspunkt für das Hilfsprogramm|Ja|Nein|Nein|Nein|Nein|    
|Pufferpoolerweiterung|Ja|Ja|Nein|Nein|Nein| 
  
 <sup>1</sup> Weitere Informationen finden Sie unter [Überlegungen zur Installation von SQL Server mit SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
 
<sup>2</sup> Gilt für [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (CPSA) für alle Editionen.      
  
##  <a name="development-tools"></a><a name="DevTools"></a> Development Tools  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Integration in Microsoft Visual Studio|Ja|Ja|Ja|Ja|Ja| 
|IntelliSense (Transact-SQL und MDX)|Ja|Ja|Ja|Ja|Ja| 
|SQL Server Data Tools (SSDT)|Ja|Ja|Ja|Ja|Nein|    
|MDX-Bearbeitungs-, MDX-Debug- und MDX-Entwurfstools|Ja|Ja|Nein|Nein|Nein |   
  
##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Grundlegende Integration von R|Ja|Ja|Ja|Ja|Nein|   
|Erweiterte Integration von R|Ja|Nein|Nein|Nein|Nein| 
|R-Server (eigenständig)|Ja|Nein|Nein|Nein|Nein|   
|PolyBase-Computeknoten|Ja|Ja<sup>1</sup>|Ja <sup>1</sup>, <sup>2</sup>|Ja <sup>1</sup>, <sup>2</sup>|Ja <sup>1</sup>, <sup>2</sup>| 
|PolyBase-Hauptknoten|Ja|Nein|Nein|Nein|Nein| 
|JSON|Ja|Ja|Ja|Ja|Ja|   
|Abfragespeicher|Ja|Ja|Ja|Ja|Ja|   
|Temporal|Ja|Ja|Ja|Ja|Ja|   
|Common Language Runtime (CLR)-Integration|Ja|Ja|Ja|Ja|Ja|   
|Systemeigene XML-Unterstützung|Ja|Ja|Ja|Ja|Ja| 
|XML-Indizierung|Ja|Ja|Ja|Ja|Ja| 
|MERGE- und UPSERT-Funktionen|Ja|Ja|Ja|Ja|Ja|   
|FILESTREAM-Unterstützung|Ja|Ja|Ja|Ja|Ja| 
|FileTable|Ja|Ja|Ja|Ja|Ja| 
|Datums- und Uhrzeitdatentypen|Ja|Ja|Ja|Ja|Ja|  
|Internationalisierungsunterstützung|Ja|Ja|Ja|Ja|Ja| 
|Volltextsuche und semantische Suche|Ja|Ja|Ja|Ja|Nein| 
|Angabe der Sprache in einer Abfrage|Ja|Ja|Ja|Ja|Nein|   
|Service Broker (Messaging)|Ja|Ja|Nein (nur Client)|Nein (nur Client)|Nein (nur Client)|   
|Transact-SQL-Endpunkte|Ja|Ja|Ja|Nein|Nein| 

<sup>1</sup> Die Aufskalierung mit mehreren Computeknoten erfordert einen Hauptknoten.

<sup>2</sup> Gilt für [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (CPSA) für alle Editionen.     
  
## <a name="integration-services"></a><a name="IS"></a> Integration Services

Informationen über die Features von Integration Services (SSIS), die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Integration Services Features Supported by the Editions of SQL Server (Von den SQL Server-Editionen unterstützte Integration Services-Funktionen)](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="master-data-services"></a><a name="MDS"></a> Master Data Services  
 Informationen über die [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] - und Data Quality Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)]unterstützt werden, finden Sie unter [Master Data Services and Data Quality Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server 2016 unterstützte Master Data Services- und Data Quality Services-Features)](../master-data-services/master-data-services-and-data-quality-services-features-support.md). 

  
##  <a name="data-warehouse"></a><a name="DW"></a> Data Warehouse  
  
|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Erstellen von Cubes ohne Datenbank|Ja|Ja|Nein|Nein|Nein |   
|Automatisches Generieren von Staging- und Data Warehouse-Schemas|Ja|Ja|Nein|Nein|Nein| 
|Erfassung geänderter Daten|Ja|Ja<sup>1</sup>|Nein|Nein|Nein| 
|Optimierung von Sternjoin-Abfragen|Ja|Nein|Nein|Nein|Nein| 
|Skalierbare schreibgeschützte Analysis Services-Konfiguration|Ja|Nein|Nein|Nein|Nein| 
|Parallele Abfrageverarbeitung bei partitionierten Tabellen und Indizes|Ja|Nein|Nein|Nein|Nein|   
|Globale Batchaggregation|Ja|Nein|Nein|Nein|Nein| 

<sup>1</sup> Gilt für [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] SP1 im Rahmen der Erstellung einer allgemeinen Programmierbarkeitsoberfläche (CPSA) für alle Editionen.     

##  <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services  
  
Informationen über die Analysis Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Analysis Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Analysis Services-Features)](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016). 
  
##  <a name="bi-semantic-model-multi-dimensional"></a><a name="BIMD"></a> BI Semantic Model (Multi Dimensional)  
  
Informationen über die Analysis Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Analysis Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Analysis Services-Features)](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
   
##  <a name="bi-semantic-model-tabular"></a><a name="BIT"></a> BI Semantic Model (Tabular)  
  
Informationen über die Analysis Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Analysis Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Analysis Services-Features)](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="power-pivot-for-sharepoint"></a><a name="PPSP"></a> Power Pivot for SharePoint  
  
Informationen über die Power Pivot for SharePoint-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Analysis Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Analysis Services-Features)](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="data-mining"></a><a name="DM"></a> Data Mining  
  
Informationen über die Data Mining-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Analysis Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Analysis Services-Features)](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services  
  
Informationen über die Reporting Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Reporting Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Reporting Services-Features)](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

##  <a name="business-intelligence-clients"></a><a name="BIC"></a> Business Intelligence-Clients  

Informationen über die Business Intelligence-Clientfeatures, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Analysis Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Analysis Services Features)](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016) oder [Reporting Services Features Supported by the Editions of SQL Server (Von den Editionen von SQL Server unterstützte Reporting Services Features)](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Spatial and Location Services  
  
|Funktionsname|Enterprise|Standard|Web|Express mit Advanced Services|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Räumlichkeitsindizes|Ja|Ja|Ja|Ja|Ja|   
|Planarer und geodätischer Datentyp|Ja|Ja|Ja|Ja|Ja| 
|Erweiterte räumliche Bibliotheken|Ja|Ja|Ja|Ja|Ja|   
|Importieren/Exportieren räumlicher Industriestandard-Datenformate|Ja|Ja|Ja|Ja|Ja|   
  
##  <a name="additional-database-services"></a><a name="ADS"></a> Additional Database Services  
  
|Funktionsname|Enterprise|Standard|Web|Express mit Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Migration Assistant|Ja|Ja|Ja|Ja|Ja|   
|Datenbank-E-Mail|Ja|Ja|Ja|Nein|Nein| 
  
##  <a name="other-components"></a><a name="Other"></a> Other Components  
  
|Funktionsname|Enterprise|Standard|Web|Express mit Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------|  
|StreamInsight|StreamInsight Premium-Edition|StreamInsight Standard-Edition|StreamInsight Standard-Edition|Nein|Nein| 
|StreamInsight HA|StreamInsight Premium-Edition|Nein|Nein|Nein|Nein|   
  
> [![SSMS herunterladen](https://docs.microsoft.com/analysis-services/analysis-services/media/download.png)](../ssms/download-sql-server-management-studio-ssms.md) **[Laden Sie die neueste Version von SQL Server Management Studio herunter.](../ssms/download-sql-server-management-studio-ssms.md)**      
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von SQL Server](../database-engine/install-windows/install-sql-server.md)  
 
  
