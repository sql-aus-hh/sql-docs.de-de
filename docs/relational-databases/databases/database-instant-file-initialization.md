---
title: Sofortige Datenbankdateiinitialisierung
description: Hier erfahren Sie mehr über die schnelle Dateiinitialisierung und deren Aktivierung für die SQL Server-Datenbank.
ms.custom: contperf-fy20q4
ms.date: 07/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- initializing files [SQL Server]
- instant file initialization [SQL Server]
- fast file initialization [SQL Server]
- file initialization [SQL Server]
- IFI [SQL Server]
- database instant file initialization [SQL Server]
ms.assetid: 1ad468f5-4f75-480b-aac6-0b01b048bd67
author: stevestein
ms.author: sstein
ms.openlocfilehash: 47bb374abb29374fade7a4c2c158ea4e76abe71b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171742"
---
# <a name="database-instant-file-initialization"></a>Sofortige Datenbankdateiinitialisierung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
In diesem Artikel erfahren Sie mehr über die schnelle Dateiinitialisierung und darüber, wie Sie diese aktivieren, um die Vergrößerung Ihrer SQL Server-Datenbankdateien zu beschleunigen.  

Daten- und Protokolldateien werden standardmäßig initialisiert, um vorhandene Daten zu überschreiben, die von zuvor gelöschten Dateien auf dem Datenträger zurückgelassen wurden. Daten- und Protokolldateien werden erstmals durch Ausfüllen der Dateien mit Nullen initialisiert, wenn Sie einen der folgenden Vorgänge durchführen:  
  
- Erstellen einer Datenbank  
- Fügen Sie Dateien oder Protokolldateien zu einer vorhandenen Datenbank hinzu.  
- Vergrößern einer vorhanden Datei (einschließlich Vorgängen zur automatischen Vergrößerung).  
- Wiederherstellen einer Datenbank oder Dateigruppe  

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht die schnelle Dateiinitialisierung (IFI) eine schnellere Ausführung der zuvor erwähnten Dateivorgänge, da sie verwendeten Speicherplatz freigibt, ohne diesen mit Nullen zu füllen. Stattdessen wird Datenträgerinhalt überschrieben, wenn neue Daten an die Dateien geschrieben werden. Protokolldateien können nicht sofort initialisiert werden.


## <a name="enable-instant-file-initialization"></a>Aktivieren der schnellen Dateiinitialisierung

Die schnelle Dateiinitialisierung ist nur verfügbar, wenn dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienststartkonto *SE_MANAGE_VOLUME_NAME* erteilt wurde. Mitglieder der Windows-Administratorengruppe verfügen über dieses Recht und können es anderen Benutzern erteilen, indem sie diese der Sicherheitsrichtlinie **Durchführen von Volumewartungsaufgaben** hinzufügen.  
> [!IMPORTANT]
> Die Verwendung bestimmter Features wie [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) kann die schnelle Dateiinitialisierung blockieren.  

> [!NOTE]
> Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] kann diese Berechtigung dem Dienstkonto beim Setup im Rahmen einer Installation erteilt werden. <br><br>Wenn Sie eine [Installation über die Eingabeaufforderung](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) durchführen, fügen Sie das Argument /SQLSVCINSTANTFILEINIT hinzu oder aktivieren im [Installationsassistenten](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md) das Kontrollkästchen *SQL Server-Datenbank-Engine-Dienst Berechtigung zum Ausführen von Volumewartungstasks gewähren*.
  
So erteilen Sie einem Konto die Berechtigung `Perform volume maintenance tasks` :  
  
1.  Öffnen Sie auf dem Computer, auf dem die Datendatei erstellt wird, die Anwendung für **lokale Sicherheitsrichtlinien** (`secpol.msc`).  
  
1.  Erweitern Sie im linken Bereich **Lokale Richtlinien**, und klicken Sie dann auf **Zuweisen von Benutzerrechten**.  
  
1.  Doppelklicken Sie im rechten Bereich auf **Durchführen von Volumewartungsaufgaben**.  
  
1.  Klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und fügen Sie das Konto hinzu, über das der SQL Server-Dienst ausgeführt wird.  
  
1.  Klicken Sie auf **Übernehmen**, und schließen Sie dann alle Dialogfelder von **Lokale Sicherheitsrichtlinie** .  

1. Starten Sie den SQL Server-Dienst neu.

1. Überprüfen Sie beim Start das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll.
   
  
    **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 und [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher)
    1. Wenn *SE_MANAGE_VOLUME_NAME* für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienststartkonto erteilt wurde, wird eine Meldung ähnlich der folgenden protokolliert:

        `Database Instant File Initialization: enabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`

    1. Wenn *SE_MANAGE_VOLUME_NAME* **nicht** für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienststartkonto erteilt wurde, wird eine Meldung ähnlich der folgenden protokolliert:

        `Database Instant File Initialization: disabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`
    > [!NOTE]
    > Sie können auch die Spalte *instant_file_initialization_enabled* in der DMV [sys.dm_server_services](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md) verwenden, um festzustellen, ob die schnelle Dateiinitialisierung aktiviert ist.

## <a name="security-considerations"></a>Sicherheitshinweise

Es wird empfohlen, die schnelle Dateiinitialisierung zu aktivieren, da die Vorteile das Sicherheitsrisiko überwiegen können.

Bei Verwendung der schnellen Dateiinitialisierung wird der gelöschte Datenträgerinhalt nur überschrieben, wenn neue Daten in die Datei geschrieben werden. Aus diesem Grund kann ein nicht autorisierter Prinzipal möglicherweise so lange auf den gelöschten Inhalt zugreifen, bis andere Daten in diesen Bereich der Datendatei geschrieben werden.

Während die Datenbankdatei an die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]angefügt ist, wird diese Gefahr einer Offenlegung von Informationen durch die besitzerverwaltete Zugriffssteuerungsliste (Discretionary Access Control List, DACL) in der Datei verringert. Diese DACL gewährt den Dateizugriff nur für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto und den lokalen Administrator. Wenn die Datei jedoch getrennt wird, kann möglicherweise ein Benutzer oder Dienst darauf zugreifen, der nicht über *SE_MANAGE_VOLUME_NAME* verfügt.

Ähnliche Überlegungen existieren, wenn...:

* *...die Datenbank gesichert wird.* Der gelöschte Inhalt kann für einen nicht autorisierten Benutzer oder Dienst verfügbar werden, wenn die Sicherungsdatei nicht mit einer entsprechenden DACL geschützt wird.  

* *...eine Datei mit der IFI vergrößert wird.* Ein SQL Server-Administrator könnte Zugriff auf die grundlegenden Inhalte der Seite und bereits gelöschte Inhalte erhalten.

* *...die Datenbankdateien auf einem Storage Area Network gehostet werden.* Es kann außerdem sein, dass das Storage Area Network neue Seiten immer als vorinitialisiert anzeigt. Die erneute Initialisierung der Seiten durch das Betriebssystem würde einen unnötigen Mehraufwand bedeuten.

Wenn Sie sich Gedanken über eine potenzielle Offenlegung von Informationen machen, sollten Sie eine oder beide der folgenden Maßnahmen treffen:  
  
- Stellen Sie immer sicher, dass alle angebundenen Daten- und Sicherungsdateien über restriktive DACLs verfügen.  
- Deaktivieren Sie die schnelle Dateiinitialisierung für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    Hierzu rufen Sie *SE_MANAGE_VOLUME_NAME* über das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienststartkonto auf.
    
    > [!NOTE]
    > Durch das Deaktivieren verlängert sich die Belegungszeit für Datendateien. Es wirkt sich nur auf Dateien aus, die nach dem Widerrufen des Benutzerrechts erstellt oder vergrößert werden.
  
### <a name="se_manage_volume_name-user-right"></a>Benutzerrecht SE_MANAGE_VOLUME_NAME

Die Benutzerberechtigung *SE_MANAGE_VOLUME_NAME* kann in **Windows-Verwaltungsprogrammen** im Applet **Lokale Sicherheitsrichtlinie** zugewiesen werden. Klicken Sie unter **Lokale Richtlinien** auf **User Right Assignment** (Zuweisung von Benutzerrechten), und ändern Sie die Eigenschaft **Durchführen von Volumewartungsaufgaben**.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Beim Initialisierungsprozess der Datenbankdatei werden Nullen in die neuen Bereiche der initialisierten Datei geschrieben. Die Dauer dieses Vorgangs hängt von der Größe des zu initialisierenden Dateiteils und der Antwortzeit und der Kapazität des Speichersystems ab. Wenn die Initialisierung lange dauert, werden möglicherweise die folgenden Meldungen im SQL Server-Fehlerprotokoll und im Anwendungsprotokoll aufgezeichnet.

```
Msg 5144
Autogrow of file '%.*ls' in database '%.*ls' was cancelled by user or timed out after %d milliseconds.  Use ALTER DATABASE to set a smaller FILEGROWTH value for this file or to explicitly set a new file size.
```

```
Msg 5145
Autogrow of file '%.*ls' in database '%.*ls' took %d milliseconds.  Consider using ALTER DATABASE to set a smaller FILEGROWTH for this file.
```

Eine lange automatische Vergrößerung einer Datenbank und/oder Transaktionsprotokolldatei kann zu Problemen hinsichtlich der Abfrageleistung führen. Dies liegt daran, dass ein Vorgang während der Dauer der Dateivergrößerung, bei dem die automatische Vergrößerung einer Datei erforderlich ist, für Ressourcen wie Sperren oder Latches durchgeführt wird. Möglicherweise werden für Latches für Zuordnungsseiten lange Wartezeiten angezeigt. Der Vorgang, der die lange automatische Vergrößerung erfordert, zeigt den Wartetyp PREEMPTIVE_OS_WRITEFILEGATHER an.





## <a name="see-also"></a>Weitere Informationen  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)