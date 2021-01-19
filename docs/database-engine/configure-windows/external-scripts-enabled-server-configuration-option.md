---
title: Externe Skripts aktiviert – Serverkonfigurationsoption
description: Hier erfahren Sie mehr über die Option „Externe Skripts aktiviert“ in SQL Server. Wenn Sie die Option aktiviert haben, können Sie externe Skripts in unterstützten Sprachen wie R oder Python ausführen.
ms.date: 06/30/2020
ms.prod: sql
ms.technology: machine-learning-services
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- external scripts enabled
- external_scripts_enabled_TSQL
helpviewer_keywords:
- external scripts enabled option
ms.assetid: 9d0ce165-8719-4007-9ae8-00f85cab3a0d
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 90a24ba11ce482e8d8608e1641d64ace067ad41b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170972"
---
# <a name="external-scripts-enabled-server-configuration-option"></a>Externe Skripts aktiviert – Serverkonfigurationsoption
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Aktivieren Sie mit der Option **external scripts enabled** die Ausführung von Skripts mit bestimmten Remotespracherweiterungen. Diese Eigenschaft ist standardmäßig deaktiviert. Beim Setup kann diese Eigenschaft optional auf TRUE festgelegt werden, wenn **Machine Learning Services** installiert wird.

## <a name="remarks"></a>Bemerkungen

Bevor Sie mit der gespeicherten Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ein externes Skript ausführen können, müssen Sie die Option „external script enabled“ aktivieren. Verwenden Sie **sp_execute_external_script** zum Ausführen von Skripts, die in einer unterstützten Sprache wie R oder Python geschrieben sind. 

+ Für [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]

    [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] bietet Unterstützung für die Sprache R in [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und einen Satz von R-Arbeitsstationstools und -Verbindungsbibliotheken.

    Installieren Sie die Funktion **R Services** während des Setups von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], um die Ausführung von R-Skripts zu aktivieren.

+ Für [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] und höher:

    [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] unterstützt die Programmiersprachen R und Python.

    Installieren Sie die Funktion **Machine Learning Services** während des Setups von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], um die Ausführung externer Skripts zu aktivieren. Achten Sie darauf, dass Sie während der ersten Installation mindestens eine Sprache auswählen: R oder Python oder beides.
    
+ Für [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] und höher verfügt [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] über Unterstützung für alle Versionen von R, Python und Java sowie andere Drittanbietersprachen.

Installieren Sie das Feature „Machine Learning-Dienste und -Spracherweiterungen“ beim Einrichten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], um die Ausführung externer Skripts für alle unterstützten Sprachen zu aktivieren.

## <a name="additional-requirements"></a>Zusätzliche Anforderungen

Führen Sie nach der Installation das folgende Skript aus, um externe Skripts zu aktivieren:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

Weitere Informationen finden Sie unter [Installieren von SQL Server Machine Learning Services (Python und R) unter Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md) oder [Linux](../../linux/sql-server-linux-setup-machine-learning-docker.md?toc=/sql/machine-learning/toc.json).

## <a name="see-also"></a>Weitere Informationen

+ [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
+ [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)
+ [sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [SQL Machine Learning-Dokumentation](../../machine-learning/index.yml)
