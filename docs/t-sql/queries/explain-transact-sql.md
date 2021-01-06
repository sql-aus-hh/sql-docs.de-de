---
description: EXPLAIN (Transact-SQL)
title: EXPLAIN (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: conceptual
ms.assetid: 4846a576-57ea-4068-959c-81e69e39ddc1
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: dd8efdcb33f29d70280e356ced6e769c0528dd00
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644014"
---
# <a name="explain-transact-sql"></a>EXPLAIN (Transact-SQL) 

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

  Dieser Befehl gibt den Abfrageplan für eine [!INCLUDE[ssDW](../../includes/ssdw-md.md)] [!INCLUDE[DWsql](../../includes/dwsql-md.md)]-Anweisung zurück, ohne die Anweisung auszuführen. Verwenden Sie EXPLAIN, um eine Vorschau der Vorgänge zu erhalten, die Datenverschiebungen erfordern, und um die ungefähren Kosten der Abfragevorgänge anzuzeigen. `WITH RECOMMENDATIONS` gilt für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
EXPLAIN [WITH_RECOMMENDATIONS] SQL_statement  
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumente

 *SQL_statement*  

 Die [!INCLUDE[DWsql](../../includes/dwsql-md.md)]-Anweisung, auf der EXPLAIN ausgeführt wird. *SQL_statement* kann einer der folgenden Befehle sein: SELECT, INSERT, UPDATE, DELETE, CREATE TABLE AS SELECT, CREATE REMOTE TABLE.

*WITH_RECOMMENDATIONS* 

Gibt den Abfrageplan mit Empfehlungen zur Leistungsoptimierung der SQL-Anweisung zurück.  
  
## <a name="permissions"></a>Berechtigungen

 Erfordert die **SHOWPLAN**-Berechtigung und die Berechtigung zum Ausführen von *SQL_statement*. Weitere Informationen finden Sie unter [Berechtigungen: GRANT, DENY, REVOKE &#40;Azure Synapse Analytics, Parallel Data Warehouse&#41;](../../t-sql/statements/permissions-grant-deny-revoke-azure-sql-data-warehouse-parallel-data-warehouse.md).  
  
## <a name="return-value"></a>Rückgabewert

 Der Rückgabewert des Befehls **EXPLAIN** ist ein XML-Dokument mit der unten gezeigten Struktur. Dieses XML-Dokument listet alle Vorgänge im Abfrageplan für die angegebene Abfrage auf, die alle durch den Tag `<dsql_operation>` eingeschlossen werden. Der Typ des Rückgabewerts ist **nvarchar(max)** .  
  
 Der zurückgegebene Abfrageplan zeigt sequenzielle SQL-Anweisungen. Das Ausführen der Abfrage kann parallelisierte Vorgänge erfordern, weshalb einige der angezeigten sequenziellen Anweisungen möglicherweise zur gleichen Zeit ausgeführt werden.  
  
```  
\<?xml version="1.0" encoding="utf-8"?>  
<dsql_query>  
  <sql>. . .</sql>  
  <params />  
  <dsql_operations>  
    <dsql_operation>  
     . . .      
    </dsql_operation>  
    [ . . . n ]  
  <dsql_operations>  
</dsql_query>  
```  
  
 Die XML-Tags enthalten folgende Informationen:  
  
|XML Tag|Zusammenfassungen, Attribute und Inhalt|  
|-------------|--------------------------------------|  
|\<dsql_query>|Element auf der obersten Ebene/Dokumentelement.|
|\<sql>|Wiederholt *sql_statement*.|  
|\<params>|Dieses Tag wird zu diesem Zeitpunkt nicht verwendet.|
|\<materialized_view_candidates> (Vorschau)|Enthält die CREATE-Anweisung der empfohlenen materialisierten Sicht für eine bessere Leistung der SQL-Anweisung.| 
|\<dsql_operations>|Enthält Abfrageschritte, fasst diese zusammen und enthält Informationen zu den Kosten der Abfrage. Enthält auch alle `<dsql_operation>`-Blöcke. Dieses Tag enthält Informationen zur Anzahl für die gesamte Abfrage:<br /><br /> `<dsql_operations total_cost=total_cost total_number_operations=total_number_operations>`<br /><br /> *total_cost* ist die geschätzte Gesamtzeit für die Ausführung der Abfrage in Millisekunden.<br /><br /> *total_number_operations* ist die Gesamtzahl von Vorgängen in einer Abfrage. Ein Vorgang, der parallelisiert wird und auf mehreren Knoten ausgeführt wird, wird als einzelner Vorgang gezählt.|  
|\<dsql_operation>|Beschreibt einen einzelnen Vorgang innerhalb des Abfrageplans. Das Tag \<dsql_operation> enthält den Vorgangstyp als Attribut:<br /><br /> `<dsql_operation operation_type=operation_type>`<br /><br /> *operation_type* ist einer der Werte in [sys.dm_pdw_request_steps (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).<br /><br /> Der Inhalt des `\<dsql_operation>`-Blocks ist abhängig vom Vorgangstyp.<br /><br /> Siehe Tabelle unten.|  
  
|Vorgangstyp|Inhalt|Beispiel|  
|--------------------|-------------|-------------|  
|BROADCAST_MOVE, DISTRIBUTE_REPLICATED_TABLE_MOVE, MASTER_TABLE_MOVE, PARTITION_MOVE, SHUFFLE_MOVE und TRIM_MOVE|`<operation_cost>`-Element mit diesen Attributen. Die Werte spiegeln nur den lokalen Vorgang wieder:<br /><br /> -   *cost* sind die Kosten für den lokalen Operator und zeigen die geschätzte Zeit für die Ausführung des Vorgangs in Millisekunden an.<br />-   *accumulative_cost* ist die Summe aller betrachteten Vorgänge im Plan einschließlich der addierten Werte für parallele Vorgänge in Millisekunden.<br />-   *average_rowsize* ist die geschätzte durchschnittliche Zeilengröße von Zeilen (in Byte), die während des Vorgangs abgerufen und übergeben werden.<br />-   *output_rows* ist die Ausgabekardinalität (Knoten) und zeigt die Anzahl der ausgegebenen Zeilen an.<br /><br /> `<location>`: Die Knoten oder Verteilungen, in denen der Vorgang stattfindet. Folgende Optionen sind verfügbar: „Control“, „ComputeNode“, „AllComputeNodes“, „AllDistributions“, „SubsetDistributions“, „Distribution“ und „SubsetNodes“.<br /><br /> `<source_statement>`: Die Quelldaten für den SHUFFLE_MOVE-Vorgang.<br /><br /> `<destination_table>`: Die interne temporäre Tabelle, in die die Daten verschoben werden.<br /><br /> `<shuffle_columns>`: (Gilt nur für SHUFFLE_MOVE-Vorgänge). Eine oder mehrere Spalten, die als Verteilungsspalten für die temporäre Tabelle verwendet werden.|`<operation_cost cost="40" accumulative_cost="40" average_rowsize = "50" output_rows="100"/>`<br /><br /> `<location distribution="AllDistributions" />`<br /><br /> `<source_statement type="statement">SELECT [TableAlias_3b77ee1d8ccf4a94ba644118b355db9d].[dist_date] FROM [qatest].[dbo].[flyers] [TableAlias_3b77ee1d8ccf4a94ba644118b355db9d]       </source_statement>`<br /><br /> `<destination_table>Q_[TEMP_ID_259]_[PARTITION_ID]</destination_table>`<br /><br /> `<shuffle_columns>dist_date;</shuffle_columns>`|  
|MetaDataCreate_Operation|`<source_table>`: Die Quelltabelle für den Vorgang.<br /><br /> `<destination_table>`: Die Zieltabelle für den Vorgang.|`<source_table>databases</source_table>`<br /><br /> `<destination_table>MetaDataCreateLandingTempTable</destination_table>`|  
|EIN|`<location>`: Siehe `<location>` weiter oben.<br /><br /> `<sql_operation>`: Identifiziert den SQL-Befehl, der auf einem Knoten ausgeführt wird.|`<location permanent="false" distribution="AllDistributions">Compute</location>`<br /><br /> `<sql_operation type="statement">CREATE TABLE [tempdb].[dbo]. [Q_[TEMP_ID_259]]_ [PARTITION_ID]]]([dist_date] DATE) WITH (DISTRIBUTION = HASH([dist_date]),) </sql_operation>`|  
|RemoteOnOperation|`<DestinationCatalog>`: Der Zielkatalog.<br /><br /> `<DestinationSchema>`: Das Zielschema in DestinationCatalog.<br /><br /> `<DestinationTableName>`: Name der Zieltabelle oder „TableName“.<br /><br /> `<DestinationDatasource>`: Der Name der Zieldatenquelle.<br /><br /> `<Username>` und `<Password>`: Diese Felder geben an, dass möglicherweise Benutzername und Kennwort für das Ziel erforderlich sind.<br /><br /> `<CreateStatement>`: Die Anweisung zum Erstellen der Tabelle für die Zieldatenbank.|`<DestinationCatalog>master</DestinationCatalog>`<br /><br /> `<DestinationSchema>dbo</DestinationSchema>`<br /><br /> `<DestinationTableName>TableName</DestinationTableName>`<br /><br /> `<DestinationDatasource>DestDataSource</DestinationDatasource>`<br /><br /> `<Username>...</Username>`<br /><br /> `<Password>...</Password>`<br /><br /> `<CreateStatement>CREATE TABLE [master].[dbo].[TableName] ([col1] BIGINT) ON [PRIMARY] WITH(DATA_COMPRESSION=PAGE);</CreateStatement>`|  
|RETURN|`<resultset>`: Der Bezeichner für das Resultset.|`<resultset>RS_19</resultset>`|  
|RND_ID|`<identifier>`: Der Bezeichner für das erstellte Objekt.|`<identifier>TEMP_ID_260</identifier>`|  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 **EXPLAIN** kann nur auf *optimierbare* Abfragen angewendet werden. Dies sind Abfragen, die auf Basis der Ergebnisse eines **EXPLAIN**-Befehls verbessert oder geändert werden können. Die unterstützten **EXPLAIN**-Befehle sind oben aufgeführt. Der Versuch, **EXPLAIN** mit einem nicht unterstützen Abfragetyp zu verwenden, führt entweder zu einem Fehler oder wiederholt die Abfrage.  
  
 **EXPLAIN** wird in einer Benutzertransaktion nicht unterstützt.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel zeigt einen **EXPLAIN**-Befehl, der in einer **SELECT**-Anweisung ausgeführt wird, sowie das XML-Ergebnis.  
  
 **Übermitteln einer EXPLAIN-Anweisung**  
  
 Der übermittelte Befehl in diesem Beispiel lautet:  
  
```sql  
-- Uses AdventureWorks  
  
EXPLAIN   
    SELECT CAST (AVG(YearlyIncome) AS int) AS AverageIncome,   
        CAST(AVG(FIS.SalesAmount) AS int) AS AverageSales,   
        G.StateProvinceName, T.SalesTerritoryGroup  
    FROM dbo.DimGeography AS G  
    JOIN dbo.DimSalesTerritory AS T  
        ON G.SalesTerritoryKey = T.SalesTerritoryKey  
    JOIN dbo.DimCustomer AS C  
        ON G.GeographyKey = C.GeographyKey  
    JOIN dbo.FactInternetSales AS FIS  
        ON C.CustomerKey = FIS.CustomerKey  
    WHERE T.SalesTerritoryGroup IN ('North America', 'Pacific')  
        AND Gender = 'F'  
    GROUP BY G.StateProvinceName, T.SalesTerritoryGroup  
    ORDER BY AVG(YearlyIncome) DESC;  
GO  
```  
  
 Nach der Ausführung der Anweisung mit der **EXPLAIN**-Option zeigt die Registerkarte „Message“ (Meldung) eine einzelne Zeile mit dem Titel **explain** an, die mit dem XML-Text `\<?xml version="1.0" encoding="utf-8"?>` beginnt. Klicken Sie auf die XML, um den gesamten Text in einem XML-Fenster zu öffnen. Um die folgenden Kommentare besser zu verstehen, sollten Sie die Anzeige von Zeilennummern in SSDT aktivieren.  
  
#### <a name="to-turn-on-line-numbers"></a>So aktivieren Sie Zeilennummern  
  
1.  Wenn die Ausgabe auf der SSDT-Registerkarte **explain** angezeigt wird, wählen Sie im Menü **TOOLS** **Options** (Optionen) aus.  
  
2.  Erweitern Sie den Abschnitt **Text-Editor** sowie **XML**, und klicken Sie anschließend auf **General** (Allgemein).  
  
3.  Überprüfen Sie im Bereich **Anzeige** die **Zeilennummern**.  
  
4.  Klicken Sie auf **OK**.  
  
 **Beispiel EXPLAIN-Ausgabe**  
  
 Das XML-Ergebnis des **EXPLAIN**-Befehls mit aktivierten Zeilennummern lautet:  
  
```  
1  \<?xml version="1.0" encoding="utf-8"?>  
2  <dsql_query>  
3    <sql>SELECT CAST (AVG(YearlyIncome) AS int) AS AverageIncome,   
4          CAST(AVG(FIS.SalesAmount) AS int) AS AverageSales,   
5          G.StateProvinceName, T.SalesTerritoryGroup  
6      FROM dbo.DimGeography AS G  
7      JOIN dbo.DimSalesTerritory AS T  
8          ON G.SalesTerritoryKey = T.SalesTerritoryKey  
9      JOIN dbo.DimCustomer AS C  
10          ON G.GeographyKey = C.GeographyKey  
11      JOIN dbo.FactInternetSales AS FIS  
12          ON C.CustomerKey = FIS.CustomerKey  
13      WHERE T.SalesTerritoryGroup IN ('North America', 'Pacific')  
14          AND Gender = 'F'  
15      GROUP BY G.StateProvinceName, T.SalesTerritoryGroup  
16      ORDER BY AVG(YearlyIncome) DESC</sql>  
17    <dsql_operations total_cost="0.926237696" total_number_operations="9">  
18      <dsql_operation operation_type="RND_ID">  
19        <identifier>TEMP_ID_16893</identifier>  
20      </dsql_operation>  
21      <dsql_operation operation_type="ON">  
22        <location permanent="false" distribution="AllComputeNodes" />  
23        <sql_operations>  
24          <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_16893] ([CustomerKey] INT NOT NULL, [GeographyKey] INT, [YearlyIncome] MONEY ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>  
25        </sql_operations>  
26      </dsql_operation>  
27      <dsql_operation operation_type="BROADCAST_MOVE">  
28        <operation_cost cost="0.121431552" accumulative_cost="0.121431552" average_rowsize="16" output_rows="31.6228" />  
29        <source_statement>SELECT [T1_1].[CustomerKey] AS [CustomerKey],  
30         [T1_1].[GeographyKey] AS [GeographyKey],  
31         [T1_1].[YearlyIncome] AS [YearlyIncome]  
32  FROM   (SELECT [T2_1].[CustomerKey] AS [CustomerKey],  
33                 [T2_1].[GeographyKey] AS [GeographyKey],  
34                 [T2_1].[YearlyIncome] AS [YearlyIncome]  
35          FROM   [AdventureWorksPDW2012].[dbo].[DimCustomer] AS T2_1  
36          WHERE  ([T2_1].[Gender] = CAST (N'F' COLLATE Latin1_General_100_CI_AS_KS_WS AS NVARCHAR (1)) COLLATE Latin1_General_100_CI_AS_KS_WS)) AS T1_1</source_statement>  
37        <destination_table>[TEMP_ID_16893]</destination_table>  
38      </dsql_operation>  
39      <dsql_operation operation_type="RND_ID">  
40        <identifier>TEMP_ID_16894</identifier>  
41      </dsql_operation>  
42      <dsql_operation operation_type="ON">  
43        <location permanent="false" distribution="AllDistributions" />  
44        <sql_operations>  
45          <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_16894] ([StateProvinceName] NVARCHAR(50) COLLATE Latin1_General_100_CI_AS_KS_WS, [SalesTerritoryGroup] NVARCHAR(50) COLLATE Latin1_General_100_CI_AS_KS_WS NOT NULL, [col] BIGINT, [col1] MONEY NOT NULL, [col2] BIGINT, [col3] MONEY NOT NULL ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>  
46        </sql_operations>  
47      </dsql_operation>  
48      <dsql_operation operation_type="SHUFFLE_MOVE">  
49        <operation_cost cost="0.804806144" accumulative_cost="0.926237696" average_rowsize="232" output_rows="108.406" />  
50        <source_statement>SELECT [T1_1].[StateProvinceName] AS [StateProvinceName],  
51         [T1_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
52         [T1_1].[col2] AS [col],  
53         [T1_1].[col] AS [col1],  
54         [T1_1].[col3] AS [col2],  
55         [T1_1].[col1] AS [col3]  
56  FROM   (SELECT ISNULL([T2_1].[col1], CONVERT (MONEY, 0.00, 0)) AS [col],  
57                 ISNULL([T2_1].[col3], CONVERT (MONEY, 0.00, 0)) AS [col1],  
58                 [T2_1].[StateProvinceName] AS [StateProvinceName],  
59                 [T2_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
60                 [T2_1].[col] AS [col2],  
61                 [T2_1].[col2] AS [col3]  
62          FROM   (SELECT   COUNT_BIG([T3_2].[YearlyIncome]) AS [col],  
63                           SUM([T3_2].[YearlyIncome]) AS [col1],  
64                           COUNT_BIG(CAST ((0) AS INT)) AS [col2],  
65                           SUM([T3_2].[SalesAmount]) AS [col3],  
66                           [T3_2].[StateProvinceName] AS [StateProvinceName],  
67                           [T3_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
68                  FROM     (SELECT [T4_1].[SalesTerritoryKey] AS [SalesTerritoryKey],  
69                                   [T4_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
70                            FROM   [AdventureWorksPDW2012].[dbo].[DimSalesTerritory] AS T4_1  
71                            WHERE  (([T4_1].[SalesTerritoryGroup] = CAST (N'North America' COLLATE Latin1_General_100_CI_AS_KS_WS AS NVARCHAR (13)) COLLATE Latin1_General_100_CI_AS_KS_WS)  
72                                    OR ([T4_1].[SalesTerritoryGroup] = CAST (N'Pacific' COLLATE Latin1_General_100_CI_AS_KS_WS AS NVARCHAR (7)) COLLATE Latin1_General_100_CI_AS_KS_WS))) AS T3_1  
73                           INNER JOIN  
74                           (SELECT [T4_1].[SalesTerritoryKey] AS [SalesTerritoryKey],  
75                                   [T4_2].[YearlyIncome] AS [YearlyIncome],  
76                                   [T4_2].[SalesAmount] AS [SalesAmount],  
77                                   [T4_1].[StateProvinceName] AS [StateProvinceName]  
78                            FROM   [AdventureWorksPDW2012].[dbo].[DimGeography] AS T4_1  
79                                   INNER JOIN  
80                                   (SELECT [T5_2].[GeographyKey] AS [GeographyKey],  
81                                           [T5_2].[YearlyIncome] AS [YearlyIncome],  
82                                           [T5_1].[SalesAmount] AS [SalesAmount]  
83                                    FROM   [AdventureWorksPDW2012].[dbo].[FactInternetSales] AS T5_1  
84                                           INNER JOIN  
85                                           [tempdb].[dbo].[TEMP_ID_16893] AS T5_2  
86                                           ON ([T5_1].[CustomerKey] = [T5_2].[CustomerKey])) AS T4_2  
87                                   ON ([T4_2].[GeographyKey] = [T4_1].[GeographyKey])) AS T3_2  
88                           ON ([T3_1].[SalesTerritoryKey] = [T3_2].[SalesTerritoryKey])  
89                  GROUP BY [T3_2].[StateProvinceName], [T3_1].[SalesTerritoryGroup]) AS T2_1) AS T1_1</source_statement>  
90        <destination_table>[TEMP_ID_16894]</destination_table>  
91        <shuffle_columns>StateProvinceName;</shuffle_columns>  
92      </dsql_operation>  
93      <dsql_operation operation_type="ON">  
94        <location permanent="false" distribution="AllComputeNodes" />  
95        <sql_operations>  
96          <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_16893]</sql_operation>  
97        </sql_operations>  
98      </dsql_operation>  
99      <dsql_operation operation_type="RETURN">  
100        <location distribution="AllDistributions" />  
101        <select>SELECT   [T1_1].[col] AS [col],  
102           [T1_1].[col1] AS [col1],  
103           [T1_1].[StateProvinceName] AS [StateProvinceName],  
104           [T1_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
105           [T1_1].[col2] AS [col2]  
106  FROM     (SELECT CONVERT (INT, [T2_1].[col], 0) AS [col],  
107                   CONVERT (INT, [T2_1].[col1], 0) AS [col1],  
108                   [T2_1].[StateProvinceName] AS [StateProvinceName],  
109                   [T2_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
110                   [T2_1].[col] AS [col2]  
111            FROM   (SELECT CASE  
112                            WHEN ([T3_1].[col] = CAST ((0) AS BIGINT)) THEN CAST (NULL AS MONEY)  
113                            ELSE ([T3_1].[col1] / CONVERT (MONEY, [T3_1].[col], 0))  
114                           END AS [col],  
115                           CASE  
116                            WHEN ([T3_1].[col2] = CAST ((0) AS BIGINT)) THEN CAST (NULL AS MONEY)  
117                            ELSE ([T3_1].[col3] / CONVERT (MONEY, [T3_1].[col2], 0))  
118                           END AS [col1],  
119                           [T3_1].[StateProvinceName] AS [StateProvinceName],  
120                           [T3_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
121                    FROM   (SELECT ISNULL([T4_1].[col], CONVERT (BIGINT, 0, 0)) AS [col],  
122                                   ISNULL([T4_1].[col1], CONVERT (MONEY, 0.00, 0)) AS [col1],  
123                                   ISNULL([T4_1].[col2], CONVERT (BIGINT, 0, 0)) AS [col2],  
124                                   ISNULL([T4_1].[col3], CONVERT (MONEY, 0.00, 0)) AS [col3],  
125                                   [T4_1].[StateProvinceName] AS [StateProvinceName],  
126                                   [T4_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
127                            FROM   (SELECT   SUM([T5_1].[col]) AS [col],  
128                                             SUM([T5_1].[col1]) AS [col1],  
129                                             SUM([T5_1].[col2]) AS [col2],  
130                                             SUM([T5_1].[col3]) AS [col3],  
131                                             [T5_1].[StateProvinceName] AS [StateProvinceName],  
132                                             [T5_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
133                                    FROM     [tempdb].[dbo].[TEMP_ID_16894] AS T5_1  
134                                    GROUP BY [T5_1].[StateProvinceName], [T5_1].[SalesTerritoryGroup]) AS T4_1) AS T3_1) AS T2_1) AS T1_1  
135  ORDER BY [T1_1].[col2] DESC</select>  
136      </dsql_operation>  
137      <dsql_operation operation_type="ON">  
138        <location permanent="false" distribution="AllDistributions" />  
139        <sql_operations>  
140          <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_16894]</sql_operation>  
141        </sql_operations>  
142      </dsql_operation>  
143    </dsql_operations>  
144  </dsql_query>  
  
```  
  
 **Bedeutung der EXPLAIN-Ausgabe**
  
 Die oben angezeigte Ausgabe enthält 144 nummerierte Zeilen. Ihre Ausgabe für diese Abfrage kann sich geringfügig unterscheiden. Die folgende Liste beschreibt wichtige Abschnitte.  
  
-   Zeilen 3 bis 16 enthalten eine Beschreibung der Abfrage, die analysiert wird.  
  
-   Zeile 17 gibt an, dass die Gesamtanzahl von Vorgängen 9 ist. Sie können den Beginn jeder Operation ermitteln, indem Sie nach den Worten **dsql_operation** suchen.  
  
-   Zeile 18 startet Vorgang 1. Zeilen 18 und 19 weisen darauf hin, dass ein **RND_ID**-Vorgang eine zufällige ID erstellt, die für eine Objektbeschreibung verwendet wird. Das Objekt, das in der Ausgabe beschrieben wird, ist **TEMP_ID_16893**. Sie werden eine andere Zahl erhalten.  
  
-   Zeile 20 startet Vorgang 2. Zeilen 21 bis 25: Erstellen temporärer Tabellen mit dem Namen **TEMP_ID_16893** auf allen Computeknoten.  
  
-   Zeile 26 startet Vorgang 3. Zeilen 27 bis 37: Verschieben von Daten nach **TEMP_ID_16893** mittels BROADCAST_MOVE. Die an jeden Computeknoten gesendete Abfrage ist angegeben. Zeile 37 gibt an, dass die Zieltabelle **TEMP_ID_16893** ist.  
  
-   Zeile 38 startet Vorgang 4. Zeilen 39 bis 40: Erstellen einer zufälligen ID für eine Tabelle. **TEMP_ID_16894** ist die ID im oben gezeigten Beispiel. Sie werden eine andere Zahl erhalten.  
  
-   Zeile 41 startet Vorgang 5. Zeilen 42 bis 46: Erstellen einer temporären Tabelle mit dem Namen **TEMP_ID_16894** auf allen Knoten.  
  
-   Zeile 47 startet Vorgang 6. Zeilen 48 bis 91: Verschieben von Daten aus verschiedenen Tabellen (einschließlich **TEMP_ID_16893**) in Tabelle **TEMP_ID_16894** mithilfe eines SHUFFLE_MOVE-Vorgangs. Die an jeden Computeknoten gesendete Abfrage ist angegeben. Zeile 90 gibt **TEMP_ID_16894** als Zieltabelle an. Linie 91 gibt die Spalten an.  
  
-   Zeile 92 startet Vorgang 7. Zeilen 93 bis 97: Löschen der temporären Tabelle **TEMP_ID_16893** auf allen Computeknoten.  
  
-   Zeile 98 startet Vorgang 8. Zeilen 99 bis 135: Zurückgeben von Ergebnissen an den Client. Verwendet die zur Verfügung gestellte Abfrage, um Ergebnisse abzurufen.  
  
-   Zeile 136 startet Vorgang 9. Zeilen 137 bis 140: Löschen der temporären Tabelle **TEMP_ID_16894** auf allen Knoten.  

**Übermitteln einer EXPLAIN-Anweisung mit WITH_RECOMMENDATIONS**

```sql
EXPLAIN WITH_RECOMMENDATIONS
select count(*)
from ((select distinct c_last_name, c_first_name, d_date
       from store_sales, date_dim, customer
       where store_sales.ss_sold_date_sk = date_dim.d_date_sk
         and store_sales.ss_customer_sk = customer.c_customer_sk
         and d_month_seq between 1194 and 1194+11)
       except
      (select distinct c_last_name, c_first_name, d_date
       from catalog_sales, date_dim, customer
       where catalog_sales.cs_sold_date_sk = date_dim.d_date_sk
         and catalog_sales.cs_bill_customer_sk = customer.c_customer_sk
         and d_month_seq between 1194 and 1194+11)
) top_customers
```

**Beispielausgabe für EXPLAIN WITH_RECOMMENDATIONS**  

Die Ausgabe unten umfasst das Erstellen einer empfohlenen materialisierten Sicht namens „View1“.  

```
<?xml version="1.0" encoding="utf-8"?>
<dsql_query number_nodes="1" number_distributions="8" number_distributions_per_node="8">
  <sql>select count(*) 
from ((select distinct c_last_name, c_first_name, d_date
       from store_sales, date_dim, customer
       where store_sales.ss_sold_date_sk = date_dim.d_date_sk
         and store_sales.ss_customer_sk = customer.c_customer_sk
         and d_month_seq between 1194 and 1194+11)
       except
      (select distinct c_last_name, c_first_name, d_date
       from catalog_sales, date_dim, customer
       where catalog_sales.cs_sold_date_sk = date_dim.d_date_sk
         and catalog_sales.cs_bill_customer_sk = customer.c_customer_sk
         and d_month_seq between 1194 and 1194+11)
) top_customers</sql>
  <materialized_view_candidates>
    <materialized_view_candidates with_constants="False">CREATE MATERIALIZED VIEW View1 WITH (DISTRIBUTION = HASH([Expr0])) AS
SELECT [tpcds10].[dbo].[customer].[c_last_name] AS [Expr0],
       [tpcds10].[dbo].[customer].[c_first_name] AS [Expr1],
       [tpcds10].[dbo].[date_dim].[d_date] AS [Expr2],
       [tpcds10].[dbo].[date_dim].[d_month_seq] AS [Expr3]
FROM [dbo].[store_sales],
     [dbo].[date_dim],
     [dbo].[customer]
WHERE ([tpcds10].[dbo].[store_sales].[ss_customer_sk]=[tpcds10].[dbo].[customer].[c_customer_sk])
  AND ([tpcds10].[dbo].[store_sales].[ss_sold_date_sk]=[tpcds10].[dbo].[date_dim].[d_date_sk])
GROUP BY [tpcds10].[dbo].[customer].[c_last_name],
         [tpcds10].[dbo].[customer].[c_first_name],
         [tpcds10].[dbo].[date_dim].[d_date],
         [tpcds10].[dbo].[date_dim].[d_month_seq]</materialized_view_candidates>
    <materialized_view_candidates with_constants="False">CREATE MATERIALIZED VIEW View2 WITH (DISTRIBUTION = HASH([Expr0])) AS
SELECT [tpcds10].[dbo].[customer].[c_last_name] AS [Expr0],
       [tpcds10].[dbo].[customer].[c_first_name] AS [Expr1],
       [tpcds10].[dbo].[date_dim].[d_date] AS [Expr2],
       [tpcds10].[dbo].[date_dim].[d_month_seq] AS [Expr3]
FROM [dbo].[catalog_sales],
    [dbo].[date_dim],
     [dbo].[customer]
WHERE ([tpcds10].[dbo].[catalog_sales].[cs_bill_customer_sk]=[tpcds10].[dbo].[customer].[c_customer_sk])
  AND ([tpcds10].[dbo].[catalog_sales].[cs_sold_date_sk]=[tpcds10].[dbo].[date_dim].[d_date_sk])
GROUP BY [tpcds10].[dbo].[customer].[c_last_name],
         [tpcds10].[dbo].[customer].[c_first_name],
         [tpcds10].[dbo].[date_dim].[d_date],
         [tpcds10].[dbo].[date_dim].[d_month_seq]</materialized_view_candidates>
    <materialized_view_candidates with_constants="True">CREATE MATERIALIZED VIEW View3 WITH (DISTRIBUTION = HASH([Expr0])) AS
SELECT [tpcds10].[dbo].[customer].[c_last_name] AS [Expr0],
       [tpcds10].[dbo].[customer].[c_first_name] AS [Expr1],
       [tpcds10].[dbo].[date_dim].[d_date] AS [Expr2]
FROM [dbo].[store_sales],
     [dbo].[date_dim],
     [dbo].[customer]
WHERE ([tpcds10].[dbo].[store_sales].[ss_customer_sk]=[tpcds10].[dbo].[customer].[c_customer_sk])
  AND ([tpcds10].[dbo].[store_sales].[ss_sold_date_sk]=[tpcds10].[dbo].[date_dim].[d_date_sk])
  AND ([tpcds10].[dbo].[date_dim].[d_month_seq]&gt;=(1194))
  AND ([tpcds10].[dbo].[date_dim].[d_month_seq]&lt;=(1205))
GROUP BY [tpcds10].[dbo].[customer].[c_last_name],
         [tpcds10].[dbo].[customer].[c_first_name],
         [tpcds10].[dbo].[date_dim].[d_date]</materialized_view_candidates>
    <materialized_view_candidates with_constants="True">CREATE MATERIALIZED VIEW View4 WITH (DISTRIBUTION = HASH([Expr0])) AS
SELECT [tpcds10].[dbo].[customer].[c_last_name] AS [Expr0],
       [tpcds10].[dbo].[customer].[c_first_name] AS [Expr1],
       [tpcds10].[dbo].[date_dim].[d_date] AS [Expr2]
FROM [dbo].[catalog_sales],
     [dbo].[date_dim],
     [dbo].[customer]
WHERE ([tpcds10].[dbo].[catalog_sales].[cs_bill_customer_sk]=[tpcds10].[dbo].[customer].[c_customer_sk])
  AND ([tpcds10].[dbo].[catalog_sales].[cs_sold_date_sk]=[tpcds10].[dbo].[date_dim].[d_date_sk])
  AND ([tpcds10].[dbo].[date_dim].[d_month_seq]&gt;=(1194))
  AND ([tpcds10].[dbo].[date_dim].[d_month_seq]&lt;=(1205))
GROUP BY [tpcds10].[dbo].[customer].[c_last_name],
         [tpcds10].[dbo].[customer].[c_first_name],
         [tpcds10].[dbo].[date_dim].[d_date]</materialized_view_candidates>
  </materialized_view_candidates>
  <dsql_operations total_cost="3472197.35650704" total_number_operations="28">
    <dsql_operation operation_type="RND_ID">
      <identifier>TEMP_ID_1</identifier>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_1] ([c_customer_sk] INT NOT NULL, [c_first_name] CHAR(20) COLLATE SQL_Latin1_General_CP1_CI_AS, [c_last_name] CHAR(30) COLLATE SQL_Latin1_General_CP1_CI_AS ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="BROADCAST_MOVE">
      <operation_cost cost="842400" accumulative_cost="842400" average_rowsize="54" output_rows="65000000" GroupNumber="44" />
      <source_statement>SELECT [T1_1].[c_customer_sk] AS [c_customer_sk],
       [T1_1].[c_first_name] AS [c_first_name],
       [T1_1].[c_last_name] AS [c_last_name]
FROM   [tpcds10].[dbo].[customer] AS T1_1</source_statement>
      <destination_table>[TEMP_ID_1]</destination_table>
    </dsql_operation>
    <dsql_operation operation_type="RND_ID">
      <identifier>TEMP_ID_2</identifier>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_2] ([d_date_sk] INT NOT NULL, [d_date] DATE ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="BROADCAST_MOVE">
      <operation_cost cost="0.62729352" accumulative_cost="842400.62729352" average_rowsize="7" output_rows="373.389" GroupNumber="43" />
      <source_statement>SELECT [T1_1].[d_date_sk] AS [d_date_sk],
       [T1_1].[d_date] AS [d_date]
FROM   (SELECT [T2_1].[d_date_sk] AS [d_date_sk],
               [T2_1].[d_date] AS [d_date]
        FROM   [tpcds10].[dbo].[date_dim] AS T2_1
        WHERE  (([T2_1].[d_month_seq] &gt;= CAST ((1194) AS INT))
                AND ([T2_1].[d_month_seq] &lt;= CAST ((1205) AS INT)))) AS T1_1</source_statement>
      <destination_table>[TEMP_ID_2]</destination_table>
    </dsql_operation>
    <dsql_operation operation_type="RND_ID">
      <identifier>TEMP_ID_3</identifier>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllDistributions" />
      <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_3] ([cs_bill_customer_sk] INT, [d_date] DATE ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="SHUFFLE_MOVE">
      <operation_cost cost="610362.9" accumulative_cost="1452763.52729352" average_rowsize="7" output_rows="2906490000" GroupNumber="57" />
      <source_statement>SELECT [T1_1].[cs_bill_customer_sk] AS [cs_bill_customer_sk],
       [T1_1].[d_date] AS [d_date]
FROM   (SELECT [T2_2].[cs_bill_customer_sk] AS [cs_bill_customer_sk],
               [T2_1].[d_date] AS [d_date]
        FROM   [tempdb].[dbo].[TEMP_ID_2] AS T2_1
               INNER JOIN
               [tpcds10].[dbo].[catalog_sales] AS T2_2
               ON ([T2_2].[cs_sold_date_sk] = [T2_1].[d_date_sk])) AS T1_1</source_statement>
      <destination_table>[TEMP_ID_3]</destination_table>
      <shuffle_columns>d_date;</shuffle_columns>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_2]</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="RND_ID">
      <identifier>TEMP_ID_4</identifier>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_4] ([c_customer_sk] INT NOT NULL, [c_first_name] CHAR(20) COLLATE SQL_Latin1_General_CP1_CI_AS, [c_last_name] CHAR(30) COLLATE SQL_Latin1_General_CP1_CI_AS ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="BROADCAST_MOVE">
      <operation_cost cost="842400" accumulative_cost="2295163.52729352" average_rowsize="54" output_rows="65000000" GroupNumber="36" />
      <source_statement>SELECT [T1_1].[c_customer_sk] AS [c_customer_sk],
       [T1_1].[c_first_name] AS [c_first_name],
       [T1_1].[c_last_name] AS [c_last_name]
FROM   [tpcds10].[dbo].[customer] AS T1_1</source_statement>
      <destination_table>[TEMP_ID_4]</destination_table>
    </dsql_operation>
    <dsql_operation operation_type="RND_ID">
      <identifier>TEMP_ID_5</identifier>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_5] ([d_date_sk] INT NOT NULL, [d_date] DATE ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="BROADCAST_MOVE">
      <operation_cost cost="0.62729352" accumulative_cost="2295164.15458704" average_rowsize="7" output_rows="373.389" GroupNumber="35" />
      <source_statement>SELECT [T1_1].[d_date_sk] AS [d_date_sk],
       [T1_1].[d_date] AS [d_date]
FROM   (SELECT [T2_1].[d_date_sk] AS [d_date_sk],
               [T2_1].[d_date] AS [d_date]
        FROM   [tpcds10].[dbo].[date_dim] AS T2_1
        WHERE  (([T2_1].[d_month_seq] &gt;= CAST ((1194) AS INT))
                AND ([T2_1].[d_month_seq] &lt;= CAST ((1205) AS INT)))) AS T1_1</source_statement>
      <destination_table>[TEMP_ID_5]</destination_table>
    </dsql_operation>
    <dsql_operation operation_type="RND_ID">
      <identifier>TEMP_ID_6</identifier>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllDistributions" />
      <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_6] ([ss_customer_sk] INT, [d_date] DATE ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="SHUFFLE_MOVE">
      <operation_cost cost="1177033.2" accumulative_cost="3472197.35458704" average_rowsize="7" output_rows="5604920000" GroupNumber="54" />
      <source_statement>SELECT [T1_1].[ss_customer_sk] AS [ss_customer_sk],
       [T1_1].[d_date] AS [d_date]
FROM   (SELECT [T2_2].[ss_customer_sk] AS [ss_customer_sk],
               [T2_1].[d_date] AS [d_date]
        FROM   [tempdb].[dbo].[TEMP_ID_5] AS T2_1
               INNER JOIN
               [tpcds10].[dbo].[store_sales] AS T2_2
               ON ([T2_2].[ss_sold_date_sk] = [T2_1].[d_date_sk])) AS T1_1</source_statement>
      <destination_table>[TEMP_ID_6]</destination_table>
      <shuffle_columns>d_date;</shuffle_columns>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_5]</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="Control" />
     <sql_operations>
        <sql_operation type="statement">CREATE TABLE [tempdb].[QTables].[QTable_87367172aa554f06b73cf3ed97e5b985] ([col] BIGINT ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="PARTITION_MOVE">
      <operation_cost cost="0.00192" accumulative_cost="3472197.35650704" average_rowsize="8" output_rows="1" GroupNumber="66" />
      <location distribution="AllDistributions" />
      <source_statement>SELECT [T1_1].[col] AS [col]
FROM   (SELECT   COUNT_BIG(CAST ((0) AS INT)) AS [col]
        FROM     (SELECT   0 AS [col]
                  FROM     [tempdb].[dbo].[TEMP_ID_4] AS T3_1
                           INNER JOIN
                           [tempdb].[dbo].[TEMP_ID_6] AS T3_2
                           ON ([T3_2].[ss_customer_sk] = [T3_1].[c_customer_sk])
                  GROUP BY [T3_1].[c_last_name], [T3_1].[c_first_name], [T3_2].[d_date]
                  HAVING   NOT EXISTS (SELECT   1 AS C1
                                       FROM     [tempdb].[dbo].[TEMP_ID_1] AS T4_1
                                                INNER JOIN
                                                [tempdb].[dbo].[TEMP_ID_3] AS T4_2
                                                ON ([T4_2].[cs_bill_customer_sk] = [T4_1].[c_customer_sk])
                                       GROUP BY [T4_1].[c_last_name], [T4_1].[c_first_name], [T4_2].[d_date]
                                       HAVING   (([T3_1].[c_last_name] = [T4_1].[c_last_name]
                                                  OR ([T3_1].[c_last_name] IS NULL
                                                      AND [T4_1].[c_last_name] IS NULL))
                                                 AND ([T3_1].[c_first_name] = [T4_1].[c_first_name]
                                                      OR ([T3_1].[c_first_name] IS NULL
                                                          AND [T4_1].[c_first_name] IS NULL))
                                                     AND ([T3_2].[d_date] = [T4_2].[d_date]
                                                          OR ([T3_2].[d_date] IS NULL
                                                              AND [T4_2].[d_date] IS NULL))))) AS T2_1
        GROUP BY [T2_1].[col]) AS T1_1</source_statement>
      <destination>Control</destination>
      <destination_table>[QTable_87367172aa554f06b73cf3ed97e5b985]</destination_table>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllDistributions" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_6]</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_4]</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllDistributions" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_3]</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="AllComputeNodes" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_1]</sql_operation>
      </sql_operations>
    </dsql_operation>
    <dsql_operation operation_type="RETURN">
      <location distribution="Control" />
      <select>SELECT [T1_1].[col] AS [col]
FROM   (SELECT CONVERT (INT, [T2_1].[col], 0) AS [col]
        FROM   (SELECT ISNULL([T3_1].[col], CONVERT (BIGINT, 0, 0)) AS [col]
                FROM   (SELECT SUM([T4_1].[col]) AS [col]
                        FROM   [tempdb].[QTables].[QTable_87367172aa554f06b73cf3ed97e5b985] AS T4_1) AS T3_1) AS T2_1) AS T1_1</select>
    </dsql_operation>
    <dsql_operation operation_type="ON">
      <location permanent="false" distribution="Control" />
      <sql_operations>
        <sql_operation type="statement">DROP TABLE [tempdb].[QTables].[QTable_87367172aa554f06b73cf3ed97e5b985]</sql_operation>
      </sql_operations>
    </dsql_operation>
  </dsql_operations>
</dsql_query>
```

## <a name="see-also"></a>Weitere Informationen
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Katalogsichten von Azure Synapse Analytics und Parallel Data Warehouse Catalog](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[In Azure Synapse Analytics unterstützte Systemsichten](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[In Azure Synapse Analytics unterstützte T-SQL-Anweisungen](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)