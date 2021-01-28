---
description: Azure-Blob-Uploadtask
title: Azure-Blob-Uploadtask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobuptask.f1
- sql14.dts.designer.afpblobuptask.f1
ms.assetid: 6ea068b0-4cd8-45b5-b89d-09b8f25040c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 588bb693d5bcf8561d7f697dc910c483d63160a7
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783077"
---
# <a name="azure-blob-upload-task"></a>Azure-Blob-Uploadtask

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Der **Azure-Blob-Uploadtask** ermöglicht einem SSIS-Paket das Hochladen von Dateien in Azure Blob Storage.
    
Um einen **Azure-Blob-Uploadtask** hinzuzufügen, legen Sie ihn mittels Drag &amp; Drop auf dem SSIS-Designer ab, und doppelklicken Sie darauf, oder klicken Sie mit der rechten Maustaste, und klicken Sie anschließend auf **Bearbeiten** , um das folgende Dialogfeld **Azure-Blob-Uploadtask-Editor** anzuzeigen.  
  
 Der **Azure-Blob-Uploadtask** ist eine Komponente des [SQL Server Integration Services-Feature Packs (SSIS) für Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
  
 Die folgende Tabelle enthält Beschreibungen für die Felder in diesem Dialogfeld.  

|**Feld**|**Beschreibung**|  
|---|---|  
|AzureStorageConnection|Geben Sie einen vorhandenen Azure Storage-Verbindungs-Manager an, oder erstellen Sie einen neuen, der sich auf ein Azure-Speicherkonto bezieht, das auf den Speicherort der Blob-Dateien verweist.|  
|BlobContainer|Gibt den Namen des Blobcontainers an, in dem die hochgeladenen Dateien als Blobs enthalten sind.|  
|BlobDirectory|Gibt das Blobverzeichnis an, in dem die hochgeladene Datei als Blockblob gespeichert wird. Das Blobverzeichnis ist eine virtuelle hierarchische Struktur. Wenn das Blob bereits vorhanden ist, wird es ersetzt.|  
|LocalDirectory|Geben Sie das lokale Verzeichnis mit den Dateien an, die hochgeladen werden sollen.|  
|SearchRecursively|Geben Sie an, ob in Unterverzeichnissen rekursiv gesucht werden soll.|  
|FileName|Gibt einen Namensfilter an, um Dateien mit dem angegebenen Namensmuster auszuwählen. Beispielsweise enthält `MySheet*.xls\*` Dateien wie `MySheet001.xls` und `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Gibt einen Zeitbereichsfilter an. Dateien, die nach **TimeRangeFrom** und vor **TimeRangeTo** geändert wurden, sind eingeschlossen.|  
