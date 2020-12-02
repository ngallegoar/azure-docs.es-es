---
title: Relocalización general en Swift
description: Explicación detallada de cómo crear y localizar anclajes mediante relocalización general en Swift.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7cc964ca8339e82aa7388aeab8718a6102a23d1b
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "95999771"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-swift"></a>Creación y localización de anclajes mediante relocalización general en Swift

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

- Conocimientos básicos de Swift.
- Ha leído completamente [Introducción a Azure Spatial Anchors](../overview.md).
- Ha completado uno de los [inicios rápidos en 5 minutos](../index.yml).
- Lea el artículo sobre [procedimientos de creación y ubicación de anclajes](../create-locate-anchors-overview.md).

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```swift
// Create the sensor fingerprint provider
var sensorProvider: ASAPlatformLocationProvider?
sensorProvider = ASAPlatformLocationProvider()

// Allow GPS
let sensors = locationProvider?.sensors
sensors.geoLocationEnabled = true

// Allow WiFi scanning
sensors.wifiEnabled = true

// Populate the set of known BLE beacons' UUIDs
let uuids = [String]()
uuids.append("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1")
uuids.append("a63819b9-8b7b-436d-88ec-ea5d8db2acb0")

// Allow the set of known BLE beacons
sensors.bluetoothEnabled = true
sensors.knownBeaconProximityUuids = uuids
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```swift
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession!.locationProvider = sensorProvider

// Configure the near-device criteria
let nearDeviceCriteria = ASANearDeviceCriteria()!
nearDeviceCriteria.distanceInMeters = 5.0
nearDeviceCriteria.maxResultCount = 25

// Set the session's locate criteria
let anchorLocateCriteria = ASAAnchorLocateCriteria()!
anchorLocateCriteria.nearDevice = nearDeviceCriteria
cloudSpatialAnchorSession!.createWatcher(anchorLocateCriteria)
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]