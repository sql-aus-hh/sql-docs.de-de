---
description: SET STATISTICS XML (Transact-SQL)
title: SET STATISTICS XML (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_XML_TSQL
- SET STATISTICS XML
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- STATISTICS XML option
- SET STATISTICS XML statement
- statements [SQL Server], statistical information
- XML [SQL Server], statement execution information
ms.assetid: 2b6d4c5a-a7f5-4dd1-b10a-7632265b1af7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b99a042e889c2de3baa5c544e2255a1f8bceb855
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079746"
---
# <a name="set-statistics-xml-transact-sql"></a>SET STATISTICS XML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Bewirkt, dass Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen ausführt und weitere Informationen zur Ausführung der Anweisungen in Form eines definierten XML-Dokuments generiert.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET STATISTICS XML { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Hinweise
 Die Einstellung von SET STATISTICS XML wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.  
  
 Wenn SET STATISTICS XML auf ON festgelegt ist, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Ausführungsinformationen für jede Anweisung nach deren Ausführung zurück. Nachdem diese Option auf ON festgelegt wird, werden Informationen zu allen weiteren [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen zurückgegeben, bis die Option auf OFF festgelegt wird. SET STATISTICS XML muss nicht die einzige Anweisung in einem Batch sein.  
  
 SET STATISTICS XML gibt eine Ausgabe als **nvarchar(max)** für Anwendungen zurück, wie z.B. das Dienstprogramm **sqlcmd**, wobei die XML-Ausgabe nachfolgend von weiteren Tools für die Anzeige und Verarbeitung der Abfrageplaninformationen verwendet wird.  
  
 SET STATISTICS XML gibt Informationen als eine Gruppe von XML-Dokumenten zurück. Jede Anweisung nach der SET STATISTICS XML ON-Anweisung ist in der Ausgabe als einzelnes Dokument enthalten. Jedes Dokument enthält den Text der Anweisung, gefolgt von den Informationen zu den Ausführungsschritten. Die Ausgabe zeigt Laufzeitinformationen an, wie z. B. Kosten, zugegriffene Indizes und Typen der ausgeführten Vorgänge, Joinreihenfolge, Anzahl von Ausführungen eines physischen Vorgangs sowie die Anzahl der von einem physischen Operator erstellten Zeilen usw.  
  
 Das Dokument mit dem XML-Schema für die XML-Ausgabe von SET STATISTICS XML wird beim Setup in ein lokales Verzeichnis auf dem Computer kopiert, auf dem Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist. Es wird auf dem Laufwerk mit den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installationsdateien gespeichert unter:  
  
 \Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd  
  
 Das Showplanschema kann auf [dieser Website](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409) gefunden werden.  
  
 SET STATISTICS PROFILE und SET STATISTICS XML sind Gegenstücke zueinander. SET STATISTICS PROFILE erstellt Textausgaben, während SET STATISTICS XML XML-Ausgaben erstellt. In künftigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen werden neue Informationen zum Abfrageausführungsplan nur über die SET STATISTICS XML-Anweisung und nicht die SET STATISTICS PROFILE-Anweisung angezeigt.  
  
> [!NOTE]  
>  Wenn **Tatsächlichen Ausführungsplan einschließen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ausgewählt ist, generiert diese SET-Option keine XML-Showplanausgabe mehr. Deaktivieren Sie das Kontrollkästchen **Tatsächlichen Ausführungsplan einschließen**, bevor Sie diese SET-Option verwenden.  
  
## <a name="permissions"></a>Berechtigungen  
 Für die Verwendung von SET STATISTICS XML und die Anzeige der Ausgabe benötigen Benutzer die folgenden Berechtigungen:  
  
-   Entsprechende Berechtigungen zum Ausführen der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen.  
  
-   Die SHOWPLAN-Berechtigung für alle Datenbanken mit Objekten, auf die von den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen verwiesen wird.  
  
 Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die keine STATISTICS XML-Resultsets erstellen, werden nur die Berechtigungen zum Ausführen der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen benötigt. Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die STATISTICS XML-Resultsets erstellen, werden sowohl die Ausführungsberechtigung für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen als auch die SHOWPLAN-Berechtigung benötigt, da die Ausführung der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung andernfalls abgebrochen und keine Showplaninformationen generiert werden.  
  
## <a name="examples"></a>Beispiele  
 In den beiden folgenden Anweisungen werden die SET STATISTICS XML-Einstellungen verwendet, um zu zeigen, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Verwendung von Indizes in Abfragen analysiert und optimiert. In der ersten Abfrage wird der Vergleichsoperator Gleich (=) in der WHERE-Klausel auf eine indizierte Spalte angewendet. In der zweiten Abfrage wird der LIKE-Operator in der WHERE-Klausel verwendet. Deshalb muss [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Scan des gruppierten Indexes durchführen, um die Daten zu finden, die die Bedingung in der WHERE-Klausel erfüllen. Die Werte in den Attributen **EstimateRows** und **EstimatedTotalSubtreeCost** sind bei der ersten indizierten Abfrage kleiner, was auf eine deutlich schnellere Verarbeitung und die Verwendung weniger Ressourcen als bei der nicht indizierten Abfrage hinweist.  
  
```sql
USE AdventureWorks2012;  
GO  
SET STATISTICS XML ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, JobTitle   
FROM HumanResources.Employee  
WHERE JobTitle LIKE 'Production%';  
GO  
SET STATISTICS XML OFF;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET SHOWPLAN_XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-xml-transact-sql.md)   
 [SQLCMD-Hilfsprogramm](../../tools/sqlcmd-utility.md)  
  
  
