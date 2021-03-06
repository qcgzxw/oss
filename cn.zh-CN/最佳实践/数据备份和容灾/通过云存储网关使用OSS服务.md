# 通过云存储网关使用OSS服务 {#concept_jnq_2ww_y2b .concept}

## 简介 {#section_g2s_gww_y2b .section}

对象存储OSS是海量分布式对象存储服务，提供标准、低频、归档存储类型，覆盖从热到冷的不同存储场景。OSS提供RESTful API，您可以从任何位置访问OSS，存储容量和处理能力弹性扩展。

阿里云云存储网关是一款帮助客户在现有本地应用程序、基础设施和数据存储与阿里云的存储服务之间实现无缝集成的数据服务。通过可在本地和云上部署的兼容行业标准存储协议的虚拟设备，云存储网关将现有的存储应用程序和工作负载无缝对接阿里云的存储和计算服务。有关云存储网关更多详情，请参见[云存储网关产品详情页](https://www.aliyun.com/product/hcs)。

## 应用场景 {#section_y34_kww_y2b .section}

云存储网关与OSS结合，主要有以下几种应用场景：

-   云端扩容

    云存储网关以阿里云OSS为后端存储。OSS是阿里云提供的海量、安全、低成本、高可靠、高可用的云存储服务。相比阿里云的EBS和NAS服务，OSS可以提供更高的容量以及更低的存储成本，这对于有存放海量数据需求的客户来说是一个很好的选择。OSS默认支持RESTful的对象接口，或者通过Hadoop来访问OSS。

    您还可以使用云存储网关通过NFS、SMB、iSCSI来访问OSS，使得传统应用程序可以像使用文件夹或者块设备一样使用OSS。您无需改造现有应用，只需开通和简单配置云存储网关即可。主要有以下几种应用场景：

    -   共享文件池：在不同计算集群之间共享文件和数据。
    -   数据备份：通过类似Veaam、NBU等备份软件，将应用数据按一定的策略通过云存储网关将数据备份到阿里云的OSS存储服务。
    -   冷数据归档：通过云存储网关将冷数据从线下或者ECS实例中写入OSS的低频和归档存储类型，从而释放本地空间，并减少存储成本。
-   跨地域共享和数据分发

    云存储网关对接的OSS存储服务可以通过网络实现线下和线上多地访问。通过云存储网关，您可以轻松实现一处写入多地读取的数据共享服务。

    在下图的示例场景中，杭州的用户数据中心产生大量数据，但因本地计算能力不足，您可以在阿里云上弹性扩展ECS集群。您在阿里云ECS集群里的应用既可以直接读取网关上传到OSS的对象数据，也可以部署和线下一样的应用程序，通过云存储网关读取OSS共享的数据。

    计算任务完成后，最终的结果写回到OSS。如果需要将数据发布到北京，您可以通过存储网关把数据反向同步到北京的数据中心，也可以直接通过OSS的外链功能把数据开放给认证的客户端下载。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18894/153564281510760_zh-CN.png)

-   适配传统应用

    很多云上业务都是新旧业务的组合。其中，从数据中心迁移过来的旧业务使用的是标准的存储协议，如NFS、CIFS和iSCSI；新业务一般采用比较新的技术，支持对象访问的协议。要实现新旧业务之间的数据通讯需要大量的改造工作。而云存储网关恰好可以起到桥梁作用，便捷地进行新旧业务的数据交换。

    在下图的示例场景中，传统应用集群可能是文件处理、邮件服务器、图片处理等传统的应用，它们只支持文件或者块访问协议。此时云存储网关的在线版本可以兼容不同的数据访问协议，让这些传统应用集群可以直接访问OSS中的数据。传统应用集群产生的中间数据存储在OSS后，新应用集群可以直接从OSS中读取。反之，新应用集群收集了数据并存放在OSS中，传统应用集群通过云存储网关也可以无缝访问和处理OSS中的数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18894/153564281510750_zh-CN.png)

-   云容灾

    随着云计算的普及，越来越多的用户选择将业务部署到云上。为确保数据安全性和业务连续性，跨云容灾提供了高效可靠的本地备份恢复和云上故障转移。借助云存储网关对虚拟化的全面支持，可以轻松应对各种第三方云厂商对接阿里云的数据容灾。整体架构如下图所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18894/153564281510752_zh-CN.png)

    通过在第三方云厂商导入阿里云的云储存网关（OVA、VHD格式），并配置公网IP，即可搭建阿里云OSS的数据通路。完成数据复制后，部署在阿里云上的ECS即可通过云上部署的阿里云存储网关获取容灾数据，并恢复服务。


## 注意事项 {#section_nrb_lzw_y2b .section}

云存储网关目前还在公测和开发中。使用云储存网关前，您需要了解以下注意事项：

-   云存储网关的HA能力还在开发中。如果有HA需求的业务，可以使用阿里云的其它存储服务。
-   云存储网关目前最大的IOPS依赖于底层的缓存设备能力。公测设备的最大IOPS为5000，带宽为140MByte/s（文件）和900MByte/s（块透写模式）。如果有更高的性能要求，推荐使用阿里云的高性能NAS服务。

