---
description: DROP ASSEMBLY (Transact-SQL)
title: DROP ASSEMBLY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP ASSEMBLY
- DROP_ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing assemblies
- DROP ASSEMBLY statement
- deleting assemblies
- assemblies [CLR integration], removing
- dropping assemblies
- WITH NO DEPENDENTS option
ms.assetid: 452d181a-a8e6-44a3-975d-29966d01b18d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8dba825de1f507e736761a24cab70ed6520101ad
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171242"
---
# <a name="drop-assembly-transact-sql"></a>DROP ASSEMBLY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Entfernt eine Assembly und alle zugehörigen Dateien aus der aktuellen Datenbank. Assemblys werden mithilfe von [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md) erstellt und mithilfe von [ALTER ASSEMBLY](../../t-sql/statements/alter-assembly-transact-sql.md) geändert.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP ASSEMBLY [ IF EXISTS ] assembly_name [ ,...n ]  
[ WITH NO DEPENDENTS ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Löscht die Assembly nur, wenn diese bereits vorhanden ist.  
  
 *assembly_name*  
 Der Name der zu löschenden Assembly.  
  
 WITH NO DEPENDENTS  
 Wird dieser Wert angegeben, wird nur *assembly_name* gelöscht, jedoch keine der abhängigen Assemblys, auf die die Assembly verweist. Wenn kein Wert angegeben wird, löscht DROP ASSEMBLY *assembly_name* sowie alle abhängigen Assemblys.  
  
## <a name="remarks"></a>Bemerkungen  
 Durch Löschen einer Assembly werden die Assembly sowie alle zugehörigen Dateien, wie Quellcode und Debugdateien, aus der Datenbank entfernt.  
  
 Wenn WITH NO DEPENDENTS nicht angegeben wird, löscht DROP ASSEMBLY *assembly_name* sowie alle abhängigen Assemblys. Schlägt der Versuch fehl, alle abhängigen Assemblys zu löschen, gibt DROP ASSEMBLY einen Fehler zurück.  
  
 DROP ASSEMBLY gibt einen Fehler zurück, wenn eine andere Assembly in der Datenbank auf die Assembly verweist oder diese von CLR-basierten Funktionen (Common Language Runtime), Prozeduren, Triggern, benutzerdefinierten Typen oder Aggregaten in der aktuellen Datenbank verwendet wird.  
  
 DROP ASSEMBLY schränkt Code nicht ein, der auf die zurzeit ausgeführte Assembly verweist. Nach der Ausführung von DROP ASSEMBLY wird jedoch bei allen Versuchen, den Assemblycode aufzurufen, ein Fehler erzeugt.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert den Besitz der Assembly oder die CONTROL-Berechtigung für die Assembly.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird davon ausgegangen, dass die Assembly `HelloWorld` bereits in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz erstellt wurde.  
  
```sql  
DROP ASSEMBLY Helloworld ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Abrufen von Informationen zu Assemblys](../../relational-databases/clr-integration/assemblies-getting-information.md)  
  
  
