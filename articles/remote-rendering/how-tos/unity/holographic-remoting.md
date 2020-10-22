---
title: Uso de Holographic Remoting y Remote Rendering en Unity
description: Cómo se puede usar la versión preliminar de Holographic Remoting en combinación con Azure Remote Rendering
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: bd69710b6e4404a76d3cca385b6c07ea7c1f3f5c
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92201824"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Uso de Holographic Remoting y Remote Rendering en Unity

[Holographic Remoting](/windows/mixed-reality/holographic-remoting-player) y Azure Remote Rendering se excluyen mutuamente dentro de una aplicación. Por tanto, el [modo de reproducción de Unity](/windows/mixed-reality/unity-play-mode) tampoco está disponible.

En cada ejecución del editor de Unity, solo se puede usar uno de los dos. Para usar el otro, reinicie Unity primero.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>Uso del modo de reproducción de Unity para obtener una vista previa en HoloLens 2

 El modo de reproducción de Unity todavía se puede usar, por ejemplo, para probar la interfaz de usuario de la aplicación. Sin embargo, es vital que ARR nunca se inicialice. De lo contrario, se bloqueará.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Uso de un casco de realidad virtual de WMR para obtener una vista previa en el escritorio

Si se dispone de un casco de realidad virtual de Windows Mixed Reality, se puede usar para obtener una vista previa dentro de Unity. En este caso, es preciso inicializar ARR; sin embargo, no será posible conectarse a una sesión mientras se use el casco de WMR.