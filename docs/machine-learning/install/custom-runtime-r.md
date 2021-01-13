---
title: Installieren einer benutzerdefinierten R-Laufzeit
description: Erfahren Sie, wie Sie eine benutzerdefinierte R-Laufzeit für SQL Server installieren.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 9029dd87712ee7128504a9b495b699c9c58e000d
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804347"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Installieren einer benutzerdefinierten R-Laufzeit für SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird beschrieben, wie Sie eine benutzerdefinierte Laufzeit zum Ausführen von R-Skripts mit SQL Server installieren. Die benutzerdefinierte Runtime verwendet Technologie zur Sprachenerweiterung, die auf einem Erweiterbarkeitsframework für die Ausführung von externem Code aufbaut. Die benutzerdefinierte Laufzeit für R kann in folgenden Szenarien verwendet werden:

+ In einer Installation von SQL Server mit dem Erweiterbarkeitsframework.

+ In einer Installation von Machine Learning Services mit SQL Server 2019. Die Spracherweiterung kann mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) verwendet werden, nachdem einige zusätzliche Konfigurationsschritte abgeschlossen wurden.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie eine benutzerdefinierte Laufzeit für R unter Windows installieren. Informationen zur Installation unter Linux finden Sie unter [Installieren einer benutzerdefinierten R-Laufzeit für SQL Server unter Linux](custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="pre-install-checklist"></a>Prüfliste vor der Installation

Vor der Installation einer benutzerdefinierten R-Laufzeit müssen Sie Folgendes installieren:

+ [SQL Server 2019 für Windows (kumulatives Update 3 oder höher)](../../database-engine/install-windows/install-sql-server.md).

+ [SQL Server-Spracherweiterungen unter Windows mit dem Erweiterbarkeitsframework](../../language-extensions/install/windows-java.md).

+ [R, Version 3.3 oder höher](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-windows"></a>Hinzufügen der SQL Server-Spracherweiterungen für Windows

> [!NOTE]
> Wenn Sie Machine Learning Services unter SQL Server 2019 installiert haben, ist das Erweiterbarkeitsframework für Spracherweiterungen mit dem Launchpad-Dienst bereits installiert, und Sie können diesen Schritt überspringen.

Spracherweiterungen verwenden das Erweiterbarkeitsframework zum Ausführen von externem Code. Die Codeausführung ist von den Prozessen der Kern-Engine isoliert, aber vollständig in die Ausführung von SQL Server-Abfragen integriert.

1. Starten Sie den Setup-Assistenten für SQL Server 2019.

1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

    ![SQL Server 2019-Installation CU3 oder höher](../install/media/2019setup-installation-page-mlsvcs.png)

1. Wählen Sie diese Optionen auf der Seite **Funktionsauswahl** aus:

    - **Datenbank-Engine-Dienste**

        Sie müssen eine Instanz der Datenbank-Engine installieren, um Spracherweiterungen mit SQL Server verwenden zu können. Sie können entweder eine Standardinstanz oder eine benannte Instanz verwenden.

    - **Machine Learning-Dienste und -Spracherweiterungen**

        Wählen Sie **Machine Learning-Dienste und -Spracherweiterungen** aus. Es ist nicht erforderlich, R auszuwählen.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Setup für die SQL Server 2019-Spracherweiterungen":::

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.

    + -Datenbank-Engine-Dienste
    + Machine Learning-Dienste und -Spracherweiterungen

1. Wenn Sie nach Abschluss des Setups dazu aufgefordert werden, starten Sie jetzt den Computer neu. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="install-r"></a>Installieren von R

> [!NOTE]
>Für SQL Machine Learning Services ist R bereits im Ordner **R_SERVICES** der SQL Server-Instanz installiert. Beispiel: C:\Programme\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES. Wenn Sie diesen Pfad weiterhin als R_HOME verwenden möchten, fahren Sie mit dem nächsten Schritt und der Installation von Rcpp fort. Wenn Sie andernfalls eine andere Laufzeit von R verwenden möchten, fahren Sie hier mit der Installation fort.

Installieren Sie [R (3.3 oder höher)](https://cran.r-project.org/bin/windows/base/), und notieren Sie sich den Pfad, in dem die Installation erfolgt ist. Dieser Pfad ist Ihr **R_HOME**. Wie hier gezeigt, ist R_HOME beispielsweise „C:\Programme\R\R-4.0.2“.

![Installieren von benutzerdefiniertem R](../install/media/custom-r-installation.png)

> [!NOTE]
>In den folgenden Anweisungen ist %R_HOME% der Pfad zu Ihrer R-Installation und sollte durch diesen Wert ersetzt werden.

## <a name="install-rcpp-package"></a>Installieren des Rcpp-Pakets

+ Suchen Sie die ausführbare R-Datei in „%R_HOME%\bin“. Standardmäßig befindet sie sich in `C:\Program Files\R\R-4.0.2\bin`.

+ Starten Sie R an einer Eingabeaufforderung mit *erhöhten Rechten*:

```CMD
%R_HOME%\bin\R.exe
```

Führen Sie in dieser R-Eingabeaufforderung mit *erhöhten Rechten* (%R_HOME%\bin\R.exe) das folgende Skript aus, um das Rcpp-Paket im Ordner „%R_HOME%\library“ zu installieren.

```R
install.packages("Rcpp", lib="%R_HOME%/library");
```

## <a name="update-the-system-environment-variables"></a>Aktualisieren der Umgebungsvariablen des Systems

1. Fügen Sie **R_HOME** als Systemumgebungsvariable hinzu, oder ändern Sie diese.
    + Geben Sie im Windows-Suchfeld „environment“ ein, und wählen Sie **Systemumgebungsvariablen bearbeiten** aus.
    + Klicken Sie auf der Registerkarte **Erweitert** auf **Umgebungsvariablen**.

    + Wählen Sie unter **Systemvariablen** die Option **Neu** aus, um „R_HOME“ zu erstellen.
    Wählen Sie zum Ändern **Bearbeiten** aus, um die Systemvariable zu ändern. Ändern Sie ihren Wert so, dass er auf den benutzerdefinierten R-Installationspfad verweist.

    ![Erstellen Sie die R_HOME-Systemumgebungsvariable.](../install/media/sys-env-r-home.png)

2. Aktualisieren Sie die **PATH**-Umgebungsvariable.
    Fügen Sie den Pfad zu **R.dll** an die **PATH**-Systemumgebungsvariable an. Wählen Sie **PATH** und dann **Bearbeiten** aus, und fügen Sie `%R_HOME%\bin\x64` der Liste der Pfade hinzu.

    ![Fügen Sie den Pfad an die PATH-Systemumgebungsvariable an.](../install/media/sys-env-path-r.png)

3. Wählen Sie **OK** aus, um die restlichen Fenster zu schließen.

    Zum Festlegen dieser Umgebungsvariablen über eine Eingabeaufforderung mit *erhöhten Rechten* können Sie alternativ die folgenden Befehle ausführen. Stellen Sie sicher, dass Sie den benutzerdefinierten R-Installationspfad verwenden.

```CMD
setx /m R_HOME "path\to\installation\of\R"
setx /m PATH "path\to\installation\of\R\bin\x64;%PATH%"
```

## <a name="grant-access-to-the-custom-r-installation-folder"></a>Gewähren des Zugriffs auf den benutzerdefinierten R-Installationsordner

> [!NOTE]
>Wenn Sie R am Standardspeicherort **C:\Programme\R\R-Version** installiert haben, können Sie diesen Schritt überspringen.

Führen Sie die **icacls**-Befehle in einer neuen Eingabeaufforderung mit *erhöhten Rechten* aus, um **Benutzername des SQL Server-Launchpad-Diensts** und SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**) LESE- und AUSFÜHRUNGSZUGRIFF zu gewähren. Der Benutzername des Launchpad-Diensts weist die Form `NT Service\MSSQLLAUNCHPAD$INSTANCENAME` auf, wobei `INSTANCENAME` der Instanzname Ihrer SQL Server-Instanz ist.

Die Befehle gewähren rekursiv Zugriff auf alle Dateien und Ordner unter dem angegebenen Verzeichnispfad.

Fügen Sie den Instanznamen an `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`) an. In diesem Beispiel ist `INSTANCENAME` die Standardinstanz `MSSQLSERVER`.

1. Erteilen von Berechtigungen für **Benutzername des SQL Server-Launchpad Diensts**

    ```cmd
    icacls "%R_HOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T
    ```
2. Erteilen von Berechtigung für **SID S-1-15-2-1**

    ```cmd
    icacls "%R_HOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.


```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

Klicken Sie alternativ mit der rechten Maustaste auf den SQL Server-Launchpad-Dienst in der App **Dienste** des Systems, und wählen Sie den **Neu starten** aus. Verwenden Sie alternativ den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md) zum erneuten Starten des Diensts.

## <a name="download-r-language-extension"></a>Herunterladen der R-Spracherweiterung

Laden Sie die [ZIP-Datei herunter, die die R-Sprachenerweiterung für Windows](https://github.com/microsoft/sql-server-language-extensions/releases) enthält. Es wird empfohlen, die Releaseversion in der Produktionsumgebung zu verwenden. Verwenden Sie die Debugversion in der Entwicklungs- oder Testumgebung, da sie ausführliche Protokollierungsinformationen zur Untersuchung eventueller Fehler liefert.

## <a name="register-external-language"></a>Registrieren einer externen Sprache

Registrieren Sie diese R-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) für jede Datenbank, in der Sie sie verwenden möchten. Verwenden Sie [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio), um eine Verbindung mit SQL Server herzustellen und den folgenden T-SQL-Befehl auszuführen.
Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei („R-lang-extension.zip“) anzugeben.

> [!NOTE]
>**R** ist ein reserviertes Wort. Verwenden Sie einen anderen Namen für die externe Sprache, z. B. „myR“.

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

Sie könne SQL Server unter Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) und Ubuntu installieren. Weitere Informationen finden Sie [im Abschnitt „Unterstützte Plattformen“ im Leitfaden für die Installation von SQL Server unter Linux](../../linux/sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> In diesem Artikel wird beschrieben, wie eine benutzerdefinierte Laufzeit für R unter Linux installiert wird. Informationen zur Installation unter Windows finden Sie unter [Installieren einer benutzerdefinierten R-Laufzeit für SQL Server unter Windows](custom-runtime-r.md?view=sql-server-ver15&preserve-view=true).

## <a name="pre-install-checklist"></a>Prüfliste vor der Installation

Vor der Installation einer benutzerdefinierten R-Laufzeit müssen Sie Folgendes installieren:

+ [SQL Server 2019 für Linux (kumulatives Update 3 oder höher)](../../linux/sql-server-linux-setup.md).
Bevor Sie SQL Server unter Linux installieren, müssen Sie ein Microsoft-Repository konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von Repositorys](../../linux/sql-server-linux-change-repo.md).

+ [SQL Server-Spracherweiterungen unter Linux mit dem Erweiterbarkeitsframework](../../linux/sql-server-linux-setup-language-extensions-java.md).

+ [R, Version 3.3 oder höher](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-linux"></a>Hinzufügen der SQL Server-Spracherweiterungen für Linux

> [!NOTE]
> Wenn Sie Machine Learning Services unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Spracherweiterungen verwenden das Erweiterbarkeitsframework zum Ausführen von externem Code. Die Codeausführung ist von den Prozessen der Kern-Engine isoliert, aber vollständig in die Ausführung von SQL Server-Abfragen integriert.

Verwenden Sie die folgenden Befehle, um Spracherweiterungen abhängig von Ihrer Linux-Version zu installieren.

### <a name="ubuntu"></a>Ubuntu
> [!Tip]
> Führen Sie nach Möglichkeit `sudo apt-get update` aus, um Pakete auf dem System vor der Installation zu aktualisieren. Ubuntu verfügt möglicherweise nicht über die Option „https apt transport“. Verwenden Sie `sudo apt-get install apt-transport-https`, um diese zu installieren.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### <a name="red-hat"></a>Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### <a name="suse"></a>Suse
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Installieren von R

>[!NOTE]
>Für SQL Machine Learning Services ist R bereits in `/opt/microsoft/ropen/3.5.2/lib64/R` installiert. Wenn Sie diesen Pfad weiterhin als R_HOME verwenden möchten, fahren Sie mit dem nächsten Schritt und der **Installation von Rcpp** fort. 

Wenn Sie eine andere Laufzeit von R verwenden möchten, müssen Sie zunächst `microsoft-r-open-mro` entfernen, bevor Sie mit der Installation einer neuen Version fortfahren können. Beispiel für Ubuntu:

```bash
sudo apt remove microsoft-r-open-mro-3.5.2
```

Befolgen Sie die [Anweisungen](https://cran.r-project.org/bin/linux/), um die Installation von R (3.3 oder höher) für die jeweilige Linux-Plattform abzuschließen. Standardmäßig wird R in **/usr/lib/R** installiert. Dieser Pfad ist Ihr **R_HOME**. Wenn Sie R an einem anderen Speicherort installieren, notieren Sie sich diesen Pfad als R_HOME.

Beispielanweisungen für Ubuntu. Ändern Sie die unten angegebene Repository-URL für Ihre Version von R.

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
#
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
sudo apt-get update

# Install R runtime.
#
sudo apt-get -y install r-base-core
```

## <a name="install-rcpp-package"></a>Installieren des Rcpp-Pakets

In den folgenden Anweisungen ist ${R_HOME} der Pfad zu Ihrer R-Installation. 

+ Suchen Sie die R-Binärdatei in ${R_HOME}/bin. Standardmäßig befindet sie sich in **/usr/lib/R/bin**.

+ Starten von R

  ```bash
  sudo ${R_HOME}/bin/R
  ```

+ Führen Sie in dieser R-Eingabeaufforderung mit *erhöhten Rechten* (${R_HOME}/bin/R) das folgende Skript aus, um das **Rcpp**-Paket im Ordner „${R_HOME}/library“ zu installieren.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="using-a-custom-installation-of-r"></a>Verwenden einer benutzerdefinierten Installation von R

> [!NOTE]
>Wenn Sie R am Standardspeicherort **/usr/lib/R** installiert haben, können Sie diesen Abschnitt überspringen.

### <a name="update-the-environment-variables"></a>Aktualisieren der Umgebungsvariablen

1. Bearbeiten Sie den Dienst mssql-launchpadd, um der Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` die R_HOME-Umgebungsvariable hinzuzufügen.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

    + Fügen Sie den folgenden Text in die Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` ein, die geöffnet wird. Legen Sie den Wert R_HOME auf den benutzerdefinierten R-Installationspfad fest.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

    + Speichern und schließen Sie die Datei.

2. Stellen Sie sicher, dass **libR.so** geladen werden kann.

    + Erstellen Sie eine Datei „custom-r.conf“ in **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

    + Fügen Sie in der Datei, die geöffnet wird, den Pfad zu **libR.so** aus der benutzerdefinierten R-Installation hinzu.

    ```vi editor
    /path/to/installation/of/R/lib
    ```

    + Speichern und schließen Sie die neue Datei.

    + Führen Sie `ldconfig` aus, und vergewissern Sie sich, dass **libR.so** geladen werden kann, indem Sie den folgenden Befehl ausführen und überprüfen, ob alle abhängigen Bibliotheken gefunden werden.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Gewähren des Zugriffs auf den benutzerdefinierten R-Installationsordner

Legen Sie die `datadirectories`-Option im Erweiterbarkeitsabschnitt der Datei „/var/opt/mssql/mssql.conf“ auf die benutzerdefinierte R-Installation fest.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Neustarten des mssql-launchpadd-Diensts

> [!NOTE]
>Wenn Sie R am Standardspeicherort **/usr/lib/R** installiert haben, können Sie diesen Schritt überspringen.

```bash
sudo systemctl restart mssql-launchpadd
```

## <a name="download-r-language-extension"></a>Herunterladen der R-Spracherweiterung

Laden Sie die [ZIP-Datei herunter, die die R-Sprachenerweiterung für Linux](https://github.com/microsoft/sql-server-language-extensions/releases) enthält. Es wird empfohlen, die Releaseversion in der Produktionsumgebung zu verwenden. Verwenden Sie die Debugversion in der Entwicklungs- oder Testumgebung, da sie ausführliche Protokollierungsinformationen zur Untersuchung eventueller Fehler liefert.

## <a name="register-external-language"></a>Registrieren einer externen Sprache

Registrieren Sie diese R-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) für jede Datenbank, in der Sie sie verwenden möchten. Verwenden Sie [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio), um eine Verbindung mit SQL Server herzustellen und den folgenden T-SQL-Befehl auszuführen. 
Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei („r-lang-extension.zip“) anzugeben.


> [!NOTE]
>**R** ist ein reserviertes Wort. Verwenden Sie einen anderen Namen für die externe Sprache, z. B. „myR“.

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>Aktivieren der externen Skriptausführung in SQL Server

Ein externes Skript in R kann über die gespeicherte Prozedur [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausgeführt werden, die für SQL Server ausgeführt wird. 

Um externe Skripts zu aktivieren, führen Sie die folgenden SQL-Befehle mit [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) aus, während eine Verbindung mit SQL Server besteht.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="verify-language-extension-installation"></a>Überprüfen der Installation der Spracherweiterung

Mit diesem SQL-Skript wird die erfolgreiche Installation der benutzerdefinierten R-Spracherweiterung überprüft. Die Ausgabe dieses Skripts sollte R_HOME, den Pfad zu R und die Version der benutzerdefinierten R-Laufzeit anzeigen. Es wird bestätigt, dass das Skript Ihre benutzerdefinierte Laufzeit verwendet.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>Überprüfen von Parametern und Datasets mit unterschiedlichen Datentypen

Dieses Skript testet verschiedene Datentypen für Eingabe-/Ausgabeparameter und Datasets.

```sql
DECLARE @sumVal INT = 12;
DECLARE @charVal VARCHAR(30) = N'Hello';
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(sumVal);
print(charVal);
sumVal <- sumVal + 300;
OutputDataSet <- InputDataSet;'
    ,@input_data_1 = N'select 1, cast(1.4 as real), ''Hi'', cast(''1'' as bit)'
    ,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
    ,@sumVal = @sumVal OUTPUT
    ,@charVal =  @charVal
WITH RESULT SETS ((intCol INT, doubleCol REAL, charCol CHAR(2), logicalCol BIT));
PRINT @sumVal;
```

## <a name="see-also"></a>Weitere Informationen

+ [Erweiterbarkeitsframework in SQL Server](../concepts/extensibility-framework.md)
+ [Übersicht über Spracherweiterungen](../../language-extensions/language-extensions-overview.md)