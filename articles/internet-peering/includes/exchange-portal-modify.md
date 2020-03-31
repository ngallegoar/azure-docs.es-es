---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e3e87e59f5b3c95051d9ee53e4b8d87afe9d9ba8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773747"
---
En esta sección se describe cómo realizar las siguientes operaciones de modificación para el emparejamiento directo:

### <a name="add-exchange-peering-connections"></a>Adición de conexiones de emparejamiento de Exchange

1. Haga clic en el botón **+ Agregar conexiones** en la parte superior y configure una nueva conexión de emparejamiento.
    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-exchange-modify-addconnection.png)
1. Rellene el formulario de **conexión de emparejamiento de Exchange** y haga clic en **Guardar**. Para obtener ayuda con la configuración de una conexión de emparejamiento, revise los pasos descritos en la sección anterior "Creación y aprovisionamiento de un emparejamiento directo" anteriormente.
    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Eliminación de conexiones de emparejamiento de Exchange

1. Haga clic en una conexión de emparejamiento que desee eliminar y, a continuación, haga clic en el botón **...**  > **Eliminar conexión**.
    > [!div class="mx-imgBorder"]
    > ![Eliminación de la conexión de emparejamiento](../media/setup-exchange-modify-deleteconnection.png)
1. Escriba el identificador de recurso en el cuadro **Confirmar eliminación** como se muestra en los cuadros resaltados y haga clic en **Eliminar**.
    > [!div class="mx-imgBorder"]
    > ![Confirmación de la eliminación de la conexión de emparejamiento](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Adición de sesión IPv4/IPv6 en conexiones activas

1. Haga clic en una conexión de emparejamiento que desee modificar y, a continuación, haga clic en el botón **...**  > **Editar conexión**.
    > [!div class="mx-imgBorder"]
    > ![Edición de la conexión de emparejamiento](../media/setup-exchange-modify-editconnection.png)
1. Agregue la información de la **dirección IPv4** o la **dirección IPv6** y haga clic en **Guardar**.
    > [!div class="mx-imgBorder"]
    > ![Modificación de la conexión de emparejamiento](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Eliminación de la sesión IPv4/IPv6 en conexiones activas

La eliminación de una sesión IPv4/IPv6 desde una conexión existente no se admite actualmente en el portal. Póngase en contacto con el equipo de [emparejamiento de Microsoft](mailto:peeringexperience@microsoft.com).