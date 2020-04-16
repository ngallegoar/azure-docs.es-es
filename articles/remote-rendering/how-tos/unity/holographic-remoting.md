---
title: Uso de Holographic Remoting y Remote Rendering en Unity
description: Cómo se puede usar la versión preliminar de Holographic Remoting en combinación con Azure Remote Rendering
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: ac47a2922e92233f0acabf75817a712671306bc1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679206"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Uso de Holographic Remoting y Remote Rendering en Unity

[Holographic Remoting](https://docs.microsoft.com/windows/mixed-reality/holographic-remoting-player) y Azure Remote Rendering se excluyen mutuamente dentro de una aplicación. Por tanto, el [modo de reproducción de Unity](https://docs.microsoft.com/windows/mixed-reality/unity-play-mode) tampoco está disponible.

En cada ejecución del editor de Unity, solo se puede usar uno de los dos. Para usar el otro, reinicie Unity primero.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>Uso del modo de reproducción de Unity para obtener una vista previa en Hololens 2

 El modo de reproducción de Unity todavía se puede usar, por ejemplo, para probar la interfaz de usuario de la aplicación. Sin embargo, es vital que ARR nunca se inicialice. De lo contrario, se bloqueará.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Uso de un casco de realidad virtual de WMR para obtener una vista previa en el escritorio

Si se dispone de un casco de realidad virtual de Windows Mixed Reality, se puede usar para obtener una vista previa dentro de Unity. En este caso, es preciso inicializar ARR; sin embargo, no será posible conectarse a una sesión mientras se use el casco de WMR.
