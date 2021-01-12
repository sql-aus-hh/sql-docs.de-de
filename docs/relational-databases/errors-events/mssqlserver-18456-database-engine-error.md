---
title: MSSQLSERVER_18456 | Microsoft-Dokumentation
description: Ein Verbindungsversuch wird aufgrund eines Fehlers mit einem ungültigen Kennwort oder Benutzernamen in SQL Server abgelehnt. Hier finden Sie eine Erläuterung des Fehlers sowie mögliche Lösungen.
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 18456 (Database Engine error)
ms.assetid: c417631d-be1f-42e0-8844-9f92c77e11f7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9eec7ad0a11116b1eb9e8bf1c33b57ff3bd2b419
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099293"
---
# <a name="mssqlserver_18456"></a>MSSQLSERVER_18456
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|18456|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|LOGON_FAILED|  
|Meldungstext|Fehler bei der Anmeldung für den Benutzer '%.*ls'.%.\*ls|  
  
## <a name="explanation"></a>Erklärung  
Wird ein Verbindungsversuch aufgrund eines Authentifizierungsfehlers im Zusammenhang mit einem falschen Kennwort oder Benutzernamen zurückgewiesen, wird eine Meldung vergleichbar mit der folgenden an den Client zurückgegeben:  "Fehler bei der Anmeldung für den Benutzer <Benutzername>" (Microsoft SQL Server, Fehler: 18456)".  
  
Zusätzlich enthalten die an den Client zurückgegebenen Informationen Folgendes:  
  
"Fehler bei der Anmeldung für den Benutzer <Benutzername>" (.Net SqlClient-Datenanbieter)"  
  
-----------------------------\-  
  
"Servername: <Computername>"  
  
"Fehlernummer: 18456"  
  
"Schweregrad: 14"  
  
"Status: 1"  
  
"Zeilennummer: 65536"  
  
Möglicherweise wird auch die folgende Meldung zurückgegeben:  
  
"Meldung 18456, Ebene 14, Status 1, Server <Computername>, Zeile 1"  
  
"Fehler bei der Anmeldung für den Benutzer <Benutzername>."  
  
## <a name="additional-error-information"></a>Zusätzliche Fehlerinformationen  
Zur Verbesserung der Sicherheit bleibt die Art des Authentifizierungsfehlers in der an den Client zurückgegebenen Fehlermeldung absichtlich verborgen. Im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll hingegen enthält ein entsprechender Fehler einen Fehlerzustand, der einem Authentifizierungsfehler zugeordnet werden kann. Vergleichen Sie den Fehlerzustand mit der folgenden Liste, um den Grund für den Anmeldefehler zu bestimmen.  
  
|State|BESCHREIBUNG|  
|---------|---------------|  
|1|Es sind keine Fehlerinformationen verfügbar. Dieser Status bedeutet normalerweise, dass Sie keine Berechtigung haben, die Fehlerdetails zu empfangen. Weitere Informationen erhalten Sie beim [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administrator.|  
|2|Die Benutzer-ID ist nicht gültig.|  
|5|Die Benutzer-ID ist nicht gültig.|  
|6|Es wurde der Versuch unternommen, einen Windows-Anmeldenamen für die SQL Server-Authentifizierung zu verwenden.|  
|7|Der Anmeldename ist deaktiviert, und das Kennwort ist falsch.|  
|8|Das Kennwort ist falsch.|  
|9|Das Kennwort ist ungültig.|  
|11|Der Anmeldename ist gültig, beim Serverzugriff ist jedoch ein Fehler aufgetreten. Dieser Fehler ist möglicherweise darauf zurückzuführen, dass der Windows-Benutzer als Mitglied der lokalen Administratorgruppe Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat, Windows aber keine Administratoranmeldeinformationen bereitstellt. Starten Sie das Programm zum Herstellen einer Verbindung mit der Option **Als Administrator ausführen**, und fügen Sie anschließend den Windows-Benutzer als spezifischen Anmeldenamen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hinzu.|  
|12|Der Anmeldename ist gültig, beim Serverzugriff ist jedoch ein Fehler aufgetreten.|  
|18|Das Kennwort muss geändert werden.|  
|38, 46|Die vom Benutzer angeforderte Datenbank konnte nicht gefunden werden.|
|58| Wenn SQL Server nur zur Verwendung der Windows Authentifizierung konfiguriert ist und ein Client versucht, sich mit SQL-Authentifizierung anzumelden. Eine weitere Ursache sind nicht übereinstimmende SIDs.|
|102 – 111|AAD-Fehler|
|122 – 124|Fehler: Der Benutzername oder das Kennwort ist leer.|
|126|Die vom Benutzer angeforderte Datenbank ist nicht vorhanden.|
|132 – 133|AAD-Fehler|
  
Andere Fehlerzustände liegen vor und weisen auf einen unerwarteten internen Verarbeitungsfehler hin.  
  
**Weitere ungewöhnliche mögliche Ursache**  
  
In folgenden Situationen kann folgender Fehler gemeldet werden: **Fehler bei der Anmeldung mit der SQL-Authentifizierung. Der Server ist nur für die Windows-Authentifizierung konfiguriert.** kann in den folgenden Situationen zurückgegeben werden.  
  
-   Der Server ist für die Authentifizierung im gemischten Modus konfiguriert, für eine ODBC-Verbindung wird das TCP-Protokoll verwendet, und für die Verbindung wurde nicht explizit angegeben, dass eine vertrauenswürdige Verbindung verwendet werden soll.  
  
-   Der Server ist für die Authentifizierung im gemischten Modus konfiguriert, für eine ODBC-Verbindung werden Named Pipes verwendet, mit den vom Client zum Öffnen der Named Pipe verwendeten Anmeldeinformationen wird automatisch die Identität des Benutzers angenommen, und für die Verbindung wurde nicht explizit angegeben, dass eine vertrauenswürdige Verbindung verwendet werden soll.  
  
Um dieses Problem zu beheben, schließen Sie **TRUSTED_CONNECTION = TRUE** in die Verbindungszeichenfolge ein.  
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel ist der Fehlerzustand des Authentifizierungsfehlers 8. Dies zeigt an, dass das Kennwort falsch ist.  
  
|Date|`Source`|`Message`|  
|--------|----------|-----------|  
|2007-12-05 20:12:56.34|Anmeldung|Error: 18456, Schweregrad: 14, Status: 8.|  
|2007-12-05 20:12:56.34|Anmeldung|Fehler bei der Anmeldung für den Benutzer <Benutzername>. [CLIENT: <ip address>]|  
  
> [!NOTE]  
> Wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des Windows-Authentifizierungsmodus installiert und später in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und den Windows-Authentifizierungsmodus geändert, wird der Anmeldename **sa** zunächst deaktiviert. Dies führt zum Fehler mit dem Status 7: "Fehler bei der Anmeldung für den Benutzer 'sa'." Informationen zum Aktivieren der **sa**-Anmeldung finden Sie unter [Ändern des Serverauthentifizierungsmodus](~/database-engine/configure-windows/change-server-authentication-mode.md).  
  
## <a name="user-action"></a>Benutzeraktion  
Wenn Sie versuchen, eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung herzustellen, überprüfen Sie, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im gemischten Authentifizierungsmodus konfiguriert ist. Weitere Informationen zum Ändern des Authentifizierungsmodus für SQL Server finden Sie unter [Ändern des Serverauthentifizierungsmodus](../../database-engine/configure-windows/change-server-authentication-mode.md)
  
Wenn Sie versuchen, eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung herzustellen, überprüfen Sie, dass die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen vorhanden sind und dass sie richtig geschrieben sind.  
  
Wenn Sie versuchen, eine Verbindung mit Windows-Authentifizierung herzustellen, überprüfen Sie, dass Sie ordnungsgemäß bei der richtigen Domäne angemeldet sind.  
  
Wenn der Fehler den Status 1 angibt, wenden Sie sich an den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administrator.  
  
Wenn Sie mit Ihren Administratoranmeldeinformationen eine Verbindung herstellen möchten, starten Sie die Anwendung mit der Option **Als Administrator ausführen**. Fügen Sie den Windows-Benutzer nach dem Herstellen der Verbindung als einzelne Anmeldung hinzu.  
  
Wenn das [!INCLUDE[ssDE](../../includes/ssde-md.md)] eigenständige Datenbanken unterstützt, vergewissern Sie sich, dass die Anmeldung nach der Migration zu einem Benutzer einer eigenständigen Datenbank nicht gelöscht wurde.  
  
Wenn Sie lokal eine Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herstellen, müssen Dienste, die unter **NT AUTHORITY\NETWORK SERVICE** ausgeführt werden, mithilfe des vollqualifizierten Domänennamens des Computers authentifiziert werden. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Ermitteln, Verwenden des Netzwerkdienstkontos für den Zugriff auf Ressourcen in ASP.NET](/previous-versions/msp-n-p/ff647402(v=pandp.10))