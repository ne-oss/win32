---
Description: Retrieves a value that specifies that the RMS cluster supports a licensing service.
audience: developer
author: REDMOND\\markl
manager: REDMOND\\mbaldwin
ms.assetid: 69224df1-0bc3-4cc2-9f65-87a84901769d
ms.prod: windows-server-dev
ms.technology: active-directory-rights-management
ms.tgt_platform: multiple
title: Constants.ClusterTypeLicensing property
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Constants.ClusterTypeLicensing property

The **ClusterTypeLicensing** property retrieves a value that specifies that the RMS cluster supports a licensing service.

This property is read-only.

## Syntax


```VB
Constants.ClusterTypeLicensing
```



## Property value

This property returns an integer value (0x1).

## Remarks

You can use this property value with the [**ClusterType**](clusterinformation-clustertype-property.md) property on the [**ClusterInformation**](clusterinformation-object.md) object. The **ClusterType** property retrieves a value that specifies whether the AD RMS cluster supports the RMS Licensing web service or the RMS Account Certification web service.

## Examples


```VB
DIM config_manager
DIM admin_role

' *******************************************************************
' Create and initialize a ConfigurationManager object.

SUB InitObject()

  CALL WScript.Echo( "Create ConfigurationManager object...")
  SET config_manager = CreateObject _
    ("Microsoft.RightsManagementServices.Admin.ConfigurationManager")      
  CheckError()
    
  CALL WScript.Echo( "Initialize...")
  admin_role=config_manager.Initialize(false,"localhost",80,"","","")
  CheckError()

END SUB

' *******************************************************************
' Retrieve the cluster type.

SUB GetAdminMail()

  DIM cluster_info
  DIM type
  DIM constant
 
  ' Retrieve the ClusterInformation object.
  SET cluster_info = config_manager.ClusterInformation
  CheckError()

  ' Retrieve the Constants object.
  SET constant = config_manager.Constants

  ' Retrieve the cluster type.
  type = cluster_info.ClusterType

  ' Display the type.
  IF type = constant.ClusterTypeLicensing THEN
    CALL WScript.Echo("Licensing is supported.")
  ELSEIF type = constant.ClusterTypeCertification THEN
    CALL WScript.Echo("Certification is supported.")
  ELSEIF type = 0
    CALL WScript.Echo("Support is undefined.")
  ELSE
   CALL WScript.Echo("ClusterType error.")
  END IF

END SUB


' *******************************************************************
' Error checking function.

FUNCTION CheckError()
  CheckError = Err.number
  IF Err.number <> 0 THEN
    CALL WScript.Echo( vbTab & "*****Error Number: " _
                       & Err.number _
                       & " Desc:" _
                       & Err.Description _
                       & "*****")
    WScript.StdErr.Write(Err.Description)
    WScript.Quit( Err.number )
  END IF
END FUNCTION

' *******************************************************************
' Generate a runtime error.

SUB RaiseError(errId, desc)
  CALL Err.Raise( errId, "", desc )
  CheckError()
END SUB
```



## Requirements



|                                     |                                                                                                                         |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | None supported<br/>                                                                                               |
| Minimum supported server<br/> | Windows Server 2008<br/>                                                                                          |
| Assembly<br/>                 | <dl> <dt>Microsoft.RightsManagementServices.Admin.dll</dt> </dl> |



## See also

<dl> <dt>

[**Constants**](constants-object.md)
</dt> <dt>

[**ClusterTypeCertification**](constants-clustertypecertification-property.md)
</dt> </dl>

 

 



