---
title: Installieren einer benutzerdefinierten Python-Laufzeit
description: Erfahren Sie, wie Sie eine benutzerdefinierte Python-Runtime für SQL Server mithilfe der Spracherweiterungen installieren. Die benutzerdefinierte Python-Runtime kann für das maschinelle Lernen verwendet werden.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 91aace4333b4496338b782344e64cdfea2b886bd
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804311"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Installieren einer benutzerdefinierten Python-Laufzeit für SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird beschrieben, wie Sie eine benutzerdefinierte Python-Runtime zum Ausführen von externen Python-Skripts mit SQL Server installieren. Die benutzerdefinierte Runtime verwendet die [SQL Server-Spracherweiterungen](../../language-extensions/language-extensions-overview.md) und kann für die Ausführung von Skripts für maschinelles Lernen verwendet werden.

Die benutzerdefinierte Python-Runtime ermöglicht es Ihnen, Ihre eigene Version der Python-Runtime anstelle der Standardversion der Runtime mit SQL Server zu verwenden, die mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) installiert wird.

::: zone pivot="python-custom-runtime-windows"

## <a name="prerequisites"></a>Voraussetzungen

Vor der Installation einer benutzerdefinierten Python-Laufzeit müssen Sie Folgendes installieren:

+ Installieren Sie das [kumulative Update (CU) 3 oder höher](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) für SQL Server 2019.

+ Installieren Sie [Python 3.7](https://www.python.org/downloads/) auf dem Server.

    Die für die benutzerdefinierte Python-Runtime verwendete Python-Spracherweiterung unterstützt derzeit nur Python 3.7. Wenn Sie eine andere Version von Python verwenden möchten, folgen Sie den Anweisungen im [GitHub-Repository für die Python-Spracherweiterung](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), um die Erweiterung zu ändern und neu zu erstellen.

    > [!IMPORTANT]
    > Aktivieren Sie während der Installation von Python die Option **Add Python 3.7 to PATH** (Python 3.7 zu PATH hinzufügen).

## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, sind die Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Schritte aus, um [SQL Server-Spracherweiterungen](../../language-extensions/language-extensions-overview.md) zu installieren, die für die benutzerdefinierte Python-Runtime verwendet werden.

1. Starten Sie den Setup-Assistenten für SQL Server 2019.
  
1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

1. Wählen Sie diese Optionen auf der Seite **Funktionsauswahl** aus:
  
    + **Datenbank-Engine-Dienste**
  
        Sie müssen eine Instanz der Datenbank-Engine installieren, um Spracherweiterungen mit SQL Server verwenden zu können. Sie können entweder eine neue oder eine vorhandene Instanz verwenden.
  
    + **Machine Learning-Dienste und -Spracherweiterungen**

        Wählen Sie **Machine Learning-Dienste und -Spracherweiterungen** aus. Wählen Sie nicht Python aus, da Sie die benutzerdefinierte Python-Runtime später installieren werden.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Setup für die SQL Server 2019-Spracherweiterungen":::

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.
  
    + -Datenbank-Engine-Dienste
    + Machine Learning-Dienste und -Spracherweiterungen

1. Starten Sie nach Abschluss des Setups den Computer neu, wenn Sie dazu aufgefordert werden.

> [!IMPORTANT]
> Wenn Sie eine neue Instanz von SQL Server 2019 mit Spracherweiterungen installieren, installieren Sie das [kumulative Update (CU) 3 oder höher](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md), bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="install-pandas"></a>Installieren von Pandas

Installieren Sie das [Pandas](https://pandas.pydata.org/)-Paket für Python über eine Eingabeaufforderung mit *erhöhten Rechten*:

```bash
python.exe -m pip install pandas
```

## <a name="add-environment-variable"></a>Hinzufügen einer Umgebungsvariablen

Fügen Sie die Systemumgebungsvariable **PYTHONHOME** hinzu, oder ändern Sie diese.

1. Geben Sie im Windows-Suchfeld *environment* ein, und wählen Sie **Systemumgebungsvariablen bearbeiten** aus.
1. Klicken Sie auf der Registerkarte **Erweitert** auf **Umgebungsvariablen**.
1. Wählen Sie unter **Systemvariablen** die Option **Neu** aus, um **PYTHONHOME** zu erstellen und auf Ihren Python 3.7-Installationsspeicherort zu verweisen. Wenn PYTHONHOME bereits vorhanden ist, wählen Sie **Bearbeiten** aus, um auf den Installationsspeicherort von Python 3.7 zu verweisen.
1. Klicken Sie zum Schließen sämtlicher Fenster auf **OK**.

    :::image type="content" source="media/pythonhome-env-variable.png" alt-text="PYTHONHOME-Umgebungsvariable":::

## <a name="grant-access-to-python-folder"></a>Gewähren des Zugriffs auf den Python-Ordner

Führen Sie die folgenden **icacls**-Befehle in einer neuen Eingabeaufforderung mit *erhöhten Rechten* aus, um **PYTHONHOME** für den **SQL Server-Launchpad-Dienst** und SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**) **LESE- und AUSFÜHRUNGSZUGRIFF** zu gewähren.

1. Erteilen Sie Berechtigungen für **Benutzername des SQL Server-Launchpad Diensts**.

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Bei einer benannter Instanz ist der Befehl `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` für eine Instanz namens **SQL01**.

2. Erteilen Sie die Berechtigung für **SID S-1-15-2-1**.

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Der vorangehende Befehl erteilt Berechtigungen für die Computer-SID **S-1-15-2-1**, was **ALLEN ANWENDUNGSPAKETEN** auf einer englischen Version von Windows entspricht. Alternativ können Sie `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in einer englischen Version von Windows verwenden.

## <a name="restart-sql-server-launchpad"></a>Neustart von SQL Server-Launchpad

Führen Sie die folgenden Schritte aus, um den SQL Server-Launchpad-Dienst neu zu starten.

1. Öffnen Sie den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md).

1. Klicken Sie unter **SQL Server-Dienste** mit der rechten Maustaste auf **SQL Server-Launchpad (MSSQLSERVER)** , und wählen Sie **Neu starten** aus. Wenn Sie eine benannte Instanz verwenden, wird der Instanzname anstelle von **(MSSQLSERVER)** angezeigt.

## <a name="register-language-extension"></a>Registrieren der Spracherweiterung

Führen Sie die folgenden Schritte aus, um die Python-Spracherweiterung herunterzuladen und zu registrieren, die für die benutzerdefinierte Python-Runtime verwendet wird.

1. Laden Sie die Datei **python-lang-extension-windows.zip** aus dem [GitHub-Repository für die SQL Server-Spracherweiterungen](https://github.com/microsoft/sql-server-language-extensions/releases) herunter.

    Alternativ können Sie auch die Debugversion (**python-lang-extension-windows-debug.zip**) in einer Entwicklungs- oder Testumgebung verwenden. Die Debugversion bietet ausführliche Protokollierungsinformationen, um eventuelle Fehler zu untersuchen, und wird nicht für Produktionsumgebungen empfohlen.

1. Verwenden Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen, und führen Sie den folgenden T-SQL-Befehl aus, um die Python-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) zu registrieren. 

    Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei (**python-lang-extension-windows.zip**) anzugeben.

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-windows.zip', FILE_NAME = 'pythonextension.dll');
    GO
    ```

    Führen Sie die Anweisung für jede Datenbank aus, in der Sie die Python-Spracherweiterung verwenden möchten.

    > [!NOTE]
    > **Python** ist ein reserviertes Wort und kann nicht als Name für einen neuen externen Sprachnamen verwendet werden. Verwenden Sie stattdessen einen anderen Namen. Die obige Anweisung verwendet z. B. **myPython**.

::: zone-end

::: zone pivot="python-custom-runtime-linux"

## <a name="prerequisites"></a>Voraussetzungen

Vor der Installation einer benutzerdefinierten Python-Laufzeit müssen Sie Folgendes installieren:

+ Installieren Sie SQL Server 2019 für Linux. Sie könne SQL Server unter Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) und Ubuntu installieren. Weitere Informationen finden Sie im [Leitfaden für die Installation von SQL Server unter Linux](../../linux/sql-server-linux-setup.md).

+ Führen Sie ein Upgrade auf das kumulative Update (CU) 3 oder höher für SQL Server 2019 durch. Führen Sie die folgenden Schritte aus:
    1. Konfigurieren Sie die Repositorys für kumulative Updates. Weitere Informationen finden Sie unter [Konfigurieren von Repositorys zum Installieren und Upgraden von SQL Server für Linux](../../linux/sql-server-linux-change-repo.md).

    1. Aktualisieren Sie das Paket **mssql-server** auf das neueste kumulative Update. Weitere Informationen finden Sie [im Abschnitt „Update oder Upgrade von SQL Server“ im Leitfaden für die Installation von SQL Server unter Linux](../../linux/sql-server-linux-setup.md#upgrade).

+ Installieren Sie [Python 3.7](https://www.python.org/downloads/) auf dem Server.

    Die für die benutzerdefinierte Python-Runtime verwendete Python-Spracherweiterung unterstützt derzeit nur Python 3.7. Wenn Sie eine andere Version von Python verwenden möchten, folgen Sie den Anweisungen im [GitHub-Repository für die Python-Spracherweiterung](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), um die Erweiterung zu ändern und neu zu erstellen.

## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Befehle aus, um [SQL Server-Spracherweiterungen](../../language-extensions/language-extensions-overview.md) unter Linux zu installieren, die für die benutzerdefinierte Python-Runtime verwendet werden.

#### <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

1. Führen Sie diesen Befehl wenn möglich aus, um die Pakete auf dem System vor der Installation zu aktualisieren.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu verfügt möglicherweise nicht über die Option „https apt transport“. Führen Sie diesen Befehl aus, um sie zu installieren.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Installieren Sie **mssql-server-extensibility** mit diesem Befehl.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

#### <a name="red-hat-enterprise-linux-rhel"></a>[Red Hat Enterprise Linux (RHEL)](#tab/rhel)

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

#### <a name="suse-linux-enterprise-server-sles"></a>[SUSE Linux Enterprise Server (SLES)](#tab/sles)

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

---

## <a name="install-python-37-and-pandas"></a>Installieren von Python 3.7 and Pandas

Installieren Sie Python 3.7, die libpython3.7-Bibliothek und das Pandas-Paket. 

Im Folgenden finden Sie Beispielbefehle für Ubuntu:

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

## <a name="custom-installation-of-python"></a>Benutzerdefinierte Installation von Python

> [!NOTE]
> Wenn Sie Python 3.7 am Standardspeicherort von `/usr/lib/python3.7` installiert haben, können Sie diesen Abschnitt überspringen und mit dem Abschnitt [Registrieren der Spracherweiterung](#register-language-extension-linux) fortfahren.

Wenn Sie Ihre eigene Version von Python 3.7 erstellt haben, verwenden Sie die folgenden Befehle, um SQL Server über Ihre benutzerdefinierte Installation zu informieren.

### <a name="add-environment-variable"></a>Hinzufügen einer Umgebungsvariablen

Bearbeiten Sie zunächst den Dienst **mssql-launchpadd**, um der Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` die **PYTHONHOME**-Umgebungsvariable hinzuzufügen.

1. Öffnen Sie die Datei mit systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Fügen Sie den folgenden Text in die Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` ein, die geöffnet wird. Legen Sie den Wert von **PYTHONHOME** auf den benutzerdefinierten Python-Installationspfad fest.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Speichern Sie die Datei, und schließen Sie den Editor.

Stellen Sie als nächstes sicher, dass `libpython3.7m.so.1.0` geladen werden kann.

1. Erstellen Sie eine benutzerdefinierte Datei „python.conf“ in `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. Fügen Sie in der Datei, die geöffnet wird, den Pfad zu **libpython3.7m.so.1.0** aus der benutzerdefinierten Python-Installation hinzu.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Speichern Sie die neue Datei, und schließen Sie den Editor.

1. Führen Sie `ldconfig` aus, und vergewissern Sie sich, dass `libpython3.7m.so.1.0` geladen werden kann, indem Sie den folgenden Befehl ausführen und überprüfen, ob die abhängigen Bibliotheken gefunden werden.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Gewähren des Zugriffs auf den Python-Ordner

Legen Sie die `datadirectories`-Option im Erweiterbarkeitsabschnitt der Datei „/var/opt/mssql/mssql.conf“ auf die benutzerdefinierte Python-Installation fest.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Neustart von mssql-launchpadd

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Registrieren der Spracherweiterung

Führen Sie die folgenden Schritte aus, um die Python-Spracherweiterung herunterzuladen und zu registrieren, die für die benutzerdefinierte Python-Runtime verwendet wird.

1. Laden Sie die Datei **python-lang-extension-linux.zip** aus dem [GitHub-Repository für die SQL Server-Spracherweiterungen](https://github.com/microsoft/sql-server-language-extensions/releases) herunter.

    Alternativ können Sie auch die Debugversion (**python-lang-extension-linux-debug.zip**) in einer Entwicklungs- oder Testumgebung verwenden. Die Debugversion bietet ausführliche Protokollierungsinformationen, um eventuelle Fehler zu untersuchen, und wird nicht für Produktionsumgebungen empfohlen.

1. Verwenden Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen, und führen Sie den folgenden T-SQL-Befehl aus, um die Python-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) zu registrieren. 

    Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei (**python-lang-extension-linux.zip**) anzugeben.

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux.zip', FILE_NAME = 'libPythonExtension.so.1.0');
    GO
    ```

    Führen Sie die Anweisung für jede Datenbank aus, in der Sie die Python-Spracherweiterung verwenden möchten.

    > [!NOTE]
    > **Python** ist ein reserviertes Wort und kann nicht als Name für einen neuen externen Sprachnamen verwendet werden. Verwenden Sie stattdessen einen anderen Namen. Die obige Anweisung verwendet z. B. **myPython**.

::: zone-end

## <a name="enable-external-script"></a>Aktivieren des externen Skripts

Sie können ein externes Python-Skript mit der gespeicherten Prozedur [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausführen.

Zum Aktivieren externer Skripts verwenden Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), um die folgende Anweisung auszuführen.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Überprüfen der Installation

Verwenden Sie das folgende SQL-Skript, um die Installation und Funktionalität der benutzerdefinierten Python-Runtime zu überprüfen.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Nächste Schritte

+ [Installieren einer benutzerdefinierten R-Laufzeit für SQL Server](custom-runtime-r.md)
+ [Erweiterbarkeitsframework in SQL Server](../concepts/extensibility-framework.md)
+ [Übersicht über Spracherweiterungen](../../language-extensions/language-extensions-overview.md)
