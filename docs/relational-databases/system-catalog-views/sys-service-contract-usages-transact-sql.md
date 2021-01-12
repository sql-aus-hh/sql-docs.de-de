---
description: sys.service_contract_usages (Transact-SQL)
title: sys.service_contract_usages (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- service_contract_usages
- sys.service_contract_usages
- sys.service_contract_usages_TSQL
- service_contract_usages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_contract_usages catalog view
ms.assetid: 20af425e-1152-4a46-b1ac-94cff5fc9f02
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0a45631f2abc9db25428cd095e50aa1add456b08
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096740"
---
# <a name="sysservice_contract_usages-transact-sql"></a>sys.service_contract_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Diese Katalogsicht enthält eine Zeile pro (Dienst-, Vertrags-) Paar.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**service_id**|**int**|Bezeichner des Diensts, der den Vertrag verwendet. Lässt keine NULL-Werte zu.|  
|**service_contract_id**|**int**|Bezeichner des vom Dienst verwendeten Vertrags. Lässt keine NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
