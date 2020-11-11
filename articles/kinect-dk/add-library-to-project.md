---
title: Adición de la biblioteca de Azure Kinect a su proyecto de Visual Studio
description: Obtenga información sobre cómo agregar el paquete NuGet de Azure Kinect a su proyecto de Visual Studio.
author: wes-b
ms.author: wesbarc
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, sensor, SDK, Visual Studio 2017, Visual Studio 2019, NuGet
ms.openlocfilehash: fd71f0d327b8c828cc9ddac5810757cccdffbcea
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359619"
---
# <a name="add-azure-kinect-library-to-your-visual-studio-project"></a>Adición de la biblioteca de Azure Kinect a su proyecto de Visual Studio

En este artículo se le guiará a través del proceso de agregar el paquete NuGet de Azure Kinect a su proyecto de Visual Studio.

## <a name="install-azure-kinect-nuget-package"></a>Instalación del paquete NuGet de Azure Kinect

Para instalar el paquete NuGet de Azure Kinect:

1. Puede encontrar instrucciones detalladas para instalar un paquete NuGet en Visual Studio en [Inicio rápido: Instalación y uso de un paquete en Visual Studio](/nuget/quickstart/install-and-use-a-package-in-visual-studio).
2. Para agregar el paquete, puede usar la interfaz de usuario del administrador de paquetes; para ello, haga clic con el botón derecho en Referencias y elija Administrar paquetes NuGet desde el Explorador de soluciones.
3. Elija [nuget.org](https://www.nuget.org) como origen del paquete, seleccione la pestaña Examinar y busque `Microsoft.Azure.Kinect.Sensor`.
4. Seleccione ese paquete de la lista e instálelo.

## <a name="use-azure-kinect-nuget-package"></a>Uso del paquete NuGet de Azure Kinect

Una vez agregado el paquete, agregue las instrucciones include del archivo de encabezado al código fuente, como:

- `#include <k4a/k4a.h>`
- `#include <k4arecord/record.h>`
- `#include <k4arecord/playback.h>`

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Ahora está listo para compilar su primera aplicación](build-first-app.md)