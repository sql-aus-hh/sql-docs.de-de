---
title: Auswerten von Clusterprotokollen mit einem Kibana-Dashboard
titleSuffix: SQL Server Big Data Clusters
description: Überwachen eines Clusters mit einem Kibana-Dashboard in einem SQL Server 2019-Big Data-Cluster.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 55dc3056b9f66f7a96b55ab750a5f74fe9bfd394
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091710"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Auswerten von Clusterprotokollen mit einem Kibana-Dashboard

In diesem Artikel wird beschrieben, wie Sie eine Anwendung in einem Big Data-Cluster in SQL Server überwachen.

## <a name="prerequisites"></a>Voraussetzungen

- [SQL Server 2019: Big Data-Cluster](deployment-guidance.md)
- [Befehlszeilen-Hilfsprogramm „azdata“](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Funktionen

In SQL Server 2019 können Sie Ihre Anwendung erstellen, löschen, beschreiben, initialisieren, auflisten und aktualisieren. In der folgenden Tabelle werden die Befehle für die Anwendungsbereitstellung beschrieben, die Sie mit **azdata** verwenden können.

|Get-Help |BESCHREIBUNG |
|:---|:---|
|`azdata bdc endpoint list` | Listet die Endpunkte für den Big Data-Cluster auf. |


Sie können das folgende Beispiel verwenden, um den Endpunkt eines **Kibana-Dashboards** aufzulisten:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

In der Ausgabe wird der Endpunkt angegeben, den Sie für die Anmeldung mit dem Benutzernamen und dem Kennwort Ihres Clusters verwenden können. 

![Kibana-Dashboard](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Der Link zu einem Kibana-Dashboard:

![Kibana-Dashboard](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Der (alte) Microsoft Edge-Browser ist nicht mit Kibana kompatibel. Sie müssen den Chromium-basierten Browser verwenden, damit das Dashboard korrekt angezeigt wird. Wenn Sie die Dashboards mit einem nicht unterstützten Browser laden, wird eine leere Seite angezeigt. Klicken Sie hier, um unterstützte Browser für Kibana anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).