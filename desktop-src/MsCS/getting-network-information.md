---
title: Getting Network Information
description: The following table lists network-related information and objects and the recommended API elements for obtaining them. If the specified API element does not have an example, look in the Index of Examples for a similar API element.
audience: developer
author: REDMOND\\markl
manager: REDMOND\\markl
ms.assetid: 96efb0da-b072-4e68-bcd1-32c28b754eca
ms.prod: windows-server-dev
ms.technology: failover-clustering
ms.tgt_platform: multiple
keywords:
- networks Failover Cluster , retrieving information
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Getting Network Information

The following table lists [network](networks.md)-related information and objects and the recommended API elements for obtaining them. If the specified API element does not have an example, look in the [Index of Examples](index-of-examples.md) for a similar API element.



| Information or object to get                                | API element to use                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cluster that contains the network                           | [**GetClusterFromNetwork**](/previous-versions/windows/desktop/api/ClusAPI/nc-clusapi-pclusapi_get_cluster_from_network)                                                                                                                                                                                                                                                                                                                            |
| GUID identifying the network                                | [CLUSCTL\_NETWORK\_GET\_ID](clusctl-network-get-id.md)                                                                                                                                                                                                                                                                                                                           |
| Name of the network                                         | [CLUSCTL\_NETWORK\_GET\_NAME](clusctl-network-get-name.md)                                                                                                                                                                                                                                                                                                                       |
| [Network interfaces](network-interfaces.md) on the network | [**ClusterNetworkOpenEnum**](/previous-versions/windows/desktop/api/ClusAPI/nc-clusapi-pclusapi_cluster_network_open_enum), [**ClusterNetworkEnum**](/previous-versions/windows/desktop/api/ClusAPI/nc-clusapi-pclusapi_cluster_network_enum), [**ClusterNetworkCloseEnum**](/previous-versions/windows/desktop/api/ClusAPI/nc-clusapi-pclusapi_cluster_network_close_enum)                                                                                                                                                                                                            |
| Properties of the network                                   | [CLUSCTL\_NETWORK\_GET\_COMMON\_PROPERTIES](clusctl-network-get-common-properties.md), [CLUSCTL\_NETWORK\_GET\_RO\_COMMON\_PROPERTIES](clusctl-network-get-ro-common-properties.md), [CLUSCTL\_NETWORK\_GET\_PRIVATE\_PROPERTIES](clusctl-network-get-private-properties.md), [CLUSCTL\_NETWORK\_GET\_RO\_PRIVATE\_PROPERTIES](clusctl-network-get-ro-private-properties.md), |
| Property names                                              | [CLUSCTL\_NETWORK\_ENUM\_COMMON\_PROPERTIES](clusctl-network-enum-common-properties.md), [CLUSCTL\_NETWORK\_ENUM\_PRIVATE\_PROPERTIES](clusctl-network-enum-private-properties.md)                                                                                                                                                                                              |
| State of the network                                        | [**GetClusterNetworkState**](/previous-versions/windows/desktop/api/ClusAPI/nc-clusapi-pclusapi_get_cluster_network_state)                                                                                                                                                                                                                                                                                                                          |



 

For additional network-related API elements, see [Network Management Functions](network-management-functions.md) and [Network Control Codes](network-control-codes.md).

 

 



