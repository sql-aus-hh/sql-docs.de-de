---
description: sp_change_log_shipping_secondary_database (Transact-SQL)
title: sp_change_log_shipping_secondary_database (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 81a5c562ddade5c60458ad491a401dab916c59d4
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620294"
---
# <a name="sp_change_log_shipping_secondary_database-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert Einstellungen sekundärer Datenbanken.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>Argumente  
`[ @restore_delay = ] 'restore_delay'` Die Zeitspanne in Minuten, die der sekundäre Server wartet, bevor er eine bestimmte Sicherungsdatei wiederherstellt. *restore_delay* ist vom Datentyp **int** und kann nicht NULL sein. Der Standardwert ist 0.  
  
`[ @restore_all = ] 'restore_all'` Wenn der Wert auf 1 festgelegt ist, stellt der sekundäre Server bei Ausführung des Wiederherstellungs Auftrags alle verfügbaren Transaktionsprotokoll Sicherungen wieder her. Andernfalls wird Sie beendet, nachdem eine Datei wieder hergestellt wurde. *restore_all* ist vom **Bit** und kann nicht NULL sein.  
  
`[ @restore_mode = ] 'restore_mode'` Der Wiederherstellungs Modus für die sekundäre Datenbank.  
  
 0 = Wiederherstellungsprotokoll mit NORECOVERY.  
  
 1 = RESTORE LOG with Standby.  
  
 *Restore* ist ein **Bit** und kann nicht NULL sein.  
  
`[ @disconnect_users = ] 'disconnect_users'` Wenn der Wert auf 1 festgelegt ist, werden die Benutzer von der sekundären Datenbank getrennt, wenn ein Wiederherstellungs Vorgang ausgeführt wird. Standardwert = 0. *disconnect_users* ist vom **Bit** und kann nicht NULL sein.  
  
`[ @block_size = ] 'block_size'` Die Größe in Bytes, die als Blockgröße für das Sicherungsmedium verwendet wird. *block_size* ist vom Datentyp **int** und hat den Standardwert-1.  
  
`[ @buffer_count = ] 'buffer_count'` Die Gesamtanzahl der Puffer, die vom Sicherungs-oder Wiederherstellungs Vorgang verwendet werden. *buffer_count* ist vom Datentyp **int** und hat den Standardwert-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Die Größe der maximalen Eingabe-oder Ausgabeanforderung in Bytes, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an das Sicherungsmedium ausgegeben wird. *max_transfersize* ist vom Datentyp **int** und kann NULL sein.  
  
`[ @restore_threshold = ] 'restore_threshold'` Die zulässige Anzahl von Minuten zwischen Wiederherstellungs Vorgängen, bevor eine Warnung generiert wird. *restore_threshold* ist vom Datentyp **int** und kann nicht NULL sein.  
  
`[ @threshold_alert = ] 'threshold_alert'` Die Warnung, die ausgelöst werden soll, wenn der Wiederherstellungs Schwellenwert überschritten wird. *threshold_alert* ist vom Datentyp **int** und hat den Standardwert 14421.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Gibt an, ob eine Warnung ausgelöst wird, wenn *restore_threshold* überschritten wird. 1 = aktiviert; 0 = deaktiviert. *threshold_alert_enabled* ist vom **Bit** und kann nicht NULL sein.  
  
`[ @history_retention_period = ] 'history_retention_period'` Der Zeitraum in Minuten, in dem der Verlauf beibehalten wird. *history_retention_period* ist vom Datentyp **int**. Der Wert 1440 wird verwendet, wenn kein Wert angegeben wird.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_change_log_shipping_secondary_database** muss von der **Master** -Datenbank auf dem sekundären Server ausgeführt werden. Diese gespeicherte Prozedur führt folgende Aktionen aus:  
  
1.  Ändert die Einstellungen in den **log_shipping_secondary_database** Datensätzen nach Bedarf.  
  
2.  Ändert den lokalen Überwachungsdaten Satz in **log_shipping_monitor_secondary** auf dem sekundären Server mithilfe der angegebenen Argumente, falls erforderlich.  

## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** können diese Prozedur ausführen.  
  
## <a name="examples"></a>Beispiele  
 Dieses Beispiel veranschaulicht die Verwendung von **sp_change_log_shipping_secondary_database** , um die Parameter der sekundären Datenbank für die **LogShipAdventureWorks**-Datenbank zu aktualisieren.  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
