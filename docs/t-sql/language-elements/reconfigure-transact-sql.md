---
description: RECONFIGURE (Transact-SQL)
title: RECONFIGURE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/20/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RECONFIGURE
- RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- reconfiguring configuration options
- configuration options [SQL Server], reconfiguring
- updating configuration options
- RECONFIGURE, RECONFIGURE statement
- RECONFIGURE
- RECONFIGURE, WITH OVERRIDE statement
ms.assetid: 2e6e4eeb-b70b-4f45-a253-28ac4e595d75
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2fa09a4ac7fb59ec39524a709387ee4ff91aed63
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98083016"
---
# <a name="reconfigure-transact-sql"></a>RECONFIGURE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aktualisiert den derzeit konfigurierten Wert (die **config_value**-Spalte im **sp_configure**-Resultset) einer Konfigurationsoption, die mit der gespeicherten Systemprozedur **sp_configure** geändert wurde. Da einige Konfigurationsoptionen einen Serverstopp und -neustart erfordern, um den derzeit wirksamen Wert zu aktualisieren, aktualisiert RECONFIGURE nicht immer den derzeit wirksamen Wert (die **run_value**-Spalte im **sp_configure**-Resultset) mit einem geänderten Konfigurationswert.    
    
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Syntax    
    
```syntaxsql
RECONFIGURE [ WITH OVERRIDE ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 RECONFIGURE    
 Gibt an, dass der derzeit wirksame Wert aktualisiert werden soll, wenn die Konfigurationseinstellung keinen Serverstopp und Neustart erfordert. RECONFIGURE überprüft darüber hinaus die neuen Konfigurationswerte entweder auf ungültige Werte (z.B. einen Sortierreihenfolgenwert, der nicht in **syscharsets** vorhanden ist) oder auf nicht empfohlene Werte. Bei den Konfigurationsoptionen, die keinen Serverstopp und -neustart erfordern, sollten der derzeit wirksame Wert und der derzeit konfigurierte Wert der Konfigurationsoption nach Angabe von RECONFIGURE identisch sein.    
    
 WITH OVERRIDE    
 Deaktiviert die Überprüfung des Konfigurationswerts (auf ungültige oder nicht empfohlene Werte) für die erweiterte Konfigurationsoption **recovery interval**.    
    
 Fast alle Konfigurationsoptionen können mithilfe der Option WITH OVERRIDE neu konfiguriert werden. Einige schwerwiegende Fehler werden jedoch weiterhin verhindert. So kann beispielsweise der Wert der Konfigurationsoption **min server memory** nicht mit einem höheren Wert konfiguriert werden als der in der Konfigurationsoption **max server memory** angegebene Wert.
      
## <a name="remarks"></a>Hinweise    
 **sp_configure** akzeptiert keine neuen Werte für eine Konfigurationsoption, die für diese Konfigurationsoption außerhalb der dokumentierten gültigen Bereiche liegen.    
    
 RECONFIGURE ist in einer expliziten oder impliziten Transaktion nicht zulässig. Wenn Sie mehrere Optionen gleichzeitig neu konfigurieren und bei einem der Neukonfigurierungsvorgänge ein Fehler auftritt, wird keine der Neukonfigurierungen wirksam.    
    
 Beachte Sie beim Konfigurieren von Resource Governor die Option RECONFIGURE von [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md).    
    
## <a name="permissions"></a>Berechtigungen    
 RECONFIGURE-Berechtigungen erhalten standardmäßig ALTER SETTINGS-Berechtigte. Diese Berechtigung erhalten implizit die festen Serverrollen **sysadmin** und **serveradmin**.    
    
## <a name="examples"></a>Beispiele    
 Im folgenden Beispiel wird der obere Grenzwert für die Konfigurationsoption `recovery interval` auf `75` Minuten festgelegt und mithilfe von `RECONFIGURE WITH OVERRIDE` installiert. Wiederherstellungsintervalle über 60 Minuten werden nicht empfohlen und sind standardmäßig nicht zulässig. Da jedoch die Option `WITH OVERRIDE` angegeben wurde, wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht überprüft, ob es sich beim angegebenen Wert (`75`) um einen gültigen Wert für die Konfigurationsoption `recovery interval` handelt.    
    
```sql    
EXEC sp_configure 'recovery interval', 75    
RECONFIGURE WITH OVERRIDE;    
GO    
```    
    
## <a name="see-also"></a>Weitere Informationen    
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)     
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)    
    
  
