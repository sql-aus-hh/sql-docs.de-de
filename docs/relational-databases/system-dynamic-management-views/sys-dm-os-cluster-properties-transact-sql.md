---
description: sys.dm_os_cluster_properties (Transact-SQL)
title: sys.dm_os_cluster_properties (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 98e5e9b90449836a4370771c35e12ea82007aed6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099750"
---
# <a name="sysdm_os_cluster_properties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile mit den aktuellen Einstellungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clusterressourceneigenschaften zurück, die in diesem Thema angegeben wurden. Wenn diese Sicht in einer eigenständigen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird, werden keine Daten zurückgegeben.  
  
 Mit diesen Eigenschaften werden die Werte festgelegt, die sich auf die Fehlererkennung, Fehlerantwortzeit und die Protokollierung zum Überwachen des Integritätsstatus der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz auswirken.  
  

|Spaltenname|Eigenschaft|BESCHREIBUNG|  
|-----------------|--------------|-----------------|  
|VerboseLogging|BIGINT|Der Protokolliergrad für den SQL Server-Failovercluster. Die ausführliche Protokollierung kann aktiviert werden, um in den Fehlerprotokollen weitere Details für die Problembehandlung bereitzustellen. Einer der folgenden Werte:<br /><br /> 0: Die Protokollierung ist deaktiviert (Standard)<br /><br /> 1 – Nur Fehler<br /><br /> 2: Fehler und Warnungen<br /><br /> Weitere Informationen finden Sie unter [Alter Server Configuration &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).|  
|SqlDumperDumpFlags|BIGINT|SQLDumper-Dumpflags bestimmen den Typ der mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generierten Dumpdateien. Die Standardeinstellung ist 0.|  
|SqlDumperDumpPath|nvarchar(260)|Der Speicherort, an dem das Hilfsprogramm SQLDumper die Dumpdateien generiert.|  
|SqlDumperDumpTimeOut|BIGINT|Der Timeoutwert in Millisekunden für die Generierung eines Dumps durch das Hilfsprogramm SQLDumper bei einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehler. Der Standardwert ist 0.|  
|FailureConditionLevel|BIGINT|Legt die Bedingungen fest, unter denen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failovercluster einen Fehler verursacht oder einen Neustart durchführt. Der Standardwert ist 3. Eine ausführliche Erläuterung oder zum Ändern der Eigenschaften Einstellungen finden Sie unter [Konfigurieren von failureconditionlevel-Eigenschafts Einstellungen](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).|  
|HealthCheckTimeout|BIGINT|Der Timeoutwert, der festlegt, wie lange die Ressourcen-DLL der SQL Server-Datenbank-Engine auf Informationen über den Serverzustand warten soll, bevor eine Instanz von SQL Server als nicht reagierend eingestuft wird. Der Timeoutwert wird in Millisekunden angegeben. Der Standardwert ist 60000. Weitere Informationen oder zum Ändern dieser Eigenschafts Einstellung finden Sie unter [Konfigurieren der healthchecktimeout-Eigenschafts Einstellungen](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md).|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert VIEW SERVER STATE-Berechtigungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden die Eigenschafteneinstellungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterressource mithilfe von sys.dm_os_cluster_properties zurückgegeben.  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 Hier ist ein Beispielresultset.  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|NULL|0|3|60000|  
  
  
