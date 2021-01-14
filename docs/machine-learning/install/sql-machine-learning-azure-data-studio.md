---
title: Azure Data Studio-Notebooks (Python, R)
description: Erfahren Sie, wie Sie mit SQL Server Machine Learning Services Python- und R-Skripts in einem Notebook in Azure Data Studio ausführen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/09/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: b1e3d81e0229d028b9fab6a4521ee11cb20ce1b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091548"
---
# <a name="run-python-and-r-scripts-in-azure-data-studio-notebooks-with-sql-server-machine-learning-services"></a>Ausführen von Python- und R-Skripts in Azure Data Studio-Notebooks mit SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

In diesem Artikel erhalten Sie Informationen zum Ausführen von Python- und R-Skripts in [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)-Notebooks mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Azure Data Studio ist ein plattformübergreifendes Datenbanktool.

## <a name="prerequisites"></a>Voraussetzungen

- [Laden Sie Azure Data Studio herunter, und installieren Sie das Tool](../../azure-data-studio/download-azure-data-studio.md) auf Ihrer Arbeitsstation. Azure Data Studio ist plattformübergreifend und kann unter Windows, macOS und Linux ausgeführt werden.

- Ein Server, auf dem SQL Server Machine Learning Services installiert und aktiviert ist Sie können Machine Learning Services unter Windows oder Linux oder auf Big Data-Clustern verwenden:

  - [Installieren von SQL Server Machine Learning Services (Python und R) unter Windows](sql-machine-learning-services-windows-install.md)
  - [Installieren von SQL Server Machine Learning Services (Python und R) unter Linux](../../linux/sql-server-linux-setup-machine-learning.md)
  - [Ausführen von Python- und R-Skripts mit Machine Learning Services auf SQL Server-Big Data-Clustern](../../big-data-cluster/machine-learning-services.md)

## <a name="create-a-sql-notebook"></a>Erstellen eines SQL-Notebooks

> [!IMPORTANT]
> Machine Learning Services wird als Teil von SQL Server ausgeführt. Daher muss ein SQL-Kernel verwendet werden und kein Python-Kernel.

Sie können Machine Learning Services in Azure Data Studio mit einem SQL-Notebook verwenden. Führen Sie die folgenden Schritte aus, um ein neues Notebook zu erstellen:

1. Klicken Sie auf **Datei** und dann auf **Neues Notebook**, um ein neues Notebook zu erstellen. Das Notebook verwendet standardmäßig den **SQL-Kernel**.

1. Klicken Sie auf **Anfügen an** und **Verbindung ändern**. 

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebook Verbindung ändern](media/ads-attach-to-connection.png)
    
1. Stellen Sie eine Verbindung zu einem vorhandenen oder neuen SQL Server her. Sie haben folgende Möglichkeiten:

    1. Wählen Sie unter **Letzte Verbindungen** oder **Gespeicherte Verbindungen** eine vorhandene Verbindung aus.

    1. Stellen Sie unter **Verbindungsdetails** eine neue Verbindung her. Füllen Sie die Verbindungsdetails für Ihren SQL Server und Ihre Datenbank aus.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebook Verbindungsdetails](media/ads-connection-details.png)  

## <a name="run-python-or-r-scripts"></a>Ausführen von Python- oder R-Skripts

SQL-Notebooks bestehen aus Code- und Textzellen. Codezellen werden dafür verwendet, Python- oder R-Skripts über die gespeicherte Prozedur [sp_execute_external_scripts](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) auszuführen. Textzellen können dafür verwendet werden, den Code im Notebook zu dokumentieren.

### <a name="run-a-python-script"></a>Ausführen eines Python-Skripts

Gehen Sie folgendermaßen vor, um ein Python-Skript auszuführen:

1. Klicken Sie auf **+ Code**, um eine Codezelle hinzuzufügen.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebooks Codeblock hinzufügen](media/ads-add-code.png)  

1. Geben Sie das folgende Skript in die Codezelle ein:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Klicken Sie auf **Zelle ausführen** (den Pfeil im schwarzen Kreis), oder drücken Sie **F5**, um eine einzelne Zelle auszuführen.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebooks Python-Code ausführen](media/ads-run-python.png)  

1. Das Ergebnis wird unter der Codezelle angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebook Python-Code Ausgabe](media/ads-run-python-output.png)  

### <a name="run-an-r-script"></a>Ausführen eines R-Skripts

Gehen Sie folgendermaßen vor, um ein R-Skript auszuführen:

1. Klicken Sie auf **+ Code**, um eine Codezelle hinzuzufügen.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebooks Codeblock hinzufügen](media/ads-add-code.png)  

1. Geben Sie das folgende Skript in die Codezelle ein:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Klicken Sie auf **Zelle ausführen** (den Pfeil im schwarzen Kreis), oder drücken Sie **F5**, um eine einzelne Zelle auszuführen.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebooks R-Code ausführen](media/ads-run-r.png)  

1. Das Ergebnis wird unter der Codezelle angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL-Notebook R-Code Ausgabe](media/ads-run-r-output.png)  

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Notebooks in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md)
- [Erstellen und Ausführen eines SQL Server-Notebooks](../../azure-data-studio/notebooks/notebooks-sql-kernel.md)
- [Schnellstart: Ausführen einfacher Python-Skripts mit SQL Server Machine Learning Services](../tutorials/quickstart-python-create-script.md)
- [Schnellstart: Ausführen einfacher R-Skripts mit SQL Server Machine Learning Services](../tutorials/quickstart-r-create-script.md)