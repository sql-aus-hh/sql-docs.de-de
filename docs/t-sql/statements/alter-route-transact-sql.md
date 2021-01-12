---
description: ALTER ROUTE (Transact-SQL)
title: ALTER ROUTE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_ROUTE_TSQL
- ALTER ROUTE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ROUTE statement
- dropping routes
- modifying routes
- removing routes
- routes [Service Broker], modifying
ms.assetid: 8dfb7b16-3dac-4e1e-8c97-adf2aad07830
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 504f1eed0f50a711a671301b10001b44d6097330
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092038"
---
# <a name="alter-route-transact-sql"></a>ALTER ROUTE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Verändert die Routeninformationen für eine vorhandene Route in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ALTER ROUTE route_name  
WITH    
  [ SERVICE_NAME = 'service_name' [ , ] ]  
  [ BROKER_INSTANCE = 'broker_instance' [ , ] ]  
  [ LIFETIME = route_lifetime [ , ] ]  
  [ ADDRESS =  'next_hop_address' [ , ] ]  
  [ MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *route_name*  
 Der Name der zu ändernden Route. Server-, Datenbank- und Schemaname können nicht angegeben werden.  
  
 WITH  
 Führt die Klauseln ein, die die Route definieren, die gerade geändert wird.  
  
 SERVICE_NAME **='** _service\_name_ **'**  
 Gibt den Namen des Remotediensts an, auf den diese Route zeigt. Der *service_name* muss genau mit dem Namen übereinstimmen, der vom Remotedienst verwendet wird. [!INCLUDE[ssSB](../../includes/sssb-md.md)] führt einen bitweisen Vergleich mit der *service_name*-Zeichenfolge aus. Anders ausgedrückt: Bei dem Vergleich wird die Groß-/Kleinschreibung beachtet, die aktuelle Sortierung hingegen wird nicht berücksichtigt. Eine Route mit dem Dienstnamen **'SQL/ServiceBroker/BrokerConfiguration'** ist eine Route zu einem Broker-Konfigurationsdienst. Eine Route zu diesem Dienst kann keine Broker-Instanz angeben.  
  
 Wird die SERVICE_NAME-Klausel weggelassen, ändert sich der Dienstname für die Route nicht.  
  
 BROKER_INSTANCE **='** _broker\_instance_ **'**  
 Gibt die Datenbank an, auf der sich der Zieldienst befindet. Bei dem *broker_instance*-Parameter muss es sich um den Broker-Instanzbezeichner für die Remotedatenbank handeln, der durch das Ausführen der folgenden Abfrage in der ausgewählten Datenbank abgerufen werden kann:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 Wird die BROKER_INSTANCE-Klausel weggelassen, ändert sich die Broker-Instanz für die Route nicht.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
 LIFETIME **=** _route\_lifetime_  
 Gibt die Zeitspanne in Sekunden an, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Route in der Routingtabelle aufbewahrt. Am Ende ihrer Lebensdauer läuft die Route ab, und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berücksichtigt die Route nicht bei der Auswahl einer Route für eine neue Konversation. Wird die Klausel weggelassen, bleibt die Lebensdauer der Route unverändert.  
  
 ADDRESS **='** _next\_hop\_address_'  

 Für Azure SQL Managed Instance muss `ADDRESS` lokal sein.

 Gibt die Netzwerkadresse für diese Route an. *next_hop_address* gibt eine TCP/IP-Adresse mit folgendem Format an:  
  
 **TCP://** { *DNS_Name* | *NetBIOS_Name* |*IP_Adresse* } **:** *Portnummer*  
  
 Der angegebene Wert für *port_number* muss mit der Portnummer für den [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Endpunkt einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem angegebenen Computer übereinstimmen. Dieser kann durch Ausführen der folgenden Abfrage in der ausgewählten Datenbank abgerufen werden:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Gibt eine Route **'LOCAL'** für *next_hop_address* an, wird die Nachricht an einen Dienst innerhalb der aktuellen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übermittelt.  
  
 Gibt eine Route **'TRANSPORT'** für *next_hop_address* an, wird die Netzwerkadresse auf der Basis der Netzwerkadresse im Dienstnamen ermittelt. Eine Route, die **'TRANSPORT'** angibt, kann einen Dienstnamen oder eine Broker-Instanz angeben.  
  
 Wenn *next_hop_address* der Prinzipalserver für den Datenbankspiegel ist, müssen Sie für den Spiegelserver auch MIRROR_ADDRESS angeben. Andernfalls führt diese Route kein automatisches Failover zum Spiegelserver durch.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Gibt die Netzwerkadresse für den Spiegelserver eines gespiegelten Paars an, dessen Prinzipalserver die Adresse *next_hop_address* besitzt. *next_hop_mirror_address* gibt eine TCP/IP-Adresse mit folgendem Format an:  
  
 **TCP://** { *DNS_Name* | *NetBIOS_Name* | *IP_Adresse* } **:** *Portnummer*  
  
 Der angegebene Wert für *port_number* muss mit der Portnummer für den [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Endpunkt einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem angegebenen Computer übereinstimmen. Dieser kann durch Ausführen der folgenden Abfrage in der ausgewählten Datenbank abgerufen werden:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Wird MIRROR_ADDRESS angegeben, muss die Route die SERVICE_NAME-Klausel und die BROKER_INSTANCE-Klausel angeben. Eine Route, die **'LOCAL'** oder **'TRANSPORT'** für *next_hop_address* angibt, gibt möglicherweise keine Spiegeladresse an.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Routingtabelle, in der die Routen gespeichert werden, ist eine Metadatentabelle, die über die **sys.routes**-Katalogsicht gelesen werden kann. Die Routingtabelle kann nur mit der CREATE ROUTE-, ALTER ROUTE- und DROP ROUTE-Anweisung aktualisiert werden.  
  
 Klauseln, die im ALTER ROUTE-Befehl nicht angegeben werden, bleiben unverändert. Deshalb ist es nicht möglich, über ALTER festzulegen, dass für die Route kein Timeout gilt, dass die Route mit jedem Dienstnamen übereinstimmt oder dass die Route mit jeder Broker-Instanz übereinstimmt. Wenn Sie diese Merkmale einer Route ändern möchten, müssen Sie die vorhandene Route löschen und eine neue Route mit den neuen Informationen erstellen.  
  
 Gibt eine Route **'TRANSPORT'** für *next_hop_address* an, wird die Netzwerkadresse auf der Basis der Netzwerkadresse im Dienstnamen ermittelt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Dienstnamen erfolgreich verarbeiten, die mit einer Netzwerkadresse in einem für eine *next_hop_address* gültigen Format beginnen. Für Dienste mit Namen, die gültige Netzwerkadressen enthalten, erfolgt das Routing an die Netzwerkadresse im Dienstnamen.  
  
 Die Routingtabelle kann eine beliebige Anzahl von Routen enthalten, die den gleichen Dienst, die gleiche Netzwerkadresse und/oder Broker-Instanz-ID angeben. In diesem Fall wählt [!INCLUDE[ssSB](../../includes/sssb-md.md)] eine Route mithilfe einer Prozedur aus, die darauf ausgerichtet ist, eine möglichst genaue Übereinstimmung zwischen den in der Konversation angegebenen Informationen und den Informationen in der Routingtabelle festzustellen.  
  
 Verwenden Sie die ALTER AUTHORIZATION-Anweisung, wenn Sie AUTHORIZATION für einen Dienst ändern möchten.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Ändern einer Route erhalten standardmäßig der Besitzer der Route, Mitglieder der festen Datenbankrollen **db_ddladmin** oder **db_owner**  sowie Mitglieder der festen Serverrolle **sysadmin**.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-service-for-a-route"></a>A. Ändern des Diensts für eine Route  
 Im folgenden Beispiel wird die Route `ExpenseRoute` so geändert, dass sie auf den Remotedienst `//Adventure-Works.com/Expenses` zeigt.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     SERVICE_NAME = '//Adventure-Works.com/Expenses';  
```  
  
### <a name="b-changing-the-target-database-for-a-route"></a>B. Ändern der Zieldatenbank für eine Route  
 Im folgenden Beispiel wird die Zieldatenank für die Route `ExpenseRoute` in die Datenbank geändert, die durch folgenden eindeutigen Bezeichner angegeben wird: `D8D4D268-00A3-4C62-8F91-634B89B1E317.`  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317';  
```  
  
### <a name="c-changing-the-address-for-a-route"></a>C. Ändern der Adresse für eine Route  
 Im folgenden Beispiel wird die Netzwerkadresse für die Route `ExpenseRoute` in den TCP-Port `1234` auf dem Host mit der IP-Adresse `10.2.19.72` geändert.  
  
```sql  
ALTER ROUTE ExpenseRoute   
   WITH   
     ADDRESS = 'TCP://10.2.19.72:1234';  
```  
  
### <a name="d-changing-the-database-and-address-for-a-route"></a>D: Ändern der Datenbank und Adresse für eine Route  
 Im folgenden Beispiel wird die Netzwerkadresse für die Route `ExpenseRoute` in den TCP-Port `1234` auf dem Host mit dem DNS-Namen `www.Adventure-Works.com` geändert. Zudem wird die Zieldatenbank in die durch den eindeutigen Bezeichner `D8D4D268-00A3-4C62-8F91-634B89B1E317` identifizierte Datenbank geändert.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317',  
     ADDRESS = 'TCP://www.Adventure-Works.com:1234';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [DROP ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
