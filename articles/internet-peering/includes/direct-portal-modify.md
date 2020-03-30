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
ms.openlocfilehash: d9ff01cf0180dae7f75d9753ba889d0caddad937
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773987"
---
En esta sección se describe cómo realizar las siguientes operaciones de modificación para el emparejamiento directo:

### <a name="add-direct-peering-connections"></a>Adición de conexiones de emparejamiento directo
1. Haga clic en el botón **+ Agregar conexiones** en la parte superior y configure una nueva conexión de emparejamiento.
    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-direct-modify-addconnection.png)
1. Rellene el formulario de **conexión de emparejamiento directo** y haga clic en **Guardar**. Para obtener ayuda con la configuración de una conexión de emparejamiento, revise los pasos descritos en la sección anterior "Creación y aprovisionamiento de un emparejamiento directo" anteriormente.
    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-direct-modify-savenewconnection.png)

### <a name="remove-direct-peering-connections"></a>Eliminación de conexiones de emparejamiento directo

La eliminación de una conexión no se admite actualmente en el portal. Póngase en contacto con el equipo de [emparejamiento de Microsoft](mailto:peeringexperience@microsoft.com).

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Actualización o cambio a una versión anterior del ancho de banda en las conexiones activas
1. Haga clic en una conexión de emparejamiento que desee modificar y, a continuación, haga clic en el botón **...**  > **Editar conexión**.
    > [!div class="mx-imgBorder"]
    > ![Edición de la conexión de emparejamiento](../media/setup-direct-modify-editconnection.png)
1. Modifique el ancho de banda como se muestra a continuación y haga clic en **Guardar**.
    > [!div class="mx-imgBorder"]
    > ![Modificación del ancho de banda de la conexión de emparejamiento](../media/setup-direct-modify-editconnectionsettings.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Adición de sesión IPv4/IPv6 en conexiones activas
1. Haga clic en una conexión de emparejamiento que desee modificar y, a continuación, haga clic en el botón **...**  > **Editar conexión** como se mostró anteriormente.
1. Agregue la información del **prefijo IPv4 de sesión** o el **prefijo IPv6 de sesión** y haga clic en **Guardar**.

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Eliminación de la sesión IPv4/IPv6 en conexiones activas
    Removing a **Session IPv4 prefix** or **Session IPv6 prefix** info is not currently supported on portal. Contact [Microsoft peering](mailto:peeringexperience@microsoft.com).
