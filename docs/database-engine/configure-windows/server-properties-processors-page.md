---
title: Servereigenschaften (Seite „Prozessoren“)
description: Informationen zu Prozessoreinstellungen in SQL Server Hier erfahren Sie, welche Optionen die Anzahl der Arbeitsthreads, die Prozessorzuweisung und andere Eigenschaften steuern.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, matteot
ms.custom: ''
ms.date: 12/17/2020
ms.openlocfilehash: 874cbbae2b418e9b9e06c7a95d62d34a99af38f5
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684216"
---
# <a name="server-properties-processors-page"></a>Servereigenschaften (Seite Prozessoren)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Auf dieser Seite können Sie die Prozessoroptionen anzeigen und ändern. Prozessoraffinitätsoptionen sind nur aktiviert, wenn mehr als ein Prozessor installiert ist.  

## <a name="options"></a>Tastatur

### <a name="processor-affinity"></a>Prozessoraffinität
Weist Prozessoren bestimmte Threads zu, um ein Neuladen des Prozessors zu verhindern und die Threadmigration zwischen Prozessoren zu reduzieren. Weitere Informationen finden Sie unter [Affinitätsmaske (Serverkonfigurationsoption)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

### <a name="io-affinity"></a>E/A-Affinität
Verbindet die Datenträger-E/A von [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server mit einer angegebenen Teilmenge von CPUs. Weitere Informationen finden Sie unter [Affinity I/O Mask (Serverkonfigurationsoption)](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md).

### <a name="automatically-set-processor-affinity-mask-for-all-processors"></a>Prozessor-Affinitätsmaske für alle Prozessoren automatisch festlegen
Ermöglicht SQL Server das Festlegen der Prozessoraffinität.

### <a name="automatically-set-io-affinity-mask-for-all-processors"></a>E/A-Affinitätsmaske für alle Prozessoren automatisch festlegen
Ermöglicht SQL Server das Festlegen der E/A-Affinität.

### <a name="maximum-worker-threads"></a>Maximale Arbeitsthreadanzahl
0 ermöglicht es SQL Server, die Anzahl der Arbeitsthreads dynamisch festzulegen. Diese Einstellung ist für die meisten Systeme am besten geeignet. In Abhängigkeit von der Systemkonfiguration kann jedoch durch Festlegen dieser Option auf einen bestimmten Wert manchmal die Leistung verbessert werden. Weitere Informationen finden Sie unter [konfigurieren Sie die max Worker Threads Server Configuration Option](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).  

### <a name="boost-sql-server-priority"></a>SQL Server-Priorität höher stufen
Gibt an, ob SQL Server unter Microsoft Windows mit höherer Planungspriorität als andere Prozesse auf demselben Computer ausgeführt werden soll. Weitere Informationen finden Sie unter [Configure the priority boost Server Configuration Option](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).  

> [!Note]
> Diese Option ist für SSMS 18.x und höhere Versionen nicht verfügbar.

### <a name="use-windows-fibers-lightweight-pooling"></a>Windows-Fibers verwenden ('Lightweightpooling')
Verwendet Windows-Fibers für den SQL Server-Dienst statt Threads. Diese Option ist nur unter Windows 2003 Server Edition verfügbar. Weitere Informationen finden Sie unter [Lightweightpooling (Serverkonfigurationsoption)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

> [!Note]
> Diese Option ist für SSMS 18.x und höhere Versionen nicht verfügbar.

### <a name="configured-values"></a>Konfigurierte Werte
Zeigt für die Optionen in diesem Bereich konfigurierte Werte an. Wenn Sie diese Werte ändern, wählen Sie anschließend **Ausgeführte Werte** aus, um zu sehen, ob die Änderungen wirksam sind. Sind sie nicht wirksam, muss die Instanz von SQL Server zuerst neu gestartet werden.

### <a name="running-values"></a>Ausgeführte Werte
Zeigt die gegenwärtig ausgeführten Werte für die Optionen in diesem Bereich an. Diese Werte sind schreibgeschützt.

## <a name="see-also"></a>Weitere Informationen
[Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  


