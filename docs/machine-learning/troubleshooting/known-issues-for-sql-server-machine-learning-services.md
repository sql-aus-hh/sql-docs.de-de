---
title: Bekannte Probleme bei Python und R
description: In diesem Artikel werden bekannte Probleme und Einschränkungen der Python- und R-Komponenten beschrieben, die in SQL Server Machine Learning Services und SQL Server 2016 R Services bereitgestellt werden.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/13/2020
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 51d729bcdb25900af855e7429e953ee44dc1388b
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766309"
---
# <a name="known-issues-in-sql-server-machine-learning-services"></a>Bekannte Probleme in SQL Server-Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Artikel werden bekannte Probleme und Einschränkungen der Python- und R-Komponenten beschrieben, die in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) und [SQL Server 2016 R Services](../r/sql-server-r-services.md) bereitgestellt werden.

## <a name="setup-and-configuration-issues"></a>Einrichtungs- und Konfigurationsprobleme

Eine Beschreibung der Prozesse zur ersten Einrichtung und Konfiguration finden Sie unter [Installieren von SQL Server-Machine Learning Services](../install/sql-machine-learning-services-windows-install.md). Sie enthält Informationen über Upgrades, parallele Installationen und die Installation neuer R- oder Python-Komponenten.

### <a name="1-inconsistent-results-in-mkl-computations-due-to-missing-environment-variable"></a>1. Inkonsistente Ergebnisse in MKL-Berechnungen aufgrund fehlender Umgebungsvariablen

**Anwendungsbereich:** R_SERVER-Binärdateien 9.0, 9.1, 9.2 oder 9.3.

R_SERVER verwendet die Intel Math Kernel Library (MKL). Bei Berechnungen unter Einbeziehung der MKL können inkonsistente Ergebnisse auftreten, wenn in Ihrem System eine Umgebungsvariable fehlt. 

Legen Sie die Umgebungsvariable `'MKL_CBWR'=AUTO` fest, um die bedingte numerische Reproduzierbarkeit in R_SERVER sicherzustellen. Weitere Informationen finden Sie unter [Introduction to Conditional Numerical Reproducibility (CNR)](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) (Einführung in die bedingte numerische Reproduzierbarkeit).

**Problemumgehung**

1. Klicken Sie in der Systemsteuerung auf **System und Sicherheit** > **System** > **Erweiterte Systemeinstellungen** > **Umgebungsvariablen**.

2. Erstellen Sie eine neue Benutzer- oder Systemvariable. 

   + Legen Sie den Variablennamen auf „MKL_CBWR“ fest.
   + Legen Sie den „Variablenwert“ auf „AUTO“ fest.

3. Starten Sie R_SERVER neu. Auf SQL Server können Sie den SQL Server-Launchpad-Dienst neu starten.

> [!NOTE]
> Wenn Sie die SQL Server 2019 unter Linux ausführen, bearbeiten oder erstellen Sie *.bash_profile* in Ihrem Benutzerstammverzeichnis, und fügen Sie die Zeile `export MKL_CBWR="AUTO"` hinzu. Führen Sie diese Datei aus, indem Sie `source .bash_profile` in der Bash-Eingabeaufforderung eingeben. Starten Sie R_SERVER neu, indem Sie an der R-Eingabeaufforderung `Sys.getenv()` eingeben.

### <a name="2-r-script-runtime-error-sql-server-2017-cu5-cu7-regression"></a>2. Runtimefehler des R-Skripts (SQL Server 2017: Regression in CU5-CU7)

Bei SQL Server 2017 tritt in den kumulativen Updates 5 bis 7 eine Regression in der Datei **rlauncher.config** auf, wo der Dateipfad des temporären Verzeichnisses ein Leerzeichen enthält. Diese Regression wurde in CU8 korrigiert.

Beim Ausführen des R-Skripts werden folgende Fehlermeldungen angezeigt:

> *Fehler beim Kommunizieren mit der Runtime für das „R“-Skript. Überprüfen Sie die Anforderungen der „R“-Runtime.*
>
> STDERR-Meldung(en) vom externen Skript: 
>
> *Schwerwiegender Fehler: „R_TempDir“ kann nicht erstellt werden.*

**Problemumgehung**

Wenden Sie CU8 an, wenn es verfügbar ist. Alternativ können Sie **rlauncher.config** neu erstellen, indem Sie **registerrext** mit „uninstall/install“ an einer Eingabeaufforderung mit erhöhten Rechten ausführen. 

```cmd
<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /uninstall /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>

<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /install /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>
```

Im folgenden Beispiel werden die Befehle mit „C:\Programme\Microsoft SQL Server\" als Installationsort der Standardinstanz „MSSQL14.MSSQLSERVER“ angezeigt:

```cmd
"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /uninstall /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER

"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /install /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER
```

### <a name="3-unable-to-install-sql-server-machine-learning-features-on-a-domain-controller"></a>3. Machine Learning-Features von SQL Server können nicht auf einem Domänencontroller installiert werden

Wenn Sie versuchen, SQL Server 2016 R Services oder SQL Server-Machine Learning Services auf einem Domänencontroller zu installieren, treten beim Setup folgende Fehler auf:

> *Fehler während des Setupvorgangs für die Funktion*
>
> *Gruppe mit Identität kann nicht gefunden werden*
>
> *Komponentenfehlercode: 0x80131509*

Der Fehler tritt auf, weil der Dienst auf einem Domänencontroller nicht die zum Ausführen von Machine Learning erforderlichen 20 lokalen Konten erstellen kann. Im Allgemeinen sollten Sie SQL Server nicht auf einem Domänencontroller installieren. Weitere Informationen finden Sie im [Supportbulletin 2032911](https://support.microsoft.com/help/2032911/you-may-encounter-problems-when-installing-sql-server-on-a-domain-cont).

### <a name="4-install-the-latest-service-release-to-ensure-compatibility-with-microsoft-r-client"></a>4. Installieren des aktuellsten Service Release, um die Kompatibilität mit Microsoft R Client sicherzustellen

Wenn Sie die neueste Version von Microsoft R Client installieren und anschließend verwenden, um R in einem Remotecomputekontext auf SQL Server auszuführen, erhalten Sie möglicherweise folgende Fehlermeldung:

> *Sie führen Version 9.x.x von Microsoft R Client auf Ihrem Computer aus. Diese ist nicht mit der Microsoft R Server-Version 8.x.x kompatibel. Laden Sie eine kompatible Version herunter und installieren Sie sie.*

SQL Server 2016 erfordert, dass die R-Bibliotheken auf dem Client exakt mit den R-Bibliotheken auf dem Server übereinstimmen. Die Einschränkung wurde für spätere Releases als R Server 9.0.1 aufgehoben. Wenn dieser Fehler auftritt, überprüfen Sie jedoch die Version der R-Bibliotheken, die von Ihrem Client und dem Server verwendet werden, und aktualisieren Sie ggf. den Client entsprechend der Serverversion.

Die Version von R, die mit SQL Server R Services installiert ist, wird aktualisiert, wenn ein Service Release für SQL Server installiert wird. Installieren Sie unbedingt alle Servicepacks, um sicherzustellen, dass Sie stets über die aktuellsten Versionen von R-Komponenten verfügen.

Um die Kompatibilität mit Microsoft R Client 9.0.0 sicherzustellen, installieren Sie die Updates, die in diesem [Supportartikel](https://support.microsoft.com/kb/3210262) beschrieben werden.

Um Probleme mit R-Paketen zu vermeiden, können Sie auch die Version der R-Bibliotheken aktualisieren, die auf dem Server installiert sind, indem Sie den Servicevertrag so ändern, dass die Modern Lifecycle Support-Richtlinie angewendet wird, wie im [nächsten Abschnitt](#bkmk_sqlbindr) beschrieben. In diesem Fall wird die mit SQL Server installierte Version von R nach dem gleichen Zeitplan aktualisiert, der für Updates von Machine Learning Server (früher Microsoft R Server) verwendet wird.

**Anwendungsbereich:** SQL Server 2016 R Services mit R Server Version 9.0.0 oder früher

### <a name="5-r-components-missing-from-cu3-setup"></a>5. R-Komponenten fehlen im CU3-Setup

Eine begrenzte Anzahl von virtuellen Azure-Computern wurde ohne die R-Installationsdateien bereitgestellt, die in SQL Server eingeschlossen werden sollten. Das Problem betrifft virtuelle Computer, die vom 05.01.2018 bis 23.01.2018 bereitgestellt wurden. Dieses Problem könnte sich auch auf lokale Installationen auswirken, wenn Sie das CU3-Update für SQL Server 2017 vom 05.01.2018 bis 23.01.2018 angewendet haben.

Es wurde ein Service Release bereitgestellt, das die richtige Version der R-Installationsdateien enthält.

+ [Kumulatives Updatepaket 3 für SQL Server 2017 KB4052987](https://www.microsoft.com/download/details.aspx?id=56128).

Zum Installieren der Komponenten und Reparieren von SQL Server 2017 CU3 müssen Sie CU3 deinstallieren und die aktualisierte Version neu installieren:

1. Laden Sie die aktualisierte CU3-Installationsdatei herunter, die die R-Installationsprogramme enthält.
2. Deinstallieren Sie CU3. Suchen Sie in der Systemsteuerung nach **Update deinstallieren** , und wählen Sie dann „Hotfix 3015 für SQL Server 2017 (KB4052987) (64-Bit)“ aus. Fahren Sie mit der Deinstallation fort.
3. Installieren Sie das CU3-Update neu, indem Sie auf das soeben heruntergeladene Update für KB4052987 doppelklicken: `SQLServer2017-KB4052987-x64.exe`. Befolgen Sie die Installationsanweisungen.

### <a name="6-unable-to-install-python-components-in-offline-installations-of-sql-server-2017-ctp-20-or-later"></a>6. Python-Komponenten können nicht in Offlineinstallationen von SQL Server 2017 CTP 2.0 oder höher installiert werden

Wenn Sie eine Vorabversion von SQL Server 2017 auf einem Computer ohne Internetzugriff installieren, kann das Installationsprogramm möglicherweise nicht die Seite anzeigen, auf der Sie zur Eingabe des Speicherorts der heruntergeladenen Python-Komponenten aufgefordert werden. In diesem Fall können Sie das Machine Learning Services-Feature installieren, aber nicht die Python-Komponenten.

Dieses Problem wurde in der endgültigen Produktversion behoben. Außerdem gilt diese Einschränkung nicht für R-Komponenten.

**Anwendungsbereich:** SQL Server 2017 mit Python

### <a name="warning-of-incompatible-version-when-you-connect-to-an-older-version-of-sql-server-r-services-from-a-client-by-using-sssqlv14_md"></a><a name="bkmk_sqlbindr"></a> Warnung vor einer inkompatiblen Version beim Herstellen einer Verbindung mit einer älteren Version von SQL Server R Services von einem Client mithilfe von [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]

Wenn Sie R-Code in einem SQL Server 2016-Computekontext ausführen, wird möglicherweise folgende Fehlermeldung angezeigt:

> *Sie führen Version 9.0.0 von Microsoft R Client auf Ihrem Computer aus. Diese ist nicht mit der Microsoft R Server-Version 8.0.3 kompatibel. Laden Sie eine kompatible Version herunter und installieren Sie sie.*

Diese Meldung wird angezeigt, wenn eine der beiden folgenden Aussagen zutrifft:

+ Sie haben R Server (eigenständig) mithilfe des Setup-Assistenten für [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] auf einem Clientcomputer installiert.
+ Sie haben Microsoft R Server mithilfe des [separaten Windows-Installationsprogramms](/machine-learning-server/install/r-server-install-windows) installiert.

Um sicherzustellen, dass Server und Client dieselbe Version verwenden, müssen Sie möglicherweise _binding_ verwenden, was für Microsoft R Server 9.0 und spätere Versionen unterstützt wird, um die R-Komponenten in SQL Server 2016-Instanzen zu aktualisieren. Ob für Ihre Version von R Services Support für Upgrades verfügbar ist, erfahren Sie unter [Aktualisieren von Machine Learning-Komponenten (R und Python) in SQL Server-Instanzen](../install/upgrade-r-and-python.md).

**Anwendungsbereich:** SQL Server 2016 R Services mit R Server Version 9.0.0 oder früher

### <a name="7-setup-for-sql-server-2016-service-releases-might-fail-to-install-newer-versions-of-r-components"></a>7. Setup für SQL Server 2016 Service Releases kann unter Umständen keine neueren R-Komponenten installieren

Wenn Sie ein kumulatives Update installieren oder ein Servicepack für SQL Server 2016 auf einem Computer installieren, der nicht mit dem Internet verbunden ist, zeigt der Setup-Assistent möglicherweise nicht die Aufforderung an, mit der Sie die R-Komponenten mithilfe heruntergeladener CAB-Dateien aktualisieren können. Dieser Fehler tritt üblicherweise auf, wenn mehrere Komponenten zusammen mit der Datenbank-Engine installiert wurden.

Um dieses Problem zu umgehen, können Sie das Service Release installieren, indem Sie die Befehlszeile verwenden und das Argument `MRCACHEDIRECTORY`, das CU1-Updates installiert, wie in diesem Beispiel gezeigt angeben:

`C:\<path to installation media>\SQLServer2016-KB3164674-x64.exe /Action=Patch /IACCEPTROPENLICENSETERMS /MRCACHEDIRECTORY=<path to CU1 CAB files>`

Wie Sie aktuelle Installationsprogramme erhalten, erfahren Sie unter [Installieren von Machine Learning-Komponenten ohne Internetzugang](../install/sql-ml-component-install-without-internet-access.md).

**Anwendungsbereich:** SQL Server 2016 R Services mit R Server Version 9.0.0 oder früher

### <a name="8-launchpad-services-fails-to-start-if-the-version-is-different-from-the-r-version"></a>8. Launchpad-Dienste werden nicht gestartet, wenn die Version von der R-Version abweicht

Wenn Sie SQL Server R Services getrennt von der Datenbank-Engine installieren und die Buildversionen unterschiedlich sind, wird möglicherweise der folgende Fehler im Systemereignisprotokoll angezeigt:

> *Der Dienst SQL Server-Launchpad wurde aufgrund folgenden Fehlers nicht gestartet: Der Dienst antwortete nicht rechtzeitig auf die Start- oder Steuerungsanforderung.*

Dieser Fehler tritt möglicherweise auf, wenn Sie die Datenbank-Engine mithilfe der endgültigen Produktversion installieren, einen Patch anwenden, um die Datenbank-Engine zu aktualisieren, und anschließend das R Services-Feature mithilfe der endgültigen Produktversion hinzufügen.

Um dieses Problem zu vermeiden, verwenden Sie ein Hilfsprogramm wie den Datei-Manager, um die Versionen von „Launchpad. exe“ mit einer Version von SQL-Binärdateien wie „sqldk.dll“ zu vergleichen. Alle Komponenten sollten dieselbe Versionsnummer aufweisen. Wenn Sie eine Komponente aktualisieren, müssen Sie dasselbe Upgrade auf alle anderen installierten Komponenten anwenden.

Suchen Sie im `Binn`-Ordner der Instanz nach Launchpad. In einer Standardinstallation von SQL Server 2016 kann der Pfad beispielsweise `C:\Program Files\Microsoft SQL Server\MSSQL.13.InstanceNameMSSQL\Binn` lauten. 

### <a name="9-remote-compute-contexts-are-blocked-by-a-firewall-in-sql-server-instances-that-are-running-on-azure-virtual-machines"></a>9. Remotecomputekontexte werden in SQL Server-Instanzen, die auf virtuellen Azure-Computern ausgeführt werden, durch die Firewall blockiert

Falls Sie [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] auf einem virtuellen Azure-Computer installiert haben, können Sie Computekontexte, die die Verwendung des Arbeitsbereichs des virtuellen Computers erfordern, möglicherweise nicht verwenden. Der Grund ist, dass die Firewall auf virtuellen Azure-Computern standardmäßig eine Regel enthält, die den Netzwerkzugriff für lokale R-Benutzerkonten blockiert.

Um dieses Problem zu umgehen, öffnen Sie auf der Azure-VM **Windows Firewall mit erweiterter Sicherheit**, wählen Sie **Ausgehende Regel** aus, und deaktivieren Sie die folgende Regel: **Netzwerkzugriff für lokale R-Benutzerkonten in der SQL Server-Instanz MSSQLSERVER blockieren**. Sie können die Regel auch aktiviert lassen, aber ändern Sie dann die Sicherheitseigenschaft in **Zulassen, falls sicher**.

### <a name="10-implied-authentication-in-sqlexpress"></a>10. Implizite Authentifizierung in SQLEXPRESS

Beim Ausführen von R-Aufträgen auf einer Data Science-Remotearbeitsstation mithilfe der integrierten Windows-Authentifizierung verwendet SQL Server die *implizite Authentifizierung*, um lokale ODBC-Aufrufe zu generieren, die das Skript möglicherweise benötigt. Im RTM-Build der SQL Server Express Edition funktionierte diese Funktion jedoch nicht.

Um das Problem zu beheben, empfehlen wir die Aktualisierung auf ein neueres Service Release.

Wenn ein Upgrade nicht möglich ist, können Sie als Problemumgehung eine SQL-Anmeldung verwenden, um R-Remoteaufträge auszuführen, die unter Umständen eingebettete ODBC-Aufrufe benötigen.

**Anwendungsbereich:** SQL Server 2016 R Services Express Edition

### <a name="11-performance-limits-when-libraries-used-by-sql-server-are-called-from-other-tools"></a>11. Leistungsgrenzen, wenn von SQL Server verwendete Bibliotheken von anderen Tools aufgerufen werden

Es ist möglich, die Machine Learning-Bibliotheken aufzurufen, die von einer externen Anwendung wie RGui für SQL Server installiert werden. Dies ist unter Umständen die einfachste Möglichkeit, bestimmte Aufgaben wie das Installieren neuer Pakete oder Ausführen von Ad-hoc-Tests für sehr kurze Codebeispiele zu erledigen. Allerdings könnte die Leistung außerhalb von SQL Server eingeschränkt sein.

Selbst wenn Sie beispielsweise die Enterprise Edition von SQL Server verwenden, wird R im Singlethread-Modus ausgeführt, wenn Sie den R-Code mit externen Tools ausführen. Um die Leistungsvorteile in SQL Server zu nutzen, initiieren Sie eine SQL Server-Verbindung, und rufen Sie mit [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) die Runtime des externen Skripts auf.

Vermeiden Sie im Allgemeinen, die von SQL Server verwendeten Machine Learning-Bibliotheken von externen Tools aufzurufen. Wenn Sie R- oder Python-Code debuggen müssen, ist dies in der Regel außerhalb von SQL Server einfacher. Um dieselben Bibliotheken zu erhalten, die sich in SQL Server befinden, können Sie Microsoft R Client oder [SQL Server 2017 Machine Learning Server (eigenständig)](../install/sql-machine-learning-standalone-windows-install.md) installieren.

### <a name="12-sql-server-data-tools-does-not-support-permissions-required-by-external-scripts"></a>12. SQL Server Data Tools unterstützt keine Berechtigungen, die für externe Skripts erforderlich sind

Wenn Sie Visual Studio oder SQL Server Data Tools zum Veröffentlichen eines Datenbankprojekts verwenden, erhalten Sie möglicherweise eine Fehlermeldung wie die folgende, wenn ein Prinzipal über Berechtigungen verfügt, die für die Ausführung externer Skripts spezifisch sind:

> *TSQL-Modell: Beim Reverse Engineering der Datenbank wurde ein Fehler erkannt. Die Berechtigung wurde nicht erkannt und nicht importiert.*

Das DACPAC-Modell unterstützt derzeit nicht die von R Services oder Machine Learning Services verwendeten Berechtigungen wie GRANT ANY EXTERNAL SCRIPT oder EXECUTE ANY EXTERNAL SCRIPT. Dieses Problem wird in einer künftigen Version behoben werden.

Um dieses Problem zu umgehen, führen Sie die zusätzlichen GRANT-Anweisungen in einem Skript nach der Bereitstellung aus.

### <a name="13-external-script-execution-is-throttled-due-to-resource-governance-default-values"></a>13. Externe Skriptausführung wird aufgrund von Ressourcenkontrolle-Standardwerten gedrosselt

In Enterprise Edition können Sie Ressourcenpools verwenden, um externe Skriptprozesse zu verwalten. In einigen frühen Releasebuilds betrug der maximale Arbeitsspeicher, der den R-Prozessen zugeordnet werden konnte, 20 Prozent. Wenn der Server über 32 GB RAM-Speicher verfügte, konnten die ausführbaren R-Dateien („RTerm.exe“ und „BxlServer.exe“) in einer einzelnen Anforderung maximal 6,4 GB verwenden.

Wenn Ressourcenbeschränkungen auftreten, überprüfen Sie die aktuelle Standardeinstellung. Wenn der Standardwert von 20 Prozent nicht ausreicht, entnehmen Sie der Dokumentation für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], wie Sie diesen Wert ändern.

**Anwendungsbereich:** SQL Server 2016 R Services, Enterprise Edition

### <a name="14-error-when-using-sp_execute_external_script-without-libcso-on-linux"></a>14. Fehler beim Verwenden von `sp_execute_external_script` ohne `libc++.so` unter Linux

Auf einem sauberen Linux-Computer, auf dem nicht `libc++.so` installiert ist, tritt beim Ausführen eines `sp_execute_external_script` (SPEES) mit Java oder einer externen Sprache ein Fehler auf, weil `commonlauncher.so``libc++.so` nicht geladen werden kann.

Beispiel:

```sql
EXECUTE sp_execute_external_script @language = N'Java'
    , @script = N'JavaTestPackage.PassThrough'
    , @parallel = 0
    , @input_data_1 = N'select 1'
WITH RESULT SETS((col1 INT NOT NULL))
GO
```

Hierzu wird eine Fehlermeldung ähnlich der folgenden ausgegeben:

```text
Msg 39012, Level 16, State 14, Line 0

Unable to communicate with the runtime for 'Java' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Java' runtime.
```

Die `mssql-launchpadd`-Protokolle enthalten eine Fehlermeldung mit etwa folgendem Wortlaut:

```text
Oct 18 14:03:21 sqlextmls launchpadd[57471]: [launchpad] 2019/10/18 14:03:21 WARNING: PopulateLauncher failed: Library /opt/mssql-extensibility/lib/commonlauncher.so not loaded. Error: libc++.so.1: cannot open shared object file: No such file or directory
```

**Problemumgehung**

Sie können eine der folgenden Problemumgehungen verwenden:

1. Kopieren Sie `libc++*` aus `/opt/mssql/lib` in den Standardsystempfad `/lib64`.

1. Fügen Sie `/var/opt/mssql/mssql.conf` die folgenden Einträge hinzu, um den Pfad verfügbar zu machen:

   ```text
   [extensibility]
   readabledirectories = /opt/mssql
   ```

**Anwendungsbereich:** SQL Server 2019 unter Linux

### <a name="15-installation-or-upgrade-error-on-fips-enabled-servers"></a>15. Installations- oder Upgradefehler auf FIPS-fähigen Servern

Wenn Sie SQL Server 2019 mit dem Feature **Machine Learning-Dienste und -Spracherweiterungen** installieren oder ein Upgrade für die SQL Server-Instanz auf ein [FIPS](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing)-fähigen Server (Federal Information Processing Standard) durchführen, wird der folgende Fehler ausgelöst:

> *An error occurred while installing extensibility feature with error message: AppContainer Creation Failed with error message NONE, state This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.* (Beim Installieren des Erweiterbarkeitsfeatures ist ein Fehler mit der folgenden Fehlermeldung aufgetreten: Erstellung von AppContainer ist mit der Fehlermeldung „NONE, state“ fehlgeschlagen. Diese Implementation ist nicht Teil der Windows Platform FIPS-überprüften kryptografischen Algorithmen.)

**Problemumgehung**

Deaktivieren Sie FIPS vor der Installation von SQL Server 2019 mit dem Feature **Machine Learning-Dienste und -Spracherweiterungen** oder vor dem Upgrade der SQL Server-Instanz. Sobald die Installation oder das Upgrade abgeschlossen ist, können Sie FIPS wieder aktivieren.

**Anwendungsbereich:** SQL Server 2019

## <a name="r-script-execution-issues"></a>Probleme mit der Ausführung des R-Skripts

Dieser Abschnitt enthält bekannte Probleme, die für die Ausführung von R auf SQL Server spezifisch sind, sowie einige Probleme im Zusammenhang mit den R-Bibliotheken und -Tools einschließlich RevoScaler, die von Microsoft veröffentlicht werden.

Weitere bekannte Probleme, die sich auf R-Lösungen auswirken können, finden Sie auf der [Machine Learning Server](/machine-learning-server/resources-known-issues)-Website.

### <a name="1-access-denied-warning-when-executing-r-scripts-on-sql-server-in-a-non-default-location"></a>1. Zugriffsverweigerungswarnung beim Ausführen von R-Skripts unter SQL Server an einem nicht standardmäßigen Speicherort

Wenn die Instanz von SQL Server an einem nicht standardmäßigen Speicherort (z. B. außerhalb des `Program Files`-Ordners) installiert wurde, wird die Warnung ACCESS_DENIED ausgelöst, wenn Sie versuchen, Skripts auszuführen, mit denen ein Paket installiert wird. Beispiel:

> *In `normalizePath(path.expand(path), winslash, mustWork)` : path[2]="~ExternalLibraries/R/8/1": Der Zugriff wird verweigert*

Der Grund hierfür ist, dass eine R-Funktion versucht, den Pfad zu lesen, und einen Fehler erzeugt, wenn die integrierte Benutzergruppe **SQLRUserGroup** keinen Lesezugriff hat. Die ausgelöste Warnung blockiert nicht die Ausführung des aktuellen R-Skripts, kann jedoch immer dann wiederholt auftreten, wenn der Benutzer ein anderes R-Skript ausführt.

Wenn Sie SQL Server am Standardspeicherort installiert haben, tritt dieser Fehler nicht auf, da alle Windows-Benutzer über Leseberechtigungen für den Ordner `Program Files` verfügen.

Dieses Problem wird in einem zukünftigen Service Release behandelt. Um dieses Problem zu umgehen, gewähren Sie der Gruppe **SQLRUserGroup** Lesezugriff für alle übergeordneten Ordner von `ExternalLibraries`.

### <a name="2-serialization-error-between-old-and-new-versions-of-revoscaler"></a>2. Serialisierungsfehler zwischen alten und neuen Versionen von RevoScaleR

Wenn Sie ein Modell mit einem serialisierten Format an eine Remote-SQL Server-Instanz übergeben, erhalten Sie möglicherweise folgende Fehlermeldung: 

> *Fehler in memDecompress(data, type = decompress) interner Fehler -3 in memDecompress(2).*

Dieser Fehler wird ausgelöst, wenn Sie das Modell mit einer neueren Version der Serialisierungsfunktion, [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel), gespeichert haben, aber die SQL Server-Instanz, auf der Sie das Modell deserialisieren, über eine ältere Version der RevoScaleR-APIs verfügt, von SQL Server 2017 CU2 oder früherer.

Um dieses Problem zu umgehen, können Sie die SQL Server 2017-Instanz auf CU3 oder höher aktualisieren.

Der Fehler wird nicht angezeigt, wenn die API-Version identisch ist, oder wenn Sie ein Modell, das mit einer älteren Serialisierungsfunktion gespeichert ist, auf einen Server verschieben, auf dem eine neuere Version der Serialisierungs-API verwendet wird.

Mit anderen Worten: Verwenden Sie für Serialisierungs- und Deserialisierungsvorgänge die gleiche Version von RevoScaleR.

### <a name="3-real-time-scoring-does-not-correctly-handle-the-_learningrate_-parameter-in-tree-and-forest-models"></a>3. Echtzeitbewertung behandelt den _learningRate_-Parameter in Struktur- und Gesamtstrukturmodellen nicht ordnungsgemäß

Wenn Sie ein Modell mithilfe einer Entscheidungswald- oder Entscheidungsstrukturmethode erstellen und die Lernrate angeben, werden möglicherweise im Gegensatz zur Verwendung von `rxPredict` inkonsistente Ergebnisse angezeigt, wenn Sie `sp_rxpredict` oder die SQL-Funktion `PREDICT` verwenden.

Die Ursache ist ein Fehler in der API, die serialisierte Modelle verarbeitet, und auf den `learningRate`-Parameter beschränkt: z. B. in [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees), oder

Dieses Problem wird in einem zukünftigen Service Release behandelt.

### <a name="4-limitations-on-processor-affinity-for-r-jobs"></a>4. Einschränkungen der Prozessoraffinität für R-Aufträge

Im ersten Releasebuild von SQL Server 2016 konnte die Prozessoraffinität nur für CPUs in der ersten k-Gruppe festgelegt werden. Wenn es sich bei dem Server um einen 2-Socket-Computer mit zwei k-Gruppen handelt, werden beispielsweise nur Prozessoren der ersten k-Gruppe für die R-Prozesse verwendet. Dieselbe Einschränkung gilt beim Konfigurieren der Ressourcenkontrolle für R-Skriptaufträge.

Dieses Problem wurde in SQL Server 2016 Service Pack 1 behoben. Sie sollten auf das aktuelle Service Release aktualisieren.

**Anwendungsbereich:** SQL Server 2016 R Services, RTM-Version

### <a name="5-changes-to-column-types-cannot-be-performed-when-reading-data-in-a-sql-server-compute-context"></a>5. Änderungen an Spaltentypen nicht möglich, wenn Daten in SQL Server-Rechenkontext gelesen werden

Wenn der Rechenkontext auf die SQL Server-Instanz festgelegt ist, können Sie das Argument _ColClasses_ (oder andere ähnliche Argumente) nicht verwenden, um den Datentyp von Spalten in Ihrem R-Code zu ändern.

Die folgende Anweisung würde beispielsweise zu einem Fehler würden, wenn die Spalte CRSDepTimeStr keine Ganzzahl ist:

```R
data <- RxSqlServerData(
  sqlQuery = "SELECT CRSDepTimeStr, ArrDelay  FROM AirlineDemoSmall", 
  connectionString = connectionString, 
  colClasses = c(CRSDepTimeStr = "integer"))
```

Um dieses Problem zu umgehen, können Sie die SQL-Abfrage so umschreiben, dass sie CAST oder CONVERT verwendet, um R die Daten mit dem richtigen Datentyp vorzulegen. Generell ist die Leistung besser, wenn Sie SQL verwenden, um mit Daten zu arbeiten, anstatt Daten im R-Code zu ändern.

**Anwendungsbereich:** SQL Server 2016 R Services

### <a name="6-limits-on-size-of-serialized-models"></a>6. Beschränkungen der Größe serialisierter Modelle

Wenn Sie ein Modell in einer SQL Server-Tabelle speichern, müssen Sie das Modell serialisieren und in einem Binärformat speichern. Theoretisch beträgt die maximale Größe eines Modells, das mit dieser Methode gespeichert werden kann, 2 GB. Dies ist die maximale Größe von varbinary-Spalten in SQL Server.

Wenn Sie größere Modelle verwenden müssen, sind die folgenden Problemumgehungen verfügbar:

+ Ergreifen Sie Maßnahmen, um die Größe des Modells zu verringern. Bei einigen Open-Source-R-Paketen sind zahlreiche Informationen im Modellobjekt enthalten, und viele dieser Informationen können für die Bereitstellung entfernt werden. 
+ Entfernen Sie unnötige Spalten mit der Featureauswahl.
+ Bei einem Open-Source-Algorithmus sollten Sie eine ähnliche Implementierung mithilfe des entsprechenden Algorithmus in MicrosoftML oder RevoScaleR verwenden. Diese Pakete wurden für Bereitstellungsszenarien optimiert.
+ Stellen Sie nach der Rationalisierung des Modells und der Reduzierung der Größe mit den vorangehenden Schritten fest, ob die [memCompress](https://www.rdocumentation.org/packages/base/versions/3.4.1/topics/memCompress)-Funktion in Basis-R verwendet werden kann, um die Größe des Modells zu verringern, bevor es an SQL Server übergeben wird. Diese Option ist am besten geeignet, wenn das Modell in der Nähe des Limits von 2 GB liegt.
+ Bei größeren Modellen können Sie das [FileTable](../../relational-databases/blob/filetables-sql-server.md)-Feature von SQL Server verwenden, um die Modelle zu speichern, anstatt eine varbinary-Spalte zu verwenden.

    Um FileTables verwenden zu können, müssen Sie eine Firewallausnahme hinzufügen, da die in FileTables gespeicherten Daten in SQL Server vom Filestream-Dateisystemtreiber verwaltet werden und die standardmäßigen Firewallregeln den Zugriff auf Netzwerkdateien blockieren. Weitere Informationen finden Sie unter [Aktivieren der erforderlichen Komponenten für FileTable](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md).

    Nachdem Sie FileTable aktiviert haben, um das Modell zu schreiben, erhalten Sie mithilfe der FileTable-API einen Pfad von SQL, und dann schreiben Sie das Modell aus dem Code in diesen Speicherort. Wenn Sie das Modell lesen müssen, erhalten Sie den Pfad von SQL, und Sie können das Modell mit dem Pfad aus dem Skript abrufen. Weitere Informationen finden Sie unter [Zugreifen auf FileTables mit Datei-E/A-APIs](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md).

### <a name="7-avoid-clearing-workspaces-when-you-execute-r-code-in-a-ssnoversion-compute-context"></a>7. Löschen von Arbeitsbereichen vermeiden, wenn Sie R-Code in einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computekontext ausführen

Wenn Sie einen R-Befehl verwenden, um Objekte aus Ihrem Arbeitsbereich zu entfernen, während R-Code in einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computekontext ausgeführt wird, oder wenn Sie den Arbeitsbereich im Rahmen eines R-Skripts löschen, das mit [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) aufgerufen wird, erhalten Sie möglicherweise die folgende Fehlermeldung: *Arbeitsbereichsobjekt revoScriptConnection wurde nicht gefunden*.

`revoScriptConnection` ist ein Objekt im R-Arbeitsbereich, das Informationen über eine R-Sitzung enthält, die aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aufgerufen wird. Wenn Ihr R-Code jedoch einen Befehl zum Löschen des Arbeitsbereichs enthält (wie z.B. `rm(list=ls()))`), werden alle Informationen über die Sitzung und andere Objekte im R-Arbeitsbereich ebenfalls gelöscht.

Um dieses Problem zu umgehen, sollten Sie willkürliches Löschen von Variablen und anderen Objekten während der Ausführung von R in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vermeiden. Das Löschen des Arbeitsbereichs ist bei der Arbeit auf der R-Konsole zwar üblich, kann aber zu unerwarteten Ergebnissen führen.

+ Um bestimmte Variablen zu löschen, verwenden Sie die Funktion `remove` von R: z. B. `remove('name1', 'name2', ...)`.
+ Wenn mehrere zu löschende Variablen vorhanden sind, speichern Sie die Namen temporärer Variablen in einer Liste und führen eine regelmäßige automatische Speicherbereinigung durch.

### <a name="8-restrictions-on-data-that-can-be-provided-as-input-to-an-r-script"></a>8. Einschränkungen von Daten, die als Eingabe für ein R-Skript bereitgestellt werden können

Sie können die folgenden Typen von Abfrageergebnissen nicht in einem R-Skript verwenden:

- Daten aus einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage, die auf „AlwaysEncrypted“-Spalten verweist.
  
- Daten aus einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage, die auf maskierte Spalten verweist.
  
     Wenn Sie maskierte Daten in einem R-Skript verwenden müssen, ist eine mögliche Problemumgehung das Erstellen einer Kopie der Daten in einer temporären Tabelle und anschließende Verwenden dieser Daten.

### <a name="9-use-of-strings-as-factors-can-lead-to-performance-degradation"></a>9. Verwendung von Zeichenfolgen als Faktoren kann zu Leistungseinbußen führen

Die Verwendung von Zeichenfolgentyp-Variablen als Faktoren kann die Menge des für R-Vorgänge verwendeten Arbeitsspeichers erheblich erhöhen. Dies ist ein im Allgemeinen bekanntes Problem bei R, und es gibt viele Artikel zu diesem Thema. Informationen hierzu finden Sie unter [Factors are not first-class citizens in R](https://www.r-bloggers.com/factors-are-not-first-class-citizens-in-r/) (Faktoren sind in R keine Bürger erster Klasse) von John Mount in R-bloggers, oder [stringsAsFactors: An unauthorized biography](https://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/) (stringsAsFactors: Eine nicht autorisierte Biografie), von Roger Peng. 

Obwohl das Problem nicht für SQL Server spezifisch ist, kann sich dies erheblich auf die Leistung von R-Code auswirken, der in SQL Server ausgeführt wird. Zeichenfolgen werden in der Regel als „varchar“ oder „nvarchar“ gespeichert, und wenn eine Spalte mit Zeichenfolgendaten viele eindeutige Werte aufweist, kann der Prozess der internen Umwandlung in ganze Zahlen und zurück in Zeichenfolgen durch R sogar zu Speicherbelegungsfehlern führen.

Wenn Sie nicht unbedingt für andere Vorgänge einen Zeichenfolgen-Datentyp benötigen, ist die Zuordnung der Zeichenfolgenwerte zu einem numerischen Datentyp (Integer) als Teil der Datenvorbereitung aus Leistungs- und Skalierungsperspektive vorteilhaft.

Eine Erörterung dieses Problems und weitere Tipps finden Sie unter [Leistung für R Services – Datenoptimierung](../r/r-and-data-optimization-r-services.md).

### <a name="10-arguments-varstokeep-and-varstodrop-are-not-supported-for-sql-server-data-sources"></a>10. Argumente *varsToKeep* und *varsToDrop* werden für SQL Server-Datenquellen nicht unterstützt

Wenn Sie die Funktion rxDataStep zum Schreiben von Ergebnissen in eine Tabelle verwenden, ist die Verwendung von *varsToKeep* und *varsToDrop* eine praktische Möglichkeit, um die Spalten anzugeben, die als Teil des Vorgangs eingeschlossen oder ausgeschlossen werden sollen. Diese Argumente werden für SQL Server-Datenquellen jedoch nicht unterstützt.

### <a name="11-limited-support-for-sql-data-types-in-sp_execute_external_script"></a>11. Eingeschränkte Unterstützung für SQL-Datentypen in sp\_execute\_external\_script

Nicht alle in SQL unterstützten Datentypen können in R verwendet werden. Um dieses Problem zu umgehen, erwägen Sie, den nicht unterstützten Datentyp in einen unterstützten Datentypen umzuwandeln, bevor die Daten an sp\_execute\_external\_script übergeben werden.

Weitere Informationen finden Sie unter [R-Bibliotheken und Datentypen](../r/r-libraries-and-data-types.md).

### <a name="12-possible-string-corruption-using-unicode-strings-in-varchar-columns"></a>12. Mögliche Zeichenfolgenbeschädigung bei Verwendung von Unicode-Zeichenfolgen in varchar-Spalten

Die Übergabe von Unicode-Daten in varchar-Spalten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an R/Python kann zu einer Zeichenfolgenbeschädigung führen. Dies liegt daran, dass die Codierung für diese Unicode-Zeichenfolge in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierungen möglicherweise nicht mit der in R/Python verwendeten standardmäßigen UTF-8-Codierung identisch ist. 

Um Nicht-ASCII-Zeichenfolgendaten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an R/Python zu senden, verwenden Sie die UTF-8-Codierung (in [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] verfügbar), oder verwenden Sie den nvarchar-Typ für den gleichen Zweck.

### <a name="13-only-one-value-of-type-raw-can-be-returned-from-sp_execute_external_script"></a>13. Es kann nur ein Wert des Typs `raw` von `sp_execute_external_script` zurückgegeben werden

Wenn ein binärer Datentyp (der R-Datentyp **raw**) von R zurückgegeben wird, muss der Wert an den Ausgabedatenrahmen gesendet werden.

Mit anderen Datentypen als **raw** können Sie Parameterwerte zusammen mit den Ergebnissen der gespeicherten Prozedur zurückgeben, indem Sie das Schlüsselwort OUTPUT hinzufügen. Weitere Informationen finden Sie unter [Parameter](../../relational-databases/stored-procedures/parameters.md).

Wenn Sie mehrere Ausgaberesultsets verwenden möchten, die Werte des Typs **raw** enthalten, ist das mehrfache Aufrufen der gespeicherten Prozedur oder Zurücksenden der Resultsets an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe von ODBC als Problemumgehung möglich.

### <a name="14-loss-of-precision"></a>14. Genauigkeitsverlust

Weil [!INCLUDE[tsql](../../includes/tsql-md.md)] und R unterschiedliche Datentypen unterstützen, kann es bei der Konvertierung numerischer Datentypen zu einem Genauigkeitsverlust kommen.

Weitere Informationen zur impliziten Datentypkonvertierung finden Sie unter [Datentypzuordnungen zwischen R und SQL Server](../r/r-libraries-and-data-types.md).

### <a name="15-variable-scoping-error-when-you-use-the-transformfunc-parameter"></a>15. Variablenbereichsfehler bei Verwendung des transformFunc-Parameters

Um Daten zu transformieren, während Sie ein Modell erstellen, können Sie ein *transfodermFunc*-Argument in einer Funktion wie `rxLinmod` oder `rxLogit` übergeben. Allerdings können Aufrufe geschachtelter Funktionen im SQL Server-Computekontext zu Fehlern bei der Bereichsdefinition führen, selbst wenn die Aufrufe im lokalen Computekontext ordnungsgemäß funktionieren.

> *Das Beispieldataset für die Analyse enthält keine Variablen.*

Angenommen, Sie haben die beiden Funktionen `f` und `g` in Ihrer lokalen globalen Umgebung definiert, und `g` ruft `f` auf. Bei verteilten oder Remoteaufrufen unter Verwendung von `g` kann der Aufruf von `g` mit diesem Fehler erfolglos sein, da `f` nicht gefunden werden kann, selbst wenn Sie `f` und `g` an den Remoteaufruf übergeben haben.

Wenn dieses Problem auftritt, können Sie es umgehen, indem Sie die Definition von `f` innerhalb der Definition von `g`an einer Stelle einbetten, vor der `g` normalerweise `f`aufrufen würde.

Beispiel:

```R
f <- function(x) { 2*x * 3 }
g <- function(y) {
              a <- 10 * y
               f(a)
}
```

Um den Fehler zu vermeiden, schreiben Sie die Definition wie folgt um:

```R
g <- function(y){
              f <- function(x) { 2*x +3}
              a <- 10 * y
              f(a)
}
```

### <a name="16-data-import-and-manipulation-using-revoscaler"></a>16. Importieren und Bearbeiten von Daten mithilfe von RevoScaleR

Beim Lesen von **varchar**-Spalten aus einer Datenbank werden Leerzeichen abgeschnitten. Um dies zu verhindern, schließen Sie Zeichenfolgen in Zeichen ein, die keine Leerzeichen sind.

Wenn Sie Funktionen wie `rxDataStep` zum Erstellen von Datenbanktabellen mit **varchar**-Spalten nutzen, wird die Spaltenbreite basierend auf einer Stichprobe der Daten geschätzt. Wenn die Breite variieren kann, könnte es notwendig sein, alle Zeichenfolgen so aufzufüllen, dass ihre Länge gleich ist.

Das Verwenden einer Transformation zum Ändern des Datentyps einer Variablen wird nicht unterstützt, wenn wiederholte Aufrufe von `rxImport` oder `rxTextToXdf` zum Importieren und Anfügen von Zeilen verwendet werden, wobei mehrere Eingabedateien zu einer einzelnen XDF-Datei kombiniert werden.

### <a name="17-limited-support-for-rxexec"></a>17. Eingeschränkte Unterstützung für rxExec

In SQL Server 2016 kann die `rxExec`-Funktion, die vom RevoScaleR-Paket bereitgestellt wird, nur im Singlethread-Modus verwendet werden.

### <a name="18-increase-the-maximum-parameter-size-to-support-rxgetvarinfo"></a>18. Erhöhen der maximalen Parametergröße zur Unterstützung von rxGetVarInfo

Wenn Sie Datensätze mit einer sehr großen Anzahl von Variablen (z. B. über 40.000) verwenden, legen Sie das `max-ppsize`-Flag beim Starten von R fest, um Funktionen wie z. B. `rxGetVarInfo` zu verwenden. Das `max-ppsize` -Flag gibt die maximale Pointer Protection Stack-Größe an.

Wenn Sie die R-Konsole verwenden (z. B. „RGui.exe“ oder „RTerm.exe“), können Sie den Wert von _max-ppsize_ auf 500.000 festlegen, indem Sie Folgendes eingeben:

```R
R --max-ppsize=500000
```

### <a name="19-issues-with-the-rxdtree-function"></a>19. Probleme mit der rxDTree-Funktion

Die `rxDTree` -Funktion unterstützt derzeit keine formelinternen Transformationen. Insbesondere das Verwenden der `F()` -Syntax zum Erstellen von Faktoren bei laufendem Betrieb wird nicht unterstützt. Numerische Daten werden jedoch automatisch klassifiziert.

Geordnete Faktoren werden in allen RevoScaleR-Analysefunktionen außer `rxDTree`genauso wie Faktoren behandelt.

### <a name="20-datatable-as-an-outputdataset-in-r"></a>20. Data.table als OutputDataSet in R

Die Verwendung von `data.table` als `OutputDataSet` in R wird im kumulativen Update 13 (CU13) und früheren Updates von SQL Server 2017 nicht unterstützt. Möglicherweise wird die folgende Meldung angezeigt:

``` text
Msg 39004, Level 16, State 20, Line 2
A 'R' script error occurred during execution of 
'sp_execute_external_script' with HRESULT 0x80004004.
Msg 39019, Level 16, State 2, Line 2
An external script error occurred: 
Error in alloc.col(newx) : 
  Internal error: length of names (0) is not length of dt (11)
Calls: data.frame ... as.data.frame -> as.data.frame.data.table -> copy -> alloc.col

Error in execution.  Check the output for more information.
Error in eval(expr, envir, enclos) : 
  Error in execution.  Check the output for more information.
Calls: source -> withVisible -> eval -> eval -> .Call
Execution halted
```

`data.table` als `OutputDataSet` in R wird im kumulativen Update 14 (CU14) und späteren Updates von SQL Server 2017 unterstützt.

### <a name="21-running-a-long-script-fails-while-installing-a-library"></a>21. Ausführen eines langen Skripts schlägt fehl, wenn eine Bibliothek installiert wird

Wenn das DBO parallel zu einer zeitintensiven Sitzung eines externen Skripts versucht, eine Bibliothek in einer anderen Datenbank zu installieren, kann das Skript beendet werden.

Beispielsweise wird dieses externe Skript für „MASTER“ ausgeführt:

```sql
USE MASTER
DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(max) = N'Sys.sleep(100)'
DECLARE @input_data_1 nvarchar(max) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1 with result sets none
go
```

Währenddessen installiert das DBO parallel eine Bibliothek in LibraryManagementFunctional:

```sql
USE [LibraryManagementFunctional]
go

CREATE EXTERNAL LIBRARY [RODBC] FROM (CONTENT = N'/home/ani/var/opt/mssql/data/RODBC_1.3-16.tar.gz') WITH (LANGUAGE = 'R')
go

DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(14) = N'library(RODBC)'
DECLARE @input_data_1 nvarchar(8) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1
go
```

Das vorherige für „MASTER“ ausgeführte zeitintensive externe Skript wird mit der folgenden Fehlermeldung beendet:

> *Unerwarteter „R“-Skriptfehler beim Ausführen von „sp_execute_external_script“ mit HRESULT 0x800704d4.*

**Problemumgehung**

Führen Sie die Bibliotheksinstallation nicht parallel zur zeitintensiven Abfrage aus. Sie können die zeitintensive Abfrage auch ausführen, sobald die Installation beendet ist.

**Anwendungsbereich:** Nur für SQL Server 2019 unter Linux und Big Data-Clusters.

### <a name="22-sql-server-stops-responding-when-executing-r-scripts-containing-parallel-execution"></a>22. SQL Server reagiert nicht mehr, wenn R-Skripts mit paralleler Ausführung ausgeführt werden.

SQL Server 2019 enthält eine Regression, die sich auf R-Skripts mit paralleler Ausführung auswirkt. Beispiele sind die Verwendung von `rxExec` mit `RxLocalPar` Computekontext und -skripts, die das parallele Paket verwenden. Dieses Problem wird durch Fehler verursacht, auf die das parallele Paket beim Schreiben auf das NULL-Gerät stößt, während es in SQL Server ausgeführt wird.

**Anwendungsbereich:** SQL Server 2019.

### <a name="23-precision-loss-for-moneynumericdecimalbigint-data-types"></a>23. Genauigkeitsverlust bei den Datentypen „money/numeric/decimal/bigint“

Die Ausführung eines R-Skripts mit `sp_execute_external_script` lässt die Datentypen „money“, „numeric“, „decimal“ und „bigint“ als Eingabedaten zu. Da diese jedoch in den numerischen Typ von R konvertiert werden, verlieren sehr hohe Werte oder Werte mit Dezimalstellen an Genauigkeit.

+ **money:** Manchmal könnten Cent-Werte ungenau sein, und es würde folgende Warnung ausgegeben: *Warning: unable to precisely represent cents values* (Warnung: Cent-Werte können nicht genau dargestellt werden).  
+ **numeric/decimal:** `sp_execute_external_script` mit einem R-Skript unterstützt nicht die vollständigen Bereiche der Datentypen und würde die letzten paar Dezimalstellen ändern, insbesondere Bruchzahlen.
+ **bigint:** R unterstützt nur ganze Zahlen bis zu 53 Bit. Höhere Werte verlieren an Genauigkeit.

## <a name="python-script-execution-issues"></a>Probleme mit der Ausführung des Python-Skripts

Dieser Abschnitt enthält bekannte Probleme, die für die Ausführung von Python auf SQL Server spezifisch sind, sowie Probleme im Zusammenhang mit den von Microsoft veröffentlichten Python-Paketen, einschließlich [revoscalepy](/r-server/python-reference/revoscalepy/revoscalepy-package) und [microsoftml](/r-server/python-reference/microsoftml/microsoftml-package).

### <a name="1-call-to-pretrained-model-fails-if-path-to-model-is-too-long"></a>1. Fehler beim Aufruf des vortrainierten Modells, wenn der Pfad zum Modell zu lang ist

Wenn Sie die vorab trainierten Modelle in einem frühen Release von SQL Server 2017 installiert haben, ist der gesamte Pfad zur Datei des trainierten Modells möglicherweise so lang, dass Python ihn nicht lesen kann. Diese Einschränkung wird in einem späteren Service Release korrigiert.

Mehrere Problemumgehungen sind möglich:

+ Wenn Sie die vorab trainierten Modelle installieren, wählen Sie einen benutzerdefinierten Speicherort aus.
+ Installieren Sie die SQL Server-Instanz nach Möglichkeit unter einem kürzeren, benutzerdefinierten Installationspfad, z. B. „C:\SQL\MSSQL14.MSSQLSERVER“.
+ Verwenden Sie das Windows-Hilfsprogramm [Fsutil](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc788097(v=ws.11)), um einen festen Link zu erstellen, der die Modelldatei einem kürzeren Pfad zuordnet.
+ Aktualisieren Sie auf das neueste Service Release.

### <a name="2-error-when-saving-serialized-model-to-sql-server"></a>2. Fehler beim Speichern des serialisierten Modells in SQL Server

Wenn Sie ein Modell an eine Remote-SQL Server-Instanz übergeben und versuchen, das binäre Modell mit der `rx_unserialize`-Funktion in [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) zu lesen, erhalten Sie möglicherweise folgende Fehlermeldung: 

> *NameError: Name „rx_unserialize_model“ ist nicht definiert*

Dieser Fehler wird ausgelöst, wenn Sie das Modell mit einer neueren Version der Serialisierungsfunktion gespeichert haben, aber die SQL Server-Instanz, auf der Sie das Modell deserialisieren, die Serialisierungs-API nicht erkennt.

Um dieses Problem zu lösen, können Sie die SQL Server 2017-Instanz auf CU3 oder höher aktualisieren.

### <a name="3-failure-to-initialize-a-varbinary-variable-causes-an-error-in-bxlserver"></a>3. Fehler beim Initialisieren einer varbinary-Variablen verursacht einen Fehler in BxlServer

Wenn Sie Python-Code in SQL Server mit `sp_execute_external_script` ausführen und der Code Ausgabevariablen vom Typ „varbinary(max)“, „varchar(max)“ oder ähnliche Typen aufweist, muss die Variable als Teil des Skripts initialisiert oder festgelegt werden. Andernfalls löst die Datenaustauschkomponente BxlServer einen Fehler aus und funktioniert nicht mehr.

Diese Einschränkung wird in einem zukünftigen Service Release korrigiert. Um dieses Problem zu umgehen, stellen Sie sicher, dass die Variable innerhalb des Python-Skripts initialisiert wird. Jeder gültige Wert kann verwendet werden, wie in den folgenden Beispielen:

```sql
declare @b varbinary(max);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N'b = 0x0'
  , @params = N'@b varbinary(max) OUTPUT'
  , @b = @b OUTPUT;
go
```

```sql
declare @b varchar(30);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N' b = ""  '
  , @params = N'@b varchar(30) OUTPUT'
  , @b = @b OUTPUT;
go
```

### <a name="4-telemetry-warning-on-successful-execution-of-python-code"></a>4. Telemetriedatenwarnung bei erfolgreicher Ausführung von Python-Code

Ab SQL Server 2017 CU2 wird möglicherweise die folgende Meldung angezeigt, auch wenn Python-Code anderweitig erfolgreich ausgeführt wird:

> *STDERR-Meldung(en) aus dem externen Skript:* 
>  *~PYTHON_SERVICES\lib\site-packages\revoscalepy\utils\RxTelemetryLogger*
> *SyntaxWarning: telemetry_state wird vor der globalen Deklaration verwendet*

Dieses Problem wurde im kumulativen Update 3 (CU3) von SQL Server 2017 behoben. 

### <a name="5-numeric-decimal-and-money-data-types-not-supported"></a>5. Numerische, dezimale und money-Datentypen werden nicht unterstützt

Seit dem kumulativen Update 12 (CU12) von SQL Server 2017 werden numerische, dezimale und money-Datentypen bei Verwendung von Python mit `sp_execute_external_script` in WITH RESULT SETS nicht unterstützt. Möglicherweise werden folgende Meldungen angezeigt:

> *[Code: 39004, SQL-Status: S1000]  Unerwarteter „Python“-Skriptfehler beim Ausführen von „sp_execute_external_script“ mit HRESULT 0x80004004.*
>
> *[Code: 39019, SQL-Status: S1000] Ein externer Skriptfehler ist aufgetreten:*
>
> *SqlSatelliteCall-Fehler: Im Ausgabeschema nicht unterstützter Typ. Unterstützte Typen: „bit“, „smallint“, „int“, „datetime“, „smallmoney“, „real“ und „float“. „char“ und „varchar“ werden teilweise unterstützt.*

Dieses Problem wurde im kumulativen Update 14 (CU14) von SQL Server 2017 behoben.

### <a name="6-bad-interpreter-error-when-installing-python-packages-with-pip-on-linux"></a>6. Fehler „ungültiger Interpreter“ beim Installieren von Python-Paketen mit PIP unter Linux 

Wenn Sie unter SQL Server 2019 versuchen, **PIP** zu verwenden. Beispiel:

```bash
/opt/mssql/mlservices/runtime/python/bin/pip -h
```

Sie erhalten dann diese Fehlermeldung:

> *bash: /opt/mssql/mlservices/runtime/python/bin/pip: /opt/microsoft/mlserver/9.4.7/bin/python/python: ungültiger Interpreter: No such file or directory* (chroot: Fehler beim Ausführen des Befehls „/bin/bash“: Datei oder Verzeichnis nicht vorhanden),

**Problemumgehung**

Installieren Sie **PIP** von der [Python Package Authority (PyPA)](https://www.pypa.io):

```bash
wget 'https://bootstrap.pypa.io/get-pip.py' 
/opt/mssql/mlservices/bin/python/python ./get-pip.py 
```

**Empfehlung**

Siehe [Installieren von Python-Paketen mit sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md).

**Anwendungsbereich:** SQL Server 2019 unter Linux

### <a name="7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows"></a>7. Python-Pakete können nicht mithilfe von PIP installiert werden, nachdem SQL Server 2019 unter Windows installiert wurde

Nach der Installation von SQL Server 2019 unter Windows tritt beim Versuch, ein Python-Paket über **PIP** von einer DOS-Befehlszeile zu installieren, ein Fehler auf. Beispiel:

```bash
pip install quantfolio
```

Dies gibt folgenden Fehler zurück:

> *PIP ist für Standorte konfiguriert, die TLS/SSL erfordern, aber das SSL-Modul in Python ist nicht verfügbar.*

Dies ist ein spezifisches Problem des Anaconda-Pakets. Es wird in einem zukünftigen Service Release korrigiert.

**Problemumgehung**

Kopieren Sie die folgenden Dateien:

+ `libssl-1_1-x64.dll`
+ `libcrypto-1_1-x64.dll`

aus dem Ordner <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\Library\bin`

in den Ordner <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\DLLs`

Öffnen Sie dann eine neue DOS-Befehlsshell-Eingabeaufforderung.

**Anwendungsbereich:** SQL Server 2019 unter Windows

### <a name="8-error-when-using-sp_execute_external_script-without-libcaboso-on-linux"></a>8. Fehler beim Verwenden von `sp_execute_external_script` ohne `libc++abo.so` unter Linux

Auf einem sauberen Linux-Computer, auf dem `libc++abi.so` nicht installiert ist, tritt beim Ausführen einer `sp_execute_external_script`-Abfrage (SPEES) ein Fehler mit der Meldung „Datei oder Verzeichnis nicht vorhanden“ auf.

Beispiel:

```text
EXEC sp_execute_external_script
    @language = N'Python'
    , @script = N'
OutputDataSet = InputDataSet'
    , @input_data_1 = N'select 1'
    , @input_data_1_name = N'InputDataSet'
    , @output_data_1_name = N'OutputDataSet'
    WITH RESULT SETS (([output] int not null));
Msg 39012, Level 16, State 14, Line 0
Unable to communicate with the runtime for 'Python' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Python' runtime.
STDERR message(s) from external script:

Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.

SqlSatelliteCall error: Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.
STDOUT message(s) from external script:
SqlSatelliteCall function failed. Please see the console output for more information.
Traceback (most recent call last):
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/computecontext/RxInSqlServer.py", line 605, in rx_sql_satellite_call
    rx_native_call("SqlSatelliteCall", params)
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/RxSerializable.py", line 375, in rx_native_call
    ret = px_call(functionname, params)
RuntimeError: revoscalepy function failed.
Total execution time: 00:01:00.387
```

**Problemumgehung**

Führen Sie den folgenden Befehl aus:

```bash
sudo cp /opt/mssql/lib/libc++abi.so.1 /opt/mssql-extensibility/lib/
```

**Anwendungsbereich:** SQL Server 2019 unter Linux

### <a name="9-cannot-install-tensorflow-package-using-sqlmlutils"></a>9. Installation des **tensorflow**-Pakets mit **sqlmlutils** ist nicht möglich

Das [sqlmlutils-Paket](../package-management/install-additional-python-packages-on-sql-server.md) wird zum Installieren von Python-Paketen in SQL Server 2019 verwendet. Sie müssen das [Microsoft Visual C++ 2015–2019 Redistributable (x64)](https://visualstudio.microsoft.com/downloads/) herunterladen, installieren und aktualisieren. Allerdings kann das Paket **tensorflow** nicht mit sqlmlutils installiert werden. Das tensorflow-Paket hängt von einer numpy-Version ab, die neuer als die in SQL Server installierte Version ist. Bei numpy handelt es sich jedoch um ein vorinstalliertes Systempaket, das sqlmlutils nicht aktualisieren kann, wenn versucht wird, tensorflow zu installieren.

**Problemumgehung**

Führen Sie den folgenden Befehl mithilfe einer Eingabeaufforderung im Administratormodus aus, und ersetzen Sie „MSSQLSERVER“ dabei durch den Namen Ihrer SQL-Instanz:

   ```cmd
   "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES\python.exe" -m pip install --upgrade tensorflow
   ```

   Wenn ein „TLS/SSL-Fehler“ auftritt, finden Sie weitere Informationen unter [7. Python-Pakete können nicht mithilfe von PIP installiert werden](#7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows) weiter oben in diesem Artikel.

**Anwendungsbereich:** SQL Server 2019 unter Windows

## <a name="revolution-r-enterprise-and-microsoft-r-open"></a>Revolution R Enterprise und Microsoft R Open

In diesem Abschnitt werden Probleme aufgeführt, die speziell von Revolution Analytics bereitgestellte Konnektivitäts-, Entwicklungs- und Leistungstools für R betreffen. Diese Tools wurden in früheren Vorabversionen von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] bereitgestellt.

Generell sollten Sie diese Vorabversionen deinstallieren und die neueste Version von SQL Server oder Microsoft R Server installieren.

### <a name="1-revolution-r-enterprise-is-not-supported"></a>1. Revolution R Enterprise wird nicht unterstützt

Das parallele Installieren von Revolution R Enterprise mit einer beliebigen Version von [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] wird nicht unterstützt.

Wenn Sie eine Lizenz für Revolution R Enterprise besitzen, müssen Sie diese auf einem Computer verwenden, der sowohl von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz als auch allen Arbeitsstationen getrennt ist, über die Sie eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herstellen möchten.

Einige Vorabversionen von [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] enthalten eine von Revolution Analytics erstellte R-Entwicklungsumgebung für Windows. Dieses Tool wird nicht mehr zur Verfügung gestellt und nicht unterstützt.

Zur Kompatibilität mit [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] sollten Sie stattdessen Microsoft R Client installieren. [R Tools für Visual Studio](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019) und [Visual Studio Code](https://code.visualstudio.com/) unterstützen auch Microsoft R-Lösungen.

### <a name="2-compatibility-issues-with-sqlite-odbc-driver-and-revoscaler"></a>2. Kompatibilitätsprobleme mit SQLite-ODBC-Treiber und RevoScaleR

Revision 0.92 des SQLite-ODBC-Treibers ist mit RevoScaleR nicht kompatibel. Die Revisionen von 0.88-0.91 sowie 0.93 und höher sind bekanntermaßen kompatibel.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung bei Machine Learning in SQL Server](machine-learning-troubleshooting-overview.md)