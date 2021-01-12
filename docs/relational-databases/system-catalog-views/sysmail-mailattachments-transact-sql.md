---
description: sysmail_mailattachments (Transact-SQL)
title: sysmail_mailattachments (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fe55316cc27c21849379afe400d2950a5bcf882
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100132"
---
# <a name="sysmail_mailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Anlage, die an die Datenbank-E-Mail übermittelt wurde. Verwenden Sie diese Sicht, wenn Sie Informationen zu Datenbank-E-Mail-Anlagen benötigen. Zum Überprüfen aller von Datenbank-E-Mail verarbeiteten e-Mails verwenden Sie [sysmail_allitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md).  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**int**|Der Bezeichner für die Anlage.|  
|**mailitem_id**|**int**|Der Bezeichner für das E-Mail-Element, das die Anlage enthält.|  
|**filename**|**nvarchar (520)**|Der Dateiname der Anlage. Wenn **attach_query_result** 1 und **query_attachment_filename** NULL ist, erstellt Datenbank-E-Mail einen beliebigen Dateinamen.|  
|**Filesize**|**int**|Die Größe der Anlage in Bytes.|  
|**Angeklag**|**varbinary(max)**|Der Inhalt der Anlage.|  
|**last_mod_date**|**datetime**|Das Datum und die Uhrzeit der letzten Änderung der Zeile.|  
|**last_mod_user**|**sysname**|Der Benutzer, der die Zeile zuletzt geändert hat.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie Probleme mit der Datenbank-E-Mail behandeln, können Sie diese Sicht verwenden, um die Eigenschaften der Anlagen anzuzeigen.  
  
 In den Systemtabellen gespeicherte Anlagen können dazu führen, dass die **msdb** -Datenbank vergrößert wird. Verwenden Sie **sysmail_delete_mailitems_sp** , um e-Mail-Elemente und ihre zugehörigen Anlagen zu löschen Weitere Informationen finden Sie unter [Erstellen eines SQL Server-Agent Auftrags zum Archivieren Datenbank-E-Mail Nachrichten und Ereignisprotokollen](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Wird der festen Server Rolle **sysadmin** und der Daten Bank Rolle **DatabaseMailUserRole** gewährt. Wenn Sie von einem Mitglied der festen Server Rolle **sysadmin** ausgeführt werden, werden in dieser Ansicht alle Anlagen angezeigt. Für alle anderen Benutzer werden nur die von ihnen übermittelten Anlagen angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sysmail_allitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
