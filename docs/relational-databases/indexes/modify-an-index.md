---
description: Ändern eines Indexes
title: Ändern eines Indexes | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], modifying
- modifying indexes
- index changes [SQL Server]
ms.assetid: 97e3110d-fde7-4f5d-9309-dc1697960aeb
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 0b653936dd08b5c124fd1249697dec35e9ae7ba9
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766310"
---
# <a name="modify-an-index"></a>Ändern eines Indexes
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  In diesem Thema wird beschrieben, wie Sie einen Index in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]ändern.  
  
> [!IMPORTANT]  
>  Indizes, die als Ergebnis einer PRIMARY KEY- oder UNIQUE-Einschränkung erstellt wurden, können mit dieser Methode nicht geändert werden. In diesem Fall muss die Einschränkung geändert werden.  
  
 **In diesem Thema**  
  
-   **Ändern eines Indexes mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-modify-an-index"></a>So ändern Sie einen Index  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **Datenbanken** und dann die Datenbank, zu der die Tabelle gehört, und klicken Sie anschließend auf **Tabellen**.  
  
3.  Erweitern Sie die Tabelle, in die der Index gehört, und erweitern Sie dann **Indizes**.  
  
4.  Klicken Sie mit der rechten Maustaste auf den Index, den Sie ändern möchten, und klicken Sie dann auf **Eigenschaften**.  
  
5.  Nehmen Sie im Dialogfeld **Indexeigenschaften** die gewünschten Änderungen vor. Sie können z. B. eine Spalte im Indexschlüssel hinzufügen oder entfernen oder die Einstellung einer Indexoption ändern.  
  
#### <a name="to-modify-index-columns"></a>So ändern Sie Indexspalten  
  
1.  Wenn Sie die Position einer Indexspalte hinzufügen, entfernen oder ändern möchten, wählen Sie im Dialogfeld **Indexeigenschaften** die Seite **Allgemein** aus.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-modify-an-index"></a>So ändern Sie einen Index  
  
Im folgenden Beispiel wird ein vorhandener Index für die `ProductID`-Spalte der `Production.WorkOrder`-Tabelle in der AdventureWorks-Datenbank mithilfe der Option `DROP_EXISTING` gelöscht und neu erstellt. Die Optionen `FILLFACTOR` und `PAD_INDEX` sind ebenfalls festgelegt.  
  
[!code-sql[IndexDDL#CreateIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_1.sql)]  
  
Im folgenden Beispiel werden mithilfe von ALTER INDEX mehrere Optionen für den `AK_SalesOrderHeader_SalesOrderNumber`-Index festgelegt.  
  
[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_2.sql)]  
  
#### <a name="to-modify-index-columns"></a>So ändern Sie Indexspalten  
  
1.  Wenn Sie die Position einer Indexspalte hinzufügen, entfernen oder ändern möchten, müssen Sie den Index löschen und neu erstellen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Festlegen von Indexoptionen](../../relational-databases/indexes/set-index-options.md)   
 [Umbenennen von Indizes](../../relational-databases/indexes/rename-indexes.md)  
  
  
