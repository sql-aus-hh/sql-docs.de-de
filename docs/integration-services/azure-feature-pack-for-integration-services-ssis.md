---
description: Azure Feature Pack für Integration Services (SSIS)
title: Azure Feature Pack für Integration Services (SSIS) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.SSIS.AZURE.F1
- SQL14.SSIS.AZURE.F1
ms.assetid: 31de555f-ae62-4f2f-a6a6-77fea1fa8189
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b22910f0a794925d07358fa73f3bd6d7a7c6e15a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091660"
---
# <a name="azure-feature-pack-for-integration-services-ssis"></a>Azure Feature Pack für Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Das SQL Server Integration Services-Feature Pack (SSIS) für Azure ist eine Erweiterung, die die auf dieser Seite für SSIS aufgelisteten Komponenten für Verbindungen mit Azure-Diensten, für die Übertragung von Daten zwischen Azure und lokalen Datenquellen und für die Verarbeitung der in Azure gespeicherten Daten bereitstellt.

[![Herunterladen des SSIS-Feature Packs für Azure](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/download/details.aspx?id=100430) **Herunterladen**

- Für SQL Server 2019 – [Microsoft SQL Server 2019 Integration Services-Feature Pack für Azure](https://www.microsoft.com/download/details.aspx?id=100430)
- Für SQL Server 2017 – [Microsoft SQL Server 2017 Integration Services-Feature Pack für Azure](https://www.microsoft.com/download/details.aspx?id=54798)
- Für SQL Server 2016 – [Microsoft SQL Server 2016 Integration Services-Feature Pack für Azure](https://www.microsoft.com/download/details.aspx?id=49492)
- Für SQL Server 2014 – [Microsoft SQL Server 2014 Integration Services-Feature Pack für Azure](https://www.microsoft.com/download/details.aspx?id=47366)
- Für SQL Server 2012 – [Microsoft SQL Server 2012 Integration Services-Feature Pack für Azure](https://www.microsoft.com/download/details.aspx?id=47367)

Die Downloadseiten enthalten auch Informationen zu erforderlichen Komponenten. Stellen Sie sicher, dass Sie SQL Server installieren, bevor Sie Azure Feature Pack auf einem Server installieren, andernfalls sind die Komponenten im Feature Pack womöglich nicht verfügbar, wenn Sie Pakete für die SSIS-Katalogdatenbank (SSISDB) auf dem Server bereitstellen.

## <a name="components-in-the-feature-pack"></a>Komponenten im Feature Pack
-   Verbindungs-Manager

    -   [Azure Data Lake Analytics-Verbindungs-Manager](connection-manager/azure-data-lake-analytics-connection-manager.md)

    -   [Azure Data Lake Store Connection Manager (Azure Data Lake Store-Verbindungs-Manager)](../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)
    
    -   [Azure HDInsight-Verbindungs-Manager](../integration-services/connection-manager/azure-hdinsight-connection-manager.md)

    -   [Azure Resource Manager-Verbindungs-Manager](../integration-services/connection-manager/azure-resource-manager-connection-manager.md)
    
    -   [Azure Storage-Verbindungs-Manager](../integration-services/connection-manager/azure-storage-connection-manager.md)

    -   [Verbindungs-Manager für Azure-Abonnements](../integration-services/connection-manager/azure-subscription-connection-manager.md)
    
-   Aufgaben

    -   [Azure Blob-Download-Task](../integration-services/control-flow/azure-blob-download-task.md)

    -   [Azure-Blob-Uploadtask](../integration-services/control-flow/azure-blob-upload-task.md)

    -   [Azure Data Lake Analytics-Task](control-flow/azure-data-lake-analytics-task.md)

    -   [Azure Data Lake Store-Dateisystemtask](../integration-services/control-flow/azure-data-lake-store-file-system-task.md)

    -   [Azure HDInsight Create Cluster-Task](../integration-services/control-flow/azure-hdinsight-create-cluster-task.md)

    -   [Azure HDInsight-Delete Cluster-Task](../integration-services/control-flow/azure-hdinsight-delete-cluster-task.md)
    
    -   [Hive-Aufgabe in Azure HDInsight](../integration-services/control-flow/azure-hdinsight-hive-task.md)

    -   [Azure HDInsight Pig-Task](../integration-services/control-flow/azure-hdinsight-pig-task.md)

    -   [Azure SQL DW-Uploadtask](../integration-services/control-flow/azure-sql-dw-upload-task.md)

    -   [Flexibler Dateitask](../integration-services/control-flow/flexible-file-task.md)

-   Datenflusskomponenten

    -   [Azure-Blob-Quelle](../integration-services/data-flow/azure-blob-source.md)

    -   [Azure-BLOB-Ziel](../integration-services/data-flow/azure-blob-destination.md)
    
    -   [Azure Data Lake Store Source](../integration-services/data-flow/azure-data-lake-store-source.md)
    
    -   [Azure Data Lake Store Destination](../integration-services/data-flow/azure-data-lake-store-destination.md)

    -   [Flexible Dateiquelle](../integration-services/data-flow/flexible-file-source.md)

    -   [Flexibles Dateiziel](../integration-services/data-flow/flexible-file-destination.md)

-   Azure Blob, Azure Data Lake Store und Data Lake Storage Gen2-Datei-Enumerator. Siehe [Foreach Loop Container](../integration-services/control-flow/foreach-loop-container.md) (Foreach-Schleifencontainer).

## <a name="use-tls-12"></a>Verwenden von TLS 1.2

Die vom Azure Feature Pack verwendete TLS-Version folgt den .NET Framework-Einstellungen.
Wenn Sie TLS 1.2 verwenden möchten, fügen Sie einen `REG_DWORD`-Wert mit dem Namen `SchUseStrongCrypto` für „data“ (Daten) unter den folgenden zwei Registrierungsschlüsseln `1` hinzu.

1. `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319`
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319`

## <a name="dependency-on-java"></a>Abhängigkeit von Java

Java ist erforderlich, um ORC-/Parquet-Dateiformate mit Azure Data Lake Store/flexiblen Dateiconnectors zu verwenden.  
Die Architektur (32 oder 64 Bit) des Java-Builds muss mit der zu verwendenden SSIS-Runtime übereinstimmen.
Die folgenden Java-Builds wurden getestet.

- [OpenJDK 8u192 für Zulu](https://www.azul.com/downloads/zulu/zulu-windows/)
- [Oracle Java SE Runtime Environment 8u192](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)

### <a name="set-up-zulus-openjdk"></a>Einrichten von OpenJDK für Zulu

1. Laden Sie das ZIP-Paket für die Installation herunter, und extrahieren Sie es.
2. Führen Sie über die Eingabeaufforderung `sysdm.cpl` aus.
3. Klicken Sie auf der Registerkarte **Erweitert** auf **Umgebungsvariablen**.
4. Klicken Sie im Abschnitt **Systemvariablen** auf **Neu**.
5. Geben Sie `JAVA_HOME` für den **Variablennamen** ein.
6. Klicken Sie auf **Verzeichnis durchsuchen**, navigieren Sie zum extrahierten Ordner, und wählen Sie den Unterordner `jre` aus.
   Wählen Sie anschließend **OK** aus. Daraufhin wird der **Variablenwert** automatisch aufgefüllt.
7. Klicken Sie auf **OK**, um das Dialogfeld **New System Variable** (Neue Systemvariable) zu schließen.
8. Klicken Sie auf **OK**, um das Dialogfeld **Umgebungsvariablen** zu schließen.
9. Klicken Sie auf **OK**, um das Dialogfeld **Systemeigenschaften** zu schließen.

> [!TIP]
> Wenn Sie das Parquet-Format verwenden und die Fehlermeldung "An error occurred when invoking java, message: **java.lang.OutOfMemoryError:Java heap space**" (Beim Aufrufen von Java ist ein Fehler aufgetreten. Meldung: java.lang.OutOfMemoryError:Java heap space) auftritt, können Sie die Umgebungsvariable *`_JAVA_OPTIONS`* hinzufügen, um die minimale/maximale Heapgröße für JVM anzupassen.
>
>![JVM-Heap](media/azure-feature-pack-jvm-heap-size.png)
>
> Beispiel: Legen Sie für die Variable *`_JAVA_OPTIONS`* den Wert *`-Xms256m -Xmx16g`* fest. Das Flag „Xms“ gibt den anfänglichen Speicherzuweisungspool für eine Java Virtual Machine (JVM) an, während Xmx den maximalen Speicherzuweisungspool angibt. Das bedeutet, dass die JVM mit einer Speichergröße von *`Xms`* gestartet wird und eine maximale Speichergröße von *`Xmx`* verwenden kann. Die Standardwerte sind mindestens 64 MB und maximal 1 GB.

### <a name="set-up-zulus-openjdk-on-azure-ssis-integration-runtime"></a>Einrichten des Zulu OpenJDK in Azure-SSIS Integration Runtime

Dies sollte über eine [benutzerdefinierte Setupschnittstelle](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) für Azure-SSIS Integration Runtime erfolgen.
Angenommen, es wird `zulu8.33.0.1-jdk8.0.192-win_x64.zip` verwendet.
Der Blobcontainer könnte wie folgt organisiert werden.

~~~
main.cmd
install_openjdk.ps1
zulu8.33.0.1-jdk8.0.192-win_x64.zip
~~~

Als Einstiegspunkt löst `main.cmd` die Ausführung des PowerShell-Skripts `install_openjdk.ps1` aus, welches wiederum `zulu8.33.0.1-jdk8.0.192-win_x64.zip` extrahiert und `JAVA_HOME` entsprechend festlegt.

**main.cmd**

~~~
powershell.exe -file install_openjdk.ps1
~~~

> [!TIP]
> Wenn Sie das Parquet-Format verwenden und die Fehlermeldung "An error occurred when invoking java, message: **java.lang.OutOfMemoryError:Java heap space**" (Beim Aufrufen von Java ist ein Fehler aufgetreten. Meldung: java.lang.OutOfMemoryError:Java heap space) auftritt, können Sie einen Befehl in *`main.cmd`* hinzufügen, um die minimale/maximale Heapgröße für JVM anzupassen. Beispiel:
> ~~~
> setx /M _JAVA_OPTIONS "-Xms256m -Xmx16g"
> ~~~
> Das Flag „Xms“ gibt den anfänglichen Speicherzuweisungspool für eine Java Virtual Machine (JVM) an, während Xmx den maximalen Speicherzuweisungspool angibt. Das bedeutet, dass die JVM mit einer Speichergröße von *`Xms`* gestartet wird und eine maximale Speichergröße von *`Xmx`* verwenden kann. Die Standardwerte sind mindestens 64 MB und maximal 1 GB.

**install_openjdk.ps1**

~~~
Expand-Archive zulu8.33.0.1-jdk8.0.192-win_x64.zip -DestinationPath C:\
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\zulu8.33.0.1-jdk8.0.192-win_x64\jre", "Machine")
~~~

### <a name="set-up-oracles-java-se-runtime-environment"></a>Einrichten von Oracle Java SE Runtime Environment

1. Laden Sie das EXE-Installationsprogramm herunter, und führen Sie es aus.
2. Führen Sie die Installationsanweisungen aus, um das Setup abzuschließen.

## <a name="scenario-processing-big-data"></a>Szenario: Verarbeitung großer Datenmengen
 Verwenden Sie den Azure Connector zum Ausführen der folgenden Big Data-Verarbeitungsaufgaben:

1.  Verwenden Sie den Azure Blob Upload-Task zum Hochladen von Eingabedaten in den Azure BLOB-Speicher.

2.  Verwenden Sie den Azure HDInsight Create Cluster-Task zum Erstellen eines Azure HDInsight-Clusters. Dieser Schritt ist optional, wenn Sie einen eigenen Cluster verwenden möchten.

3.  Verwenden Sie den Azure HDInsight Hive-Task oder Azure HDInsight Pig-Task zum Aufrufen eines Pig- oder Hive-Auftrags auf dem Azure HDInsight-Cluster.

4.  Verwenden Sie den Azure HDInsight Delete Cluster-Task zum Löschen des HDInsight-Clusters nach der Verwendung, wenn Sie in Schritt 2 einen HDInsight-Bedarfscluster erstellt haben.

5.  Verwenden Sie den Azure HDInsight Blob Download-Task zum Herunterladen der Pig/Hive-Ausgabedaten vom Azure BLOB-Speicher.

![Screenshot: SSIS-Big-Data-Szenario in Azure Connector](../integration-services/media/ssis-azureconnector-bigdatascenario.png)
 
## <a name="scenario-managing-data-in-the-cloud"></a>Szenario: Verwalten von Daten in der Cloud
 Verwenden Sie das Azure BLOB-Ziel in einem SSIS-Paket, um Ausgabedaten in einen Azure BLOB-Speicher zu schreiben, oder verwenden Sie die Azure Blob-Quelle zum Lesen von Daten aus einem Azure BLOB-Speicher.

![Screenshot: Datenfluss von der OLE DB-Quelle zum Azure-Blobziel](../integration-services/media/ssis-azureconnector-cloudarchive-1.png)
 
 ![Screenshot: Datenfluss von der Azure-Blobquelle zum OLE DB-Ziel](../integration-services/media/ssis-azureconnector-cloudarchive-2.png)

 Verwenden Sie den Foreach-Schleifencontainer mit dem Azure Blob-Enumerator, um Daten in mehreren BLOB-Dateien zu verarbeiten.

![Screenshot: Foreach-Schleifencontainer der Ablaufsteuerung](../integration-services/media/ssis-azureconnector-cloudarchive-3.png)

## <a name="release-notes"></a>Versionsinformationen

### <a name="version-1190"></a>Version 1.19.0

#### <a name="improvements"></a>Verbesserungen

1. Azure Storage-Verbindungs-Manager wurde Unterstützung für SAS-Authentifizierung (Shared Access Signature) hinzugefügt.

### <a name="version-1180"></a>Version 1.18.0

#### <a name="improvements"></a>Verbesserungen

1. Für flexible Dateitasks wurden drei Verbesserungen eingeführt: (1) Es wurde Unterstützung für Platzhalter bei Kopier-/Löschvorgängen hinzugefügt. (2) Der Benutzer kann die rekursive Suche bei Löschvorgängen aktivieren/deaktivieren. (3) Der Name der Zieldatei kann bei Kopiervorgängen leer bleiben, um den Namen der Quelldatei beizubehalten.

### <a name="version-1170"></a>Version 1.17.0

Hierbei handelt es sich um eine ausschließlich für SQL Server 2019 veröffentlichte Hotfixversion.

#### <a name="bugfixes"></a>Fehlerbehebungen

1. Beim Ausführen in Visual Studio 2019 mit SQL Server 2019 als Ziel kann es bei flexiblen Dateitasks/Quellen/Zielen zu folgender Fehlermeldung kommen: `Attempted to access an element as a type incompatible with the array.`
1. Beim Ausführen in Visual Studio 2019 mit SQL Server 2019 als Ziel kann es bei flexiblen Dateitasks/Zielen bei Verwendung des ORC-/Parquet-Formats zu folgender Fehlermeldung kommen: `Microsoft.DataTransfer.Common.Shared.HybridDeliveryException: An unknown error occurred. JNI.JavaExceptionCheckException.` (Microsoft.DataTransfer.Common.Shared.HybridDeliveryException: Unbekannter Fehler. JNI.JavaExceptionCheckException.).

### <a name="version-1160"></a>Version 1.16.0

#### <a name="bugfixes"></a>Fehlerbehebungen

1. In bestimmten Fällen meldet die Paketausführung „Fehler: Die Datei oder Assembly ‚Newtonsoft.Json, Version=11.0.0.0, Culture=neutral, PublicKeyToken=30ad4fe6b2a6aeed‘ oder eine ihrer Abhängigkeit konnte nicht gefunden werden.“

### <a name="version-1150"></a>Version 1.15.0

#### <a name="improvements"></a>Verbesserungen

1. Vorgang zum Löschen eines Ordners bzw. einer Datei zum flexiblen Dateitask hinzugefügt
1. Funktion zum Konvertieren des externen bzw. Ausgabedatentyps zur flexiblen Dateiquelle hinzugefügt

#### <a name="bugfixes"></a>Fehlerbehebungen

1. In bestimmten Fällen treten Fehler beim Testen der Verbindung für Data Lake Storage Gen2 auf, und die Fehlermeldung „Es wurde versucht, auf ein Element zuzugreifen, dessen Typ mit dem Array nicht kompatibel ist“ wird angezeigt.
1. Unterstützung für den Azure-Speicheremulator wieder verfügbar