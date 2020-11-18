---
title: Archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/09/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 62d466e81309765540bcbd52714733b97d241ebc
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93353912"
---
Desde un explorador, vaya a Azure Portal e inicie sesión con su cuenta de Azure.

1. Vaya a la página de Virtual WAN. En el portal, haga clic en **+Crear un recurso**. Escriba **Virtual WAN** en el cuadro de búsqueda y seleccione **ENTRAR**.
1. Seleccione **Virtual WAN** en los resultados. En la página Virtual WAN, seleccione **Crear** para abrir la página Crear una red WAN.
1. Dentro de la página **Crear una red WAN**, en la pestaña **Aspectos básicos**, rellene los campos siguientes:

   :::image type="content" source="./media/virtual-wan-create-vwan-include/basics.png" alt-text="Captura de pantalla que muestra el panel Crear una red WAN con la pestaña Aspectos básicos seleccionada.":::

   * **Suscripción**: seleccione la suscripción que quiere usar.
   * **Grupo de recursos**: cree uno nuevo o utilice uno ya existente.
   * **Ubicación del grupo de recursos**: elija una ubicación para los recursos en la lista desplegable. Una red WAN es un recurso global y no reside en una región determinada. No obstante, tiene que seleccionar una región con el fin de administrar y ubicar el recurso de WAN que cree.
   * **Nombre**: escriba el nombre que desea dar a la WAN.
   * **Tipo**: Básico o Estándar. Seleccione **Estándar**. Si selecciona VWAN Básico, tenga en cuenta que las instancias de VWAN Básico solo pueden contener concentradores Básicos, lo que limita el tipo de conexión a sitio a sitio.
1. Cuando termine de rellenar los campos, haga clic en **Revisión y creación**.
1. Una vez que se haya superado la validación, seleccione **Crear** para crear la WAN virtual.
