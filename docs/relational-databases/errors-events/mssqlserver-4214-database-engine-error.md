---
description: MSSQLSERVER_
title: MSSQLSERVER_4214
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 4214 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: b448b97f5f6eeaf403b0c5c218edb8cd7dd7f8ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101859"
---
# <a name="mssqlserver_4214"></a>MSSQLSERVER_4214
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|4214|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|DMPX_NODBBACKUP|
|Meldungstext|BACKUP LOG cannot be performed because there is no current database backup (BACKUP LOG kann nicht ausgeführt werden, weil keine aktuelle Datenbanksicherung vorhanden ist)|
||

## <a name="explanation"></a>Erklärung

Der Fehler wird ausgelöst, wenn Sie versuchen, eine Transaktionsprotokollsicherung durchzuführen, bevor Sie eine vollständige Sicherung einer Datenbank durchführen. Bevor Sie versuchen, das Transaktionsprotokoll für eine Datenbank zu sichern, müssen Sie eine vollständige Datenbanksicherung durchführen. Außerdem wird Ihnen im Fehlerprotokoll die folgende Meldung angezeigt:

> \<Datetime> Sicherungsfehler: 3041, Schweregrad: 16, Status: 1.  
\<Datetime>  Backup     BACKUP failed to complete the command BACKUP LOG \<db_name>. (BACKUP-Fehler beim Abschließen des Befehls BACKUP LOG \<db_name>.) Ausführliche Informationen finden Sie im Sicherungsanwendungsprotokoll.

## <a name="user-action"></a>Benutzeraktion

Führen Sie eine vollständige Datenbanksicherung durch, bevor Sie versuchen, das Transaktionsprotokoll für eine Datenbank zu sichern.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen finden Sie unter: [Sichern und Wiederherstellen von SQL Server-Datenbanken.](../backup-restore/back-up-and-restore-of-sql-server-databases.md)