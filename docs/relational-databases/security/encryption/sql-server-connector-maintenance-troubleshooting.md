---
title: SQL Server Connector – Wartung und Problembehandlung
description: Erfahren Sie mehr über die Wartungsanweisungen und die allgemeinen Schritte zur Problembehandlung für den SQL Server Connector.
ms.custom: seo-lt-2019
ms.date: 10/08/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, appendix
ms.assetid: 7f5b73fc-e699-49ac-a22d-f4adcfae62b1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: fa78eb8ef2da01514e161c58b05146b1699c93f7
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151266"
---
# <a name="sql-server-connector-maintenance--troubleshooting"></a>SQL Server-Connector – Verwaltung und Problembehandlung

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dieses Thema enthält ergänzende Informationen zum [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connector. Weitere Informationen zum [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector finden Sie unter [Erweiterbare Schlüsselverwaltung mit Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md), [Installationsschritte für die Erweiterbare Schlüsselverwaltung mit Azure Key Vault.](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md) und [Verwenden von SQL Server-Connector mit SQL-Verschlüsselungsfunktionen](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
##  <a name="a-maintenance-instructions-for-ssnoversion-connector"></a><a name="AppendixA"></a> A. Wartungsanweisungen für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connector  
  
### <a name="key-rollover"></a>Schlüsselrollover  
  
> [!IMPORTANT]  
> Für den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector ist es erforderlich, dass im Schlüsselnamen nur die Zeichen „a-z“, „A-Z“, „0-9“ und „-“ bei einem Zeichenlimit von 26 Zeichen verwendet werden.
> Verschiedene Schlüsselversionen unter dem gleichen Schlüsselnamen in Azure Key Vault funktionieren in Verbindung mit dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connector nicht. Um die Möglichkeit zur Rotation eines Azure Key Vault-Schlüssels zu bieten, der von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verwendet wird, muss ein neuer Schlüssel mit einem neuen Schlüsselnamen erstellt werden.  
  
 In der Regel müssen asymmetrische Serverschlüssel für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verschlüsselung alle ein bis zwei Jahre versioniert werden. Es ist wichtig zu beachten, dass der Schlüsseltresor die Änderung von Schlüsseln zwar zulässt, diese Funktion von Kunden jedoch nicht zum Implementieren einer Versionsverwaltung verwendet werden sollte. Der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connector kann nicht mit Änderungen der Version des Key Vault-Schlüssels umgehen. Zum Implementieren einer Schlüsselversionsverwaltung müssen Sie einen neuen Schlüssel im Schlüsseltresor erstellen und den Datenverschlüsselungsschlüssel in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]neu verschlüsseln.  
  
 Für TDE wird dies folgendermaßen erreicht:  
  
- **In PowerShell:** Erstellen Sie im Schlüsseltresor einen neuen asymmetrischen Schlüssel (mit einem anderen Namen als dem Ihres aktuellen asymmetrischen TDE-Schlüssels).  
  
    ```powershell  
    Add-AzKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'Key2' -Destination 'Software'  
    ```  
  
- **Mit [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] oder „sqlcmd.exe“:** Verwenden Sie die folgenden Anweisungen, wie in Schritt 3, Abschnitt 3 dargestellt.  
  
     Importieren Sie den neuen asymmetrischen Schlüssel.  
  
    ```sql  
    USE master  
    CREATE ASYMMETRIC KEY [MASTER_KEY2]
    FROM PROVIDER [EKM]
    WITH PROVIDER_KEY_NAME = 'Key2',
    CREATION_DISPOSITION = OPEN_EXISTING
    GO  
    ```  
  
     Erstellen Sie eine neue Anmeldung, die dem neuen asymmetrischen Schlüssel zugeordnet wird (wie in den TDE-Anweisungen dargestellt).  
  
    ```sql  
    USE master  
    CREATE LOGIN TDE_Login2
    FROM ASYMMETRIC KEY [MASTER_KEY2]  
    GO  
    ```  
  
     Erstellen Sie neue Anmeldeinformationen zum Zuordnen zur Anmeldung.  
  
    ```sql  
    CREATE CREDENTIAL Azure_EKM_TDE_cred2  
        WITH IDENTITY = 'ContosoDevKeyVault',
       SECRET = 'EF5C8E094D2A4A769998D93440D8115DAADsecret123456789='
    FOR CRYPTOGRAPHIC PROVIDER EKM;  
  
    ALTER LOGIN TDE_Login2  
    ADD CREDENTIAL Azure_EKM_TDE_cred2;  
    GO  
    ```  
  
     Wählen Sie die Datenbank, deren Verschlüsselungsschlüssel sie neu verschlüsseln möchten.  
  
    ```sql  
    USE [database]  
    GO  
    ```  
  
     Verschlüsseln Sie den Datenbank-Verschlüsselungsschlüssel erneut.  
  
    ```sql  
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER ASYMMETRIC KEY [MASTER_KEY2];  
    GO  
    ```  
  
### <a name="upgrade-of-ssnoversion-connector"></a>Upgrade des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connectors  

Die Versionen 1.0.0.440 und älter wurden ersetzt und werden nicht länger in Produktionsumgebungen unterstützt. Die Versionen 1.0.1.0 und neuer werden in Produktionsumgebungen unterstützt. Richten Sie sich nach den folgenden Anweisungen, um ein Upgrade auf die neueste im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45344) verfügbare Version auszuführen.

### <a name="upgrade"></a>Aktualisieren

1. Halten Sie den SQL Server-Dienst mithilfe des SQL Server-Konfigurations-Managers an.
1. Deinstallieren Sie die alte Version über Systemsteuerung\Programme\Programme und Features.
    1. Anwendungsname: SQL Server-Connector für Microsoft Azure Key Vault
    1. Version: 15.0.300.96 (oder älter)
    1. DLL-Dateidatum: 30.1.2018 15:00 Uhr (oder älter)
1. Installieren Sie eine neue Version des SQL Server-Connectors für Microsoft Azure Key Vault bzw. führen Sie ein entsprechendes Upgrade durch.
    1. Version: 15.0.2000.367
    1. DLL-Dateidatum: 11.9.2020 ‏‎05:17 Uhr
1. Starten Sie den SQL Server-Dienst.
1. Testen Sie, ob der Zugriff auf verschlüsselte Datenbanken möglich ist.

### <a name="rollback"></a>Rollback

1. Halten Sie den SQL Server-Dienst mithilfe des SQL Server-Konfigurations-Managers an.

1. Deinstallieren Sie die neue Version über Systemsteuerung\Programme\Programme und Features.
    1. Anwendungsname: SQL Server-Connector für Microsoft Azure Key Vault
    1. Version: 15.0.2000.367
    1. DLL-Dateidatum: 11.9.2020 ‏‎05:17 Uhr

1. Installieren Sie eine alte Version des SQL Server-Connectors für Microsoft Azure Key Vault.
    1. Version: 15.0.300.96
    1. DLL-Dateidatum: 30.1.2018 15:00 Uhr
1. Starten Sie den SQL Server-Dienst.

1. Überprüfen Sie, ob auf die Datenbanken, die TDE verwenden, zugegriffen werden kann.  
  
1. Nachdem Sie bestätigt haben, dass das Update funktioniert, können Sie den alten [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connectorordner löschen (wenn Sie sich in Schritt 3 dazu entschlossen haben, ihn umzubenennen statt zu deinstallieren).

### <a name="older-versions-of-the-sql-server-connector"></a>Ältere Versionen des SQL Server-Connectors
  
Deeplinks zu älteren Versionen des SQL Server-Connectors

- Stromstärke: [1.0.5.0 (Version 15.0.2000.367) – Dateidatum: 11. September 2020](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/1033_15.0.2000.367/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.5.0 (Version 15.0.300.96) – Dateidatum: 30. Januar 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.4.0: (Version 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi)

### <a name="rolling-the-ssnoversion-service-principal"></a>Ändern des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienstprinzipals

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verwendet in Azure Active Directory erstellte Dienstprinzipale als Anmeldeinformationen zum Zugriff auf den Schlüsseltresor. Der Dienstprinzipal verfügt über eine Client-ID und einen Authentifizierungsschlüssel. Für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Anmeldenamen werden der **VaultName**, die **Client-ID** und der **Authentifizierungsschlüssel** festgelegt. Der **Authentifizierungsschlüssel** ist für einen bestimmten Zeitraum (ein oder zwei Jahre) gültig. Vor Ablauf des Zeitraums muss für den Dienstprinzipal ein neuer Schlüssel in Azure AD generiert werden. Anschließend müssen die Anmeldeinformationen in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]geändert werden. [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] verwaltet einen Cache für die Anmeldeinformationen in der aktuellen Sitzung. Wenn also Anmeldeinformationen geändert werden, sollte [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] neu gestartet werden.  
  
### <a name="key-backup-and-recovery"></a>Schlüsselsicherung und -wiederherstellung

Der Schlüsseltresor sollte regelmäßig gesichert werden. Wenn ein asymmetrischer Schlüssel im Tresor verloren geht, kann er aus der Sicherung wiederhergestellt werden. Der Schlüssel muss mit dem gleichen Namen wie zuvor wiederhergestellt werden, was vom PowerShell-Befehl „Restore“ auch ausgeführt wird (siehe Schritte unten).  
Wenn der Tresor verloren geht, müssen Sie einen Tresor neu erstellen und den asymmetrischen Schlüssel unter Verwendung desselben Namens wie zuvor im Tresor wiederherstellen. Der Name für den Tresor kann vom zuvor verwendeten Namen abweichen (oder diesem entsprechen). Legen Sie für den neuen Tresor die Zugriffsberechtigungen fest, damit dem SQL Server-Dienstprinzipal der für die SQL Server-Verschlüsselungsszenarios benötigte Zugriff gewährt wird. Anschließend passen Sie die SQL Server-Anmeldeinformationen dahingehend an, dass diese den neuen Tresornamen widerspiegeln.

Zusammengefasst ergeben sich folgende Schritte:  
  
- Sichern Sie den Tresorschlüssel (mithilfe des PowerShell-Cmdlets „Backup-AzureKeyVaultKey“).  
- Im Fall eines Tresorfehlers erstellen Sie einen neuen Tresor in der gleichen geografischen Region. Der Benutzer, der den Tresor erstellt, sollte sich im gleichen Standardverzeichnis wie der Dienstprinzipal befinden, der für SQL Server eingerichtet wurde.  
- Stellen Sie den Schlüssel für den neuen Tresor wieder her mithilfe des PowerShell-Cmdlets „Restore-AzureKeyVaultKey“ – dieses stellt den Schlüssel mit dem gleichen Namen wie zuvor wieder her. Wenn bereits ein Schlüssel mit dem gleichen Namen vorhanden ist, tritt ein Fehler bei der Wiederherstellung auf.  
- Erteilen Sie dem SQL Server-Dienstprinzipal Berechtigungen zum Verwenden dieses neuen Tresors.
- Ändern Sie die von der Datenbank-Engine verwendeten SQL Server-Anmeldeinformationen so, dass sie den neuen Tresornamen widerspiegeln (falls erforderlich).  
  
Schlüsselsicherungen können in Azure-Regionen wiederhergestellt werden, solange sie in derselben geografischen Region oder nationalen Cloud verbleiben: USA, Kanada, Japan, Australien, Indien, APAC, Europa, Brasilien, China, US-Regierung oder Deutschland.  
  
##  <a name="b-frequently-asked-questions"></a><a name="AppendixB"></a> B. Häufig gestellte Fragen

### <a name="on-azure-key-vault"></a>Zum Azure-Schlüsseltresor
  
**Wie funktionieren Schlüsselvorgänge mit Azure Key Vault?**  
 Der asymmetrische Schlüssel im Schlüsseltresor dient zum Schutz des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Verschlüsselungsschlüssels. Nur der öffentliche Teil des asymmetrischen Schlüssels verlässt jemals den Tresor, der private Teil wird nie vom Tresor exportiert. Alle kryptografischen Vorgänge, die den asymmetrischen Schlüssel verwenden, erfolgen innerhalb des Azure Key Vault-Diensts und werden durch die Sicherheit des Diensts geschützt.  
  
 **Was ist ein Schlüssel-URI?**  
 Jeder Schlüssel im Azure Key Vault weist einen Uniform Resource Identifier (URI) auf, den Sie in Ihrer Anwendung verwenden können, um auf den Schlüssel zu verweisen. Verwenden Sie zum Abrufen der aktuellen Version das Format `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey` und zum Abrufen einer bestimmten Version das Format `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87` .  
  
### <a name="on-configuring-ssnoversion"></a>Zur Konfiguration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

**Auf welche Endpunkte benötigt der SQL Server-Connector Zugriff?**
Der Connector kommuniziert mit zwei Endpunkten, die zugelassen werden müssen. Der einzige Port, der für die ausgehende Kommunikation mit diesen anderen Diensten erforderlich ist, lautet 443 für Https:

- Login.microsoftonline.com/*:443
- *.Vault.Azure.NET/* :443

**Gewusst wie: Herstellen einer Verbindung mit Azure Key Vault über einen HTTP(S)-Proxy Server**
Der Connector verwendet die Proxykonfigurationseinstellungen von Internet Explorer. Diese Einstellungen können über eine [Gruppenrichtlinie](/archive/blogs/askie/how-to-configure-proxy-settings-for-ie10-and-ie11-as-iem-is-not-available) oder mithilfe der Registrierung gesteuert werden, es ist aber zu beachten, dass dies keine systemweiten Einstellungen sind, sondern zielgenau für das Dienstkonto angepasst werden müssen, das die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz ausführt. Wenn ein Datenbankadministrator die Einstellungen im Internet Explorer anzeigt oder bearbeitet, wirkt sich dies nur auf das Konto des Datenbankadministrators und nicht auf die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Engine aus. Das interaktive Anmelden beim Server mithilfe des Dienstkontos ist nicht empfehlenswert und wird in vielen sicheren Umgebungen blockiert. Damit Änderungen an den konfigurierten Proxyeinstellungen angewendet werden, ist möglicherweise ein Neustart der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz erforderlich, da die Einstellungen beim ersten Versuch des Connectors, eine Verbindung mit einem Schlüsseltresor herzustellen, zwischengespeichert werden.

**Was sind die minimal erforderlichen Berechtigungsstufen, die für die einzelnen Konfigurationsschritte in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]erforderlich sind?**  
 Zwar können Sie alle Konfigurationsschritte als Mitglied der festen Serverrolle „sysadmin“ ausführen, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] legt Ihnen jedoch dringlich nahe, die verwendeten Berechtigungen zu minimieren. Die folgende Liste definiert die minimale Berechtigungsstufe für jede Aktion.  
  
- Zum Erstellen eines Kryptografieanbieters ist die `CONTROL SERVER` -Berechtigung oder die Mitgliedschaft in der festen Serverrolle **sysadmin** erforderlich.  
  
- Zum Ändern einer Konfigurationsoption und zum Ausführen der `RECONFIGURE` -Anweisung muss Ihnen die Berechtigung `ALTER SETTINGS` auf Serverebene erteilt worden sein. Die `ALTER SETTINGS` -Berechtigung ist implizit in den festen Serverrollen „sysadmin“ und **serveradmin** enthalten.  
  
- Zum Erstellen von Anmeldeinformationen wird die `ALTER ANY CREDENTIAL` -Berechtigung benötigt.  
  
- Zum Hinzufügen von Anmeldeinformationen zu einer Anmeldung wird die `ALTER ANY LOGIN` -Berechtigung benötigt.  
  
- Zum Erstellen eines asymmetrischen Schlüssels wird die `CREATE ASYMMETRIC KEY` -Berechtigung benötigt.  

**Wie ändere ich mein Standard-Active Directory, sodass mein Schlüsseltresor im gleichen Abonnement und Active Directory erstellt wird wie der Dienstprinzipal, den ich für den [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] -Connector erstellt habe?**

![Schritte für das Ändern des Standardverzeichnisses in Azure AD](../../../relational-databases/security/encryption/media/aad-change-default-directory-helpsteps.png)

1. Navigieren Sie zum klassischen Azure-Portal unter [https://manage.windowsazure.com](https://manage.windowsazure.com).  
2. Klicken Sie links im Menü auf die Option **Einstellungen**.
3. Wählen Sie das Azure-Abonnement aus, das Sie aktuell verwenden, und klicken Sie in den Befehlen am unteren Bildschirmrand auf **Verzeichnis bearbeiten** .
4. Verwenden Sie im Popupfenster die Dropdownliste **Verzeichnis** , um das Active Directory auszuwählen, das Sie verwenden möchten. Dadurch wird es als Standardverzeichnis festgelegt.
5. Vergewissern Sie sich, dass Sie der globale Administrator des neu ausgewählten Active Directorys sind. Wenn Sie nicht der globale Administrator sind, verlieren Sie möglicherweise aufgrund des Verzeichniswechsels Ihre Verwaltungsberechtigungen.
6. Wenn Sie nach dem Schließen des Popupfensters keines Ihrer Abonnements sehen, müssen Sie möglicherweise den Filter **Nach Verzeichnis filtern** im Filter **Abonnements** im oberen rechten Menü des Bildschirms aktualisieren, um Abonnements anzuzeigen, die Ihr neu aktualisiertes Active Directory verwenden.

    > [!NOTE] 
    > Möglicherweise verfügen Sie nicht über die Berechtigung zum Ändern des Standardabonnements für Ihr Azure-Abonnement. Erstellen Sie in diesem Fall den AAD-Dienstprinzipal innerhalb Ihres Standardverzeichnisses, sodass er sich im gleichen Verzeichnis wie der später verwendete Azure Key Vault befindet.

Weitere Informationen zu Active Directory finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)
  
##  <a name="c-error-code-explanations-for-ssnoversion-connector"></a><a name="AppendixC"></a> C. Erläuterungen zu Fehlercodes für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connector

**Anbieterfehlercodes:**  
  
Fehlercode  |Symbol  |BESCHREIBUNG
---------|---------|---------  
0 | scp_err_Success | Der Vorgang war erfolgreich.
1 | scp_err_Failure | Beim Vorgang ist ein Fehler aufgetreten.
2 | scp_err_InsufficientBuffer | Dieser Fehler weist die Engine an, mehr Arbeitsspeicher für den Puffer zuzuweisen.
3 | scp_err_NotSupported | Der Vorgang wird nicht unterstützt. Beispielsweise wird der angegebene Schlüsseltyp oder Algorithmus nicht vom EKM-Anbieter unterstützt.
4 | scp_err_NotFound | Der angegebene Schlüssel oder Algorithmus wurde vom EKM-Anbieter nicht gefunden.
5 | scp_err_AuthFailure | Authentifizierungsfehler beim EKM-Anbieter.
6 | scp_err_InvalidArgument | Das angegebene Argument ist ungültig.
7 | scp_err_ProviderError | Im EKM-Anbieter ist ein nicht angegebener Fehler aufgetreten, der von der SQL-Engine abgefangen wurde.
401 | acquireToken | Der Server hat auf die Anforderung 401 zurückgegeben. Achten Sie darauf, dass die Client-ID und das Geheimnis korrekt sind und die Zeichenfolge mit Anmeldeinformationen eine Verkettung von AAD-Client-ID und -Geheimnis ohne Bindestriche ist.
404 | getKeyByName | Der Server hat mit 404 geantwortet, da der Schlüsselname nicht gefunden wurde. Überprüfen Sie, ob der Schlüsselname in Ihrem Tresor vorhanden ist.
2049 | scp_err_KeyNameDoesNotFitThumbprint | Der Schlüsselname ist zu lang, um in den Fingerabdruck der SQL-Engine zu passen. Der Schlüsselname darf 26 Zeichen nicht überschreiten.
2050 | scp_err_PasswordTooShort | Die geheime Zeichenfolge, bei der es sich um die Verkettung von AAD-Client-ID und geheimem Schlüssel handelt, ist kürzer als 32 Zeichen.
2051 | scp_err_OutOfMemory | Die SQL-Engine verfügt nicht über ausreichend Arbeitsspeicher, deshalb ist beim Zuweisen von Arbeitsspeicher für den EKM-Anbieter ein Fehler aufgetreten.
2052 | scp_err_ConvertKeyNameToThumbprint | Fehler beim Konvertieren des Schlüsselnamens in den Fingerabdruck.
2053 | scp_err_ConvertThumbprintToKeyName|  Fehler beim Konvertieren des Fingerabdrucks in den Schlüsselnamen.
2058 | scp_err_FailureInRegistry|  Der Vorgang konnte in der Registrierung nicht ausgeführt werden. Das SQL Server-Dienstkonto verfügt nicht über die Berechtigung zum Erstellen des Registrierungsschlüssels.
3000 | ErrorSuccess | Der AKV-Vorgang war erfolgreich.
3001 | ErrorUnknown | Unbekannter Fehler beim AKV-Vorgang.
3002 | ErrorHttpCreateHttpClientOutOfMemory | Aufgrund von unzureichendem Arbeitsspeicher kann kein HttpClient für den AKV-Vorgang erstellt werden.
3003 | ErrorHttpOpenSession | Aufgrund eines Netzwerkfehlers kann keine HTTP-Sitzung geöffnet werden.
3004 | ErrorHttpConnectSession | Aufgrund eines Netzwerkfehlers kann keine Verbindung zu einer HTTP-Sitzung hergestellt werden.
3005 | ErrorHttpAttemptConnect | Aufgrund eines Netzwerkfehlers kann kein Verbindungsversuch ausgeführt werden.
3006 | ErrorHttpOpenRequest | Aufgrund eines Netzwerkfehlers kann eine Anforderung nicht geöffnet werden.
3007 | ErrorHttpAddRequestHeader | Ein Anforderungsheader kann nicht hinzugefügt werden.
3008 | ErrorHttpSendRequest | Aufgrund eines Netzwerkfehlers kann eine Anforderung nicht gesendet werden.
3009 | ErrorHttpGetResponseCode | Aufgrund eines Netzwerkfehlers kann kein Antwortcode abgerufen werden.
3010 | ErrorHttpResponseCodeUnauthorized | Der Server hat auf die Anforderung 401 zurückgegeben.
3011 | ErrorHttpResponseCodeThrottled | Der Server hat die Anforderung gedrosselt.
3012 | ErrorHttpResponseCodeClientError | Die vom Connector gesendete Anforderung ist ungültig. Dies bedeutet normalerweise, dass der Name des Schlüssels ungültig ist oder ungültige Zeichen enthält.
3013 | ErrorHttpResponseCodeServerError | Der Server hat mit einem Antwortcode zwischen 500 und 600 geantwortet.
3014 | ErrorHttpQueryHeader | Es kann keine Abfrage auf den Antwortheader ausgeführt werden.
3015 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader | Der Antwortheader kann aufgrund von unzureichendem Arbeitsspeicher nicht kopiert werden.
3016 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer | Der Antwortheader kann bei der Neuzuweisung eines Puffers aufgrund von unzureichendem Arbeitsspeicher nicht abgefragt werden.
3017 | ErrorHttpQueryHeaderNotFound | Der Abfrageheader wurde in der Antwort nicht gefunden.
3018 | ErrorHttpQueryHeaderUpdateBufferLength | Beim Abfragen des Antwortheaders kann die Pufferlänge nicht aktualisiert werden.
3019 | ErrorHttpReadData | Die Antwortdaten können aufgrund eines Netzwerkfehlers nicht gelesen werden.
3076 | ErrorHttpResourceNotFound | Der Server hat mit 404 geantwortet, da der Schlüsselname nicht gefunden wurde. Überprüfen Sie, ob der Schlüsselname in Ihrem Tresor vorhanden ist.
3077 | ErrorHttpOperationForbidden | Der Server hat mit 403 geantwortet, da der Benutzer keine ordnungsgemäße Berechtigung zum Ausführen der Aktion besitzt. Überprüfen Sie, ob Sie über die Berechtigung für den angegebenen Vorgang verfügen. Der Connector benötigt zur ordnungsgemäßen Funktion mindestens die Berechtigungen „get, list, wrapKey, unwrapKey“.
3100 | ErrorHttpCreateHttpClientOutOfMemory               | Es kann kein HttpClient für den AKV-Vorgang aufgrund von unzureichendem Arbeitsspeicher erstellt werden.
3101 | ErrorHttpOpenSession                               | Aufgrund eines Netzwerkfehlers kann keine Http-Sitzung geöffnet werden.
3102 | ErrorHttpConnectSession                            | Aufgrund eines Netzwerkfehlers kann keine Verbindung mit einer Http-Sitzung hergestellt werden.
3103 | ErrorHttpAttemptConnect                            | Aufgrund eines Netzwerkfehlers kann kein Verbindungsversuch ausgeführt werden.
3104 | ErrorHttpOpenRequest                               | Aufgrund eines Netzwerkfehlers kann eine Anforderung nicht geöffnet werden.
3105 | ErrorHttpAddRequestHeader                          | Ein Anforderungsheader kann nicht hinzugefügt werden.
3106 | ErrorHttpSendRequest                               | Aufgrund eines Netzwerkfehlers kann eine Anforderung nicht gesendet werden.
3107 | ErrorHttpGetResponseCode                           | Aufgrund eines Netzwerkfehlers kann kein Antwortcode abgerufen werden.
3108 | ErrorHttpResponseCodeUnauthorized                  | Der Server hat auf die Anforderung 401 zurückgegeben. Achten Sie darauf, dass die Client-ID und das Geheimnis korrekt sind und die Anmeldeinformations-Zeichenfolge eine Verkettung von AAD-Client-ID und-Geheimnis ohne Bindestriche ist.
3109 | ErrorHttpResponseCodeThrottled                     | Der Server hat die Anforderung gedrosselt.
3110 | ErrorHttpResponseCodeClientError                    | Die Anforderung ist ungültig. Dies bedeutet normalerweise, dass der Name des Schlüssels ungültig ist oder ungültige Zeichen enthält.
3111 | ErrorHttpResponseCodeServerError                   | Der Server hat mit einem Antwortcode zwischen 500 und 600 geantwortet.
3112 | ErrorHttpResourceNotFound                          | Der Server hat mit 404 geantwortet, da der Schlüsselname nicht gefunden wurde. Überprüfen Sie, ob der Schlüsselname in Ihrem Tresor vorhanden ist.
3113 | ErrorHttpOperationForbidden                         | Der Server hat mit 403 geantwortet, da der Benutzer keine ordnungsgemäße Berechtigung zum Ausführen der Aktion besitzt. Überprüfen Sie, ob Sie über die Berechtigung für den angegebenen Vorgang verfügen. Es sind mindestens die Berechtigungen „get“, „wrapKey“ und „unwrapKey“ erforderlich.
3114 | ErrorHttpQueryHeader                               | Es kann keine Abfrage auf den Antwortheader ausgeführt werden.
3115 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader          | Der Antwortheader kann aufgrund von unzureichendem Arbeitsspeicher nicht kopiert werden.
3116 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer       | Der Antwortheader kann bei der Neuzuweisung eines Puffers aufgrund von unzureichendem Arbeitsspeicher nicht abgefragt werden.
3117 | ErrorHttpQueryHeaderNotFound                       | Der Abfrageheader wurde in der Antwort nicht gefunden.
3118 | ErrorHttpQueryHeaderUpdateBufferLength             | Beim Abfragen des Antwortheaders kann die Pufferlänge nicht aktualisiert werden.
3119 | ErrorHttpReadData                                  | Die Antwortdaten können aufgrund eines Netzwerkfehlers nicht gelesen werden.
3120 | ErrorHttpGetResponseOutOfMemoryCreateTempBuffer    | Der Antworttext kann beim Erstellen eines temporären Puffers aufgrund von nicht genügendem Arbeitsspeicher nicht abgerufen werden.
3121 | ErrorHttpGetResponseOutOfMemoryGetResultString     | Der Antworttext kann aufgrund von nicht genügendem Arbeitsspeicher beim Abrufen einer Ergebniszeichenfolge nicht abgerufen werden.
3122 | ErrorHttpGetResponseOutOfMemoryAppendResponse      | Der Antworttext kann beim Anfügen der Antwort aufgrund von nicht genügendem Arbeitsspeicher nicht abgerufen werden.
3200 | ErrorGetAADValuesOutOfMemoryConcatPath | Werte von Azure Active Directory-Challenge Headern können beim Verketten des Pfads wegen nicht genügendem Arbeitsspeicher nicht abgerufen werden.
3201 | ErrorGetAADDomainUrlStartPosition | Die Anfangsposition für die Azure Active Directory-Domänen-URL wurde im falsch formatierten Antwort-Challenge Header nicht gefunden.
3202 | ErrorGetAADDomainUrlStopPosition | Die Endposition für die Azure Active Directory-Domänen-URL wurde im falsch formatierten Antwort-Challenge Header nicht gefunden.
3203 | ErrorGetAADDomainUrlMalformatted | Der Azure Active Directory-Antwort-Challenge Header weist ein falsches Format auf und enthält die AAD-Domänen-URL nicht.
3204 | ErrorGetAADDomainUrlOutOfMemoryAlloc | Nicht genügend Arbeitsspeicher beim Zuweisen des Puffers für die Azure Active Directory Domänen-URL.
3205 | ErrorGetAADTenantIdOutOfMemoryAlloc | Nicht genügend Arbeitsspeicher beim Zuweisen des Puffers für die Azure Active Directory-Mandanten-ID.
3206 | ErrorGetAKVResourceUrlStartPosition | Die Anfangsposition für die Azure Key Vault-Ressourcen-URL wurde im falsch formatierten Antwort-Challenge Header nicht gefunden.
3207 | ErrorGetAKVResourceUrlStopPosition | Die Endposition für die Azure Key Vault-Ressourcen-URL wurde im falsch formatierten Antwort-Challenge Header nicht gefunden.
3208 | ErrorGetAKVResourceUrlOutOfMemoryAlloc | Nicht genügend Arbeitsspeicher beim Zuweisen des Puffers für die Azure Key Vault-Ressourcen-URL.
3300 | ErrorGetTokenOutOfMemoryConcatPath | Das Token kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Anforderungspfads nicht abgerufen werden.
3301 | ErrorGetTokenOutOfMemoryConcatBody | Das Token kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Antworttexts nicht abgerufen werden.
3302 | ErrorGetTokenOutOfMemoryConvertResponseString | Das Token kann aufgrund von nicht genügend Arbeitsspeicher beim Konvertieren der Antwortzeichenfolge nicht abgerufen werden.
3303 | ErrorGetTokenBadCredentials | Das Token kann aufgrund falscher Anmeldeinformationen nicht abgerufen werden. Vergewissern Sie sich, dass die Anmeldeinformations-Zeichenfolge des Zertifikats gültig ist.
3304 | ErrorGetTokenFailedToGetToken | Obwohl die Anmeldeinformationen richtig sind, konnte der Vorgang kein gültiges Token abrufen.
3305 | ErrorGetTokenRejected | Das Token ist gültig, wird aber vom Server abgelehnt.
3306 | ErrorGetTokenNotFound | Das Token wurde in der Antwort nicht gefunden.
3307 | ErrorGetTokenJsonParser | Die JSON-Antwort des Servers kann nicht analysiert werden.
3308 | ErrorGetTokenExtractToken | Das Token kann nicht aus der JSON-Antwort extrahiert werden.
3400 | ErrorGetKeyByNameOutOfMemoryConvertResponseString | Der Schlüssel kann aufgrund von nicht genügend Arbeitsspeicher beim Konvertieren der Antwortzeichenfolge nicht anhand des Namens abgerufen werden.
3401 | ErrorGetKeyByNameOutOfMemoryConcatPath | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Pfads nicht anhand des Namens abgerufen werden.
3402 | ErrorGetKeyByNameOutOfMemoryConcatHeader | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Headers nicht anhand des Namens abgerufen werden.
3403 | ErrorGetKeyByNameNoResponse | Der Schlüssel kann aufgrund einer fehlenden Antwort vom Server nicht anhand des Namens abgerufen werden.
3404 | ErrorGetKeyByNameJsonParser | Der Schlüssel kann nicht anhand des Namens abgerufen werden, weil die JSON-Antwort nicht analysiert werden konnte.
3405 | ErrorGetKeyByNameExtractKeyNode | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da der Schlüsselknoten nicht aus der Antwort extrahiert werden konnte.
3406 | ErrorGetKeyByNameExtractKeyId | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da die Schlüssel-ID nicht aus der Antwort extrahiert werden konnte.
3407 | ErrorGetKeyByNameExtractKeyType | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da der Schlüsseltyp nicht aus der Antwort extrahiert werden konnte.
3408 | ErrorGetKeyByNameExtractKeyN | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da der Schlüssel-N nicht aus der Antwort extrahiert werden konnte.
3409 | ErrorGetKeyByNameBase64DecodeN | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da der N nicht Base64-dekodiert werden konnte.
3410 | ErrorGetKeyByNameExtractKeyE | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da der Schlüssel-E nicht aus der Antwort extrahiert werden konnte.
3411 | ErrorGetKeyByNameBase64DecodeE | Der Schlüssel kann nicht anhand des Namens abgerufen werden, da der E nicht Base64-dekodiert werden konnte.
3412 | ErrorGetKeyByNameExtractKeyUri | Der Schlüssel-URI kann nicht aus der Antwort extrahiert werden.
3500 | ErrorBackupKeyOutOfMemoryConvertResponseString | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Konvertieren der Antwortzeichenfolge nicht gesichert werden.
3501 | ErrorBackupKeyOutOfMemoryConcatPath | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Pfads nicht gesichert werden.
3502 | ErrorBackupKeyOutOfMemoryConcatHeader | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Anforderungspfads nicht gesichert werden.
3503 | ErrorBackupKeyNoResponse | Der Schlüssel kann nicht gesichert werden, da der Server nicht antwortet.
3504 | ErrorBackupKeyJsonParser | Der Schlüssel kann nicht gesichert werden, weil die JSON-Antwort nicht analysiert werden konnte.
3505 | ErrorBackupKeyExtractValue | Der Schlüssel kann nicht gesichert werden, weil der Wert nicht aus der JSON-Antwort extrahiert werden konnte.
3506 | ErrorBackupKeyBase64DecodeValue | Der Schlüssel kann aufgrund eines Fehlers bei der Base64-Decodierung des Wertfelds nicht gesichert werden.
3600 | ErrorWrapKeyOutOfMemoryConvertResponseString | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Konvertieren der Antwortzeichenfolge nicht umschlossen werden.
3601 | ErrorWrapKeyOutOfMemoryConcatPath | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Pfads nicht umschlossen werden.
3602 | ErrorWrapKeyOutOfMemoryConcatHeader | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Headers nicht umschlossen werden.
3603 | ErrorWrapKeyOutOfMemoryConcatBody | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Textkörpers nicht umschlossen werden.
3604 | ErrorWrapKeyOutOfMemoryConvertEncodedBody | Der Schlüssel kann aufgrund von nicht genügendem Arbeitsspeicher beim Konvertieren des codierten Textkörpers nicht umschlossen werden.
3605 | ErrorWrapKeyBase64EncodeKey | Der Schlüssel kann nicht umschlossen werden, da bei der Base64-Codierung des Schlüssels ein Fehler auftrat.
3606 | ErrorWrapKeyBase64DecodeValue | Der Schlüssel kann aufgrund eines Fehlers bei der Base64-Decodierung des Antwortwerts nicht umschlossen werden.
3607 | ErrorWrapKeyJsonParser | Der Schlüssel kann nicht umschlossen werden, weil die JSON-Antwort nicht analysiert werden konnte.
3608 | ErrorWrapKeyExtractValue | Der Schlüssel kann aufgrund eines Fehlers beim Extrahieren des Werts aus der Antwort nicht umschlossen werden.
3609 | ErrorWrapKeyNoResponse | Der Schlüssel kann nicht umschlossen werden, da der Server nicht antwortet.
3700 | ErrorUnwrapKeyOutOfMemoryConvertResponseString | Die Umschließung des Schlüssels kann aufgrund von nicht genügendem Arbeitsspeicher beim Konvertieren der Antwortzeichenfolge nicht aufgehoben werden.
3701 | ErrorUnwrapKeyOutOfMemoryConcatPath | Die Umschließung des Schlüssels kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Pfads nicht aufgehoben werden.
3702 | ErrorUnwrapKeyOutOfMemoryConcatHeader | Die Umschließung des Schlüssels kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Headers nicht aufgehoben werden.
3703 | ErrorUnwrapKeyOutOfMemoryConcatBody | Die Umschließung des Schlüssels kann aufgrund von nicht genügendem Arbeitsspeicher beim Verketten des Textkörpers nicht aufgehoben werden.
3704 | ErrorUnwrapKeyOutOfMemoryConvertEncodedBody | Die Umschließung des Schlüssels kann aufgrund von nicht genügendem Arbeitsspeicher beim Konvertieren des codierten Textkörpers nicht aufgehoben werden.
3705 | ErrorUnwrapKeyBase64EncodeKey | Die Umschließung des Schlüssels kann nicht aufgehoben werden, da bei der Base64-Codierung des Schlüssels ein Fehler auftrat.
3706 | ErrorUnwrapKeyBase64DecodeValue | Die Umschließung des Schlüssels kann aufgrund eines Fehlers bei der Base64-Decodierung des Antwortwerts nicht aufgehoben werden.
3707 | ErrorUnwrapKeyJsonParser | Die Umschließung des Schlüssels kann aufgrund eines Fehlers beim Extrahieren des Werts aus der Antwort nicht aufgehoben werden.
3708 | ErrorUnwrapKeyExtractValue | Die Umschließung des Schlüssels kann aufgrund eines Fehlers beim Extrahieren des Werts aus der Antwort nicht aufgehoben werden.
3709 | ErrorUnwrapKeyNoResponse | Die Umschließung des Schlüssels kann nicht aufgehoben werden, da der Server nicht antwortet.
3800 | ErrorSecretAuthParamsGetRequestBody | Fehler beim Erstellen des Anforderungstexts mithilfe von AAD-ClientID und Geheimnis.
3801 | ErrorJWTTokenCreateHeader | Fehler beim Erstellen eines JWT-Token-Headers für die Authentifizierung bei AAD.
3802 | ErrorJWTTokenCreatePayloadGUID | Fehler beim Erstellen der GUID für die JWT-Token-Nutzlast für die Authentifizierung bei AAD.
3803 | ErrorJWTTokenCreatePayload | Fehler beim Erstellen der JWT-Token-Nutzlast für die Authentifizierung bei AAD.
3804 | ErrorJWTTokenCreateSignature | Fehler beim Erstellen einer JWT-Token-Signatur für die Authentifizierung bei AAD.
3805 | ErrorJWTTokenSignatureHashAlg | Fehler beim Abrufen des SHA256-Hash-Algorithmus für die Authentifizierung bei AAD.
3806 | ErrorJWTTokenSignatureHash | Fehler beim Erstellen des SHA256-Hashs für die JWT-Token-Authentifizierung bei AAD.
3807 | ErrorJWTTokenSignatureSignHash | Fehler beim Signieren des JWT-Token-Hashs für die Authentifizierung bei AAD.
3808 | ErrorJWTTokenCreateToken | Fehler beim Erstellen des JWT-Tokens für die Authentifizierung bei AAD.
3809 | ErrorPfxCertAuthParamsImportPfx | Fehler beim Importieren des PFX-Zertifikats für die Authentifizierung bei AAD.
3810 | ErrorPfxCertAuthParamsGetThumbprint | Fehler beim Abrufen des Fingerabdrucks aus dem PFX-Zertifikat für die Authentifizierung bei AAD.
3811 | ErrorPfxCertAuthParamsGetPrivateKey | Fehler beim Abrufen des privaten Schlüssels aus dem PFX-Zertifikat für die Authentifizierung bei AAD.
3812 | ErrorPfxCertAuthParamsSignAlg | Fehler beim Abrufen des RSA-Signaturalgorithmus für die PFX-Zertifikatauthentifizierung bei AAD.
3813 | ErrorPfxCertAuthParamsImportForSign | Fehler beim Importieren des privaten PFX-Schlüssels für die RSA-Signatur für die Authentifizierung bei AAD.
3814 | ErrorPfxCertAuthParamsCreateRequestBody | Fehler beim Erstellen des Antworttexts aus dem PFX-Zertifikat für die Authentifizierung bei AAD.
3815 | ErrorPEMCertAuthParamsGetThumbprint | Fehler bei der Base64-Decodierung des Fingerabdrucks für die Authentifizierung bei AAD.
3816 | ErrorPEMCertAuthParamsGetPrivateKey | Fehler beim Abrufen des privaten RSA-Schlüssels aus PEM für die Authentifizierung bei AAD.
3817 | ErrorPEMCertAuthParamsSignAlg | Fehler beim Abrufen des RSA-Signaturalgorithmus für die PEM-Authentifizierung mit dem privaten Schlüssel bei AAD.
3818 | ErrorPEMCertAuthParamsImportForSign | Fehler beim Importieren des privaten PEM-Schlüssels für die RSA-Signatur für die Authentifizierung bei AAD.
3819 | ErrorPEMCertAuthParamsCreateRequestBody | Fehler beim Erstellen des Anforderungstexts aus dem privaten PEM-Schlüssel für die Authentifizierung bei AAD.
3820 | ErrorLegacyPrivateKeyAuthParamsSignAlg | Fehler beim Abrufen des RSA-Signaturalgorithmus für die Legacy-Authentifizierung mit dem privaten Schlüssel bei AAD.
3821 | ErrorLegacyPrivateKeyAuthParamsImportForSign | Fehler beim Importieren des privaten Legacy-Schlüssels für die RSA-Signatur für die Authentifizierung bei AAD.
3822 | ErrorLegacyPrivateKeyAuthParamsCreateRequestBody        | Fehler beim Erstellen des Anforderungstexts aus dem privaten Legacy-Schlüssel für die Authentifizierung bei AAD.
3900 | ErrorAKVDoesNotExist | Fehler: Nicht aufgelöster Internetname. Dies weist normalerweise darauf hin, dass der Azure Key Vault gelöscht wurde.
4000 | ErrorCreateKeyVaultRetryManagerOutOfMemory | Es kann kein RetryManager für den AKV-Vorgang aufgrund von unzureichendem Arbeitsspeicher erstellt werden.

Wenn Sie Ihren Fehlercode in dieser Tabelle nicht finden, beachten Sie, dass ein Fehler auch noch aus einer Reihe weiterer Gründe auftreten kann:
  
- Möglicherweise haben Sie keinen Internetzugriff und können nicht auf Ihre Azure Key Vault-Instanz zugreifen. Überprüfen Sie Ihre Internetverbindung.  
  
- Der Azure Key Vault-Dienst ist möglicherweise außer Betrieb. Versuchen Sie es später erneut.  
  
- Möglicherweise haben Sie den asymmetrischen Schlüssel im Azure Key Vault oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]gelöscht. Stellen Sie den Schlüssel wieder her.  
  
- Wenn Sie den Fehler „Die Bibliothek kann nicht geladen werden“ erhalten, überprüfen Sie, ob die für die ausgeführte SQL Server-Version geeignete Version der weitervertreibbaren Visual Studio C++-Komponente installiert ist. In der folgenden Tabelle wird angegeben, welche Version aus dem Microsoft Download Center installiert werden muss.

Das Windows-Ereignisprotokoll protokolliert auch Fehler im Zusammenhang mit dem SQL Server-Connector. Dadurch kann zusätzlicher Kontext zu den eigentlichen Fehlerursachen bereitstehen. Die Quelle im Windows-Anwendungsereignisprotokoll ist „SQL Server-Connector für Microsoft Azure Key Vault“.
  
SQL Server-Version  |Link zum Installieren der weitervertreibbaren Komponente
---------|---------
2008, 2008 R2, 2012, 2014 | [Visual C++ Redistributable-Pakete für Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
2016 | [Visual C++ Redistributable für Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)
  
## <a name="additional-references"></a>Zusätzliche Referenzen

 Weitere Informationen über die erweiterbare Schlüsselverwaltung:  
  
- [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
 SQL-Verschlüsselungen, die EKM unterstützen:  
  
- [Aktivieren von TDE in SQL Server mithilfe von EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
- [Verschlüsseln der Sicherung](../../../relational-databases/backup-restore/backup-encryption.md)  
  
- [Erstellen einer verschlüsselten Sicherung](../../../relational-databases/backup-restore/create-an-encrypted-backup.md)  
  
 Verwandte [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Befehle:  
  
- [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
- [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
- [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
- [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
 Azure Key Vault-Dokumentation  
  
- [Was ist der Azure-Schlüsseltresor?](/azure/key-vault/general/basic-concepts)  
  
- [Erste Schritte mit Azure Key Vault](/azure/key-vault/general/overview)  
  
- Referenz der PowerShell- [Azure Key Vault-Cmdlets](/powershell/module/azurerm.keyvault/)  
  
## <a name="see-also"></a>Weitere Informationen

 [Erweiterbare Schlüsselverwaltung mit Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Verwenden von SQL Server-Connector mit SQL-Verschlüsselungsfunktionen](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)  
 [EKM provider enabled (Serverkonfigurationsoption)](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)  
 [Installationsschritte für die Erweiterbare Schlüsselverwaltung mit Azure Key Vault.](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)

Weitere Beispielskripts finden Sie im Blog zum Thema [SQL Server Transparent Data Encryption und erweiterbare Schlüsselverwaltung mit Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
