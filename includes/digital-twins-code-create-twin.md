---
author: baanders
description: 'incluir archivo para Azure Digital Twins: código para crear un gemelo'
ms.service: digital-twins
ms.topic: include
ms.date: 10/21/2020
ms.author: baanders
ms.openlocfilehash: 5ea73e776344094a36e46415ffabce0d8ae3525a
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92440845"
---
```csharp
// Define the model type for the twin to be created
Dictionary<string, object> meta = new Dictionary<string, object>()
{
    { "$model", "dtmi:example:Room;1" }
};
// Initialize the twin properties
Dictionary<string, object> twin = new Dictionary<string, object>()
{
    { "$metadata", meta },
    { "Temperature", temperature},
    { "Humidity", humidity},
};
//Create the twin
client.CreateDigitalTwin("myRoomID", JsonSerializer.Serialize<Dictionary<string, object>>(twin));
```