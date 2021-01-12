---
description: SET FIPS_FLAGGER (Transact-SQL)
title: SET FIPS_FLAGGER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FIPS_FLAGGER
- SET_FIPS_FLAGGER_TSQL
- FIPS_FLAGGER_TSQL
- SET FIPS_FLAGGER
dev_langs:
- TSQL
helpviewer_keywords:
- SET FIPS_FLAGGER statement
- FIPS 127-2 standard
- FIPS_FLAGGER option
ms.assetid: e82f6bee-6cf6-4061-be22-9ad2e8e9d3d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6e0fc2dedf045d08f077f7417bd70a8eaf926fcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093349"
---
# <a name="set-fips_flagger-transact-sql"></a>SET FIPS_FLAGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt an, dass die Kompatibilität mit dem FIPS 127-2-Standard überprüft wird. Diese basiert auf dem ISO-Standard. Informationen zur FIPS-Konformität mit SQL Server finden Sie unter [How to use SQL Server 2016 in FIPS 140-2-compliant mode (Verwendung von SQL Server 2016 in einem mit FIPS 140-2 konformen Modus)](https://support.microsoft.com/help/4014354/how-to-use-sql-server-2016-in-fips-140-2-compliant-mode). 
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SET FIPS_FLAGGER ( 'level' |  OFF )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 **'** *level* **'**  
 Der Grad der Kompatibilität mit dem FIPS 127-2-Standard, auf den alle Datenbankoperationen überprüft werden. Wenn bei einem Datenbankvorgang ein Konflikt mit der gewählten Stufe des ISO-Standards auftritt, generiert [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Warnung.  
  
 *level* muss einer der folgenden Werte sein.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|ENTRY|Überprüfen der Kompatibilität mit der Eingangsstufe (Entry level) des ISO-Standards.|  
|FULL|Überprüfen der vollständigen Kompatibilität mit dem ISO-Standard.|  
|INTERMEDIATE|Überprüfen der Kompatibilität mit der INTERMEDIATE-Stufe des ISO-Standards.|  
|OFF|Kein Überprüfen des Standards.|  
  
## <a name="remarks"></a>Hinweise  
 Die Einstellung von `SET FIPS_FLAGGER` wird zur Analysezeit und nicht zur Ausführungs- oder Laufzeit festgelegt. Das Festlegen zur Analysezeit bedeutet Folgendes: Wenn sich die SET-Anweisung im Batch oder in der gespeicherten Prozedur befindet, wird sie unabhängig davon wirksam, ob die Codeausführung tatsächlich diesen Punkt erreicht, und die `SET`-Anweisung wird wirksam, bevor Anweisungen ausgeführt werden. Auch wenn sich die `SET`-Anweisung z.B. in einem `IF...ELSE`-Anweisungsblock befindet, der während der Ausführung niemals erreicht wird, ist die `SET`-Anweisung dennoch wirksam, da der `IF...ELSE`-Anweisungsblock analysiert wird.  
  
 Wird `SET FIPS_FLAGGER` in einer gespeicherten Prozedur festgelegt, so wird der Wert von `SET FIPS_FLAGGER` wiederhergestellt, nachdem die gespeicherte Prozedur die Steuerung zurückgegeben hat. Daher hat eine `SET FIPS_FLAGGER`-Anweisung, die in einer dynamischem SQL-Anweisung angegeben wird, keine Auswirkung auf die Anweisungen, die der dynamischen SQL-Anweisung folgen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="see-also"></a>Siehe auch  
 [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
