---
title: xp_cmdshell (Serverkonfigurationsoption) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- xp_cmdshell
ms.assetid: c147c9e1-b81d-49c8-b800-3019f4d86a13
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ba002f0cf239e519c0ac4b7e3949dfdd96ed95b9
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47634248"
---
# <a name="xpcmdshell-server-configuration-option"></a>xp_cmdshell (Serverkonfigurationsoption)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Die **xp_cmdshell** -Option ist eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Serverkonfigurationsoption, mit der Systemadministratoren steuern können, ob auf einem System die erweiterte gespeicherte Prozedur **xp_cmdshell** ausgeführt werden kann. In neuen Installationen ist die **xp_cmdshell**-Option standardmäßig deaktiviert. Bevor Sie diese Option aktivieren, müssen Sie die möglichen Sicherheitsrisiken berücksichtigen, die mit der Verwendung dieser Option verbunden sind. In neu entwickeltem Code sollte diese Option nicht verwendet werden, weil sie grundsätzlich deaktiviert bleiben sollte. Für einige ältere Anwendungen muss sie aktiviert sein. Kann eine Anwendung nicht so geändert werden, dass auf ein Aktivieren dieser Option verzichtet werden kann, sollte die Option über die richtlinienbasierte Verwaltung oder durch Ausführen der gespeicherten Systemprozedur **sp_configure** aktiviert werden, wie dies im folgenden Codebeispiel gezeigt ist:  
  
```  
-- To allow advanced options to be changed.  
EXEC sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXEC sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Verwalten von Servern mit der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
  
