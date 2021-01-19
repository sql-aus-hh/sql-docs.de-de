---
title: Serverkonfigurationsoptionen für den Zugriffsüberprüfungscache | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den Cachespeicher für Zugriffsüberprüfungsergebnisse und die Optionen, die das Verhalten des Caches steuern. Außerdem erfahren Sie, wann diese Optionen in SQL Server geändert werden sollten.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- access check cache option
- access check cache bucket count
- access check cache quota
ms.assetid: 0a992ea8-3ec6-4a4d-97b5-460ae7326247
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 893400411c6f3d2faf6313923aa0f6127a3f2cf0
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171072"
---
# <a name="access-check-cache-server-configuration-options"></a>Serverkonfigurationsoptionen für den Zugriffsüberprüfungscache
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Beim Zugriff auf Datenbankobjekte durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wird die Zugriffsprüfung in einer internen Struktur zwischengespeichert, die als **access check result cache** bezeichnet wird. 
  
Die Option **access check cache bucket count** steuert die Anzahl der Hashbuckets, die für den Ergebniscache der Zugriffsüberprüfung verwendet werden. 

Die Option **access check cache quota** steuert die Anzahl der Einträge, die im Ergebniscache der Zugriffsüberprüfung gespeichert werden. Wenn die maximale Anzahl von Einträgen erreicht ist, werden die ältesten Einträge aus dem Ergebniscache der Zugriffsüberprüfung entfernt.
  
Die Standardwerte 0 geben an, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diese Optionen verwaltet. Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] werden die Standardwerte in die folgenden internen Konfigurationen übersetzt:
-   Bei der Bucketanzahl für den Cache der Zugriffsüberprüfung wird durch den Wert 0 ein Standardwert von 256 Buckets festgelegt.
-   Beim Cachekontingent der Zugriffsüberprüfung wird durch den Wert 0 ein Standardwert von 1.024 Einträgen festgelegt.

In seltenen Fällen kann die Leistung durch das Ändern dieser Optionen verbessert werden. Beispielsweise können Sie den Ergebniscache für die Zugriffsüberprüfung verkleinern, wenn zu viel Arbeitsspeicher verwendet wird. Sie können den Ergebniscache für die Zugriffsüberprüfung auch vergrößern, wenn Sie bei der Neuberechnung von Berechtigungen eine hohe CPU-Auslastung feststellen.
 
> [!IMPORTANT]
> Microsoft empfiehlt, diese Optionen nur auf Anweisung des Microsoft-Kundendiensts zu ändern. Wenn Sie die Werte „access check cache bucket count“ und „access check cache quota“ ändern müssen, verwenden Sie ein Verhältnis von 1:4. Beispiel: Wenn Sie den Wert „access check cache bucket count“ in 512 ändern, sollten Sie den Wert für „access check cache quota“ in 2.048 ändern. 
  
## <a name="see-also"></a>Weitere Informationen  
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
