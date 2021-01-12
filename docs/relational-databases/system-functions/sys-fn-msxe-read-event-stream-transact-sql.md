---
description: sys.fn_MSxe_read_event_stream (Transact-SQL)
title: sys.fn_MSxe_read_event_stream (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
ms.assetid: 5edb1162-625a-41e0-8ec9-1edc8ab9a74a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 739d2b4298ec91064063acee702d1c4c6df5c4a0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101342"
---
# <a name="sysfn_msxe_read_event_stream-transact-sql"></a>sys.fn_MSxe_read_event_stream (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Erweiterte Ereignisse stellen eine Tabellenwertfunktion (Table Valued Function, TVF) zur internen Verwendung in der Benutzeroberfläche für erweiterte Ereignisse bereit. Die Tabelle enthält keine verwendbaren Kundendaten.  
  
 Sie können die Ereignisdaten mit einer der folgenden Methoden anzeigen:  
  
-   Benutzeroberfläche für eine neue Sitzung mit erweiterten Ereignissen.  
  
-   fn_xe_file_target_read_file TVF. Weitere Informationen finden Sie unter [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_MSxe_read_event_stream ( session_name)  
```  
  
## <a name="arguments"></a>Argumente  
 *Session_name*  
 Der Name einer Sitzung, die auf dem Server ausgeführt wird.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|type|**Integer (4)**|Der Ereignistyp. Lässt keine NULL-Werte zu.|  
|data|**Bild (16)**|Die Bilddaten des Ereignisses. Lässt NULL-Werte zu.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten für erweiterte Ereignisse](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Katalog Sichten für erweiterte Ereignisse &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)  
  
  
