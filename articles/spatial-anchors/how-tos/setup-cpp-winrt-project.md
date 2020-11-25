---
title: Instalación de Azure Spatial Anchors para una aplicación de HoloLens C++/WinRT
description: Configure un proyecto de HoloLens C++/WinRT para que use Azure Spatial Anchors.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 69134ef87d90fe69de2d9e4e9222e90f65edc785
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "95507002"
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