---
title: 'Hochverfügbarkeit: In-Memory OLTP-Datenbanken'
description: SQL Server-Datenbanken mit speicheroptimierten Tabellen mit oder ohne nativ kompilierte gespeicherte Prozeduren werden mit Always On-Verfügbarkeitsgruppen vollständig unterstützt.
ms.custom: seo-dt-2019
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2113a916-3b1e-496c-8650-7f495e492510
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b275387efa5cc44b012cccef82fb3284e594abb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170542"
---
# <a name="high-availability-support-for-in-memory-oltp-databases"></a>Unterstützung für Hochverfügbarkeit für In-Memory OLTP-Datenbanken
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Datenbanken mit speicheroptimierten Tabellen mit bzw. ohne systemeigene kompilierte gespeicherte Prozeduren werden mit AlwaysOn-Verfügbarkeitsgruppen vollständig unterstützt.  Es gibt keinen Unterschied bei der Konfiguration und Unterstützung von Datenbanken mit [!INCLUDE[hek_2](../../includes/hek-2-md.md)]-Objekten oder ohne diese Objekte.  

 Änderungen an speicheroptimierten Tabellen im primären Replikat werden während der Rollforwardphase auf die Tabellen im sekundären Replikat angewendet. So ist ein schnelles Failover auf das sekundäre Replikat möglich, weil sich die Daten bereits im Arbeitsspeicher befinden. Tabellen sind für Leseabfragen in sekundären Replikaten verfügbar, die für den Lesezugriff konfiguriert wurden.  

  
## <a name="always-on-availability-groups-and-in-memory-oltp-databases"></a>AlwaysOn-Verfügbarkeitsgruppen und In-Memory OLTP-Datenbanken  
 Die Konfiguration von Datenbanken mit [!INCLUDE[hek_2](../../includes/hek-2-md.md)]-Komponenten bietet folgende Vorteile:  
  
-   **Eine vollständig integrierte Lösung**   
    Sie können Ihre Datenbanken mit speicheroptimierten Tabellen unter Verwendung des gleichen Assistenten mit der gleichen Ebene der Unterstützung für synchrone und asynchrone sekundäre Replikate konfigurieren. Darüber hinaus erfolgt die Systemüberwachung über das vertraute AlwaysOn-Dashboard in SQL Server Management Studio.  
  
-   **Vergleichbare Failoverzeit**   
    Sekundäre Replikate behalten den im Speicher enthaltenen Status der dauerhaften speicheroptimierten Tabellen. Bei einem automatischen oder erzwungenen Failover ist die für das Failover auf das neue primäre Replikat erforderliche Zeit mit der für datenträgerbasierte Tabellen vergleichbar, da keine Wiederherstellung notwendig ist. Speicheroptimierte Tabellen, die als SCHEMA_ONLY erstellt wurden, werden in dieser Konfiguration unterstützt. Änderungen an diesen Tabellen werden jedoch nicht protokolliert. Deshalb sind in diesen Tabellen auf dem sekundären Replikat keine Daten vorhanden.  
  
-   **Lesbares sekundäres Replikat**   
    Sie können auf speicheroptimierte Tabellen im sekundären Replikat zugreifen und sie abfragen, wenn es für den Lesezugriff konfiguriert wurde. In [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ist der Lesezeitstempel im sekundären Replikat eng mit dem Lesezeitstempel im primären Replikat synchronisiert. Änderungen am primären Replikat sind daher schnell im sekundären Replikat sichtbar. Diese enge Synchronisierung unterscheidet sich von In-Memory OLTP von [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

### <a name="considerations"></a>Überlegungen

- In SQL Server 2019 wurde eine parallele Rollforwardphase für arbeitsspeicheroptimierte Datenbanken in Verfügbarkeitsgruppen eingeführt. In SQL Server 2016 und 2017 verwenden datenträgerbasierte Tabellen keine parallele Rollforwardphase, wenn eine Datenbank in einer Verfügbarkeitsgruppe ebenfalls arbeitsspeicheroptimiert ist. 
  
## <a name="failover-clustering-instance-fci-and-in-memory-oltp-databases"></a>Failoverclustering-Instanz (FCI) und In-Memory OLTP-Datenbanken  
 Um in einer Konfiguration mit freigegebenem Speicher Hochverfügbarkeit zu erreichen, können Sie eine Failoverclusterinstanz mit Datenbanken einrichten, die speicheroptimierte Tabellen verwenden. Beim Einrichten einer FCI müssen Sie die folgenden Faktoren berücksichtigen:  
  
-   **Wiederherstellungszeit-Zielsetzung**   
    Die Failoverzeit wird wahrscheinlich höher sein, da speicheroptimierte Tabellen in den Arbeitsspeicher geladen werden müssen, bevor die Datenbank verfügbar gemacht wird.  
  
-   **SCHEMA_ONLY-Tabellen**   
    Beachten Sie, dass SCHEMA_ONLY-Tabellen nach dem Failover ohne Zeilen, also leer sind. Dies ist so von der Anwendung definiert. Es ist genau das gleiche Verhalten wie beim Neustart einer [!INCLUDE[hek_2](../../includes/hek-2-md.md)] -Datenbank mit SCHEMA_ONLY-Tabellen.  
  
## <a name="support-for-transaction-replication-in-in-memory-oltp"></a>Unterstützung für Transaktionsreplikation in In-Memory OLTP  
 Die Tabellen, die als Transaktionsreplikationsabonnenten fungieren, können (mit Ausnahme der Peer-zu-Peer-Transaktionsreplikation) als speicheroptimierte Tabellen konfiguriert werden. Andere Replikationskonfigurationen sind mit speicheroptimierten Tabellen nicht kompatibel.  Weitere Informationen finden Sie unter [Replikation mit Abonnenten von speicheroptimierten Tabellen](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always On-Verfügbarkeitsgruppen (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Aktive sekundäre Replikate: Lesbare sekundäre Replikate (Always On-Verfügbarkeitsgruppen)](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Replikation mit Abonnenten von speicheroptimierten Tabellen](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)  
  
  
