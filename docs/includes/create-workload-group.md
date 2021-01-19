Erstellt eine Arbeitsauslastungsgruppe unter Ressourcenkontrolle und verknüpft die Arbeitsauslastungsgruppe mit einem Ressourcenpool der Ressourcenkontrolle. Resource Governor ist nicht in jeder Edition von [!INCLUDE[msCoName](msconame-md.md)][!INCLUDE[ssNoVersion](ssnoversion-md.md)] verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).

:::image type="icon" source="../database-engine/configure-windows/media/topic-link.gif"::: [Transact-SQL-Syntaxkonventionen](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE WORKLOAD GROUP group_name
[ WITH
    ( [ IMPORTANCE = { LOW | MEDIUM | HIGH } ]
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]
      [ [ , ] MAX_DOP = value ]
      [ [ , ] GROUP_MAX_REQUESTS = value ] )
 ]
[ USING {
    [ pool_name | "default" ]
    [ [ , ] EXTERNAL external_pool_name | "default" ] ]
    } ]
[ ; ]
```

## <a name="arguments"></a>Argumente

*group_name*</br>
Der benutzerdefinierte Name für die Arbeitsauslastungsgruppe. *group_name* ist alphanumerisch, kann bis zu 128 Zeichen enthalten, muss innerhalb einer Instanz von [!INCLUDE[ssNoVersion](ssnoversion-md.md)] eindeutig sein und muss den Regeln für [Bezeichner](../relational-databases/databases/database-identifiers.md) entsprechen.

IMPORTANCE = { LOW | **MEDIUM** | HIGH }</br>
Gibt die relative Wichtigkeit einer Anforderung in der Arbeitsauslastungsgruppe an. Für die Wichtigkeit sind folgende Einstellungen möglich, wobei MEDIUM die Standardeinstellung ist:

- LOW
- MEDIUM (Standard)
- HIGH

> [!NOTE]
> Intern wird jede Wichtigkeitseinstellung als Zahl gespeichert, die für Berechnungen verwendet wird.

IMPORTANCE hat einen lokalen Bezug zum Ressourcenpool; Arbeitsauslastungsgruppen mit verschiedener Wichtigkeit innerhalb desselben Ressourcenpools beeinflussen sich gegenseitig, haben jedoch keine Auswirkungen auf Arbeitsauslastungsgruppen in anderen Ressourcenpools.

REQUEST_MAX_MEMORY_GRANT_PERCENT = *value*</br>
Gibt die Höchstmenge an Arbeitsspeicher an, die eine einzelne Anforderung vom Pool in Anspruch nehmen kann. *value* entspricht einem Prozentwert, der relativ zur Ressourcenpoolgröße ist, die mit MAX_MEMORY_PERCENT festgelegt wird.

*value* ist bis [!INCLUDE[ssSQL17](sssql17-md.md)] ein Integer und ab [!INCLUDE[sql-server-2019](sssqlv15-md.md)] und in Azure SQL Managed Instance eine Gleitkommazahl. Der Standardwert ist 25. Der zulässige Bereich für *value* liegt zwischen 1 und 100.

> [!IMPORTANT]  
> Die angegebene Menge bezieht sich nur auf den für die Abfrageausführung gewährten Arbeitsspeicher.
>
> Das Festlegen von *value* auf 0 (null) verhindert, dass Abfragen mit SORT- und HASH JOIN-Vorgängen in benutzerdefinierten Arbeitsauslastungsgruppen ausgeführt werden.
>
> Es wird davon abgeraten, *value* auf einen höheren Wert als 70 festzulegen, da der Server möglicherweise nicht genug freien Arbeitsspeicher reservieren kann, wenn andere Abfragen gleichzeitig ausgeführt werden. Dadurch tritt möglicherweise der Timeoutfehler 8645 auf.
>
> Wenn die Arbeitsspeicheranforderungen der Abfrage den Grenzwert überschreiten, der von diesem Parameter angegeben wird, führt der Server folgende Vorgänge aus:
>
> - Bei benutzerdefinierten Arbeitsauslastungsgruppen versucht der Server, den Grad der Parallelität für diese Abfrage zu reduzieren, bis die Arbeitsspeicheranforderung den Grenzwert unterschreitet oder bis der Grad der Parallelität dem Wert 1 entspricht. Wenn die Arbeitsspeicheranforderung der Abfrage den Grenzwert immer noch überschreitet, tritt Fehler 8657 auf.
>
> - Bei internen und Standard-Arbeitsauslastungsgruppen lässt der Server zu, dass der Abfrage der erforderliche Arbeitsspeicher zugewiesen wird.
>
> Beachten Sie, dass in beiden Fällen der Timeoutfehler 8645 auftreten kann, wenn der Server nicht über ausreichend physischen Arbeitsspeicher verfügt.

REQUEST_MAX_CPU_TIME_SEC = *value*</br>
Gibt die maximale CPU-Zeit in Sekunden an, die eine Anforderung beanspruchen kann. *value* muss 0 (null) oder ein positiver Integer sein. Die Standardeinstellung für *value* ist 0 (null), also unbegrenzt.

> [!NOTE]
> Resource Governor verhindert nicht, dass eine Anforderung bei Erreichung des maximalen Zeitlimits fortgesetzt wird. Es wird jedoch ein Ereignis generiert. Weitere Informationen finden Sie unter [CPU Threshold Exceeded (Ereignisklasse)](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).
> [!IMPORTANT]
> Ab [!INCLUDE[ssSQL15](sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](sssql17-md.md)] CU3 bricht Resource Governor mit [Ablaufverfolgungsflag 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) eine Anforderung ab, wenn die maximale Zeit überschritten wird.

REQUEST_MEMORY_GRANT_TIMEOUT_SEC = *value*</br>
Gibt die maximale Zeit in Sekunden an, die eine Abfrage auf das Freiwerden einer Speicherzuweisung (Arbeitsspeicherpuffer) wartet. *value* muss 0 (null) oder ein positiver Integer sein. Die Standardeinstellung für *value* ist 0 (null). Hierbei wird eine interne Berechnung basierend auf den Abfragekosten verwendet, um die maximale Zeit zu ermitteln.

> [!NOTE]
> Eine Abfrage schlägt nicht grundsätzlich fehl, wenn das Timeout der Arbeitsspeicherzuweisung erreicht wird. Eine Abfrage schlägt nur fehl, wenn zu viele Abfragen gleichzeitig ausgeführt werden. Andernfalls könnte die Abfrage nur die minimale Arbeitsspeicherzuweisung nutzen, was zu reduzierter Abfrageleistung führen kann.

MAX_DOP = *value*</br>
Gibt den **maximalen Grad an Parallelität (MAXDOP)** für die parallele Ausführung von Abfragen an. *value* muss 0 (null) oder ein positiver Integer sein. Der zulässige Bereich für *value* liegt zwischen 0 und 64. Die *value*-Standardeinstellung 0 verwendet die globale Einstellung. MAX_DOP wird wie folgt behandelt:

> [!NOTE]
> MAX_DOP für die Arbeitsauslastungsgruppe überschreibt die [Serverkonfiguration des maximalen Grads an Parallelität](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) und den **MAXDOP**-Wert der auf die [Datenbank beschränkten Konfiguration](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

> [!TIP]
> Verwenden Sie den [Abfragehinweis](../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**, um dies auf Abfrageebene zu erreichen. Die Festlegung des maximalen Grads an Parallelität als Abfragehinweis gilt, solange der MAX_DOP-Wert der Arbeitsauslastungsgruppe nicht überschritten wird. Wenn der MAXDOP-Wert des Abfragehinweises den von Resource Governor konfigurierten Wert überschreitet, verwendet [!INCLUDE[ssDEnoversion](ssdenoversion-md.md)] den `MAX_DOP`-Wert von Resource Governor. Der [Abfragehinweis](../t-sql/queries/hints-transact-sql-query.md) für MAXDOP überschreibt stets die [Serverkonfiguration des maximalen Grads an Parallelität](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
>
> Verwenden Sie den **MAXDOP**-Wert der auf die [Datenbank beschränkten Konfiguration](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md), um dies auf Datenbankebene zu erreichen.
>
> Verwenden Sie den **MAXDOP**-Wert (maximaler Parallelitätsgrad) der [Serverkonfigurationsoption](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md), um dies auf Serverebene zu erreichen.

GROUP_MAX_REQUESTS = *value*</br>
Gibt die maximale Anzahl gleichzeitiger Anforderungen an, die in der Arbeitsauslastungsgruppe ausgeführt werden können. *value* muss 0 (null) oder ein positiver Integer sein. Der Standardwert von *value* ist 0 (null) und lässt eine unbegrenzte Anzahl von Anforderungen zu. Wenn die maximale Anzahl gleichzeitiger Anforderungen erreicht wird, kann sich ein Benutzer dieser Gruppe zwar anmelden, wird jedoch in den Wartezustand versetzt, bis die Anzahl gleichzeitiger Anforderungen unter den angegebenen Wert gefallen ist.

USING { *pool_name* |  **"default"** }</br>
Ordnet die Arbeitsauslastungsgruppe dem benutzerdefinierten Ressourcenpool zu, der durch *pool_name* identifiziert wird. Hierdurch wird die Arbeitsauslastungsgruppe im Endeffekt im Ressourcenpool platziert. Wenn *pool_name* nicht bereitgestellt wird oder wenn das USING-Argument nicht verwendet wird, wird die Arbeitsauslastungsgruppe in den vordefinierten Standardpool von Resource Governor eingefügt.

"default" ist ein reserviertes Wort und muss bei der Verwendung mit USING in Anführungszeichen ("") oder Klammern ([]) eingeschlossen werden.

> [!NOTE]
> Für vordefinierte Arbeitsauslastungsgruppen und Ressourcenpools werden ausschließlich kleingeschriebene Namen verwendet, z. B. "default". Dies sollte bei Servern beachtet werden, die bei der Sortierung zwischen Groß-/Kleinschreibung unterscheiden. Server, die bei der Sortierung keine Groß- und Kleinschreibung unterscheiden, z. B. SQL_Latin1_General_CP1_CI_AS, behandeln "default" und "Default" gleich.

EXTERNAL external_pool_name | "default"</br>
**Gilt für**: [!INCLUDE[ssNoVersion](ssnoversion-md.md)] (ab [!INCLUDE[ssSQL15](sssql16-md.md)]).

Die Arbeitsauslastungsgruppe kann einen externen Ressourcenpool angeben. Sie können eine Arbeitsauslastungsgruppe definieren und zwei Pools zuordnen:

- Ein Ressourcenpool für [!INCLUDE[ssNoVersion](ssnoversion-md.md)]-Arbeitsauslastungen und Abfragen
- Ein externer Ressourcenpool für externe Prozesse. Weitere Informationen finden Sie unter [sp_execute_external_script &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="remarks"></a>Bemerkungen

Wenn `REQUEST_MEMORY_GRANT_PERCENT` verwendet wird, kann die Indexerstellung verwendet werden, um mehr Arbeitsbereichsspeicher als ursprünglich zugewiesen zu verwenden, damit eine bessere Leistung erzielt wird. Diese besondere Behandlung wird von der Ressourcenkontrolle in [!INCLUDE[ssCurrent](sscurrent-md.md)] unterstützt. Die Zuweisung anfänglichen und zusätzlichen Arbeitsspeichers wird jedoch durch den Ressourcenpool und die Einstellungen der Arbeitsauslastungsgruppe begrenzt.

Der Grenzwert `MAX_DOP` wird [taskbezogen](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) festgelegt. Es handelt sich nicht um einen [anforderungs](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)- oder abfragebezogenen Grenzwert. Das bedeutet, dass während einer parallelen Abfrageausführung eine einzelne Abfrage mehrere Tasks erzeugen kann, die einem [Planer](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) zugeordnet sind. Weitere Informationen finden Sie im [Handbuch zur Thread- und Taskarchitektur](../relational-databases/thread-and-task-architecture-guide.md).

Wenn `MAX_DOP` verwendet wird und die Abfrage zur Kompilierzeit als seriell markiert ist, kann sie zur Laufzeit nicht wieder in parallel geändert werden, und zwar unabhängig von der Arbeitsauslastungsgruppe oder Serverkonfigurationseinstellung. Nach der Konfiguration von `MAX_DOP` kann dieser Wert nur bei Arbeitsspeicherengpässen verringert werden. Die Neukonfiguration der Arbeitsauslastungsgruppe ist während des Wartens in der Speicherzuweisungs-Warteschlange nicht sichtbar.

### <a name="index-creation-on-a-partitioned-table"></a>Indexerstellung für eine partitionierte Tabelle

Der durch die Indexerstellung für nicht ausgerichtete partitionierte Tabellen belegte Arbeitsspeicher ist proportional zur Anzahl der beteiligten Partitionen. Wenn der insgesamt erforderliche Arbeitsspeicher die Grenze (`REQUEST_MAX_MEMORY_GRANT_PERCENT`) übersteigt, die pro Abfrage von der Resource Governor-Arbeitsauslastungsgruppe festgelegt wurde, kann die Indexerstellung möglicherweise nicht erfolgreich ausgeführt werden. Da die Arbeitsauslastungsgruppe *„default“* Abfragen zulässt, die die pro Abfrage festgelegte Grenze für mindestens erforderlichen Arbeitsspeicher übersteigen, können Benutzer dieselbe Indexerstellung in Arbeitsauslastungsgruppen des Typs *„default“* ausführen. Voraussetzung ist, dass der Ressourcenpool *„default“* über ausreichend Gesamtarbeitsspeicher verfügt, um eine solche Abfrage ausführen zu können.

## <a name="permissions"></a>Berechtigungen

Erfordert die `CONTROL SERVER`-Berechtigung.

## <a name="example"></a>Beispiel

Erstellen Sie eine Arbeitsauslastungsgruppe namens `newReports`, die Resource Governor-Standardeinstellungen verwendet und sich im Resource Governor-Standardpool befindet. Im Beispiel wird der `default`-Pool angegeben, wobei dies jedoch nicht erforderlich ist.

```sql
CREATE WORKLOAD GROUP newReports
WITH
    (REQUEST_MAX_MEMORY_GRANT_PERCENT = 2.5
      , REQUEST_MAX_CPU_TIME_SEC = 100
      , MAX_DOP = 4)    
USING "default" ;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/drop-workload-group-transact-sql.md)
- [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)