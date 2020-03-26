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
ms.openlocfilehash: 3bd9489adaf46e604393fc7059d37443bdd5ec3e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73489027"
---
1. Ubique la red WAN virtual que ha creado. En la página de Virtual WAN, en la sección **Conectividad**, seleccione **Centros**.
2. En la página Centros, haga clic en **+ Nuevo centro de conectividad** para abrir la página **Crear centro de conectividad virtual**.

    ![Conceptos básicos](./media/virtual-wan-tutorial-hub-include/basics.png "Aspectos básicos")
3. En la página **Crear centro de conectividad virtual**, en la pestaña **Aspectos básicos** rellene los siguientes campos:

    **Detalles del proyecto**

   * Región (anteriormente se conocía como ubicación)
   * Nombre
   * Espacio de direcciones privadas del centro de conectividad. El espacio de direcciones mínimo es/24 para crear un centro de conectividad, lo que implica que cualquier intervalo de/25 a/32 generará un error durante la creación.
4. Seleccione **Siguiente: De sitio a sitio**.

    ![De sitio a sitio](./media/virtual-wan-tutorial-hub-include/site-to-site.png "De sitio a sitio")

5. En la pestaña **De sitio a sitio**, rellene los siguientes campos:

   * Seleccione **Sí** para crear una VPN de sitio a sitio.
   * En la actualidad el campo de número de espacio de direcciones no es editable en el centro de conectividad virtual.
   * En la lista desplegable, seleccione el valor **Unidades de escalado de puerta de enlace**. La unidad de escalado permite elegir el rendimiento agregado de la puerta de enlace de VPN que se va a crear en el centro de conectividad virtual para conectar los sitios. Si elige 1 unidad de escalado = 500 Mbps, esto implica que se crearán dos instancias de redundancia, cada una con un rendimiento máximo de 500 Mbps. Por ejemplo, si tuviera cinco ramas, cada una con 10 Mbps en la rama, necesitará un agregado de 50 Mbps en el extremo principal. El planeamiento de la capacidad agregada de la puerta de enlace de VPN de Azure debe realizarse después de evaluar la capacidad necesaria para admitir el número de ramas en el centro de conectividad.
6. Seleccione **Revisar y crear** para validar.
7. Seleccione **Crear** para crear un centro de conectividad. Pasados 30 minutos haga clic en **Actualizar** para ver el centro de conectividad en la página **Centros**. Seleccione **Ir al recurso** para ir al recurso.