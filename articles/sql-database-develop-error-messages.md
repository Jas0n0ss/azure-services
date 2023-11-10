<properties
	pageTitle="SQL 错误代码 - 数据库连接错误 | Azure"
	description="了解有关 SQL 数据库客户端应用程序的 SQL 错误代码，例如常见的数据库连接错误、数据库复制问题和常规错误。"
	keywords="SQL 错误代码、访问 SQL、数据库连接错误、SQL 错误代码"
	services="sql-database"
	documentationCenter=""
	authors="annemill"
	manager="jhubbard"
	editor="" />


<tags
	ms.service="sql-database"
	ms.date="03/22/2016"
	wacn.date="05/16/2016"/>


# SQL 数据库客户端应用程序的 SQL 错误代码：数据库连接错误和其他问题


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


本文列出了 SQL 数据库客户端应用程序的 SQL 错误代码，包括数据库连接错误、暂时性错误（也称为暂时性故障）、资源调控错误、数据库复制问题、弹性池和其他错误。大多数类别特定于 Azure SQL 数据库，并不适用于 Microsoft SQL Server。

在客户端应用程序中，针对任何指定的错误，你可以为用户提供你自定义的消息。

<a id="bkmk_connection_errors" name="bkmk_connection_errors">&nbsp;</a>


## 数据库连接错误、暂时性错误和其他临时错误

下表涵盖了应用程序尝试访问 SQL 数据库时可能遇到的连接丢失错误和其他暂时性错误的 SQL 错误代码。


### 最常见的数据库连接错误和暂时性故障错误


出现暂时性故障错误时，客户端程序通常会发出以下错误消息之一：

- 服务器 <Azure_instance> 上的数据库 <db_name> 当前不可用。请稍后重试连接。如果问题仍然存在，请与客户支持人员联系，并向其提供 <session_id> 的会话追踪 ID。

- 服务器 <Azure_instance> 上的数据库 <db_name> 当前不可用。请稍后重试连接。如果问题仍然存在，请与客户支持人员联系，并向其提供 <session_id> 的会话追踪 ID。(Microsoft SQL Server，错误: 40613)

- 远程主机强行关闭了现有连接。

- System.Data.Entity.Core.EntityCommandExecutionException: 执行命令定义时出错。有关详细信息，请参阅内部异常。---> System.Data.SqlClient.SqlException: 在接收来自服务器的结果时发生传输级错误。(提供程序: 会话提供程序，错误: 19 - 物理连接不可用)

暂时性故障错误应该提示客户端程序运行你设计的重试逻辑来重试操作。有关重试逻辑的代码示例，请参阅：


- [SQL 数据库的客户端开发和快速入门代码示例](/documentation/articles/sql-database-develop-quick-start-client-code-samples/)

- [修复 SQL 数据库中的连接错误和暂时性错误的操作](/documentation/articles/sql-database-connectivity-issues/)


### 暂时性故障错误代码


| 错误代码 | 严重性 | 说明 |
| ---: | ---: | :--- |
| 4060 | 16 | 无法打开该登录请求的数据库“%.&#x2a;ls”。登录失败。 |
|40197|17|该服务在处理你的请求时遇到错误。请稍后重试。错误代码 %d。<br/><br/>当服务由于软件或硬件升级、硬件故障或任何其他故障转移问题而关闭时，你将收到此错误。错误 40197 的消息中嵌入的错误代码 (%d) 提供有关所发生的故障或故障转移类型的其他信息。错误 40197 的消息中嵌入的错误代码的一些示例为 40020、40143、40166 和 40540。<br/><br/>重新连接到 SQL 数据库服务器会自动将你连接到数据库的正常运行的副本。应用程序必须捕获错误 40197，记录该消息中嵌入的错误代码 (%d) 以供进行故障排除，然后尝试重新连接到 SQL 数据库，直到资源可用且再次建立连接为止。|
|40501|20|服务当前正忙。请在 10 秒钟后重试请求。事件 ID: %ls。代码: %d。<br/><br/>注意：<br/>有关一般信息，请参阅<br/>• [Azure SQL 数据库资源限制](/documentation/articles/sql-database-resource-limits/)。
|40613|17|数据库“%.&#x2a;ls”（在服务器“%.&#x2a;ls”上）当前不可用。请稍后重试连接。如果问题仍然存在，请与客户支持人员联系，并向其提供“%.&#x2a;ls”的会话追踪 ID。|
|49918|16|无法处理请求。没有足够的资源，无法处理该请求。<br/><br/>服务当前正忙。请稍后重试请求。 |
|49919|16|无法处理创建或更新请求。为订阅“%ld”处理的创建或更新请求过多。<br/><br/>服务正忙于处理订阅或服务器的多个创建或更新请求。为了优化资源，当前阻止了请求。请查询 [sys.dm\_operation\_stats](https://msdn.microsoft.com/zh-cn/library/dn270022.aspx) 以了解挂起的操作。请等到挂起的创建或更新请求完成，或删除其中一个挂起的请求，然后重试请求。 |
|49920|16|无法处理请求。为订阅“%ld”执行的操作过多。<br/><br/>服务正忙于处理此订阅的多个请求。为了优化资源，当前阻止了请求。请查询 [sys.dm\_operation\_status](https://msdn.microsoft.com/zh-cn/library/dn270022.aspx) 以了解操作状态。请等到挂起的请求完成，或删除其中一个挂起的请求，然后重试请求。 |


<a id="bkmk_b_database_copy_errors" name="bkmk_b_database_copy_errors">&nbsp;</a>

## 数据库复制错误


下表包含了你在 Azure SQL 数据库中复制数据库时你可能会遇到的不同错误。


|错误代码|严重性|说明|
|---:|---:|:---|
|40635|16|IP 地址为“%.&#x2a;ls”的客户端已被暂时禁用。|
|40637|16|创建数据库副本当前被禁用。|
|40561|16|数据库复制失败。源数据库或目标数据库不存在。|
|40562|16|数据库复制失败。源数据库已删除。|
|40563|16|数据库复制失败。目标数据库已删除。|
|40564|16|数据库复制由于内部错误而失败。请删除目标数据库，然后重试。|
|40565|16|数据库复制失败。不允许来自同一源的多个并发数据库复制。请删除目标数据库，然后重试。|
|40566|16|数据库复制由于内部错误而失败。请删除目标数据库，然后重试。|
|40567|16|数据库复制由于内部错误而失败。请删除目标数据库，然后重试。|
|40568|16|数据库复制失败。源数据库已变得不可用。请删除目标数据库，然后重试。|
|40569|16|数据库复制失败。目标数据库已变得不可用。请删除目标数据库，然后重试。|
|40570|16|数据库复制由于内部错误而失败。请删除目标数据库，然后重试。|
|40571|16|数据库复制由于内部错误而失败。请删除目标数据库，然后重试。|


<a id="bkmk_c_resource_gov_errors" name="bkmk_c_resource_gov_errors">&nbsp;</a>

## 资源调控控制


下表包含了在使用 Azure SQL 数据库时过度使用资源所造成的错误。例如：


- 事务打开的时间可能过长。
- 事务可能持有过多的锁。
- 程序可能消耗了过多的内存。
- 程序可能消耗了过多的 `TempDb` 空间。


**提示：**以下链接提供适用于本部分中的大多数或所有错误的详细信息：


- [Azure SQL 数据库资源限制](/documentation/articles/sql-database-resource-limits/)


|错误代码|严重性|说明|
|---:|---:|:---|
|10928|20|资源 ID: %d。数据库的 %s 限制是 %d 且已达到该限制。有关详细信息，请参阅 [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)。<br/><br/>资源 ID 指示已达到限制的资源。对于工作线程，资源 ID = 1。对于会话，资源 ID = 2。<br/><br/>注意：有关此错误以及如何解决它的详细信息，请参阅：<br/>[Azure SQL 数据库资源限制](/documentation/articles/sql-database-resource-limits/)。 |
|10929|20|资源 ID: %d。%s 最小保证为 %d，最大限制为 %d，数据库的当前使用率为 %d。但是，服务器当前太忙，无法支持针对该数据库的数目大于 %d 的请求。有关详细信息，请参阅 [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)。否则，请稍后重试。<br/><br/>资源 ID 指明已达到限制的资源。对于工作线程，资源 ID = 1。对于会话，资源 ID = 2。<br/><br/>注意：有关此错误以及如何解决它的详细信息，请参阅：<br/>• [Azure SQL 数据库资源限制](/documentation/articles/sql-database-resource-limits/)。|
|40544|20|数据库已达到大小配额。请将数据分区或删除、删除索引或查阅文档以找到可能的解决方案。|
|40549|16|由于你有长时间运行的事务，已终止会话。请尝试缩短事务运行时间。|
|40550|16|由于会话获取的锁过多，已终止该会话。请尝试在单个事务中读取或修改更少的行。|
|40551|16|会话已由于过多的 `TEMPDB` 使用而终止。请尝试修改你的查询以减少使用临时表空间。<br/><br/>提示：如果你在使用临时对象，则通过在会话不再需要临时对象后删除这些临时对象，可以节省 `TEMPDB` 数据库中的空间。|
|40552|16|由于过度使用事务日志空间，已终止该会话。请尝试在单个事务中修改更少的行。<br/><br/>提示：如果你在使用 `bcp.exe` 实用工具或 `System.Data.SqlClient.SqlBulkCopy` 类执行大容量插入，则尝试使用 `-b batchsize` 或 `BatchSize` 选项限制在各事务中复制到服务器的行数。如果你正在使用 `ALTER INDEX` 语句重新生成索引，请尝试使用 `REBUILD WITH ONLINE = ON` 选项。|
|40553|16|由于过度使用内存，已终止该会话。请尝试修改你的查询以处理更少的行。<br/><br/>提示：在你的 Transact-SQL 代码中减少 `ORDER BY` 和 `GROUP BY` 操作数可以帮助降低查询的内存要求。|


有关此错误以及如何解决它的详细信息，请参阅：


- [Azure SQL 数据库资源限制](/documentation/articles/sql-database-resource-limits/)。


<a id="bkmk_e_general_errors" name="bkmk_e_general_errors">&nbsp;</a>

## 弹性池错误

相关主题：

* [创建弹性数据库池 (C#)](/documentation/articles/sql-database-elastic-pool-create-csharp/) 
* [管理弹性数据库池 (C#)](/documentation/articles/sql-database-elastic-pool-manage-csharp/)。 
* [创建弹性数据库池 (PowerShell)](/documentation/articles/sql-database-elastic-pool-create-powershell/) 
* [监视和管理弹性数据库池 (PowerShell)](/documentation/articles/sql-database-elastic-pool-manage-powershell/)。

| ErrorNumber | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX\_RESOURCE | 弹性池已达到其存储限制。弹性池的存储使用不能超过 (%d) MB。 | 弹性池空间限制 (MB)。 | 到达弹性池的存储限制时，尝试向数据库写入数据。 | 在可能的情况下，请考虑增加弹性池的 DTU 数，以便提高其存储限制，降低弹性池中各数据库使用的存储，或者从弹性池中删除数据库。 |
| 10929 | EX\_USER | %s 最小保证为 %d，最大限制为 %d，数据库的当前使用率为 %d。但是，服务器当前太忙，无法支持针对该数据库的数目大于 %d 的请求。请参阅 [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) 以获得帮助。否则，请稍后再试。 | 每个数据库的 DTU 最小值；每个数据库的 DTU 最大值 | 弹性池中所有数据库的并发辅助进程（请求）总数尝试超过池限制。 | 可能情况下，请考虑增加弹性池的 DTU 数，以便提高其辅助进程限制，或者从弹性池中删除数据库。 |
| 40844 | EX\_USER | 弹性池中数据库“%ls”（位于服务器“%ls”上）是“%ls”版本的数据库，不能有连续的复制关系。 | 数据库名称、数据库版本、服务器名称 | 针对弹性池中非高级数据库发出 StartDatabaseCopy 命令。 | 即将支持 |
| 40857 | EX\_USER | 找不到服务器“%ls”的弹性池，弹性池名称:“%ls”。 | 服务器名称；弹性池名称 | 指定的弹性池在指定的服务器中不存在。 | 请提供有效的弹性池名称。 |
| 40858 | EX\_USER | 弹性池“%ls”已存在于服务器“%ls”中 | 弹性池名称、服务器名称 | 指定的弹性池已存在于指定的逻辑服务器中。 | 提供新弹性池名称。 |
| 40859 | EX\_USER | 弹性池不支持服务层“%ls”。 | 弹性池服务层 | 进行弹性池设置时，不支持指定服务层。 | 提供正确的版本，或者将服务层留空以使用默认服务层。 |
| 40860 | EX\_USER | 弹性池“%ls”和服务目标“%ls”的组合无效。 | 弹性池名称；服务级别目标名称 | 仅当服务目标指定为 ‘ElasticPool’ 的情况下，才能一起指定弹性池和服务目标。 | 请指定正确的弹性池和服务目标组合。 |
| 40861 | EX\_USER | 数据库版本“%.ls”不能不同于弹性池服务层“%.ls”。| 数据库版本、弹性池服务层 | 数据库版本不同于弹性池服务层。| 请勿指定不同于弹性池服务层的数据库版本。注意，数据库版本不需要指定。|
| 40862 | EX\_USER | 如果指定了弹性池服务目标，则必须指定弹性池名称。| 无 | 弹性池服务目标没有唯一地标识弹性池。| 如果使用弹性池服务目标，请指定弹性池名称。|
| 40864 | EX\_USER | 对于服务层“%.*ls”来说，弹性池的 DTU 数必须至少为 (%d) 个 DTU。| 弹性池的 DTU 数；弹性池服务层。| 尝试将弹性池的 DTU 数设置为最小限制以下。| 请重新尝试将弹性池的 DTU 数至少设置为最小限制。|
| 40865 | EX\_USER | 对于服务层“%.*ls”来说，弹性池的 DTU 数不能超过 (%d) 个 DTU。| 弹性池的 DTU 数；弹性池服务层。| 尝试将弹性池的 DTU 数设置为高出最大限制。| 请重新尝试将弹性池的 DTU 数设置为不超过最大限制。|
| 40867 | EX\_USER | 对于服务层“%.*ls”来说，每个数据库的 DTU 最大值必须至少为 (%d)。| 每个数据库的 DTU 最大值；弹性池服务层 | 尝试将每个数据库的 DTU 最大值设置为支持的限制以下。| 请考虑使用支持所需设置的弹性池服务层。|
| 40868 | EX\_USER | 对于服务层“%.*ls”来说，每个数据库的 DTU 最大值不能超过 (%d)。| 每个数据库的 DTU 最大值；弹性池服务层。| 尝试将每个数据库的 DTU 最大值设置为超出支持的限制。| 请考虑使用支持所需设置的弹性池服务层。|
| 40870 | EX\_USER | 对于服务层“%.*ls”来说，每个数据库的 DTU 最小值不能超出 (%d)。| 每个数据库的 DTU 最小值；弹性池服务层。| 尝试将每个数据库的 DTU 最小值设置为超出支持的限制。| 请考虑使用支持所需设置的弹性池服务层。|
| 40873 | EX\_USER | 数据库数目 (%d) 和每个数据库的 DTU 最小值 (%d) 不能超过弹性池的 DTU 数 (%d)。| 弹性池中的数据库数；每个数据库的 DTU 最小值；弹性池的 DTU 数。| 尝试指定弹性池中数据库的 DTU 最小值，该最小值超出弹性池的 DTU 数。| 请考虑增加弹性池的 DTU 数，或者降低每个数据库的 DTU 最小值，或者降低弹性池中数据库的数目。|
| 40877 | EX\_USER | 除非弹性池不含任何数据库，否则不能将其删除。| 无 | 弹性池包含一个或多个数据库，因此无法将其删除。| 请删除弹性池中的数据库，以便删除弹性池。|
| 40881 | EX\_USER | 弹性池“%.*ls”已达到其数据库计数限制。弹性池的数据库计数限制不能超出 (%d) 是针对 DTU 数为 (%d) 的弹性池的。| 弹性池名称；弹性池的数据库计数限制；资源池的 DTU 数。| 达到弹性池的数据库计数限制时，尝试创建数据库或将其添加到弹性池。| 可能情况下，请考虑增加弹性池的 DTU 数，以便提高其数据库限制，或者从弹性池中删除数据库。|
| 40889 | EX\_USER | 弹性池“%.*ls”的 DTU 数或存储限制不能降低，因为这样就无法为其数据库提供足够的存储空间。| 弹性池的名称。| 尝试将弹性池的存储限制降低到其存储使用量以下。| 请考虑降低弹性池中各个数据库的存储使用量，或者从池中删除数据库以降低其 DTU 或存储限制。|
| 40891 | EX\_USER | 每个数据库的 DTU 最小值 (%d) 不能超过每个数据库的 DTU 最大值 (%d)。| 每个数据库的 DTU 最小值；每个数据库的 DTU 最大值。| 尝试将每个数据库的 DTU 最小值设置为高于每个数据库的 DTU 最大值。| 请确保每个数据库的 DTU 最小值不超过每个数据库的 DTU 最大值。|
| 待定 | EX\_USER | 弹性池中每个数据库的存储大小不能超过“%.*ls”服务层弹性池所允许的最大大小。| 弹性池服务层 | 数据库的最大大小超过弹性池服务层允许的最大大小。| 请将数据库的最大大小设置为处于弹性池服务层允许的最大大小限制范围内。|


## 常规错误


下表列出了不属于任何前一类别的所有常规错误。


|错误代码|严重性|说明|
|---:|---:|:---|
|15006|16|<AdministratorLogin> 不是有效的名称，因为它包含无效字符。|
|18452|14|登录失败。该登录名来自不受信任的域，不能用于 Windows 身份验证。%.&#x2a;ls（此版本的 SQL Server 不支持 Windows 登录名。）|
|18456|14|用户“%.&#x2a;ls”的登录失败。%.&#x2a;ls%.&#x2a;ls（用户“%.&#x2a;ls”的登录失败。密码更改失败。此版本的 SQL Server 不支持在登录过程中更改密码。）|
|18470|14|用户“%.&#x2a;ls”的登录失败。原因：该帐户已禁用。%.&#x2a;ls|
|40014|16|不能在同一个事务中使用多个数据库。|
|40054|16|此版本的 SQL Server 不支持没有聚集索引的表。创建聚集索引，然后重试。|
|40133|15|此版本的 SQL Server 不支持此操作。|
|40506|16|指定的 SID 对此版本的 SQL Server 无效。|
|40507|16|不能使用此版本的 SQL Server 中的参数调用“%.&#x2a;ls”。|
|40508|16|USE 语句不支持在数据库间切换。请使用新连接来连接到其他数据库。|
|40510|16|此版本的 SQL Server 不支持语句“%.&#x2a;ls”|
|40511|16|此版本的 SQL Server 不支持内置函数“%.&#x2a;ls”。|
|40512|16|此版本的 SQL Server 不支持已过时的功能“%ls”。|
|40513|16|此版本的 SQL Server 不支持服务器变量“%.&#x2a;ls”。|
|40514|16|此版本的 SQL Server 不支持“%ls”。|
|40515|16|此版本的 SQL Server 不支持引用“%.&#x2a;ls”中的数据库和/或服务器名称。|
|40516|16|此版本的 SQL Server 不支持全局临时对象。|
|40517|16|此版本的 SQL Server 不支持关键字或语句选项“%.&#x2a;ls”。|
|40518|16|此版本的 SQL Server 不支持 DBCC 命令“%.&#x2a;ls”。|
|40520|16|此版本的 SQL Server 不支持安全对象类“%S\_MSG”。|
|40521|16|此版本的 SQL Server 不支持服务器范围中的安全对象类“%S\_MSG”。|
|40522|16|此版本的 SQL Server 不支持数据库主体“%.&#x2a;ls”类型。|
|40523|16|此版本的 SQL Server 不支持创建隐式用户“%.&#x2a;ls”。请在使用该用户前显式创建它。|
|40524|16|此版本的 SQL Server 不支持数据类型“%.&#x2a;ls”。|
|40525|16|此版本的 SQL Server 不支持 WITH“%.ls”。|
|40526|16|此版本的 SQL Server 不支持“%.&#x2a;ls”行集提供程序。|
|40527|16|此版本的 SQL Server 不支持链接服务器。|
|40528|16|此版本的 SQL Server 用户不能映射为证书、非对称密钥或 Windows 登录名。|
|40529|16|此版本的 SQL Server 不支持模拟上下文中的内置函数“%.&#x2a;ls”。|
|40532|11|无法打开该登录请求的服务器“%.&#x2a;ls”。登录失败。|
|40553|16|由于过度使用内存，已终止该会话。请尝试修改你的查询以处理更少的行。<br/><br/>注意：在你的 Transact-SQL 代码中减少 `ORDER BY` 和 `GROUP BY` 操作数可以帮助降低查询的内存要求。|
|40604|16|由于将超过服务器的配额，无法执行 CREATE/ALTER DATABASE。|
|40606|16|此版本的 SQL Server 不支持附加数据库。|
|40607|16|此版本的 SQL Server 不支持 Windows 登录名。|
|40611|16|服务器上最多可以定义 128 个防火墙规则。|
|40614|16|防火墙规则的开始 IP 地址不能超过结束 IP 地址。|
|40615|16|无法打开该登录请求的服务器“{0}”。不允许具有 IP 地址 '{1}' 的客户端访问服务器。若要允许访问，请使用 SQL 数据库门户，或者对 master 数据库运行 sp\_set\_firewall\_rule 以便为此 IP 地址或地址范围创建防火墙规则。为使此更改生效，最多可能需要 5 分钟。|
|40617|16|以 <rule name> 开头的防火墙规则名称过长。最大长度为 128。|
|40618|16|防火墙规则名称不能为空。|
|40620|16|用户“%.&#x2a;ls”的登录失败。密码更改失败。此版本的 SQL Server 不支持在登录过程中更改密码。|
|40627|20|正在对服务器“{0}”和数据库 '{1}' 进行操作。请等待几分钟，然后重试。|
|40630|16|密码验证失败。该密码太短，不符合策略要求。|
|40631|16|指定的密码过长。密码的长度不能超过 128 个字符。|
|40632|16|密码验证失败。该密码不够复杂，不符合策略要求。|
|40636|16|无法在此操作中使用保留的数据库名称“%.&#x2a;ls”。|
|40638|16|订阅 ID <subscription-id> 无效。订阅不存在。|
|40639|16|请求不符合架构: <schema error>。|
|40640|20|服务器遇到意外的异常。|
|40641|16|指定的位置无效。|
|40642|17|服务器当前太忙。请稍后重试。|
|40643|16|指定的 x-ms-version 标头值无效。|
|40644|14|授权访问指定的订阅失败。|
|40645|16|服务器名称 <servername> 不能为空或 null。它只能由小写字母“a”-“z”、数字 0-9 和连字符组成。连字符不能位于名称的开头或结尾。|
|40646|16|订阅 ID 不能为空。|
|40647|16|订阅 <subscription-id 不包含服务器 servername。|
|40648|17|执行了过多请求。请稍后重试。|
|40649|16|指定的内容类型无效。仅支持应用程序/xml。|
|40650|16|订阅 <subscription-id> 不存在或者不可供操作。|
|40651|16|无法创建服务器，因为订阅 <subscription-id> 被禁用。|
|40652|16|无法移动或创建服务器。订阅 <subscription-id> 将超出服务器配额。|
|40671|17|网关与管理服务之间的通信失败。请稍后重试。|
|40852|16|无法打开该登录请求的数据库 “%.*ls”（在服务器 “%. *ls” 上）。仅允许使用已启用安全性的连接字符串访问数据库。若要访问此数据库，请修改你的连接字符串以在服务器 FQDN 中包含“secure”-“服务器名称”.database.chinacloudapi.cn 应修改为 “服务器名称”.database.`secure`.chinacloudapi.cn。|
|45168|16|SQL Azure 系统负载过小，正在设置单个服务器的并发 DB CRUD 操作（例如 create database）数的上限。在错误消息中指定的服务器已超过最大并发连接数。请稍后重试。|
|45169|16|SQL Azure 系统负载过小，正在设置单个订阅的并发服务器 CRUD 操作（例如 create server）数的上限。在错误消息中指定的订阅已超过最大并发连接数，已拒绝请求。请稍后重试。|


## 相关链接

- [Azure SQL 数据库的一般性限制和指导原则](/documentation/articles/sql-database-general-limitations/)
- [Azure SQL 数据库资源限制](/documentation/articles/sql-database-resource-limits/)

<!---HONumber=Mooncake_0509_2016-->