---
title: Instalación del paquete de Remote Rendering para Unity
description: Explica cómo instalar los archivos DLL de cliente de Remote Rendering para Unity
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 3704d1a418baeec18c3303b8203a0185790cbcc7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85564297"
---
# <a name="install-the-remote-rendering-package-for-unity"></a>Instalación del paquete de Remote Rendering para Unity

Azure Remote Rendering usa un paquete Unity para encapsular la integración en Unity.

## <a name="manage-the-remote-rendering-packages-in-unity"></a>Administración de los paquetes de Remote Rendering en Unity

Los paquetes de Unity son contenedores que se pueden administrar mediante [Package Manager](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@1.8/manual/index.html) de Unity.
Este paquete contiene la API C# completa, así como todos los archivos binarios de complemento necesarios para usar Azure Remote Rendering con Unity.
En el siguiente esquema de nomenclatura de Unity para los paquetes, el paquete se denomina **com.microsoft.azure.remote-rendering**.

No forma parte del repositorio de ejemplos de [ARR](https://github.com/Azure/azure-remote-rendering) y no está disponible en el registro de paquetes internos de Unity. Para agregarlo a un proyecto, tiene que editar manualmente el archivo `manifest.md` del proyecto para incorporar lo siguiente:

```json
{
  "scopedRegistries": [
    {
      "name": "Azure Mixed Reality Services",
      "url": "https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM/",
      "scopes": ["com.microsoft.azure"]
    }
   ],
  "dependencies": {
    "com.microsoft.azure.remote-rendering": "0.1.31",
    ...existing dependencies...
  }
}
```

Una vez que se ha agregado, puede usar Unity Package Manager para asegurarse de que tiene la versión más reciente.
Encuentre instrucciones más completas en el [Tutorial: Visualización de modelos remotos](../../tutorials/unity/view-remote-models/view-remote-models.md).

## <a name="unity-render-pipelines"></a>Canalizaciones de representación de Unity

Remote Rendering funciona tanto con **:::no-loc text="Universal render pipeline":::** como con **:::no-loc text="Standard render pipeline":::** . Por motivos de rendimiento, se recomienda usar la canalización de representación universal.

Para usar **:::no-loc text="Universal render pipeline":::** , su paquete tiene que estar instalado en Unity. Esto se puede hacer en la interfaz de usuario de **Package Manager** de Unity (nombre del paquete **Universal RP**, versión 7.3.1 o más reciente) o con el archivo `Packages/manifest.json`, como se describe en el [tutorial de configuración del proyecto de Unity](../../tutorials/unity/view-remote-models/view-remote-models.md#include-the-azure-remote-rendering-package).

## <a name="next-steps"></a>Pasos siguientes

* [Interacción con componentes y objetos de juegos de Unity](objects-components.md)
* [Tutorial: Visualización de modelos remotos](../../tutorials/unity/view-remote-models/view-remote-models.md)
