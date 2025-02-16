# 将分片集群转换为副本集

本教程描述了将分片集群转换为非分片副本集的过程。将副本集转换为分片集群 。

## 将具有单个分片的集群转换为副本集

在只有一个分片的分片集群的情况下，该分片包含完整的数据集。使用以下过程将该集群转换为非分片副本集：

1. 重新配置应用程序以连接到托管单个分片的副本集的主节点，该系统将成为新的副本集。

2. 从`mongod`移除`--shardsvr`

   

   >## 提示
   >
   >更改`--shardsvr`选项`mongod`将更改侦听传入连接的端口。

   

单分片集群现在是一个非分片副本集，它将接受对数据集的读写操作。

您现在可以停用剩余的分片基础设施。

## 将分片集群转换为副本集

使用以下过程从具有多个分片的分片集群过渡到全新的副本集。

1. 在分片集群运行的情况下，除了分片集群之外，还要部署一个新的副本集。副本集必须有足够的容量来容纳来自所有当前分片的所有数据文件。在数据传输完成之前，不要将应用程序配置为连接到新的副本集。

2. 停止对分片集群的所有写入。您可以重新配置您的应用程序或停止所有`mongos`实例。如果您停止所有`mongos`实例，应用程序将无法从数据库中读取。如果您停止所有`mongos` 实例，请在该应用程序无法访问的数据迁移过程中启动一个临时`mongos`实例。启动一个临时`mongos`实例。

3. 使用mongodump 和 mongorestore将数据从`mongos`实例迁移到新的 副本集。

   

   >## 笔记
   >
   >并非所有数据库上的所有集合都必须分片。不要单独迁移分片集合。确保所有数据库和所有集合都正确迁移。

   

4. 重新配置应用程序以使用非分片副本集而不是`mongos`实例。

该应用程序现在将使用未分片的副本集进行读取和写入。您现在可以停用剩余的未使用的分片集群基础设施。





原文 -  https://docs.mongodb.com/manual/tutorial/convert-sharded-cluster-to-replica-set/ 

译者：陆文龙
