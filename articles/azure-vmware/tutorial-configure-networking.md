---
title: 'Tutorial: Configuración de redes para la nube privada de VMWare en Azure'
description: Aprenda a crear y configurar las redes necesarias para implementar una nube privada en Azure.
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 41043b98a6e270d8d9f4373de7876b3fcae86747
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82837624"
---
# <a name="tutorial-configure-networking-for-your-vmware-private-cloud-in-azure"></a>Tutorial: Configuración de redes para la nube privada de VMWare en Azure

Una nube privada de Azure VMware Solution (AVS) requiere una red virtual. Dado que AVS no admitirá la instancia de vCenter local durante la versión preliminar, se necesitan pasos adicionales para la integración con el entorno local. La configuración de un circuito ExpressRoute y una puerta de enlace de red virtual también son necesarias y se tratarán en este tutorial.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una red virtual
> * Crear una puerta de enlace de red virtual
> * Conectar el circuito ExpressRoute a la puerta de enlace
> * Buscar las direcciones URL para el administrador de NSX y vCenter

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Creación de una red virtual

Vaya al grupo de recursos que creó en el [tutorial anterior](tutorial-create-private-cloud.md) y seleccione **+ Agregar** para definir un nuevo recurso.

En el campo **Buscar en Marketplace**, escriba **Virtual Network**. Busque el recurso Virtual Network y selecciónelo.

En la página Virtual Network, seleccione **Crear** para configurar la red virtual para la nube privada.

En la página **Crear Virtual Network**, escriba los detalles pertinentes de la red virtual; en la tabla siguiente se muestra una descripción de las propiedades:

> [!IMPORTANT]
> Debe usar un espacio de direcciones que **no** se superponga con el que usó al crear la nube privada en el tutorial anterior.

En la pestaña **Aspectos básicos**, escriba un nombre para la red virtual, seleccione la región adecuada y seleccione **Siguiente: Direcciones IP**

En la pestaña **Direcciones IP**, en **Espacio de direcciones IPv4**, escriba el espacio de direcciones que creó en el tutorial anterior.

Seleccione **+ Agregar subred** y, en la página **Agregar subred**, asigne a la subred un nombre y un intervalo de direcciones adecuados. Cuando haya terminado, seleccione **Agregar**.

Seleccione **Revisar + crear**.

:::image type="content" source="./media/tutorial-configure-networking/create-virtual-network.png" alt-text="creación de una red virtual" border="true":::

Compruebe la información y seleccione **Crear**. Una vez completada la implementación, verá la red virtual en el grupo de recursos.

## <a name="create-a-virtual-network-gateway"></a>Creación de una puerta de enlace de red virtual

Ha creado una red virtual en la sección anterior, ahora creará una puerta de enlace de red virtual.

En el grupo de recursos, seleccione **+ Agregar** para agregar un recurso nuevo.

En el campo **Buscar en el Marketplace**, escriba **Puerta de enlace de red virtual**. Busque el recurso Virtual Network y selecciónelo.

En la página **Puerta de enlace de red virtual**, seleccione **Crear**.

En la pestaña Aspectos básicos de la página **Crear puerta de enlace de red virtual**, proporcione valores para los campos. En la tabla siguiente se muestra la descripción de los campos:

| Campo | Value |
| --- | --- |
| **Suscripción** | Este valor ya se ha rellenado con la suscripción a la que pertenece el grupo de recursos. |
| **Grupos de recursos** | Este valor ya se ha rellenado para el grupo de recursos actual. Debería ser el grupo de recursos que creó en una prueba anterior. |
| **Nombre** | Escriba un nombre único para la puerta de enlace de red virtual. |
| **Región** | Seleccione la ubicación geográfica de la puerta de enlace de red virtual. |
| **Tipo de puerta de enlace** | seleccione **ExpressRoute**. |
| **Tipo de VPN** | seleccione **Basada en rutas**. |
| **SKU** | Deje el valor predeterminado: **standard**. |
| **Red virtual** | Seleccione la red virtual que ha creado antes. Si no ve la red virtual, asegúrese de que la región de la puerta de enlace coincide con la región de la red virtual. |
| **Intervalo de direcciones de subred de puerta de enlace** | Este valor se rellena cuando se selecciona la red virtual. No cambie el valor predeterminado. |
| **Dirección IP pública** | Seleccione **Crear nuevo**. |

:::image type="content" source="./media/tutorial-configure-networking/create-virtual-network-gateway.png" alt-text="creación de una puerta de enlace" border="true":::

Seleccione **Revisar y crear**, en la página siguiente compruebe que los detalles son correctos y seleccione **Crear** para iniciar la implementación de la puerta de enlace de red virtual. Una vez finalizada la implementación, vaya a la siguiente sección de este tutorial para conectar ExpressRoute a la red virtual que contiene la nube privada.

## <a name="connect-expressroute-to-the-virtual-network-gateway"></a>Conexión de ExpressRoute a la puerta de enlace de red virtual

En esta sección se explica cómo agregar una conexión entre la nube privada de AVS y la puerta de enlace de red virtual que ha creado.

Vaya a la nube privada que creó en el tutorial anterior y seleccione **Conectividad**  en **Administrar** y seleccione la pestaña **ExpressRoute**.

Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione **+ Solicitar una clave de autorización**.

:::image type="content" source="./media/tutorial-configure-networking/request-auth-key.png" alt-text="solicitud de una clave de autorización" border="true":::

Vaya a la puerta de enlace de red virtual que creó en el paso anterior y, en **Configuración**, seleccione **Conexiones**. En la página **Conexiones**, seleccione **+ Agregar**.

En la página **Agregar conexión**, proporcione valores para los campos. En la tabla siguiente se muestra la descripción de los campos:

| Campo | Value |
| --- | --- |
| **Nombre**  | Escriba un nombre para la conexión.  |
| **Tipo de conexión**  | seleccione **ExpressRoute**.  |
| **Canjear autorización**  | Asegúrese de que este cuadro está seleccionado.  |
| **Puerta de enlace de red virtual** | La puerta de enlace de red virtual que ha creado antes.  |
| **Clave de autorización**  | Copie y pegue la clave de autorización de la pestaña de ExpressRoute para el grupo de recursos. |
| **URI de circuito del mismo nivel**  | Copie y pegue el identificador de ExpressRoute de la pestaña de ExpressRoute para el grupo de recursos.  |

Seleccione **Aceptar**. De esta forma, se crea la conexión entre el circuito ExpressRoute y la red virtual.

:::image type="content" source="./media/tutorial-configure-networking/add-connection.png" alt-text="adición de una conexión" border="true":::

## <a name="locate-the-urls-for-vcenter-and-nsx-manager"></a>Búsqueda de las direcciones URL para el administrador de NSX y vCenter

Para iniciar sesión en vVenter y el administrador de NSX, necesitará las direcciones URL para el cliente web de vCenter y el sitio del administrador de NSX-T. Para buscar las direcciones URL:

Vaya a la nube privada de AVS y, en **Manage** (Administrar), seleccione **Identity** (Identidad) y encontrará la información necesaria.

:::image type="content" source="./media/tutorial-configure-networking/locate-urls.png" alt-text="búsqueda de las direcciones URL de vCenter" border="true":::

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de una red virtual
> * Crear una puerta de enlace de red virtual
> * Conectar el circuito ExpressRoute a la puerta de enlace
> * Buscar las direcciones URL para el administrador de NSX y vCenter

Continúe con el siguiente tutorial para aprender a crear un host de salto para conectarse a su entorno de forma que pueda administrar la nube privada localmente.

> [!div class="nextstepaction"]
> [Acceso a una nube privada](tutorial-access-private-cloud.md)
