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
ms.openlocfilehash: 64470b42efeea49b7c778d6dffd88465b8445e36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90606696"
---
1. Ubique la red WAN virtual que ha creado. En la página de Virtual WAN, en la sección **Conectividad**, seleccione **Centros**.
2. En la página Centros, haga clic en **+ Nuevo centro de conectividad** para abrir la página **Crear centro de conectividad virtual**.

    ![Captura de pantalla que muestra el panel Crear centro de conectividad virtual con la pestaña Aspectos básicos seleccionada.](./media/virtual-wan-tutorial-hub-include/basics.png "Aspectos básicos")
3. En la página **Crear centro de conectividad virtual**, en la pestaña **Aspectos básicos** rellene los siguientes campos:

    **Detalles del proyecto**

   * Región (anteriormente se conocía como ubicación)
   * Nombre
   * Espacio de direcciones privadas del centro de conectividad. El espacio de direcciones mínimo es/24 para crear un centro de conectividad, lo que implica que cualquier intervalo de/25 a/32 generará un error durante la creación. Azure Virtual WAN, un servicio administrado por Microsoft, crea las subredes adecuadas en el centro de conectividad virtual para las diferentes puertas de enlace y servicios (por ejemplo, puertas de enlace de VPN, puertas de enlace de ExpressRoute, puertas de enlace de VPN o de punto a sitio, firewall, enrutamiento, etc.). No es necesario que el usuario planee explícitamente el espacio de direcciones de subred para los servicios del centro de conectividad virtual, ya que Microsoft lo hace como parte del servicio.
4. Seleccione **Siguiente: De sitio a sitio**.

    ![Captura de pantalla que muestra el panel Crear centro de conectividad virtual con Site to site (De sitio a sitio) seleccionado.](./media/virtual-wan-tutorial-hub-include/site-to-site.png "De sitio a sitio")

5. En la pestaña **De sitio a sitio**, rellene los siguientes campos:

   * Seleccione **Sí** para crear una VPN de sitio a sitio.
   * En la actualidad el campo de número de espacio de direcciones no es editable en el centro de conectividad virtual.
   * En la lista desplegable, seleccione el valor **Unidades de escalado de puerta de enlace**. La unidad de escalado permite elegir el rendimiento agregado de la puerta de enlace de VPN que se va a crear en el centro de conectividad virtual para conectar los sitios. Si elige 1 unidad de escalado = 500 Mbps, esto implica que se crearán dos instancias de redundancia, cada una con un rendimiento máximo de 500 Mbps. Por ejemplo, si tuviera cinco ramas, cada una con 10 Mbps en la rama, necesitará un agregado de 50 Mbps en el extremo principal. El planeamiento de la capacidad agregada de la puerta de enlace de VPN de Azure debe realizarse después de evaluar la capacidad necesaria para admitir el número de ramas en el centro de conectividad.
6. Seleccione **Revisar y crear** para validar.
7. Seleccione **Crear** para crear un centro de conectividad. Pasados 30 minutos haga clic en **Actualizar** para ver el centro de conectividad en la página **Centros**. Seleccione **Ir al recurso** para ir al recurso.
