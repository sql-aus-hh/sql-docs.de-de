---
description: sys.fn_check_object_signatures (Transact-SQL)
title: sys.fn_check_object_signatures (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_check_object_signatures_TSQL
- fn_check_object_signatures_TSQL
- fn_check_object_signatures
- sys.fn_check_object_signatures
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_check_object_signatures function
ms.assetid: 47509566-d3d7-46a9-89c1-91b4895d56b9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4efb55bdeb3455c2c98278e8b90021c5cf194146
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099671"
---
# <a name="sysfn_check_object_signatures-transact-sql"></a>sys.fn_check_object_signatures (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Gibt eine Liste mit allen signierbaren Objekten zurück und gibt an, ob ein Objekt von einem angegebenen Zertifikat oder asymmetrischem Schlüssel signiert wird. Wenn das Objekt von dem angegebenen Zertifikat dem asymmetrischen Schlüssel signiert wird, gibt sie außerdem zurück, ob die Signatur des Objekts gültig ist.  
  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn_ check_object_signatures (   
    { '@class' } , { @thumbprint }   
  )   
```  
  
## <a name="arguments"></a>Argumente  
 { '\@ *Klasse*"}"  
 Identifiziert den Typ des Fingerabdrucks, der bereitgestellt wird:  
  
-   ‚Zertifikat’  
  
-   ‚asymmetrischer Schlüssel’  
  
 \@*Class* ist vom **Datentyp vom Datentyp sysname**.  
  
 { \@ *Fingerabdruck* }  
 SHA-1-Hash des Zertifikats, mit dem der Schlüssel verschlüsselt wird, oder der GUID des asymmetrischen Schlüssels, mit dem der Schlüssel verschlüsselt wird. \@*Fingerabdruck* ist vom Datentyp **varbinary (20)**.  
  
## <a name="tables-returned"></a>Zurückgegebene Tabellen  
 In der folgenden Tabelle werden die Spalten aufgelistet, die **fn_check_object_signatures** zurückgibt.  
  
|Spalte|Typ|BESCHREIBUNG|  
|------------|----------|-----------------|  
|type|**nvarchar(120)**|Gibt die Typbeschreibung oder -assembly zurück.|  
|entity_id|**int**|Gibt die Objekt-ID des Objekts zurück, das ausgewertet wird.|  
|is_signed|**int**|Gibt 0 zurück, wenn das Objekt nicht durch den bereitgestellten Fingerabdruck signiert wird. Gibt 1 zurück, wenn das Objekt durch den bereitgestellten Fingerabdruck signiert wird.|  
|is_signature_valid|**int**|Wenn der is_signed-Wert 1 ist, wird 0 zurückgegeben, wenn die Signatur nicht gültig ist. Gibt den Wert 1 zurück, wenn die Signatur gültig ist.<br /><br /> Wenn der is_signed-Wert 0 ist, wird immer 0 zurückgegeben.|  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie **fn_check_object_signatures** , um zu bestätigen, dass die Objekte von böswilligen Benutzern nicht manipuliert wurden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DEFINITION-Berechtigung für das Zertifikat oder den asymmetrischen Schlüssel.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird das Schemasignaturzertifikat für die `master`-Datenbank gefunden und der `is_signed`-Wert von 1 und der `is_signature_valid`-Wert von 1 für diejenigen Objekte zurückgegeben, die durch das Schemasignaturzertifikat signiert werden und die gültige Signaturen aufweisen.  
  
```  
USE master;  
-- Declare a variable to hold the thumbprint.  
DECLARE @thumbprint varbinary(20) ;  
-- Populate the thumbprint variable with the master database schema signing certificate.  
SELECT @thumbprint = thumbprint   
FROM sys.certificates   
WHERE name LIKE '%SchemaSigningCertificate%' ;  
-- Evaluates the objects signed by the schema signing certificate  
SELECT type, entity_id, OBJECT_NAME(entity_id) AS [object name], is_signed, is_signature_valid  
FROM sys.fn_check_object_signatures ('certificate', @thumbprint) ;  
GO  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [IS_OBJECTSIGNED &#40;Transact-SQL&#41;](../../t-sql/functions/is-objectsigned-transact-sql.md)  
  
  
