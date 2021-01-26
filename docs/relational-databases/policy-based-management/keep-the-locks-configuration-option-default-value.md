---
description: Beibehalten des Standardwerts für die Konfigurationsoption 'locks'
title: Beibehalten des Standardwerts für die Konfigurationsoption „locks“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: f214f05b-5f0b-4786-b2ad-b8b4b6e58d72
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ab27552c473419efd942cdd1c37f7dca4b64c5b6
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597291"
---
# <a name="keep-the-locks-configuration-option-default-value"></a>Beibehalten des Standardwerts für die Konfigurationsoption 'locks'
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Diese Regel überprüft den Wert der Konfigurationsoption Sperren. Durch diese Option wird die maximale Anzahl verfügbarer Sperren festgelegt. Diese schränkt ein, wie viel Arbeitsspeicher [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] für Sperren verwendet. In der Standardeinstellung 0 kann [!INCLUDE[ssDE](../../includes/ssde-md.md)] Sperrstrukturen je nach Systemanforderungen dynamisch zuordnen bzw. deren Zuordnung aufheben.  
  
 Wenn Sperren einen Wert ungleich 0 (null) hat, werden Stapelverarbeitungsaufträge angehalten, und es wird eine Fehlermeldung angezeigt, dass keine Sperren vorhanden sind, wenn der angegebene Wert überschritten wird.  
  
## <a name="best-practices-recommendations"></a>Empfehlungen zu Best Practices  
 Verwenden Sie die gespeicherte Systemprozedur sp_configure, um den Wert von Sperren mithilfe der folgenden Anweisung auf die Standardeinstellung zu ändern:  
  
```  
EXEC sp_configure 'locks', 0;  
```  
  
## <a name="for-more-information"></a>Weitere Informationen  
 [Konfigurieren der Serverkonfigurationsoption Sperren](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md)  
  
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)  
  
 [Microsoft Knowledge Base-Artikel 271509](/troubleshoot/sql/performance/understand-resolve-blocking)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen und Erzwingen von Best Practices mit der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
