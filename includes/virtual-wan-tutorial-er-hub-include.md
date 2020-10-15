---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6f910dbe91ed8e1cb65eefa6dfc48c72a689bf25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "73491550"
---
1. Ubique la red WAN virtual que ha creado. En la página de Virtual WAN, en la sección **Conectividad**, seleccione **Centros**.
2. En la página Centros, haga clic en **+ Nuevo centro de conectividad** para abrir la página **Crear centro de conectividad virtual**.
3. En la página **Crear centro de conectividad virtual**, en la pestaña **Aspectos básicos** rellene los siguientes campos:

   ![Conceptos básicos](./media/virtual-wan-tutorial-er-hub-include/hub1.png "Aspectos básicos")

    **Detalles del proyecto**

   * Región (anteriormente se conocía como ubicación)
   * Nombre
   * Espacio de direcciones privadas del centro de conectividad. El espacio de direcciones mínimo es/24 para crear un centro de conectividad, lo que implica que cualquier intervalo de/25 a/32 generará un error durante la creación.
4. Seleccione la **pestaña ExpressRoute**.

5. En la pestaña **ExpressRoute**, rellene los siguientes campos:

   ![ExpressRoute](./media/virtual-wan-tutorial-er-hub-include/hub2.png "ExpressRoute")

   * Seleccione **Sí** para crear una puerta de enlace de **ExpressRoute**.
   * En la lista desplegable, seleccione el valor **Unidades de escalado de puerta de enlace**.
6. Seleccione **Revisar y crear** para validar.
7. Seleccione **Crear** para crear un centro de conectividad. Pasados 30 minutos haga clic en **Actualizar** para ver el centro de conectividad en la página **Centros**. Seleccione **Ir al recurso** para ir al recurso.