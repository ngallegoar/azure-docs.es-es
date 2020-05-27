---
title: 'Guía de inicio rápido: Configuración de un firewall de servidor de Azure Analysis Services | Microsoft Docs'
description: Esta inicio rápido le ayudará a configurar un firewall para el servidor de Azure Analysis Services mediante Azure Portal.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 48618815519fad31bff5d6a8d2d2edc82535f437
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697903"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Inicio rápido: Configuración del firewall del servidor: Portal

Esta guía de inicio rápido le ayudará a configurar un firewall para el servidor de Azure Analysis Services. La habilitación de un firewall y la configuración de los intervalos de direcciones IP solo para los equipos que tienen acceso al servidor son una parte importante de la protección del servidor y los datos.

## <a name="prerequisites"></a>Prerrequisitos

- Un servidor de Analysis Services en la suscripción. Para más información, vea esta [Inicio rápido: Creación de un servidor (Portal)](analysis-services-create-server.md) o [Inicio rápido: Creación de un servidor (PowerShell)](analysis-services-create-powershell.md)
- Uno o más intervalos de direcciones IP para los equipos cliente (si es necesario).
- Algunos escenarios en los que Power BI Premium se conecta a Azure Analysis Services, incluidos los informes paginados y de importación de datos (actualización), no se admiten ni siquiera si está habilitada la opción Permitir el acceso desde Power BI. El escenario más común en el que se usa Live Connect desde Power BI Premium sí se admite. Se admiten todos los escenarios de Power BI Pro.


## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal 

[Iniciar sesión en el portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Configuración de un firewall

1. Haga clic en el servidor para abrir la página Información general. 
2. En **Configuración** > **Firewall** > **Habilitar firewall**, haga clic en **Activar**.
3. Para permitir el acceso de DirectQuery desde el servicio Power BI, en **Permitir el acceso desde Power BI**, haga clic en **Activar**.  
4. (Opcional) Especifique uno o más intervalos de direcciones IP. Escriba un nombre y una dirección IP inicial y final para cada intervalo. El nombre de la regla de firewall debe estar limitado a 128 caracteres y solo puede contener caracteres en mayúsculas, caracteres en minúsculas, números, caracteres de subrayado y guiones. No se permiten espacios en blanco ni otros caracteres especiales.
5. Haga clic en **Save**(Guardar).

     ![Configuración de firewall](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine intervalos de direcciones IP o deshabilite el firewall.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha aprendido a configurar un firewall para el servidor. Ahora que tiene el servidor y está protegido con un firewall, puede agregar un modelo de datos de ejemplo básico desde el portal. Tener un modelo de ejemplo es útil para aprender cómo configurar los roles de la base de datos del modelo y probar las conexiones de cliente. Para más información, puede seguir el tutorial para agregar un modelo de ejemplo.

> [!div class="nextstepaction"]
> [Tutorial: Incorporación de un modelo de ejemplo al servidor](analysis-services-create-sample-model.md)
