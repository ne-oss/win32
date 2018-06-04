---
title: Remark
description: Provides a description of the Distributed File System resource.
audience: developer
author: REDMOND\\markl
manager: REDMOND\\markl
ms.assetid: de99484e-5526-42f2-a1aa-1be29d7c4bb1
ms.prod: windows-server-dev
ms.technology: failover-clustering
ms.tgt_platform: multiple
keywords:
- Remark Failover Cluster ,for file system
- Remark Failover Cluster
topic_type:
- apiref
api_name:
- Remark
api_type:
- NA
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Remark

Provides a description of the Distributed File System [resource](resources.md). The following table summarizes the attributes of the **Remark** property.



| Attribute | Value                                                            |
|-----------|------------------------------------------------------------------|
| Data type | Null-terminated Unicode string                                   |
| Access    | [Read/write](read-write-properties.md)                          |
| Status    | Optional                                                         |
| Structure | [**CLUSPROP\_SZ**](/previous-versions/windows/desktop/api/ClusAPI/)                              |
| Maximum   | None (but see [Maximum Property Size](maximum-string-size.md).) |
| Default   | **NULL**                                                         |



 

## Remarks

The [**CLUSPROP\_SZ\_DECLARE**](/previous-versions/windows/desktop/api/ClusAPI/nf-clusapi-clusprop_sz_declare) macro creates a [**CLUSPROP\_SZ**](/previous-versions/windows/desktop/api/ClusAPI/) structure with an array of the correct size.

## Examples

The property value portion of a [property list](property-lists.md) entry for **Remark** can be set with the following example code.


```C++
WCHAR                szRemarkData[] = L"File Share Description";
CLUSPROP_SZ_DECLARE( RemarkValue, 
                     sizeof(szRemarkData) / sizeof(WCHAR) );

RemarkValue.Syntax.dw = CLUSPROP_SYNTAX_LIST_VALUE_SZ;
RemarkValue.cbLength  =  sizeof( szRemarkData );
StringCbCopy( RemarkValue.sz, RemarkValue.cbLength, szRemarkData );
```



## Requirements



|                                     |                                                                           |
|-------------------------------------|---------------------------------------------------------------------------|
| Minimum supported client<br/> | None supported<br/>                                                 |
| Minimum supported server<br/> | Windows Server 2008 Datacenter, Windows Server 2008 Enterprise<br/> |



## See also

<dl> <dt>

[Distributed File System Private Properties](distributed-file-system-private-properties.md)
</dt> <dt>

[**CLUSPROP\_SZ**](/previous-versions/windows/desktop/api/ClusAPI/)
</dt> <dt>

[**CLUSPROP\_SZ\_DECLARE**](/previous-versions/windows/desktop/api/ClusAPI/nf-clusapi-clusprop_sz_declare)
</dt> </dl>

 

 




