---
title: 'Aprovisionamiento de un grupo en una red virtual: Azure Batch | Microsoft Docs'
description: Cómo crear un grupo de Batch en una red virtual de Azure para que los nodos de proceso puedan comunicarse de manera segura con otras máquinas virtuales en la red, como un servidor de archivos.
ms.topic: article
ms.date: 04/03/2020
ms.custom: seodec18
ms.openlocfilehash: 616118d5f75f9bfa6d97d89baac9d7ea9186cd5d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82111902"
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Creación de un grupo de Azure Batch en una red virtual

Al crear un grupo de Azure Batch, puede aprovisionarlo en una subred de una [red virtual de Azure](../virtual-network/virtual-networks-overview.md) que especifique. En este artículo se explica cómo configurar un grupo de Batch en una red virtual.

## <a name="why-use-a-vnet"></a>¿Por qué se utiliza una red virtual?

Un grupo de Azure Batch tiene una configuración para permitir que los nodos de proceso se comuniquen entre sí; por ejemplo, para ejecutar tareas de varias instancias. Esta configuración no requiere una red virtual distinta. Sin embargo, de forma predeterminada, los nodos no pueden comunicarse con máquinas virtuales que no forman parte del grupo de Batch, como un servidor de licencias o un servidor de archivos. Para permitir que los nodos de proceso del grupo se comuniquen de manera segura con otras máquinas virtuales, o con una red local, puede aprovisionar el grupo en una subred de una red virtual de Azure.

## <a name="prerequisites"></a>Prerrequisitos

* **Autenticación**. Para usar una red virtual de Azure, la API de cliente de Batch debe usar la autenticación de Azure Active Directory (AD). La compatibilidad de Azure Batch con Azure AD se documenta en [Autenticación de soluciones de servicio de Batch con Active Directory](batch-aad-auth.md).

* **Una red virtual de Azure**. Consulte la sección siguiente para conocer los requisitos de la red virtual y cómo configurarla. Para preparar una red virtual con una o varias subredes previamente, puede usar Azure Portal, Azure PowerShell, la interfaz de línea de comandos (CLI) de Azure u otros métodos.
  * Para crear una red virtual basada en Azure Resource Manager, consulte [Creación de una red virtual](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Se recomienda una red virtual basada en Resource Manager para implementaciones nuevas y solo se admite en grupos en la configuración de la máquina virtual.
  * Para crear una red virtual clásica, consulte [Creación de una red virtual (clásica) con varias subredes](../virtual-network/create-virtual-network-classic.md). Una red virtual clásica solo se admite en grupos en la configuración de Cloud Services.

## <a name="vnet-requirements"></a>Requisitos de la red virtual

[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]

## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Creación de un grupo con una red virtual en el portal

Una vez que haya creado la red virtual y le haya asignado una subred, puede crear un grupo de Batch a esa red virtual. Siga estos pasos para crear un grupo en Azure Portal: 

1. Vaya a la cuenta de Batch en Azure Portal. Esta cuenta debe estar en la misma suscripción y región que el grupo de recursos que contiene la red virtual que pretende utilizar.
2. En la ventana **Configuración** que aparece a la izquierda, seleccione el elemento de menú **Grupos**.
3. En la ventana **Grupos**, seleccione el comando **Agregar**.
4. En la ventana **Agregar grupo**, seleccione la opción que desea utilizar en el menú desplegable **Tipo de imagen**.
5. Seleccione el valor correcto de **publicador/oferta/SKU** para la imagen personalizada.
6. Especifique los valores requeridos restantes, incluido el **tamaño de nodo**, los **nodos dedicados de destino** y los **nodos de baja prioridad**, además de cualquier otro valor opcional que desee.
7. En **Virtual Network**, seleccione la red virtual y la subred que desea usar.

   ![Adición de un grupo con red virtual](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Rutas definidas por el usuario para la tunelización forzada

Es posible que en su organización se requiera redirigir (forzar) el tráfico vinculado con Internet desde la subred a la ubicación local con fines de inspección y registro. Es posible que haya habilitado la tunelización forzada para las subredes de la red virtual.

Para asegurarse de que sus nodos de proceso del grupo de Azure Batch funcionan en una red virtual que tiene habilitada la tunelización forzada, debe agregar las siguientes [rutas definidas por el usuario](../virtual-network/virtual-networks-udr-overview.md) a esa subred:

* El servicio Batch debe comunicarse con los nodos de proceso de grupo para programar tareas. Para permitir esta comunicación, agregue una ruta definida por el usuario para cada dirección IP utilizada por el servicio Batch en la región donde existe su cuenta de Batch. Para más información sobre cómo obtener la lista de direcciones IP del servicio Batch, consulte [Etiquetas de servicio en un entorno local](../virtual-network/service-tags-overview.md). Las direcciones IP del servicio Batch se asociarán a la etiqueta de servicio `BatchNodeManagement` (o la variante regional que coincida con la región de la cuenta de Batch).

* Asegúrese de que el tráfico saliente hacia Azure Storage (en concreto, las direcciones URL del formulario `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, y `<account>.blob.core.windows.net`) no está bloqueado mediante el dispositivo de red local.

Cuando se agregueuna ruta definida por el usuario, defina la ruta para cada prefijo de dirección IP de Batch relacionada y establezca **Tipo de próximo salto** en **Internet**. Vea el ejemplo siguiente:

![Ruta definida por el usuario](./media/batch-virtual-network/user-defined-route.png)

> [!WARNING]
> Las direcciones IP del servicio Batch pueden cambiar con el tiempo. Para evitar una interrupción debida a un cambio de dirección IP, se recomienda establecer un proceso periódico para actualizar automáticamente las direcciones IP del servicio Batch y mantenerlas actualizadas en la tabla de enrutamiento.

## <a name="next-steps"></a>Pasos siguientes

- Para información general más detallada acerca de Batch, consulte [Desarrollo de soluciones de procesos paralelos a gran escala con Batch](batch-api-basics.md).
- Para más información sobre cómo crear una ruta definida por el usuario, consulte [Creación de una ruta definida por el usuario - Azure Portal](../virtual-network/tutorial-create-route-table-portal.md).
