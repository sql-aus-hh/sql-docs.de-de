---
description: sysmail_unsentitems (Transact-SQL)
title: sysmail_unsentitems (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_unsentitems_TSQL
- sysmail_unsentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_unsentitems database mail view
ms.assetid: 993c12da-41e5-4e53-a188-0323feb70c67
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0e8ffcc7ae60096de1b0355ed113ffb55e02e286
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100112"
---
# <a name="sysmail_unsentitems-transact-sql"></a>sysmail_unsentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Enthält eine Zeile für jede Datenbank-E-Mail Nachricht mit dem Status **nicht gesendete** oder **Wiederholungsversuch** . Nachrichten mit dem Status unsent oder retrying werden in der E-Mail-Warteschlange aufbewahrt und können jederzeit gesendet werden. Nachrichten können aus den folgenden Gründen den Status **nicht gesendete** aufweisen:  
  
-   Eine neue Nachricht wurde erstellt, und obwohl sich die Nachricht in der E-Mail-Warteschlange befindet, bearbeitet die Datenbank-E-Mail zunächst andere Nachrichten und hat diese Nachricht noch nicht erreicht.  
  
-   Das externe Datenbank-E-Mail-Programm wird nicht ausgeführt, und es werden keine E-Mails gesendet.  
  
 Nachrichten können den **Wiederholungs** Status aus folgenden Gründen aufweisen:  
  
-   Die Datenbank-E-Mail hat versucht, die E-Mail zu senden, aber der SMTP-Mailserver war nicht erreichbar. Die Datenbank-E-Mail versucht weiterhin, die Nachricht zu senden, wobei sie andere Datenbank-E-Mail-Konten verwendet, die dem Profil, von dem aus die Nachricht gesendet wurde, zugeordnet sind. Wenn die e-Mail von keinem Konto gesendet werden kann, wartet Datenbank-E-Mail auf den Zeitraum, der für den Parameter **Wiederholungs Verzögerung** konfiguriert wurde, und versucht dann erneut, die Nachricht zu senden. Datenbank-E-Mail verwendet den Parameter für **Wiederholungs Versuche** für das Konto, um zu bestimmen, wie oft versucht werden soll, die Nachricht zu senden. Nachrichten behalten den **Wiederholungs** Status bei, sofern Datenbank-E-Mail versucht, die Nachricht zu senden.  
  
 Verwenden Sie diese Sicht, um anzuzeigen, wie viele Nachrichten darauf warten, gesendet zu werden, und seit wann diese sich in der E-Mail-Warteschlange befinden. Normalerweise ist die Anzahl der **nicht gesendeten** Nachrichten niedrig. Führen Sie unter normalen Betriebsbedingungen einen Vergleichstest durch, um eine für Ihre Betriebsabläufe angemessene Anzahl von Nachrichten in der Nachrichtenwarteschlange zu ermitteln.  
  
 Wenn Sie alle von Datenbank-E-Mail verarbeiteten Nachrichten anzeigen möchten, verwenden Sie [sysmail_allitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Verwenden Sie [sysmail_faileditems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md), um nur Nachrichten mit dem Status "Fehler" anzuzeigen. Verwenden Sie [sysmail_sentitems &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md), um nur gesendete Nachrichten anzuzeigen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Der Bezeichner des E-Mail-Elements in der E-Mail-Warteschlange.|  
|**profile_id**|**int**|Der Bezeichner des Profils, das zum Übermitteln der Nachricht verwendet wurde.|  
|**Empfängers**|**varchar(max)**|Die E-Mail-Adressen der Nachrichtenempfänger.|  
|**copy_recipients**|**varchar(max)**|Die E-Mail-Adressen derer, die Kopien der Nachricht erhalten.|  
|**blind_copy_recipients**|**varchar(max)**|Die E-Mail-Adressen derer, die Kopien der Nachricht erhalten, deren Namen jedoch nicht im Nachrichtenkopf angezeigt werden.|  
|**subject**|**nvarchar (510)**|Die Betreffzeile der Nachricht.|  
|**body**|**varchar(max)**|Der Textkörper der Nachricht.|  
|**body_format**|**varchar (20)**|Das Textkörperformat der Nachricht. Mögliche Werte sind **Text** und **HTML**.|  
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
|**send_request_user**|**sysname**|Der Benutzer, der die Nachricht übermittelt hat. Dabei handelt es sich um den Benutzer Kontext der Datenbank-e-Mail-Prozedur, nicht um das **from** -Feld der Nachricht.|  
|**sent_account_id**|**int**|Der Bezeichner des Datenbank-E-Mail-Kontos, das zum Senden der Nachricht verwendet wird. Für diese Sicht immer NULL.|  
|**sent_status**|**varchar (8)**|**Wird nicht verwendet, wenn Datenbank-E-Mail** nicht versucht hat, die e-Mail zu senden. Der Vorgang wird wieder **holt** , wenn Datenbank-E-Mail die Nachricht nicht senden konnte, aber erneut versucht.|  
|**sent_date**|**datetime**|Das Datum und die Uhrzeit, an dem bzw. zu der die Datenbank-E-Mail zuletzt versucht hat, die E-Mail zu senden. Hat die Datenbank-E-Mail nicht versucht, die Nachricht zu senden, lautet der Wert NULL.|  
|**last_mod_date**|**datetime**|Das Datum und die Uhrzeit der letzten Änderung der Zeile.|  
|**last_mod_user**|**sysname**|Der Benutzer, der die Zeile zuletzt geändert hat.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie Probleme mit der Datenbank-E-Mail behandeln, kann diese Sicht Ihnen helfen, die Ursache des Problems zu identifizieren, da sie anzeigt, wie viele Nachrichten darauf warten, gesendet zu werden, und seit wann diese Nachrichten warten. Werden keine Nachrichten gesendet, wird das externe Datenbank-E-Mail-Programm möglicherweise nicht ausgeführt, oder die Datenbank-E-Mail kann aufgrund eines Netzwerkproblems die SMTP-Server nicht erreichen. Wenn viele der nicht gesendeten Nachrichten denselben **profile_id** haben, liegt möglicherweise ein Problem mit dem SMTP-Server vor. Sie sollten eventuell dem Profil zusätzliche Konten hinzufügen. Wenn Nachrichten zwar gesendet werden, sich jedoch zu lange in der Warteschlange befinden, benötigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] möglicherweise zusätzliche Ressourcen, um das Nachrichtenvolumen zu bewältigen.  
  
## <a name="permissions"></a>Berechtigungen  
 Wird der festen Server Rolle **sysadmin** und der Daten Bank Rolle **DatabaseMailUserRole** gewährt. Wenn Sie von einem Mitglied der festen Server Rolle **sysadmin** ausgeführt wird, werden in dieser **Ansicht alle nicht** gesendeten oder **wieder** holten Nachrichten angezeigt. Alle anderen Benutzer sehen nur die gesendeten oder **wieder** holten Nachrichten **, die Sie** übermittelt haben.  
  
  
