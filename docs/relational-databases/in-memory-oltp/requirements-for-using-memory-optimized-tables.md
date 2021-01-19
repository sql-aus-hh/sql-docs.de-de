---
title: Anforderungen für die Verwendung speicheroptimierter Tabellen | Microsoft-Dokumentation
description: Informieren Sie sich über die Anforderungen für die Verwendung von In-Memory OLTP, z. B. SQL-Datenbank-Version, Überlegungen zu Arbeitsspeicher und Massenspeicher sowie Installation.
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47d9a7e8-c597-4b95-a58a-dcf66df8e572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0cbe2b75a46e63b5e388b91ace3d74c0db3be49b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170732"
---
# <a name="requirements-for-using-memory-optimized-tables"></a>Anforderungen für die Verwendung von speicheroptimierten Tabellen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Informationen zum Verwenden von In-Memory-OLTP in Azure-Datenbanken finden Sie unter [Erste Schritte mit In-Memory in SQL-Datenbanken](/azure/azure-sql/in-memory-oltp-overview).  
  
 Zusätzlich zu den [Hardware- und Softwareanforderungen für die Installation von SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) gilt für die Verwendung von In-Memory-OLTP Folgendes:  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 (oder höher), alle Editionen. Für [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] RTM (vor SP1) ist die Enterprise-, Developer- oder Evaluation-Edition erforderlich.
    
    > [!NOTE]
    > Für In-Memory OLTP ist die 64-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erforderlich.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] benötigt ausreichend Arbeitsspeicher, um die Daten in speicheroptimierten Tabellen und Indizes aufzunehmen. Des Weiteren wird zusätzlicher Speicherplatz für die Unterstützung der Online-Arbeitsauslastung benötigt. Weitere Informationen finden Sie unter [Schätzen der Arbeitsspeicheranforderungen speicheroptimierter Tabellen](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) .  

-   Stellen Sie bei der Ausführung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem virtuellen Computer (VM) sicher, dass genügend Arbeitsspeicher dem virtuellen Computer zugewiesen ist, sodass der Arbeitsspeicher unterstützt wird, der für speicheroptimierte Tabellen und Indizes benötigt wird. Abhängig von der VM-Hostanwendung wird die Konfigurationsoption zum Garantieren von Speicherbelegung für die VM „reservierter Speicher“ genannt, oder „Mindestarbeitsspeicher“ bei Verwendung von dynamischer Arbeitsspeicherverwaltung. Stellen Sie sicher, dass diese Einstellungen für die Anforderungen der Datenbanken in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausreichend sind.
  
-   Geben Sie Speicherplatz frei, der zweimal der Größe der dauerhaften speicheroptimierten Tabellen entspricht.  
  
-   Der Prozessor muss die **cmpxchg16b** -Anweisung unterstützen, um In-Memory-OLTP verwenden zu können. Alle modernen 64-Bit-Prozessoren unterstützen **cmpxchg16b**.  
  
     Wenn Sie eine VM-Hostanwendung verwenden und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Fehler anzeigt, der von einem älteren Prozessor verursacht wird, sollten Sie überprüfen, ob die Anwendung eine Konfigurationsoption zum Aktivieren von **cmpxchg16b** enthält. Andernfalls können Sie Hyper-V verwenden, das **cmpxchg16b** ohne Änderung einer Konfigurationsoption unterstützt.  
  
-   In-Memory-OLTP wird als Teil von **Database Engine Services** installiert.  
  
     Zum Installieren der Berichtgenerierung ([Bestimmen, ob eine Tabelle oder eine gespeicherte Prozedur zu In-Memory-OLTP portiert werden soll](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)) und [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (um In-Memory-OLTP über den Objekt-Explorer von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] zu verwalten) gehen Sie zu [Herunterladen von SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).   
  
## <a name="important-notes-on-using-hek_2"></a>Wichtige Anmerkungen zur Verwendung von [!INCLUDE[hek_2](../../includes/hek-2-md.md)]  
  
-   Von [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] an ist die Größe für speicheroptimierte Tabellen unbeschränkt. Dies gilt jedoch nicht für den verfügbaren Arbeitsspeicher. 

-   In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] sollte die Gesamtgröße aller dauerhaften Tabellen in einer Datenbank im Arbeitsspeicher maximal 250 GB betragen. Weitere Informationen finden Sie unter [Schätzen der Arbeitsspeicheranforderungen speicheroptimierter Tabellen](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md).  

> [!NOTE]
> Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 unterstützen Standard und Express Edition In-Memory OLTP, aber sie bedingen Kontingente für die Menge an Arbeitsspeicher, die Sie für speicheroptimierte Tabellen in einer bestimmten Datenbank verwenden können. In Standard Edition sind dies 32 GB pro Datenbank. In Express Edition sind dies 352 MB pro Datenbank. 
  
-   Wenn Sie eine oder mehrere Datenbanken mit speicheroptimierten Tabellen erstellen, sollten Sie die sofortige Dateiinitialisierung für die Instanz aktivieren (also dem Dienststartkonto für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Benutzerrecht *SE_MANAGE_VOLUME_NAME* erteilen). Ohne die sofortige Dateiinitialisierung werden speicheroptimierte Speicherdateien (Daten- und Änderungsdateien) bei der Erstellung initialisiert, was sich negativ auf die Leistung der Arbeitsauslastung auswirken kann. Weitere Informationen zur sofortigen Dateiinitialisierung, einschließlich Informationen, wann sie aktiviert werden sollte, finden Sie unter [Sofortige Datenbankdateiinitialisierung](../../relational-databases/databases/database-instant-file-initialization.md).
  
## <a name="see-also"></a>Weitere Informationen  
 [In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
 [Sofortige Datenbankdateiinitialisierung](../../relational-databases/databases/database-instant-file-initialization.md)  
 [Handbuch zur Arbeitsspeicherarchitektur](../../relational-databases/memory-management-architecture-guide.md)
