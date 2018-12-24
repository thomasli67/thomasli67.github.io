---
layout: post
title:  "SQL Server AlwaysOn 可用组服务器清单"
date:   2018-12-24 15:00:45 +0800
categories: SQL Server
tags: 
 - SQL Server
 - AlwaysOn
---

## SQL Server AlwaysOn 可用性组服务器清单


用以下SQL脚本可以获得SQL Server AlwaysOn 可用性组中的服务器清单。在PRIMARY角色的服务器上执行，可以获得所有服务器；在SECONDARY角色的服务器上执行，只获得本地服务器。    
```
SELECT
    ag.name AS 'GroupName' 
   ,cs.replica_server_name AS 'Replica'
   ,rs.role_desc AS 'Role'
   ,REPLACE(ar.availability_mode_desc,'_',' ') AS 'AvailabilityMode'
   ,ar.failover_mode_desc AS 'FailoverMode'
   ,ar.primary_role_allow_connections_desc AS 'ConnectionsInPrimaryRole'
   ,ar.secondary_role_allow_connections_desc AS 'ConnectionsInSecondaryRole'
   ,ar.seeding_mode_desc AS 'SeedingMode'
   ,ar.endpoint_url AS 'EndpointURL'
   ,al.dns_name AS 'Listener'
FROM sys.availability_groups ag
JOIN sys.dm_hadr_availability_group_states ags ON ag.group_id = ags.group_id
JOIN sys.dm_hadr_availability_replica_cluster_states cs ON ags.group_id = cs.group_id 
JOIN sys.availability_replicas ar ON ar.replica_id = cs.replica_id 
JOIN sys.dm_hadr_availability_replica_states rs  ON rs.replica_id = cs.replica_id 
LEFT JOIN sys.availability_group_listeners al ON ar.group_id = al.group_id
```

在 Primary server上的执行结果，见下图： 

![](/assets/images/2018-12-24-SQL-Server-AlwaysOn-Availability-Groups-Inventory-1.png)

在 Secondary server上的执行结果，见下图： 

![](/assets/images/2018-12-24-SQL-Server-AlwaysOn-Availability-Groups-Inventory-2.png)

## 更多参考资源:

[SQL Server AlwaysOn Availability Groups Inventory and Monitoring Scripts - Part 1](https://www.mssqltips.com/sqlservertip/5782/sql-server-alwayson-availability-groups-inventory-and-monitoring-scripts--part-1/)
[sys.dm_hadr_availability_replica_states (Transact-SQL)](https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql?view=sql-server-2017)

### Important

To obtain information about every replica in a given availability group, query sys.dm_hadr_availability_replica_states on the server instance that is hosting the primary replica. When queried on a server instance that is hosting a secondary replica of an availability group, this dynamic management view returns only local information for the availability group.





