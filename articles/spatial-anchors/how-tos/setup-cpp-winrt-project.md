---
title: Instalación de Azure Spatial Anchors para una aplicación de HoloLens C++/WinRT
description: Configure un proyecto de HoloLens C++/WinRT para que use Azure Spatial Anchors.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 43d5c1ae03c359dcbef21f8e7ba3205bc6ab0004
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92096120"
---
# <a name="configuring-azure-spatial-anchors-in-a-cwinrt-hololens-project"></a>Configuración de Azure Spatial Anchors en un proyecto de HoloLens C++/WinRT

## <a name="requirements"></a>Requisitos

* [Visual Studio 2019](https://www.visualstudio.com/downloads/) instalado con la carga de trabajo de **desarrollo de Plataforma universal de Windows** y el componente **Windows 10 SDK (10.0.18362.0 o versiones posteriores)** .

## <a name="configuring-a-project"></a>Configuración de un proyecto

Azure Spatial Anchors para HoloLens y C++/WinRT se distribuyen mediante el paquete NuGet [Microsoft.Azure.SpatialAnchors.WinRT](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/).

Siga las instrucciones que se indican [aquí](/nuget/consume-packages/install-use-packages-visual-studio) para usar el administrador de paquetes NuGet de Visual Studio para instalar el paquete NuGet [Microsoft.Azure.SpatialAnchors.WinRT](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/) en el proyecto.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Creación y localización de delimitadores en C++/WinRT](./create-locate-anchors-cpp-winrt.md)