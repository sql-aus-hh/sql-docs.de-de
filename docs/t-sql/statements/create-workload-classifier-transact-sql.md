---
description: CREATE WORKLOAD Classifier (Transact-SQL)
title: CREATE WORKLOAD CLASSIFIER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- CREATE WORKLOAD CLASSIFIER
- CREATE_WORKLOAD_CLASSIFIER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD CLASSIFIER statement
ms.assetid: ''
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 6a81c3164e183bb8a202e4ac537298c093e6d873
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644015"
---
# <a name="create-workload-classifier-transact-sql"></a>CREATE WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Erstellt ein Klassifiziererobjekt für die Verwendung in der Arbeitsauslastungsverwaltung.  Der Klassifizierer weist eingehende Anforderungen auf Grundlage der Parameter, die in der Definition der Klassifiziereranweisung angegeben sind, einer Arbeitsauslastungsgruppe zu.  Klassifizierer werden bei jeder eingereichten Anforderung ausgewertet.  Wenn eine Anforderung nicht einem Klassifizierer zugeordnet werden kann, wird sie der Standardarbeitsauslastungsgruppe zugeordnet.  Die Standardarbeitsauslastungsgruppe ist die Ressourcenklasse „smallrc“.

> [!NOTE]
> Der Arbeitsauslastungsklassifizierer ersetzt die „sp_addrolemember“-Ressourcenklassenzuweisung.  Führen Sie nach dem Erstellen von Arbeitsauslastungsklassifizierern „sp_droprolemember“ aus, um redundante Ressourcenklassenzuordnungen zu entfernen.

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax

```syntaxsql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
    (   WORKLOAD_GROUP = 'name'  
    ,   MEMBERNAME = 'security_account' 
[ [ , ] WLM_LABEL = 'label' ]  
[ [ , ] WLM_CONTEXT = 'context' ]  
[ [ , ] START_TIME = 'HH:MM' ]  
[ [ , ] END_TIME = 'HH:MM' ]  
  
[ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]) 
[;]
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumente

 *classifier_name*  
 Gibt den Namen an, mit dem der Arbeitsauslastungsklassifizierer identifiziert werden kann.  „classifier_name“ ist vom Datentyp „sysname“.  Dieses Argument kann bis zu 128 Zeichen lang sein und muss innerhalb der Instanz einen eindeutigen Namen haben.

 *WORKLOAD_GROUP* =  *'name'*    
 Wenn die Klassifiziererregeln die Bedingungen erfüllen, ordnet „name“ die Anforderung einer Arbeitsauslastungsgruppe zu.  „name“ ist vom Datentyp „sysname“.  Es kann bis zu 128 Zeichen lang sein und muss zum Zeitpunkt der Erstellung des Klassifizierers ein gültiger Arbeitsauslastungsgruppenname sein.

 Verfügbare Arbeitsauslastungsgruppen finden Sie in der Katalogansicht [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md).

 *MEMBERNAME* =  *'security_account'*     
 Das Sicherheitskonto, das für die Klassifizierung verwendet wird.  „Security_account“ ist vom Datentyp „sysname“und hat keinen Standardwert. „Security_account“ kann ein Datenbankbenutzer, eine Datenbankrolle, eine Azure Active Directory-Anmeldung oder eine Azure Active Directory-Gruppe sein.
 
 *WLM_LABEL*   
 Gibt den Bezeichnungswert an, mit dem eine Anforderung klassifiziert werden kann.  „Label“ ist ein optionaler Parameter vom Typ nvarchar(255).  Verwenden Sie [OPTION (LABEL)](/azure/sql-data-warehouse/sql-data-warehouse-develop-label) in der Anforderung, um der Klassifiziererkonfiguration zu entsprechen.

Beispiel:

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'  
 ,WLM_LABEL      = 'dimension_loads' )

SELECT COUNT(*) 
  FROM DimCustomer
  OPTION (LABEL = 'dimension_loads')
```

*WLM_CONTEXT*  
Gibt den Sitzungskontextwert an, mit dem eine Anforderung klassifiziert werden kann.  „Context“ ist ein optionaler Parameter vom Typ nvarchar(255).  Verwenden Sie [sys.sp_set_session_context](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) mit dem Variablennamen `wlm_context`, bevor Sie eine Anforderung zum Festlegen des Sitzungskontexts übermitteln.

Beispiel:

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoad WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'
 ,WLM_CONTEXT    = 'dim_load' )
 
--set session context
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = 'dim_load'

--run multiple statements using the wlm_context setting
SELECT COUNT(*) FROM stg.daily_customer_load
SELECT COUNT(*) FROM stg.daily_sales_load

--turn off the wlm_context session setting
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = null
```

*START_TIME* und *END_TIME*  
Gibt „start_time“ und „end_time“ an, mit denen eine Anforderung klassifiziert werden kann.  Sowohl „start_time“ als auch „end_time“ weisen das Format HH:MM in der UTC-Zeitzone auf.  „Start_time“ und „end_time“ müssen zusammen angegeben werden.

Beispiel:

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME     = 'ELTRole'  
 ,START_TIME     = '22:00'
 ,END_TIME       = '02:00' )
```

*IMPORTANCE* = {LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH}  
Gibt die relative Wichtigkeit einer Anforderung an.  Die Wichtigkeit kann einen der folgenden Werte aufweisen:

- LOW
- BELOW_NORMAL
- NORMAL (Standard)
- ABOVE_NORMAL
- HIGH  

Wenn Sie die Wichtigkeit nicht angeben, wird die Wichtigkeitseinstellung der Arbeitsauslastungsgruppe verwendet.  Die Standardwichtigkeit der Arbeitsauslastungsgruppe ist „normal“.  Die Priorität beeinflusst die Reihenfolge, in der Anforderungen geplant werden und somit zuerst Zugriff auf Ressourcen und Sperren erhalten.

## <a name="classification-parameter-weighting"></a>Gewichtung der Klassifizierungsparameter

Eine Anforderung kann mit mehreren Klassifizierern übereinstimmen.  Es gibt eine Gewichtung der Klassifizierungsparameter.  Der übereinstimmende Klassifizierer mit der höchsten Gewichtung wird zum Zuweisen einer Arbeitsauslastungsgruppe und der Wichtigkeit verwendet.  Die Gewichtung funktioniert wie folgt:

|Klassifizierungsparameter |Weight   |
|---------------------|---------|
|USER                 |64       |
|ROLE                 |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

Beachten Sie die folgenden Klassifiziererkonfigurationen.

```sql
CREATE WORKLOAD CLASSIFIER classifierA WITH  
( WORKLOAD_GROUP = 'wgDashboards'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = HIGH
 ,WLM_LABEL      = 'salereport' )

CREATE WORKLOAD CLASSIFIER classifierB WITH  
( WORKLOAD_GROUP = 'wgUserQueries'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = LOW
 ,START_TIME     = '18:00'
 ,END_TIME       = '07:00' )
```

Der Benutzer `userloginA` ist für beide Klassifizierer konfiguriert.  Wenn „userloginA“ zwischen 18:00 Uhr und 07:00 Uhr eine Abfrage mit der Bezeichnung `salesreport` ausführt, wird die Anforderung mit der Wichtigkeit „HIGH“ in die Arbeitsauslastungsgruppe „wgDashboards“ klassifiziert.  Es wird möglicherweise erwartet, dass die Anforderung außerhalb der Geschäftszeiten mit Wichtigkeit „LOW“ in „wgUserQueries“ klassifiziert wird. „WLM_LABEL“ weist jedoch eine höhere Gewichtung als „START_TIME/END_TIME“ auf.  Die Gewichtung von „classifierA“ ist 80 (64 für Benutzer und 16 für WLM_LABEL).  Die Gewichtung von „classiferB“ ist 68 (64 für Benutzer und 4 für START_TIME/END_TIME).  In diesem Fall können Sie WLM_LABEL zu „classifierB“ hinzufügen.

 Weitere Informationen finden Sie unter [Klassifizierung der Arbeitsauslastung](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification#classification-weighting).

## <a name="permissions"></a>Berechtigungen

 Erfordert die CONTROL DATABASE-Berechtigung.  
  
## <a name="examples"></a>Beispiele

 Das folgende Beispiel zeigt, wie ein Arbeitsauslastungsklassifizierer mit dem Namen `wgcELTRole` erstellt wird. Dieser verwendet die Arbeitsauslastungsgruppe „staticrc20“, den Benutzer `ELTRole` und legt die Priorität auf `above_normal` fest.

```sql
CREATE WORKLOAD CLASSIFIER wgcELTRole
  WITH (WORKLOAD_GROUP = 'staticrc20'
       ,MEMBERNAME = 'ELTRole'
      ,IMPORTANCE = above_normal);
```

## <a name="see-also"></a>Weitere Informationen

[Klassifizierung in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)</br>
[DROP WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-classifier-transact-sql.md)</br>
[sys.workload_management_workload_classifiers](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql.md)</br>
[sys.workload_management_workload_classifier_details](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql.md)

