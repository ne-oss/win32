---
Description: The RealTimeStylus class is part of the StylusInput application programming interfaces (APIs). The following sections describe key elements of the RealTimeStylus class and the StylusInput APIs.
ms.assetid: 6385c387-5b11-4719-a6ec-e0bebe875348
title: Working with the RealTimeStylus Class
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Working with the RealTimeStylus Class

The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) class is part of the StylusInput application programming interfaces (APIs). The following sections describe key elements of the **RealTimeStylus** class and the StylusInput APIs.

## Instantiating the RealTimeStylus Class

When you create a [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object, you have the option of attaching it to a window handle or to a control. Attaching the **RealTimeStylus** object to a window handle requires additional permissions. For more information about these permissions, see [Partial Trust Considerations for the StylusInput APIs](partial-trust-considerations-for-the-stylusinput-apis.md).

> [!Note]  
> You cannot attach the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object to a window or control in a different process.

 

If you use the default constructor, you create a [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object that can only accept input from another **RealTimeStylus** object. For more information about connecting two **RealTimeStylus** objects, see [The Cascaded RealTimeStylus Model](the-cascaded-realtimestylus-model.md).

The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object implements the [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) interface.

## Extending the RealTimeStylus Class

To allow your plug-ins to interact with the data stream from the tablet pen, the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object maintains two plug-in collections, which are accessible by the [**GetStylusSyncPlugin**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-getstylussyncplugin) and the [**GetStylusAsyncPlugin**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-getstylusasyncplugin) methods in C++ and by the [SyncPluginCollection](https://www.bing.com/search?q=SyncPluginCollection) and [AsyncPluginCollection](https://www.bing.com/search?q=AsyncPluginCollection) properties in managed code. You can add a plug-in by calling either the [**AddStylusSyncPlugin**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-addstylussyncplugin) or the [**AddStylusAsyncPlugin**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-addstylusasyncplugin) method of the collection within the appropriate property. For more information about creating and using plug-ins, see [Plug-ins and the RealTimeStylus Class](plug-ins-and-the-realtimestylus-class.md). For information about deciding whether to create a synchronous or asynchronous plug-in for a particular task, see [Threading Considerations for the StylusInput APIs](threading-considerations-for-the-stylusinput-apis.md) and [Performance Considerations for the StylusInput APIs](performance-considerations-for-the-stylusinput-apis.md).

Synchronous plug-ins must implement the [**IStylusSyncPlugin**](/windows/desktop/api/RTSCom/) interface, and asynchronous plug-ins must implement the [**IStylusAsyncPlugin**](/windows/desktop/api/RTSCom/) interface. Each plug-in has a [**IStylusSyncPlugin.DataInterest**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-datainterest) or [**IStylusAsyncPlugin.DataInterest**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-datainterest) property. The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object calls only the notification methods of the plug-in for methods in which the plug-in has subscribed. For more information about the notification methods, see [Plug-in Data and the RealTimeStylus Class](plug-in-data-and-the-realtimestylus-class.md).

The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object implements the [**IStylusAsyncPlugin**](/windows/desktop/api/RTSCom/) interface. The only way to instantiate a **RealTimeStylus** object that accepts input from another **RealTimeStylus** object is to use the default constructor and implement the cascaded **RealTimeStylus** model. For more information about connecting two **RealTimeStylus** objects see [The Cascaded RealTimeStylus Model](the-cascaded-realtimestylus-model.md).

The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object has two internal queues that carry the tablet pen data, the input queue and the output queue. The pen data is converted into instances of the classes in the [Microsoft.StylusInput.PluginData](https://www.bing.com/search?q=Microsoft.StylusInput.PluginData) namespace. The following list describes how the **RealTimeStylus** object handles the tablet pen data.

1.  The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object checks for plug-in data objects first on its input queue and then from the tablet pen data stream.
2.  The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object sends one plug-in data object to the objects in its synchronous plug-in collection. Each synchronous plug-in can add data to either the input or output queue.
3.  Once the plug-in data object has been sent to all members of the synchronous plug-in collection, the plug-in data object is placed on the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object's output queue.
4.  The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object then checks for the next plug-in data object to process.
5.  While the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object's output queue contains data, the **RealTimeStylus** object sends one plug-in data object from its output queue to the objects in its asynchronous plug-in collection. Each asynchronous plug-in can add data to either the input or output queue, but since the asynchronous plug-ins run on the user interface (UI) thread, the data is added to the queue in relation to the current pen data the **RealTimeStylus** object is processing, and not in relation to the data that the asynchronous plug-in is processing.

The following diagram illustrates the flow of tablet pen data through the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object and its plug-in collections.

![illustratiom showing tablet pc pen data flow](images/5a698bc9-833b-4b24-9fa2-70be0ca61032.gif)

In this diagram the circles lettered "A" and "B" represent tablet pen data that has already been added to the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object's output queue and that has not yet been sent to the asynchronous plug-in collection. The circle lettered "C" represents the tablet pen data that the **RealTimeStylus** object is currently processing. It is sent to the synchronous plug-in collection and placed on the output queue. The empty circle represents the position in the output queue where future tablet pen data is added.

For more information about how specific data is added to the queue and processed, see [Plug-in Data and the RealTimeStylus Class](plug-in-data-and-the-realtimestylus-class.md).

The following is a minimal scenario for using the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object on a form that collects ink.

1.  Create a form that implements the [**IStylusAsyncPlugin**](/windows/desktop/api/RTSCom/) interface.
2.  Create a [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object attached to a control on the form.
3.  Set interest in the StylusDown, Packets, and StylusUp notifications in the form's [**IStylusAsyncPlugin.DataInterest**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-datainterest) property.
4.  In the form's [**StylusDown**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-stylusdown), [**Packets**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-packets), and [**StylusUp**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-stylusup) methods, add code to handle the stylus down, packets, and stylus up notifications that are sent from the form's [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object.

For a sample of such an application, see the [RealTimeStylus Ink Collection Sample](realtimestylus-ink-collection-sample.md).

## Working with Tablet Objects

Each enabled [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object maintains a list of unique identifiers for the [**Tablet**](/windows/desktop/api/msinkaut/nn-msinkaut-iinktablet) objects it can interact with. The **RealTimeStylus** object exposes two methods for translating between the unique identifier and the **Tablet** object: the [**GetTabletContextIdFromTablet**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-gettabletcontextidfromtablet) and [**GetTabletFromTabletContextId**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-gettabletfromtabletcontextid) methods.

The [TabletPropertyDescription](https://www.bing.com/search?q=TabletPropertyDescription) object (in managed code) contains a [PacketPropertyId](https://www.bing.com/search?q=PacketPropertyId) property and a [TabletPropertyMetrics](https://www.bing.com/search?q=TabletPropertyMetrics) structure that describes the range, resolution, and units of the property for a specific [**Tablet**](/windows/desktop/api/msinkaut/nn-msinkaut-iinktablet) object. The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object's [**GetDesiredPacketDescription**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-getdesiredpacketdescription) method returns an array of globally unique identifiers (GUIDs) for the packet properties that the **RealTimeStylus** object forwards to its plug-ins when those packet properties are available. To modify the set of packet properties the **RealTimeStylus** object passes to its plug-ins, call the **RealTimeStylus** object's [**SetDesiredPacketDescription**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-setdesiredpacketdescription) method. The [GetTabletPropertyDescriptionCollection](https://www.bing.com/search?q=GetTabletPropertyDescriptionCollection) method (in managed code) of the **RealTimeStylus** object takes a unique tablet identifier and returns a collection of [TabletPropertyDescription](https://www.bing.com/search?q=TabletPropertyDescription) objects. These packet properties represent the subset of properties supported by the tablet that are returned by the **GetDesiredPacketDescription** method.

For a list of the standard packet property GUIDs, see the [PacketPropertyGuids Constants](packetpropertyguids-constants.md) class.

## Special Considerations

The following list describes other points to take into consideration when using the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object with a [**Tablet**](/windows/desktop/api/msinkaut/nn-msinkaut-iinktablet) object.

-   The [**SetDesiredPacketDescription**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-setdesiredpacketdescription) method can only be called while the [**Tablet**](/windows/desktop/api/msinkaut/nn-msinkaut-iinktablet) object is disabled. The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object may modify the list of desired packet properties; therefore, call the [**GetDesiredPacketDescription**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-getdesiredpacketdescription) method after the call to the **SetDesiredPacketDescription** method to determine which packet properties the **RealTimeStylus** object can forward to its plug-ins. A tablet is only guaranteed to support the **X**, **Y**, and **PacketStatus** values for the [PacketProperty](packetproperty-guids.md). Therefore, your plug-in design may need to account for receiving fewer packet properties than desired.
-   The [GetTabletPropertyDescriptionCollection](https://www.bing.com/search?q=GetTabletPropertyDescriptionCollection) method (for managed code) can only be called while the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object is enabled. Because notification can be sent to the async plug-ins after the **RealTimeStylus** object has been disabled, you may need to cache the information for each [**Tablet**](/windows/desktop/api/msinkaut/nn-msinkaut-iinktablet) object. The GetTabletPropertyDescriptionCollection method returns a list of the desired packet properties that are supported by the specified tablet.
-   When the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object is enabled, each plug-in receives a call to its [**RealTimeStylusEnabled**](/windows/desktop/api/RTSCom/nf-rtscom-istylusplugin-realtimestylusenabled) method. The [RealTimeStylusEnabledData](https://www.bing.com/search?q=RealTimeStylusEnabledData) object passed in the notification contains a collection of the context identifiers for the available tablets at the time the **RealTimeStylus** object is enabled.
-   When a tablet that the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object can use is added to or removed from the Tablet PC while the **RealTimeStylus** object is enabled, the **RealTimeStylus** object notifies its plug-ins that a [**Tablet**](/windows/desktop/api/msinkaut/nn-msinkaut-iinktablet) object has been added or removed. For more information, see [Plug-in Data and the RealTimeStylus Class](plug-in-data-and-the-realtimestylus-class.md).

## Working with Tablet Pens

The [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object passes information about the tablet pen to its plug-ins in a number of the notification methods. Information about the tablet pen is represented by a [Stylus](https://www.bing.com/search?q=Stylus) object, gotten through the [**GetStyluses**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-getstyluses) method. This object is a representation of the tablet pen at the time the data was gathered. Because plug-ins receive the tablet pen data as part of the tablet pen data stream, the plug-ins should use the information in the Stylus object instead of checking for the current state of a particular tablet pen through the [**Cursor**](/windows/desktop/api/msinkaut/nn-msinkaut-iinkcursor) class. For information about how tablet pen and tablet pen button data is passed to plug-ins, see [Plug-in Data and the RealTimeStylus Class](plug-in-data-and-the-realtimestylus-class.md).

To get an array of the [Stylus](https://www.bing.com/search?q=Stylus) objects that the [**RealTimeStylus**](/windows/desktop/api/RTSCom/) object has encountered since it was last enabled, use the **RealTimeStylus** object's [**GetStyluses**](/windows/desktop/api/RTSCom/nf-rtscom-irealtimestylus-getstyluses) method.

## Related topics

<dl> <dt>

[Microsoft.Ink.Tablet](https://www.bing.com/search?q=Microsoft.Ink.Tablet)
</dt> <dt>

[Microsoft.StylusInput.RealTimeStylus](https://www.bing.com/search?q=Microsoft.StylusInput.RealTimeStylus)
</dt> <dt>

[The Cascaded RealTimeStylus Model](the-cascaded-realtimestylus-model.md)
</dt> <dt>

[Partial Trust Considerations for the StylusInput APIs](partial-trust-considerations-for-the-stylusinput-apis.md)
</dt> <dt>

[Plug-in Data and the RealTimeStylus Class](plug-in-data-and-the-realtimestylus-class.md)
</dt> <dt>

[Threading Considerations for the StylusInput APIs](threading-considerations-for-the-stylusinput-apis.md)
</dt> </dl>

 

 


