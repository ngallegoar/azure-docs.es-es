---
title: 'Tutorial: Emparejamiento de entornos locales con una nube privada'
description: Aprenda a crear el emparejamiento de Global Reach de ExpressRoute y una nube privada en Azure VMware Solution.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: 4d10972a693f7c4c3ae25a5bc986f6c15e978294
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92912504"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Tutorial: Emparejamiento de entornos locales con una nube privada

Global Reach de ExpressRoute conecta su entorno local y la nubes privada de Azure VMware Solution. La conexión de Global Reach de ExpressRoute se establece entre el circuito ExpressRoute de la nube privada y una conexión existente de ExpressRoute con los entornos locales. 

El circuito ExpressRoute que se usa al [configurar redes de Azure a la nube privada](tutorial-configure-networking.md) requiere la creación y el uso de claves de autorización.  Ya habrá usado una clave de autorización del circuito ExpressRoute y, en este tutorial, va a crear una segunda clave para el emparejamiento con el circuito ExpressRoute local.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una clave de autorización para _circuit 2_ , el circuito ExpressRoute de nube privada.
> * Usar la CLI de [Azure Portal](#azure-portal-method) o la [CLI de Azure en un método de Cloud Shell](#azure-cli-in-a-cloud-shell-method), en la suscripción de _circuit 1_ para habilitar el emparejamiento de Global Reach de ExpressRoute entre el entorno nube local y una nube privada.


## <a name="before-you-begin"></a>Antes de empezar

Antes de habilitar la conectividad entre dos circuitos ExpressRoute mediante Global Reach, consulte la documentación sobre cómo [habilitar la conectividad en distintas suscripciones de Azure](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions).  


## <a name="prerequisites"></a>Requisitos previos

- Establecimiento de conectividad a y desde una nube privada de Azure VMware Solution con su circuito ExpressRoute emparejado con una puerta de enlace de ExpressRoute en una red virtual (VNet) de Azure, que es _circuit 2_ en los procedimientos de emparejamiento.  
- Un circuito ExpressRoute independiente y funcional utilizado para conectar entornos locales con Azure, que es _circuit 1_ desde la perspectiva de los procedimientos de emparejamiento.
- Un [bloque de direcciones de la red](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) /29 sin superposición para el emparejamiento de Global Reach de ExpressRoute.

> [!TIP]
> En el contexto de estos requisitos previos, su circuito ExpressRoute local es _circuit 1_ , y su circuito ExpressRoute de nube privada está en otra suscripción y se denomina _circuit 2_. 


## <a name="create-an-expressroute-authorization-key-in-the-private-cloud"></a>Creación de una clave de autorización de ExpressRoute en la nube privada

1. En la nube privada **Información general** , en Administrar, seleccione **Conectividad > ExpressRoute > Solicitar una clave de autorización**.

   :::image type="content" source="media/expressroute-global-reach/start-request-auth-key.png" alt-text="Selección de Conectividad > ExpressRoute > Solicitar una clave de autorización para iniciar una nueva solicitud.":::

2. Escriba el nombre de la clave de autorización y seleccione **Crear**. 

   :::image type="content" source="media/expressroute-global-reach/create-global-reach-auth-key.png" alt-text="Selección de Crear para crear una clave de autorización. ":::

   Una vez creada, la nueva clave aparece en la lista de claves de autorización para la nube privada. 

   :::image type="content" source="media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Confirme que la nueva clave de autorización aparece en la lista de claves de la nube privada. ":::

3. Tome nota de la clave de autorización y del identificador de ExpressRoute, junto con el bloque de direcciones /29. Los usará en el paso siguiente para completar el emparejamiento. 

## <a name="peer-private-cloud-to-on-premises-using-authorization-key"></a>Emparejamiento de la nube privada a un entorno local con clave de autorización

Ahora que ha creado una clave de autorización para el circuito ExpressRoute de la nube privada, puede emparejarlo con el circuito ExpressRoute local.  El emparejamiento se realiza desde la perspectiva del circuito ExpressRoute local en [Azure Portal](#azure-portal-method) o mediante la [CLI de Azure en un método de Cloud Shell](#azure-cli-in-a-cloud-shell-method). Con ambos métodos, se usará el identificador de recurso y la clave de autorización del circuito ExpressRoute de la nube privada para finalizar el emparejamiento.

### <a name="azure-portal-method"></a>Método de Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) mediante la misma suscripción que el circuito ExpressRoute local.

1. En la sección de **información general** de la nube privada, en Administrar, seleccione **Conectividad > Global Reach de ExpressRoute > Agregar**.

   :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="En el menú, seleccione Conectividad, la pestaña Global Reach de ExpressRoute y, a continuación, Agregar.":::

1. Para crear una conexión de nube local, lleve a cabo una de las siguientes acciones:

   - Seleccione el circuito ExpressRoute en la lista.
   - Si tiene un identificador de circuito, cópielo y péguelo.

1. Seleccione **Conectar**. La nueva conexión se muestra en la lista de conexiones a la nube local.  

>[!TIP]
>Para eliminar o desconectar una conexión de la lista, seleccione **Más**.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Desconexión o eliminación de una conexión local":::

### <a name="azure-cli-in-a-cloud-shell-method"></a>CLI de Azure en un método de Cloud Shell

Hemos ampliado los [comandos de la CLI](../expressroute/expressroute-howto-set-global-reach-cli.md) para incluir detalles y ejemplos específicos que le ayudarán a configurar el emparejamiento de Global Reach de ExpressRoute entre entornos locales y una nube privada de Azure VMware Solution.  

> [!TIP]  
> Para una salida más breve del comando de la CLI de Azure, estas instrucciones pueden usar un argumento [`–query` para ejecutar una consulta JMESPath y que se muestren solo los resultados necesarios](/cli/azure/query-azure-cli).


1. Inicie sesión en Azure Portal con la misma suscripción que el circuito ExpressRoute local y abra Cloud Shell. Deje el shell como Bash.
 
   :::image type="content" source="media/expressroute-global-reach/open-cloud-shell.png" alt-text="Inicie sesión en Azure Portal y abra Cloud Shell.":::
 
2. Escriba el comando de la CLI de Azure para crear el emparejamiento. Use su información específica, junto con el identificador de recurso, la clave de autorización y el bloque de red CIDR /29. 

   En la imagen se muestra un ejemplo del comando que usará y la salida que indica un emparejamiento correcto. El comando de ejemplo se basa en el comando que se usa en el [paso 3 de "Habilitación de la conectividad entre circuitos ExpressRoute en distintas suscripciones de Azure"](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions).

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Creación del emparejamiento de Global Reach de ExpressRoute con el comando de la CLI de Azure en Cloud Shell.":::
 
   Puede conectarse desde los entornos locales hasta la nube privada mediante el emparejamiento de Global Reach de ExpressRoute.

> [!TIP]
> Para eliminar el emparejamiento que acaba de crear siga las instrucciones de [Deshabilitación de la conectividad entre las redes locales](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks).


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a crear una segunda clave de autorización para el circuito ExpressRoute de la nube privada. También aprendió a habilitar el emparejamiento de Global Reach de ExpressRoute del entorno local a la nube privada. 

Continúe con el siguiente tutorial para aprender a implementar y configurar la solución VMware HCX para su nube privada de Azure VMware Solution.

> [!div class="nextstepaction"]
> [Implementación y configuración de VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->