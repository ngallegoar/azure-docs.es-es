---
title: Relocalización general en C++ y WinRT
description: Explicación detallada de cómo crear y localizar anclajes mediante relocalización general en C++/WinRT.
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/19/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 9eace1ee1f97acaf04608d37c9edba482622bd59
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92097378"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-cwinrt"></a>Creación y localización de anclajes mediante relocalización general en C++/WinRT

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android Java](set-up-coarse-reloc-java.md)
> * [C++/NDK](set-up-coarse-reloc-cpp-ndk.md)
> * [C++/WinRT](set-up-coarse-reloc-cpp-winrt.md)

Azure Spatial Anchors puede asociar los datos del sensor de posicionamiento del dispositivo con los anclajes que cree. Estos datos también se pueden usar para determinar rápidamente si hay algún anclaje cerca del dispositivo. Para más información, consulte [Relocalización general](../concepts/coarse-reloc.md).

## <a name="prerequisites"></a>Prerequisites

Para completar esta guía, asegúrese de que cumple los siguientes requisitos previos:

- Conocimiento básico sobre C++ y <a href="/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt" target="_blank">Windows Runtime APIs</a>.
- Ha leído completamente [Introducción a Azure Spatial Anchors](../overview.md).
- Ha completado uno de los [inicios rápidos en 5 minutos](../index.yml).
- Lea el artículo sobre [procedimientos de creación y ubicación de anclajes](../create-locate-anchors-overview.md).

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```cpp
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = PlatformLocationProvider();

// Allow GPS
SensorCapabilities sensors = sensorProvider.Sensors()
sensors.GeoLocationEnabled(true);

// Allow WiFi scanning
sensors.WifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
std::vector<winrt::hstring> uuids;
uuids.emplace_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.emplace_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

// Allow the set of known BLE beacons
sensors.BluetoothEnabled(true);
sensors.KnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```cpp
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession.LocationProvider(sensorProvider);

// Configure the near-device criteria
NearDeviceCriteria nearDeviceCriteria = NearDeviceCriteria();
nearDeviceCriteria.DistanceInMeters(5.0f);
nearDeviceCriteria.MaxResultCount(25);

// Set the session's locate criteria
anchorLocateCriteria = AnchorLocateCriteria();
anchorLocateCriteria.NearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession.CreateWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]