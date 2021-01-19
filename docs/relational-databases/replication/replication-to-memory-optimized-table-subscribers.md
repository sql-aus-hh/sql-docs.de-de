---
description: Replikation mit Abonnenten von speicheroptimierten Tabellen
title: Replikation mit Abonnenten von speicheroptimierten Tabellen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/21/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
ms.assetid: 1a8e6bc7-433e-471d-b646-092dc80a2d1a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 71a245f6263b938cef7a3841b3570a3a9137a1db
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172202"
---
# <a name="replication-to-memory-optimized-table-subscribers"></a>Replikation mit Abonnenten von speicheroptimierten Tabellen
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Die Tabellen, die als Momentaufnahmen- und Transaktionsreplikationsabonnenten fungieren, können (mit Ausnahme der Peer-zu-Peer-Transaktionsreplikation) als speicheroptimierte Tabellen konfiguriert werden. Andere Replikationskonfigurationen sind mit speicheroptimierten Tabellen nicht kompatibel. Diese Funktion ist ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]verfügbar.  
  
## <a name="two-configurations-are-required"></a>Zwei Konfigurationen sind erforderlich.  
  
-   **Konfigurieren der Abonnentendatenbank für die Unterstützung der Replikation in speicheroptimierte Tabellen**  
  
     Legen Sie die **\@memory_optimized**-Eigenschaft auf **TRUE** fest, indem Sie [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) oder [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) verwenden.  
  
-   **Konfigurieren des Artikels für die Unterstützung der Replikation in speicheroptimierte Tabellen**  
  
     Legen Sie die `@schema_option = 0x40000000000`-Option für den Artikel mit [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) oder [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) fest.  
  
#### <a name="to-configure-a-memory-optimized-table-as-a-subscriber"></a>So konfigurieren Sie eine speicheroptimierte Tabelle als Abonnent  
  
1.  Erstellen Sie eine Transaktionsveröffentlichung. Weitere Informationen finden Sie unter [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Fügen Sie der Veröffentlichung Artikel hinzu. Weitere Informationen finden Sie unter [Definieren eines Artikels](../../relational-databases/replication/publish/define-an-article.md).  
  
     Falls Sie die Konfiguration mit [!INCLUDE[tsql](../../includes/tsql-md.md)] erfolgt, legen Sie den **\@schema_option**-Parameter der gespeicherten Prozedur **sp_addarticle** folgendermaßen fest:   
    **0x40000000000** verfügbar.  
  
3.  Legen Sie im Fenster mit den Artikeleigenschaften **Enable Memory optimization** auf **true** fest.  
  
4.  Starten Sie den Auftrag des Momentaufnahme-Agents, um die Anfangsmomentaufnahme für diese Veröffentlichung zu generieren. Weitere Informationen finden Sie unter [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  Erstellen Sie dann ein neues Abonnement. Legen Sie im **Assistenten für neue Abonnements****Memory Optimized Subscription** auf **true** fest.  

 Speicheroptimierte Tabellen empfangen nun Updates vom Verleger.  
  
#### <a name="reconfigure-an-existing-transaction-replication"></a>Neukonfiguration einer vorhandenen Transaktionsreplikation  
  
1.  Gehen Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] zu den Abonnementeigenschaften, und legen Sie **Memory Optimized Subscription** auf **true** fest. Die Änderungen werden erst nach der erneuten Initialisierung des Abonnements wirksam.  
  
     Falls Sie die Konfiguration mit [!INCLUDE[tsql](../../includes/tsql-md.md)] vornehmen, legen Sie den neuen **\@memory_optimized**-Parameter der gespeicherten Prozedur **sp_addsubscription** auf TRUE fest.  
  
2.  Gehen Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] zu den Artikeleigenschaften einer Veröffentlichung, und legen Sie **Enable Memory optimization** auf "true" fest.  
  
     Falls Sie die Konfiguration mit [!INCLUDE[tsql](../../includes/tsql-md.md)] erfolgt, legen Sie den **\@schema_option**-Parameter der gespeicherten Prozedur **sp_addarticle** folgendermaßen fest:   
    **0x40000000000** verfügbar.  
  
3.  Speicheroptimierte Tabellen unterstützen keine gruppierten Indizes. Daher müssen gruppierte Indizes bei der Replikation auf dem Ziel in nicht gruppierte Indizes konvertiert werden. Hierzu muss der Parameter **Convert clustered index to nonclustered for memory optimized article** auf "true" festgelegt werden.  
  
     Wenn Sie die Konfiguration mit [!INCLUDE[tsql](../../includes/tsql-md.md)] vornehmen, legen Sie den Parameter **\@schema_option** der gespeicherten Prozedur **sp_addarticle** auf **0xx0000080000000000** fest.  
  
4.  Generieren Sie die Momentaufnahme erneut.  
  
5.  Initialisieren Sie das Abonnement erneut.  
  
## <a name="remarks-and-restrictions"></a>Hinweise und Einschränkungen  
 Es wird nur die unidirektionale Transaktionsreplikation unterstützt. Die Peer-zu-Peer-Transaktionsreplikation wird nicht unterstützt.  
  
 Speicheroptimierte Tabellen können nicht veröffentlicht werden.  
  
 Replikationstabellen auf dem Verteiler können nicht als speicheroptimierte Tabellen konfiguriert werden.  
  
 Die Mergereplikation kann speicheroptimierte Tabellen einschließen.  
  
 Auf dem Abonnenten können die Tabellen, die bei einer Transaktionsreplikation berücksichtigt werden, als speicheroptimierte Tabellen konfiguriert werden. Die Abonnententabellen müssen jedoch die Anforderungen für speicheroptimierte Tabellen erfüllen. Dies erfordert folgende Einschränkungen.  
 
-   Tabellen, die mit speicheroptimierten Tabellen auf Abonnenten repliziert werden, sind auf die Datentypen beschränkt, die für speicheroptimierte Tabellen zulässig sind. Weitere Informationen finden Sie unter [Unterstützte Datentypen für In-Memory OLTP](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).  
  
-   Nicht alle Transact-SQL-Funktionen werden von speicheroptimierten Tabellen unterstützt. Weitere Einzelheiten finden Sie unter [Von In-Memory OLTP nicht unterstützte Transact-SQL-Konstrukte](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md) .  
  
##  <a name="modifying-a-schema-file"></a><a name="Schema"></a> Ändern einer Schemadatei  
  
-   Bei Verwendung der Option für speicheroptimierte Tabellen `DURABILITY = SCHEMA_AND_DATA` muss die Tabelle einen nicht gruppierten Primärschlüsselindex aufweisen.  
  
-   ANSI_PADDING muss auf ON festgelegt sein.  
  
  
  
