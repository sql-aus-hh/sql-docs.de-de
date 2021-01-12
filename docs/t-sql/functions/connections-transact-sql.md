---
description: '&#x40;&#x40;CONNECTIONS (Transact-SQL)'
title: '@@CONNECTIONS (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@CONNECTIONS'
- '@@CONNECTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@CONNECTIONS function'
- connections [SQL Server], number of
- connections [SQL Server], attempted
- number of connection attempts
- attempted connections
ms.assetid: c59836a8-443c-4b9a-8b96-8863ada97ac7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 32275f46ea789f5507fff4a2594d7eb8d84604f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101261"
---
# <a name="x40x40connections-transact-sql"></a>&#x40;&#x40;CONNECTIONS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Die Funktion gibt die Anzahl der versuchten Verbindungen – sowohl der erfolgreichen als auch der nicht erfolgreichen – seit dem letzten Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
@@CONNECTIONS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
**integer**
  
## <a name="remarks"></a>Hinweise  
Verbindungen sind nicht von Benutzern abhängig. Eine Anwendung kann z. B. mehrere Verbindungen mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] öffnen, ohne dass der Benutzer diese Verbindungen wahrnimmt.
  
Führen Sie **sp_monitor** aus, um einen Bericht zu erstellen, der verschiedene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Statistiken enthält, einschließlich der Anzahl der Verbindungsversuche.
  
@@MAX_CONNECTIONS ist die maximal zulässige Anzahl gleichzeitiger Verbindungen mit dem Server. @@CONNECTIONS wird bei jedem Anmeldeversuch inkrementiert, daher kann @@CONNECTIONS @@MAX_CONNECTIONS überschreiten.
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird die Anzahl der Anmeldeversuche bis zum aktuellen Datum und der aktuellen Uhrzeit zurückgegeben.
  
```sql
SELECT GETDATE() AS 'Today''s Date and Time',   
@@CONNECTIONS AS 'Login Attempts';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
``` 
Today's Date and Time  Login Attempts  
---------------------- --------------  
12/5/2006 10:32:45 AM  211023         
```  
  
## <a name="see-also"></a>Siehe auch
[System Statistical Functions &#40;Transact-SQL&#41; (Statistische Systemfunktionen (Transact-SQL))](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
[sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)
  
  
