---
description: sysmail_allitems (Transact-SQL)
title: sysmail_allitems (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_allitems_TSQL
- sysmail_allitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_allitems database mail view
ms.assetid: 21fb8432-7677-4435-902f-64a58bba4cbb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6bc9b58549bbc20fe66caaf2adc61edd4345acd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096656"
---
# <a name="sysmail_allitems-transact-sql"></a>sysmail_allitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Enthält eine Zeile für jede Nachricht, die von der Datenbank-E-Mail verarbeitet wurde. Verwenden Sie diese Sicht, wenn Sie den Status aller Nachrichten anzeigen möchten.  
  
 Verwenden Sie [sysmail_faileditems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md), um nur Nachrichten mit dem Status "Fehler" anzuzeigen. Verwenden Sie [sysmail_unsentitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md), um nur nicht gesendete Nachrichten anzuzeigen. Verwenden Sie [sysmail_sentitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md), um nur gesendete Nachrichten anzuzeigen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Der Bezeichner des E-Mail-Elements in der E-Mail-Warteschlange.|  
|**profile_id**|**int**|Der Bezeichner des Profils, das zum Senden der Nachricht verwendet wurde.|  
|**Empfängers**|**varchar(max)**|Die E-Mail-Adressen der Nachrichtenempfänger.|  
|**copy_recipients**|**varchar(max)**|Die E-Mail-Adressen derer, die Kopien der Nachricht erhalten.|  
|**blind_copy_recipients**|**varchar(max)**|Die E-Mail-Adressen derer, die Kopien der Nachricht erhalten, deren Namen jedoch nicht im Nachrichtenkopf angezeigt werden.|  
|**subject**|**nvarchar (510)**|Die Betreffzeile der Nachricht.|  
|**body**|**varchar(max)**|Der Textkörper der Nachricht.|  
|**body_format**|**varchar (20)**|Das Textkörperformat der Nachricht. Mögliche Werte sind TEXT und HTML.|  
|**importance**|**varchar (6)**|Der **Wichtigkeits** Parameter der Nachricht.|  
|**/Kleinschreibung**|**varchar (12)**|Der **Sensitivitäts** Parameter der Nachricht.|  
|**file_attachments**|**varchar(max)**|Eine durch Semikolons getrennte Liste der Dateinamen, die an die E-Mail-Nachricht angehängt wurden.|  
|**attachment_encoding**|**varchar (20)**|Der Typ der E-Mail-Anlage.|  
|**query**|**varchar(max)**|Die Abfrage, die vom E-Mail-Programm ausgeführt wurde.|  
|**execute_query_database**|**sysname**|Der Datenbankkontext, in dem das E-Mail-Programm die Abfrage ausgeführt hat.|  
|**attach_query_result_as_file**|**bit**|Bei einem Wert von 0 wurden die Abfrageergebnisse hinter dem Inhalt des Textkörpers in den Textkörper der E-Mail-Nachricht eingeschlossen. Bei einem Wert von 1 wurden die Ergebnisse als Anlage zurückgegeben.|  
|**query_result_header**|**bit**|Bei einem Wert von 1 enthielten die Abfrageergebnisse Spaltenheader. Bei einem Wert von 0 enthielten die Abfrageergebnisse keine Spaltenheader.|  
|**query_result_width**|**int**|Der **query_result_width** -Parameter der Nachricht.|  
|**query_result_separator**|**char(1)**|Das Zeichen, das zum Trennen der Spalten in der Abfrageausgabe verwendet wird.|  
|**exclude_query_output**|**bit**|Der **exclude_query_output** -Parameter der Nachricht. Weitere Informationen finden Sie unter [sp_send_dbmail &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Der **append_query_error** -Parameter der Nachricht. 0 zeigt an, dass die Datenbank-E-Mail die Nachricht nicht senden soll, wenn die Abfrage einen Fehler enthält.|  
|**send_request_date**|**datetime**|Das Datum und die Uhrzeit, an dem bzw. zu der die Nachricht in der E-Mail-Warteschlange platziert wurde.|  
|**send_request_user**|**sysname**|Der Benutzer, der die Nachricht übermittelt hat. Hierbei handelt es sich um den Benutzerkontext der Datenbank-E-Mail-Prozedur, nicht um das Von-Feld der Nachricht.|  
|**sent_account_id**|**int**|Der Bezeichner des Datenbank-E-Mail-Kontos, das zum Senden der Nachricht verwendet wird.|  
|**sent_status**|**varchar (8)**|Der Status der E-Mail. Mögliche Werte:<br /><br /> **gesendet** -die e-Mail wurde gesendet.<br /><br /> nicht gesendete **Datenbank-e** -Mail versucht weiterhin, die Nachricht zu senden.<br /><br /> erneuter **Versuch** : Datenbank-E-Mail konnte die Nachricht nicht senden, sendet Sie jedoch erneut.<br /><br /> Fehler: Datenbank-e-Mail konnte die **Nachricht nicht senden** .|  
|**sent_date**|**datetime**|Das Datum und die Uhrzeit, an dem bzw. zu der die Nachricht gesendet wurde.|  
|**last_mod_date**|**datetime**|Das Datum und die Uhrzeit der letzten Änderung der Zeile.|  
|**last_mod_user**|**sysname**|Der Benutzer, der die Zeile zuletzt geändert hat.|  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die Ansicht **sysmail_allitems** , um den Status aller Nachrichten anzuzeigen, die von Datenbank-E-Mail verarbeitet werden. Wenn Sie Probleme mit der Datenbank-E-Mail behandeln, kann diese Sicht Ihnen helfen, die Ursache des Problems zu identifizieren, da sie die Attribute der gesendeten Nachrichten im Vergleich zu den Attributen der ungesendeten Nachrichten anzeigt.  
  
 Die von dieser Sicht verfügbar gemachten Systemtabellen enthalten alle Nachrichten und können dazu führen, dass die **msdb** -Datenbank vergrößert wird. Löschen Sie alte Nachrichten regelmäßig aus der Sicht, um die Größe der Tabellen zu reduzieren. Weitere Informationen finden Sie unter [Erstellen eines SQL Server-Agent Auftrags zum Archivieren Datenbank-E-Mail Nachrichten und Ereignisprotokollen](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Wird der festen Server Rolle **sysadmin** und der Daten Bank Rolle **DatabaseMailUserRole** gewährt. Wenn Sie von einem Mitglied der festen Server Rolle **sysadmin** ausgeführt wird, werden in dieser Ansicht alle Meldungen angezeigt. Für alle anderen Benutzer werden nur die von ihnen übermittelten Nachrichten angezeigt.  
  
  
