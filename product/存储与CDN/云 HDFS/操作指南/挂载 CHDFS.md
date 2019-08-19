
创建 CHDFS 及挂载点后，可以通过挂载点挂载 CHDFS，本文为您详细介绍如何挂载 CHDFS。

## 前提条件
- 确保挂载的机器或者容器内安装了 Java 1.8。
- 确保挂载的 CVM、CPM 2.0或者容器其 VPC ，与挂载点指定 VPC 相同。
- 确保挂载的 CVM、CPM 2.0或者容器其 VPC IP，与挂载点指定权限组中有一条权限规则授权地址匹配。

## 操作步骤
1.	编译 JAR 包（mvn clean package），或者下载已编译好的 JAR 包`chdfs_hadoop_plugin-x.y.z-shaded.jar`。
2.	将 JAR 包放置对应的目录下，对于 emr 集群，可同步到所有节点的`/usr/local/service/hadoop/share/hadoop/common/lib/`目录下。
3.	编辑 core-site.xml 文件，新增以下配置：
```
<property>
        <name>fs.AbstractFileSystem.chdfs.impl</name>
        <value>chdfs.x.y.z.com.qcloud.chdfs.fs.CHDFSDelegateFS</value>
</property>
<property>
        <name>fs.chdfs.impl</name>
        <value>chdfs.x.y.z.com.qcloud.chdfs.fs.CHDFSHadoopFileSystem</value>
</property>
<property>
        <name>fs.defaultFS</name>
        <value>chdfs://ChdfsMountpointDomainName</value>
</property>
```
4.	编辑 core-site.xml 文件，配置如下相应的配置参数，并且同步到所有依赖 hadoop-common 的节点上。
```
<property>
        <name>fs.chdfs.block.max.memory.cache.mb</name>
        <value>16</value>
</property>
<property>
        <name>fs.chdfs.block.max.file.cache.mb</name>
        <value>256</value>
</property>
```
5.	使用 hadoop fs 命令行工具，执行`hadoop fs –ls /`命令，如果正常列出文件列表，则说明已经成功挂载 CHDFS。
