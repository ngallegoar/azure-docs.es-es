---
title: Incidencias conocidas en los complementos de Project Acoustics
titlesuffix: Azure Cognitive Services
description: Es posible que aparezcan las siguientes incidencias conocidas en Project Acoustics.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: b71b93f271608d946d964f70dae9eefbef77e87b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "72243040"
---
# <a name="project-acoustics-known-issues"></a>Incidencias conocidas en Project Acoustics
En este artículo se describen las incidencias que puede experimentar al usar Project Acoustics.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Los parámetros acústicos se pierden al cambiar el nombre de una escena

Si cambia el nombre de una escena, los parámetros de la acústica que pertenecen a dicha escena no se transfieren automáticamente a la escena nueva. Sin embargo, siguen existiendo en el archivo de recursos antiguo. Busque el archivo *[SceneName]_AcousticParameters.asset* en el directorio *Editor* que se encuentra junto al archivo de su escena. Cambie el nombre para reflejar el nuevo nombre de la escena.

## <a name="deploy-to-android-bug-from-some-unity-versions"></a>Error de implementación en Android en algunas versiones de Unity

Algunas versiones de Unity presentan un [error](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player) en la forma en que implementan complementos de audio en Android. Asegúrese de que no usa una versión afectada por este error.

## <a name="could-not-find-metadata-file-systemsecuritydll-error"></a>Error "Could not find metadata file System.Security.dll" (No se encuentra el archivo de metadatos System.Security.dll)

Asegúrese de que la **versión de la biblioteca de tiempo de ejecución de Microsoft Scripting** de la configuración del **reproductor** es *equivalente a .NET 4.x* y reinicie Unity.

## <a name="authentication-problems-when-connecting-to-azure"></a>Problemas de autenticación al conectarse a Azure

Compruebe que:
- Usó las credenciales correctas para su cuenta de Azure.
- Su cuenta admite el tipo de nodo que solicitó en la simulación mediante "bake".
- El reloj del sistema está configurado correctamente.

## <a name="the-bake-tab-still-shows-deleting-after-you-cancel"></a>La pestaña Bake (Simular mediante "bake") sigue mostrando "eliminando" después de cancelar
Project Acoustics limpia todos los recursos de Azure de un trabajo cuando la finalización o la cancelación sean correctas. Este proceso puede tardar hasta 5 minutos.

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el contenido de ejemplo de [Unity](unity-quickstart.md) o [Unreal](unreal-quickstart.md).
