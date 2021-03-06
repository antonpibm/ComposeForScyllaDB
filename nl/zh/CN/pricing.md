---

copyright:
  years: 2016,2018
lastupdated: "2018-01-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 定价
{: #pricing}

## 基本配置

{{site.data.keyword.composeForScyllaDB_full}} 服务一开始有 3 个成员节点，每个节点有 5 GB 存储空间和 512 MB 内存，相当于 5 个资源单元。该服务_包含_复制和高可用性，因此每个单元和单元单价都_包含_所有三个节点上资源的成本。

基本配置还包含 3 个 HAProxy 门户网站，用于提供与集群的连接。每个 HAProxy 门户网站有 64 MB，支持认证、HTTPS 和 IP 白名单。

基本服务配置具有固定价格。有关以本地货币为单位的基础定价，请查阅 {{site.data.keyword.cloud_notm}} 上的“目录”磁贴。例如，以美元为单位的基础价格是 90 美元/月。

## 增加资源
如果服务需要更多存储空间或内存，您可以增加按 10:1 的磁盘存储量与内存单元比率分配的资源。增大分配给部署的磁盘还会增加分配的 RAM。一个 {{site.data.keyword.composeForScyllaDB}} 单元包含 1 GB 存储空间和 102 MB 内存，每个单元和单元单价都_包含_增加集群所有三个节点上资源的成本。

## 计算部署成本
{: #tiered-pricing}

每增加一个单元（1 GB 存储空间/102 MB 内存）都会有相应的单元单价，该单价会在服务的 {{site.data.keyword.cloud_notm}}“目录”磁贴上以本地货币列出。以美元为单位时，每增加一个单元需要的成本是 18 美元。随着所有 {{site.data.keyword.composeForScyllaDB}} 服务的_总_大小增加，单元单价会下降，如下面的分层定价表所示。

单元数|单元单价
----------|-----------
5 - 9 个单元|基础单元单价 -- 18.00 美元/单元
10 - 24 个单元|10% 折扣 -- 16.20 美元/单元
25 - 49 个单元|20% 折扣 -- 14.40 美元/单元
50 - 99 个单元|30% 折扣 -- 12.60 美元/单元
100 - 499 个单元|40% 折扣 -- 10.80 美元/单元
500 - 999 个单元|50% 折扣 -- 9.00 美元/单元
1,000 - 4,999 个单元|60% 折扣 -- 7.20 美元/单元
5,000 个及以上单元|70% 折扣 -- 5.40 美元/单元
{: caption="表 1. {{site.data.keyword.composeForScyllaDB}} 分层定价" caption-side="top"}
