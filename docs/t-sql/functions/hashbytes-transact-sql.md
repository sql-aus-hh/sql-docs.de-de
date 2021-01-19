---
description: HASHBYTES (Transact-SQL)
title: HASHBYTES (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da6f1b1bdb357a6321207720d2ea64938f4ada41
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170242"
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt den MD2-, MD4-, MD5-, SHA-, SHA1- oder SHA2-Hash der zugehörigen Eingabe in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

`<algorithm>`  
Identifiziert den für das Hashing der Eingabe zu verwendenden Hashalgorithmus. Dies ist ein erforderliches Argument ohne Standardwert. Die einfachen Anführungszeichen müssen eingegeben werden. Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] gelten alle anderen Algorithmen als SHA2_256 und SHA2_512 als veraltet.  
  
`@input`  
Gibt eine Variable mit den Daten an, für die das Hashing ausgeführt werden soll. `@input` ist **varchar**, **nvarchar** oder **varbinary**.  
  
*input*  
Gibt einen Ausdruck an, der zu einer Zeichenfolge oder Binärzeichenfolge ausgewertet wird, für die das Hashing ausgeführt werden soll.  
  
 Die Ausgabe entspricht dem Algorithmusstandard: 128 Bits (16 Bytes) für MD2, MD4 und MD5; 160 Bits (20 Bytes) für SHA und SHA1; 256 Bits (32 Bytes) für SHA2_256 und 512 Bits (64 Bytes) für SHA2_512.  
  
**Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher
  
 Bei [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und früher sind zulässige Eingabewerte auf 8.000 Byte beschränkt.  
  
## <a name="return-value"></a>Rückgabewert  
 **varbinary** (maximal 8.000 Byte)  

## <a name="remarks"></a>Hinweise  
Erwägen Sie die Verwendung von `CHECKSUM` oder `BINARY_CHECKSUM` als Alternativen zur Berechnung eines Hashwerts.

Die Algorithmen MD2, MD4, MD5, SHA und SHA1 sind ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] veraltet. Verwenden Sie stattdessen SHA2_256 oder SHA2_512. Ältere Algorithmen funktionieren weiterhin, lösen jedoch ein Ereignis aus, das auf die Veraltung hinweist.

## <a name="examples"></a>Beispiele  
### <a name="return-the-hash-of-a-variable"></a>Zurückgeben des Hashcodes einer Variablen  
 Im folgenden Beispiel wird der `SHA2_256`-Hash der in der `@HashThis`-Variablen gespeicherten **nvarchar**-Daten zurückgegeben.  
  
```sql  
DECLARE @HashThis NVARCHAR(32);  
SET @HashThis = CONVERT(NVARCHAR(32),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA2_256', @HashThis);  
```  
  
### <a name="return-the-hash-of-a-table-column"></a>Zurückgeben des Hashcodes einer Tabellenspalte  
 Im folgenden Beispiel wird der SHA2_256-Hash der Werte in der Spalte `c1` der Tabelle `Test1` zurückgegeben.  
  
```sql  
CREATE TABLE dbo.Test1 (c1 NVARCHAR(32));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA2_256', c1) FROM dbo.Test1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------------  
0x741238C01D9DB821CF171BF61D72260B998F7C7881D90091099945E0B9E0C2E3 
0x91DDCC41B761ACA928C62F7B0DA61DC763255E8247E0BD8DCE6B22205197154D  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Auswählen eines Verschlüsselungsalgorithmus](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
