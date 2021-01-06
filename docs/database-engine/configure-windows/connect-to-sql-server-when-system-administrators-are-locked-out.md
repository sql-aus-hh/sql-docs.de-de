---
title: Herstellen einer Verbindung mit SQL Server, wenn Systemadministratoren den Zugriff verloren haben | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Systemadministrator den Zugriff auf SQL Server zurückgewinnen, wenn Sie versehentlich ausgesperrt wurden.
ms.custom: contperf-fy20q4
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- connecting when locked out [SQL Server]
- locked out [SQL Server]
ms.assetid: c0c0082e-b867-480f-a54b-79f2a94ceb67
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b4cf5946fba4eb3c79150b29ef44bd232f9c155
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878946"
---
# <a name="connect-to-sql-server-when-system-administrators-are-locked-out"></a>Herstellen einer Verbindung mit SQL Server, wenn Systemadministratoren den Zugriff verloren haben 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
In diesem Artikel wird beschrieben, wie Sie als Systemadministrator den verlorenen Zugriff auf [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] wiedererlangen können.  Ein Systemadministrator kann aufgrund einer der folgenden Ursachen den Zugriff auf eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verlieren:  
  
-   Alle Anmeldedaten, die Mitglieder der festen Serverrolle sysadmin sind, wurden versehentlich entfernt.  
  
-   Alle Windows-Gruppen, die Mitglieder der festen Serverrolle sysadmin sind, wurden versehentlich entfernt.  
  
-   Die Anmeldedaten, die Mitglieder der festen Serverrolle sysadmin sind, gehören zu Mitarbeitern, die das Unternehmen verlassen haben oder nicht verfügbar sind.  
  
-   Das sa-Konto wurde deaktiviert, oder das Kennwort ist unbekannt.  
  
## <a name="resolution"></a>Lösung

Es wird empfohlen, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz im Einzelbenutzermodus zu starten, um Ihr Zugriffsproblem zu lösen. Dieser Modus verhindert, dass andere Verbindungen hergestellt werden, während Sie versuchen, den Zugriff zurückzugewinnen. Anschließend können Sie eine Verbindung zu Ihrer SQL Server-Instanz herstellen und Ihre Anmeldung zur Serverrolle **sysadmin** hinzufügen. Eine ausführliche Anleitung für diese Lösung finden Sie im Abschnitt [Schrittweise Anleitung](#step-by-step-instructions).


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen können mithilfe der Option `-m` oder `-f` über die Befehlszeile im Einzelbenutzermodus gestartet werden. Jedes Mitglied der lokalen Administratorengruppe des Computers kann als Mitglied der festen Serverrolle **sysadmin** eine Verbindung zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herstellen.  

Wenn Sie die Instanz im Einzelbenutzermodus starten, müssen Sie zunächst den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Dienst beenden. Andernfalls stellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent möglicherweise zuerst eine Verbindung her und belegt die einzige verfügbare Verbindung zum Server. Dadurch wird Ihre Anmeldung blockiert.

Die einzige verfügbare Verbindung kann auch von einer unbekannten Clientanwendung besetzt werden, bevor Sie sich anmelden können. Damit dies nicht geschieht, können Sie die `-m`-Option gefolgt von einem Anwendungsnamen verwenden, um die Verbindungen der angegebenen Anwendung auf eine einzelne Verbindung zu beschränken. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit `-mSQLCMD` starten, werden Verbindungen z. B. auf eine einzelne Verbindung beschränkt, die sich selbst als **sqlcmd**-Clientprogramm identifiziert. Für eine Verbindungsherstellung über den Abfrage-Editor in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] müssen Sie `-m"Microsoft SQL Server Management Studio - Query"` verwenden.  


> [!IMPORTANT]  
> Verwenden Sie `-m` unter keinen Umständen als Sicherheitsfeature mit einem Anwendungsnamen. Clientanwendungen geben den Anwendungsnamen über die Einstellungen der Verbindungszeichenfolgen an, sodass er problemlos mit einem falschen Namen gespooft werden kann.

In der folgenden Tabelle werden die verschiedenen Methoden zusammengefasst, mit denen die Instanz über die Befehlszeile im Einzelbenutzermodus gestartet werden kann.

| Option | BESCHREIBUNG | Verwendung |
|:---|:---|:---|
|`-m` | Beschränkt die Verbindungen auf eine einzelne Verbindung | Wenn keine anderen Benutzer versuchen, eine Verbindung mit der Instanz herzustellen, oder Sie den Namen der Anwendung nicht kennen, mit der die Verbindung zur Instanz hergestellt wird |
|`-mSQLCMD`| Beschränkt die Verbindungen auf eine einzelne Verbindung, die sich als **sqlcmd**-Clientprogramm identifizieren muss| Wenn Sie mit **sqlcmd** eine Verbindung zur Instanz herstellen und verhindern möchten, dass andere Anwendungen die einzige verfügbare Verbindung belegen |
|`-m"Microsoft SQL Server Management Studio - Query"`| Beschränkt die Verbindungen auf eine einzelne Verbindung, die sich selbst als die Anwendung **Microsoft SQL Server Management Studio - Query** identifizieren muss.| Wenn Sie über den Abfrage-Editor in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] eine Verbindung zur Instanz herstellen und verhindern möchten, dass andere Anwendungen die einzige verfügbare Verbindung belegen |
|`-f`| Beschränkt die Verbindungen auf eine einzelne Verbindung und startet die Instanz mit einer minimalen Konfiguration | Wenn eine andere Konfiguration den Start verhindert |
| &nbsp; | &nbsp; | &nbsp; |
  
Eine detaillierte Anleitung zum Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Einzelbenutzermodus finden Sie unter [Starten von SQL Server im Einzelbenutzermodus](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).

## <a name="step-by-step-instructions"></a>Schrittweise Anleitung

In der folgenden ausführlichen Anleitung wird beschrieben, wie Sie einer SQL Server-Anmeldung, die versehentlich den Zugriff verloren hat, die Berechtigungen der Rolle „Systemadministrator“ erteilen.

In dieser Anleitung wird Folgendes vorausgesetzt:

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird unter Windows 8 oder höher ausgeführt. Auf geringfügige Abweichungen bei früheren Versionen von SQL Server oder Windows wird ggf. hingewiesen.

* [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ist auf dem Computer installiert.  

Führen Sie diese Schritte aus, während Sie in Windows als Mitglied der lokalen Administratorgruppe angemeldet sind.

1.  Klicken Sie im Windows-Startmenü mit der rechten Maustaste auf das Symbol für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager, und wählen Sie **Als Administrator ausführen** aus, um Ihre Administratoranmeldeinformationen an den Konfigurations-Manager zu übergeben.  
  
2.  Wählen Sie im linken Bereich des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Managers die Option **SQL Server-Dienste** aus. Suchen Sie im rechten Bereich Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz. (Bei der Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist **(MSSQLSERVER)** nach dem Computernamen angegeben. Benannte Instanzen werden in Großbuchstaben mit demselben Namen wie unter Registrierte Server angezeigt.) Klicken Sie mit der rechten Maustaste auf die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, und klicken Sie dann auf **Eigenschaften**.  
  
3.  Geben Sie auf der Registerkarte **Startparameter** im Feld **Startparameter angeben** die Zeichenfolge `-m` ein, und klicken Sie dann auf **Hinzufügen**. (Der Parameter entspricht einem Bindestrich und dem Kleinbuchstaben m.)  
  
    > [!NOTE]  
    >  Bei einigen früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen gibt es keine Registerkarte **Startparameter** . Doppelklicken Sie in diesem Fall auf der Registerkarte **Erweitert** auf **Startparameter**. Die Parameter werden in einem sehr kleinen Fenster geöffnet. Achten Sie darauf, die vorhandenen Parameter nicht zu ändern. Fügen Sie ganz unten den neuen Parameter `;-m` hinzu, und klicken Sie auf **OK**. (Der Parameter entspricht einem Semikolon, einem Bindestrich und dem Kleinbuchstaben m.)  
  
4.  Klicken Sie auf **OK**, klicken Sie nach Ausgabe der Neustartmeldung mit der rechten Maustaste auf den Servernamen, und klicken Sie dann auf **Neu starten**.  
  
5.  Nachdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu gestartet wurde, befindet sich Ihr Server im Einzelbenutzermodus. Stellen Sie sicher, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent nicht ausgeführt wird. da er andernfalls Ihre einzige Verbindung belegt.  
  
6.  Klicken Sie im Windows-Startmenü mit der rechten Maustaste auf das Symbol für [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], und wählen Sie **Als Administrator ausführen** aus. Dadurch werden Ihre Administratoranmeldeinformationen an SQL Server Management Studio übergeben.
  
    > [!NOTE]  
    >  In früheren Windows-Versionen wird die Option **Als Administrator ausführen** als Untermenü angezeigt.  
  
     In einigen Konfigurationen versucht SSMS, mehrere Verbindungen herzustellen. Mehrere Verbindungen verursachen einen Fehler, da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Einzelbenutzermodus ausgeführt wird. Führen Sie Ihrem Szenario entsprechend die passende Aktion aus:  
  
    1.  Stellen Sie mithilfe der Windows-Authentifizierung, die Ihre Administratoranmeldeinformationen enthält, eine Verbindung zum Objekt-Explorer her. Erweitern Sie **Sicherheit** sowie **Anmeldungen**, und doppelklicken Sie auf Ihre eigene Anmeldung. Wählen Sie auf der Seite **Serverrollen** die Option **sysadmin** aus, und klicken Sie dann auf **OK**.  
  
    2.  Anstatt über den Objekt-Explorer stellen Sie in einem Abfragefenster unter Verwendung der Windows-Authentifizierung (die Ihre Administratoranmeldeinformationen enthält) eine Verbindung her. (Diese Art der Verbindung wird nur unterstützt, wenn sie nicht über den Objekt-Explorer hergestellt wurde.) Führen Sie Code (wie im folgenden Beispiel) aus, um eine neue Anmeldung mit Windows-Authentifizierung hinzuzufügen, die Mitglied der festen Serverrolle **sysadmin** ist. Im folgenden Beispiel wird ein Domänenbenutzer mit dem Namen `CONTOSO\PatK` hinzugefügt.  
  
        ```  
        CREATE LOGIN [CONTOSO\PatK] FROM WINDOWS;  
        ALTER SERVER ROLE sysadmin ADD MEMBER [CONTOSO\PatK];  
        ```  
  
    3.  Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im gemischten Authentifizierungsmodus ausgeführt wird, stellen Sie eine Verbindung in einem Abfragefenster unter Verwendung der Windows-Authentifizierung her (die Ihre Administratoranmeldeinformationen enthält). Führen Sie Code wie den folgenden aus, um eine neue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung, die Mitglied der festen Serverrolle **sysadmin** ist, mit Authentifizierung zu erstellen.  
  
        ```  
        CREATE LOGIN TempLogin WITH PASSWORD = '************';  
        ALTER SERVER ROLE sysadmin ADD MEMBER TempLogin;  
        ```  
  
        > [!WARNING]  
        >  Ersetzen Sie ************ durch ein sicheres Kennwort.  
  
    4.  Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im gemischten Authentifizierungsmodus ausgeführt wird und Sie das Kennwort des **sa** -Kontos zurücksetzen möchten, stellen Sie unter Verwendung der Windows-Authentifizierung (die Ihre Administratoranmeldeinformationen enthält) eine Verbindung zu einem Abfragefenster her. Ändern Sie das Kennwort des **sa** -Kontos mit folgender Syntax.  
  
        ```  
        ALTER LOGIN sa WITH PASSWORD = '************';  
        ```  
  
        > [!WARNING]  
        >  Ersetzen Sie ************ durch ein sicheres Kennwort.  

7. Schließen Sie [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
8. In den folgenden Schritten wird SQL Server in den Mehrbenutzermodus zurückversetzt. Wählen Sie im linken Bereich des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Managers die Option **SQL Server-Dienste** aus.

9. Klicken Sie im rechten Bereich mit der rechten Maustaste auf die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], und klicken Sie dann auf **Eigenschaften**.  
  
10. Wählen Sie auf der Registerkarte **Startparameter** im Feld **Vorhandene Parameter** den Parameter `-m` aus, und klicken Sie auf **Entfernen**.  
  
    > [!NOTE]  
    >  Bei einigen früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen gibt es keine Registerkarte **Startparameter** . Doppelklicken Sie in diesem Fall auf der Registerkarte **Erweitert** auf **Startparameter**. Die Parameter werden in einem sehr kleinen Fenster geöffnet. Entfernen Sie den zuvor hinzugefügten Parameter `;-m` , und klicken Sie dann auf **OK**.  
  
11. Klicken Sie mit der rechten Maustaste auf den Servernamen, und klicken Sie dann auf **Neu starten**. Stellen Sie sicher, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent neu gestartet wird, wenn Sie ihn vor dem Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Einzelbenutzermodus beendet haben.
  
Nun sollten Sie in der Lage sein, mit einem der Konten, das jetzt Mitglied der festen Serverrolle **sysadmin** ist, auf normale Weise eine Verbindung herzustellen.  
  
## <a name="see-also"></a>Weitere Informationen  

* [SCM-Dienste: Konfigurieren der Serverstartoptionen](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
* [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md)  
