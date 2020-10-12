---
title: Instalación de Azure Spatial Anchors para Unity
description: Configure un proyecto de Unity para que use Azure Spatial Anchors.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 09/29/2020
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: da983719dc66656aa28cab4aea0bae558c2a7162
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91530430"
---
# <a name="configuring-azure-spatial-anchors-in-a-unity-project"></a>Configuración de Azure Spatial Anchors en un proyecto de Unity

En esta guía se muestra cómo empezar a trabajar con el SDK de Azure Spatial Anchors en el proyecto de Unity.

## <a name="requirements"></a>Requisitos

Azure Spatial Anchors admite actualmente Unity 2019.4 (LTS) con las siguientes configuraciones.

* Unity 2019.4 con AR Foundation 3.1 es compatible con Azure Spatial Anchors 2.4.0 y versiones posteriores.

## <a name="configuring-a-project"></a>Configuración de un proyecto

### <a name="add-the-unity-package-manager-packages-to-your-project"></a>[Agregar los paquetes del administrador de paquetes de Unity al proyecto](#tab/UPMPackage)

Azure Spatial Anchors para Unity se distribuyen actualmente mediante paquetes del administrador de paquetes de Unity (UPM). Estos paquetes se pueden encontrar en el [registro de NPM](https://bintray.com/microsoft/AzureMixedReality-NPM). Para más información sobre cómo trabajar con registros de paquetes de ámbito en un proyecto de Unity, consulte la documentación oficial de Unity [aquí](https://docs.unity3d.com/Manual/upm-scoped.html).

#### <a name="add-the-registry-to-your-unity-project"></a>Incorporación del registro al proyecto de Unity

1. Desde un explorador de archivos, acceda a la carpeta `Packages` del proyecto de Unity. Abra el archivo de manifiesto del proyecto `manifest.json` en un editor de texto.
2. En la parte superior del archivo, en el nivel de la sección `dependencies`, agregue la entrada siguiente para incluir el registro de Azure Spatial Anchors al proyecto. La entrada `scopedRegistries` indica a Unity dónde debe buscar los paquetes del SDK de Azure Spatial Anchors.

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-19&highlight=2-10)]

#### <a name="add-the-sdk-packages-to-your-unity-project"></a>Incorporación de paquetes de SDK al proyecto de Unity

| Plataforma | Nombre del paquete                                    |
|----------|-------------------------------------------------|
| Android  | com.microsoft.azure.spatial-anchors-sdk.android |
| iOS      | com.microsoft.azure.spatial-anchors-sdk.ios     |
| HoloLens | com.microsoft.azure.spatial-anchors-sdk.windows |

1. Para cada plataforma (Android/iOS/HoloLens) que quiera admitir en el proyecto, agregue una entrada con el nombre del paquete y la versión del paquete a la sección `dependencies` del manifiesto del proyecto. Consulte un ejemplo a continuación.

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-22&highlight=12-14)]

2. Guarde y cierre el archivo `manifest.json`. Cuando vuelva a Unity, la plataforma debería detectar automáticamente el cambio de manifiesto del proyecto y recuperar los paquetes especificados. Puede expandir la carpeta `Packages` en la vista de proyecto para comprobar que se hayan importado los paquetes correctos.

#### <a name="android-only-configure-the-maintemplategradle-file"></a>Solo Android: configuración del archivo mainTemplate.gradle

1. Vaya a **Edit** > **Project Settings** > **Player** (Editar > Configuración del proyecto > Reproductor).
2. En **Inspector Panel** (Panel de inspector) de **Player Settings** (Configuración del Reproductor), seleccione el icono de **Android**.
3. En la sección **Build**, active la casilla **Custom Main Gradle Template** (Plantilla principal de Gradle personalizada) para generar una plantilla personalizada de Gradle en `Assets\Plugins\Android\mainTemplate.gradle`.
4. Abre el archivo `mainTemplate.gradle` en un editor de texto.
5. En la sección `dependencies`, pegue las siguientes dependencias:

    ```gradle
    implementation('com.squareup.okhttp3:okhttp:[3.11.0]')
    implementation('com.microsoft.appcenter:appcenter-analytics:[1.10.0]')
    ```

Cuando haya terminado, la sección `dependencies` debe tener un aspecto similar al siguiente:

[!code-gradle[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-android-gradle-setup.md?range=9-13&highlight=3-4)]

### <a name="import-the-asset-package"></a>[Importación del paquete de recursos](#tab/UnityAssetPackage)

> [!WARNING]
> La distribución del paquete de activos de Unity del SDK de Azure Spatial Anchors quedó en desuso en la versión 2.5.0 y no está disponible desde la versión 2.6.0.

1. Descargue el archivo `AzureSpatialAnchors.unitypackage` de la versión que desea como destino en la página de [versiones de GitHub](https://github.com/Azure/azure-spatial-anchors-samples/releases).
2. Siga las instrucciones que se indican [aquí](https://docs.unity3d.com/Manual/AssetPackagesImport.html) para importar el paquete de recursos de Unity en el proyecto.

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Creación y localización de delimitadores en Unity](./create-locate-anchors-unity.md)
