---
description: Abfrage Benachrichtigungen-sys.dm_qn_subscriptions
title: sys.dm_qn_subscriptions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_qn_subscriptions
- dm_qn_subscriptions_TSQL
- sys.dm_qn_subscriptions
- sys.dm_qn_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_qn_subscriptions dynamic management view
ms.assetid: a3040ce6-f5af-48fc-8835-c418912f830c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 62953d7f80a8d0fa327f37eadcc7a8dafd6fa734
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100043"
---
# <a name="query-notifications---sysdm_qn_subscriptions"></a>Abfrage Benachrichtigungen-sys.dm_qn_subscriptions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu den aktiven Abfragebenachrichtigungsabonnements auf dem Server zurück. Diese Sicht können Sie verwenden, um in der Serverdatenbank oder in einer angegebenen Datenbank eine Überprüfung auf aktive Abonnements vorzunehmen oder um auf eine Überprüfung auf einen angegebenen Serverprinzipal vorzunehmen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID eines Abonnements.|  
|**database_id**|**int**|ID der Datenbank, für die die Abfragebenachrichtigung ausgeführt wurde. In dieser Datenbank sind die mit diesem Abonnement verbundenen Informationen gespeichert.|  
|**sid**|**varbinary(85)**|Sicherheits-ID des Serverprinzipals, der dieses Abonnement erstellt hat und besitzt.|  
|**object_id**|**int**|ID der internen Tabelle, in der die Informationen zu Abonnementparametern gespeichert sind.|  
|**created**|**datetime**|Datum und Uhrzeit des Zeitpunktes, an dem das Abonnement erstellt wurde.|  
|**timeout**|**int**|Timeout für das Abonnement in Sekunden. Die Benachrichtigung wird ausgelöst, nachdem diese Zeit verstrichen ist.<br /><br /> Hinweis: die tatsächliche Auslösezeit ist möglicherweise größer als das angegebene Timeout. Wenn jedoch eine Änderung, die das Abonnement für ungültig erklärt, nach dem angegebenen Timeout erfolgt, aber bevor das Abonnement ausgelöst wird, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt sicher, dass das auslösen zu dem Zeitpunkt erfolgt, an dem die Änderung vorgenommen wurde.|  
|**status**|**int**|Gibt den Status des Abonnements an. Die Liste der Codes finden Sie in der Tabelle unter den Hinweisen.|  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Ein|type|  
|----------|--------|--------|----------|  
|**sys.dm_qn_subscriptions**|**sys.databases**|**database_id**|n:1|  
|**sys.dm_qn_subscriptions**|**sys.internal_tables**|**object_id**|n:1|  
  
## <a name="remarks"></a>Bemerkungen  
 Der Statuscode 0 gibt einen nicht definierten Status an.  
  
 Die folgenden Statuscodes geben an, dass ein Abonnement aufgrund einer Änderung ausgelöst wurde:  
  
|Code|Untergeordneter Status|Info|  
|----------|------------------|----------|  
|65798|Abonnement wurde ausgelöst, da sich Daten geändert haben.|Abonnement wurde durch eine Einfügung ausgelöst.|  
|65799|Abonnement wurde ausgelöst, da sich Daten geändert haben.|Löschen|  
|65800|Abonnement wurde ausgelöst, da sich Daten geändert haben.|Aktualisieren|  
|65801|Abonnement wurde ausgelöst, da sich Daten geändert haben.|Merge|  
|65802|Abonnement wurde ausgelöst, da sich Daten geändert haben.|Tabelle kürzen|  
|66048|Abonnement wurde ausgelöst, da das Timeout abgelaufen ist.|Nicht definierter Infomodus|  
|66315|Abonnement wurde ausgelöst, da sich ein Objekt geändert hat.|Objekt oder Benutzer wurde gelöscht.|  
|66316|Abonnement wurde ausgelöst, da sich ein Objekt geändert hat.|Objekt wurde geändert.|  
|66565|Abonnement wurde ausgelöst, da eine Datenbank getrennt oder gelöscht wurde.|Server oder Datenbank wurde neu gestartet.|  
|66571|Abonnement wurde ausgelöst, da eine Datenbank getrennt oder gelöscht wurde.|Objekt oder Benutzer wurde gelöscht.|  
|66572|Abonnement wurde ausgelöst, da eine Datenbank getrennt oder gelöscht wurde.|Objekt wurde geändert.|  
|67341|Abonnement wurde aufgrund von Ressourcenmangel auf dem Server ausgelöst.|Abonnement wurde aufgrund von Ressourcenmangel auf dem Server ausgelöst.|  
  
 Die folgenden Statuscodes geben an, dass ein Abonnement nicht erstellt wurde:  
  
|Code|Untergeordneter Status|Info|  
|----------|------------------|----------|  
|132609|Abonnement konnte nicht erstellt werden, weil die Anweisung nicht unterstützt wird.|Abfrage ist zu komplex.|  
|132610|Abonnement konnte nicht erstellt werden, weil die Anweisung nicht unterstützt wird.|Ungültige Anweisung für Abonnement.|  
|132611|Abonnement konnte nicht erstellt werden, weil die Anweisung nicht unterstützt wird.|Ungültige festgelegte Optionen für das Abonnement.|  
|132612|Abonnement konnte nicht erstellt werden, weil die Anweisung nicht unterstützt wird.|Ungültige Isolationsstufe|  
|132622|Abonnement konnte nicht erstellt werden, weil die Anweisung nicht unterstützt wird.|Wird intern verwendet.|  
|132623|Abonnement konnte nicht erstellt werden, weil die Anweisung nicht unterstützt wird.|Überschreitet den Vorlagengrenzwert pro Tabelle.|  
  
 Die folgenden Statuscodes werden intern verwendet und als Check Kill- und Init-Modi klassifiziert:  
  
|Code|Untergeordneter Status|Info|  
|----------|------------------|----------|  
|198656|Intern verwendet: Check Kill- und Init-Modus|Nicht definierter Infomodus|  
|198928|Abonnement wurde zerstört.|Abonnement wurde ausgelöst, da eine Datenbank angefügt wurde.|  
|198929|Abonnement wurde zerstört.|Abonnement wurde ausgelöst, da ein Benutzer gelöscht wurde.|  
|198930|Abonnement wurde zerstört.|Abonnement wurde aufgrund eines erneuten Abonnements gelöscht.|  
|198931|Abonnement wurde zerstört.|Abonnement wurde abgebrochen.|  
|199168|Abonnement ist aktiv.|Nicht definierter Infomodus|  
|199424|Abonnement wurde initialisiert, ist aber noch nicht aktiv.|Nicht definierter Infomodus|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
> [!NOTE]  
>  Falls der Benutzer die VIEW SERVER STATE-Berechtigung nicht besitzt, gibt diese Sicht Informationen zu Abonnements zurück, die sich im Besitz des aktuellen Benutzers befinden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-return-active-query-notification-subscriptions-for-the-current-user"></a>A. Zurückgeben von aktiven Abfragebenachrichtigungsabonnements für den aktuellen Benutzer  
 Im folgenden Beispiel werden die aktiven Abfragebenachrichtigungsabonnements des aktuellen Benutzers zurückgegeben. Wenn der Benutzer über VIEW SERVER STATE-Berechtigungen verfügt, werden alle aktiven Abonnements auf dem Server zurückgegeben.  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions;  
GO  
```  
  
### <a name="b-returning-active-query-notification-subscriptions-for-a-specified-user"></a>B. Zurückgeben von aktiven Abfragebenachrichtigungsabonnements für einen angegebenen Benutzer  
 Im folgenden Beispiel werden die aktiven Abfragebenachrichtigungsabonnements zurückgegeben, die vom Anmeldenamen `Ruth0` abonniert wurden.  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions  
WHERE sid = SUSER_SID('Ruth0');  
GO  
```  
  
### <a name="c-returning-internal-table-metadata-for-query-notification-subscriptions"></a>C. Zurückgeben von internen Tabellenmetadaten für Abfragebenachrichtigungsabonnements  
 Im folgenden Beispiel werden die Metadaten interner Tabellen für Abfragebenachrichtigungsabonnements zurückgegeben.  
  
```  
SELECT qn.id AS query_subscription_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
FROM sys.internal_tables AS it  
JOIN sys.dm_qn_subscriptions AS qn ON it.object_id = qn.object_id  
WHERE it.internal_type_desc = 'QUERY_NOTIFICATION';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Abfrage Benachrichtigungen mit dynamischen Verwaltungs Sichten &#40;Transact-SQL-&#41;](./system-dynamic-management-views.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)  
  
