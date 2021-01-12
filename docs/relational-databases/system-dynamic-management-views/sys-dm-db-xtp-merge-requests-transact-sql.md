---
description: sys.dm_db_xtp_merge_requests (Transact-SQL)
title: sys.dm_db_xtp_merge_requests (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c1224e88-af74-4c99-ae32-d5d2c552a1f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a7f8f4a38bffb8abc6be1380a2225077130f143
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099854"
---
# <a name="sysdm_db_xtp_merge_requests-transact-sql"></a>sys.dm_db_xtp_merge_requests (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Verfolgt Zusammenführungsanforderungen für Datenbanken nach. Die Merge-Anforderung wurde möglicherweise von SQL Server generiert, oder die Anforderung wurde von einem Benutzer mit [sys.sp_xtp_merge_checkpoint_files (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-merge-checkpoint-files-transact-sql.md)erstellt.

> [!NOTE]
> Diese dynamische Verwaltungs Sicht (DMV), sys.dm_db_xtp_merge_requests, ist bis Microsoft SQL Server 2014 vorhanden.
> Aber ab SQL Server 2016 ist diese DMV nicht mehr anwendbar.

## <a name="columns-in-the-report"></a>Spalten im Bericht

| Spaltenname | Datentyp | BESCHREIBUNG |
| :-- | :-- | :-- |
| request_state | TINYINT | Der Status der Zusammenführungsanforderung:<br/>0 = Requested<br/>1 = Pending<br/>2 = installiert<br/>3 = Abandoned |
| request_state_desc | nvarchar(60) | Bedeutungen für den aktuellen Status der Anforderung:<br/><br/>Angefordert: Es ist eine Merge-Anforderung vorhanden.<br/>Ausstehend: die Zusammenführung wird verarbeitet.<br/>Installiert: der Merge ist fertiggestellt.<br/>Abgebrochen: der Merge konnte nicht fertiggestellt werden, möglicherweise aufgrund eines fehlenden Speichers. |
| destination_file_id | GUID | Der eindeutige Bezeichner der Zieldatei für die Zusammenführung der Quelldateien. |
| lower_bound_tsn | BIGINT | Der minimale Zeitstempel für die als Ziel verwendete zusammengeführte Datei. Der niedrigste Transaktionszeitstempel aller zusammenzuführenden Quelldateien. |
| upper_bound_tsn | BIGINT | Der maximale Zeitstempel für die als Ziel verwendete zusammengeführte Datei. Der höchste Transaktionszeitstempel aller zusammenzuführenden Quelldateien. |
| collection_tsn | BIGINT | Der Zeitstempel, bei dem die aktuelle Zeile gesammelt werden kann.<br/><br/>Eine Zeile mit dem Status "Installed" wird entfernt, wenn checkpoint_tsn größer als collection_tsn ist.<br/><br/>Wenn checkpoint_tsn kleiner als collection_tsn ist, wird eine Zeile mit dem Status "Abandoned" entfernt. |
| checkpoint_tsn | BIGINT | Die Zeit, zu der der Prüfpunkt gestartet wurde.<br/><br/>Alle Löschvorgänge, die von Transaktionen mit einem niedrigeren Zeitstempel ausgeführt werden, werden in der neuen Datendatei berücksichtigt. Die übrigen Löschvorgänge werden in die als Ziel verwendete Änderungsdatei verschoben. |
| sourcenumber_file_id | GUID | Bis zu 16 interne Datei-IDs, die die Quelldateien in der Zusammenführung eindeutig identifizieren. |

## <a name="permissions"></a>Berechtigungen

Erfordert die VIEW DATABASE STATE-Berechtigung für die aktuelle Datenbank.

## <a name="see-also"></a>Weitere Informationen

[Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)
