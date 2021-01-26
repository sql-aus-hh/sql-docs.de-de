---
description: Deaktivieren von Fremdschlüsseleinschränkungen für die Replikation
title: Deaktivieren von Fremdschlüsseleinschränkungen für die Replikation | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- constraints [SQL Server], foreign keys
- foreign keys [SQL Server], disabling constraints
- disabling constraints
ms.assetid: 4211f2fd-d16a-4081-995c-43f1f0827f0b
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 981983821435e17a8ab00b7c30ca1e676a960cb1
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766156"
---
# <a name="disable-foreign-key-constraints-for-replication"></a>Deaktivieren von Fremdschlüsseleinschränkungen für die Replikation
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Sie können Fremdschlüsseleinschränkungen für die Replikation in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]deaktivieren. Dies kann nützlich sein, wenn Sie Daten einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]veröffentlichen.  
  
> [!NOTE]  
>  Wird eine Tabelle mithilfe einer Replikation veröffentlicht, werden Fremdschlüsseleinschränkungen automatisch für die Operationen deaktiviert, die von Replikations-Agents ausgeführt werden. Wenn ein Replikations-Agent eine Einfügung, ein Update oder eine Löschung auf einem Abonnenten ausführt, wird die Einschränkung nicht überprüft; wenn ein Benutzer eine Einfügung, ein Update oder eine Löschung ausführt, wird die Einschränkung überprüft. Die Einschränkung wird für den Replikations-Agent deaktiviert, da die Einschränkung bereits auf dem Verleger überprüft wurde, als die Daten ursprünglich eingefügt, aktualisiert oder gelöscht wurden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **So deaktivieren Sie eine Fremdschlüsseleinschränkung für die Replikation mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Tabelle.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-disable-a-foreign-key-constraint-for-replication"></a>So deaktivieren Sie eine Fremdschlüsseleinschränkung für die Replikation  
  
1.  Erweitern Sie im **Objekt-Explorer** die Tabelle mit der Fremdschlüsseleinschränkung, die geändert werden soll, und erweitern Sie dann den Ordner **Schlüssel** .  
  
2.  Klicken Sie mit der rechten Maustaste auf die Fremdschlüsseleinschränkung, und klicken Sie anschließend auf **Ändern**.  
  
3.  Wählen Sie im Dialogfeld **Fremdschlüsselbeziehungen** den Wert **Nein** für **Für Replikation erzwingen** aus.  
  
4.  Klicken Sie auf **Schließen**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-disable-a-foreign-key-constraint-for-replication"></a>So deaktivieren Sie eine Fremdschlüsseleinschränkung für die Replikation  
  
1.  Um diesen Task in [!INCLUDE[tsql](../../includes/tsql-md.md)]auszuführen, löschen Sie die Fremdschlüsseleinschränkung. Fügen Sie dann eine neue Fremdschlüsseleinschränkung hinzu, und geben Sie die Option NOT FOR REPLICATION an.  
  
 Weitere Informationen finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="TsqlExample"></a>  
