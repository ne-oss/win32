---
title: WM\_POINTERCAPTURECHANGED message
description: Sent to a window that is losing capture of an input pointer.
ms.assetid: 6eec37da-227c-4be1-bf0b-98704caa1322
keywords:
- WM_POINTERCAPTURECHANGED message Input Messages and Notifications
topic_type:
- apiref
api_name:
- WM_POINTERCAPTURECHANGED
api_location:
- Winuser.h
api_type:
- HeaderDef
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# WM\_POINTERCAPTURECHANGED message

Sent to a window that is losing capture of an input pointer.

A window receives this message through its [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573) function.


```C++
#define WM_POINTERCAPTURECHANGED           0x024C
```



## Parameters

<dl> <dt>

*wParam* 
</dt> <dd>

Contains information about the input pointer that is being lost. Use [**GET\_POINTERID\_WPARAM**](/windows/desktop/api/Winuser/nf-winuser-get_pointerid_wparam) to get the pointer ID.

</dd> <dt>

*lParam* 
</dt> <dd>

Contains a handle to the window that is capturing the input pointer. This value can be NULL if the pointer is no longer being captured by the window.

If this message is generated from internal processing, the value can be the handle of the window receiving the message.

</dd> </dl>

## Return value

If an application processes this message, it should return zero.

If the application does not process this message, it should call [**DefWindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633572).

## Remarks

A window should use this notification to stop processing subsequent messages and initiate any cleanup required for the pointer being lost. Processing of gestures associated with the pointer should also be terminated (for example, by calling [**StopInteractionContext**](https://msdn.microsoft.com/library/windows/desktop/hh437240)) and remaining contacts re-associated with the window.

Typically, if a window receives the **WM\_POINTERCAPTURECHANGED** notification, no subsequent notifications related to the input pointer are received. Because of this, do not depend on paired notifications such as [**WM\_POINTERENTER**](wm-pointerenter.md) and [**WM\_POINTERLEAVE**](wm-pointerleave.md).

**WM\_POINTERCAPTURECHANGED** does not include [**POINTER\_INFO**](/windows/desktop/api/Winuser/ns-winuser-tagpointer_info) data. Other than the [**POINTER\_FLAG\_CAPTURECHANGED**](pointer-flags-contants.md) flag being set, the data returned by [**GetPointerInfo**](/windows/desktop/api/Winuser/nf-winuser-getpointerinfo) (or any variant) is identical to that returned prior to the notification.

If the application does not process this notification, [**DefWindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633572) may generate one or more [**WM\_GESTURE**](https://msdn.microsoft.com/library/windows/desktop/dd353242) messages or, if a gesture is not recognized, **DefWindowProc** may generate mouse input.

If an application selectively consumes some pointer input and passes the rest to [**DefWindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633572), the resulting behavior is undefined.

## Requirements



|                                     |                                                                                                          |
|-------------------------------------|----------------------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows 8 \[desktop apps only\]<br/>                                                               |
| Minimum supported server<br/> | Windows Server 2012 \[desktop apps only\]<br/>                                                     |
| Header<br/>                   | <dl> <dt>Winuser.h (include Windows.h)</dt> </dl> |



## See also

<dl> <dt>

[Messages](messages.md)
</dt> </dl>

 

 




