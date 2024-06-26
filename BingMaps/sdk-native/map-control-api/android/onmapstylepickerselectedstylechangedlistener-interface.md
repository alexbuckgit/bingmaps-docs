---
title: OnMapStylePickerSelectedStyleChangedListener Interface | Microsoft Docs
description: Describes the OnMapStylePickerSelectedStyleChangedListener interface for Android and provides syntax and additional references.
ms.author: pablocan
---

# OnMapStylePickerSelectedStyleChangedListener Interface (Android only)

[!INCLUDE [bing-maps-sdk-for-android-iOS-retirement](../../../includes/bing-maps-sdk-for-android-iOS-retirement.md)]

Listener used with UserInterfaceOptions.MapStylePickerSelectedStyleChanged event.

>```java
> public interface OnMapStylePickerSelectedStyleChangedListener
>{
>    /** Called when map style picker style is selected. */
>    public boolean onSelectedStyleChanged(MapStylePickerSelectedStyleChangedEventArgs e);
>}
>```

## See Also

* [MapStylePickerSelectedStyleChangedEventArgs](MapStylePickerSelectedStyleChangedEventArgs-class.md)
* [MapUserInterfaceOptions](../MapUserInterfaceOptions-class.md)