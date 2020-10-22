---
title: 'Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer: Azure Portal'
description: Aprenda a configurar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante Azure Portal.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: cb7bfb9ac4b10b807ac186d087b0037953abd559
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91439514"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-portal"></a>Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante Azure Portal

En este artículo, aprenderá a configurar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer. 

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure.
- Un equilibrador de carga de SKU estándar existente en la suscripción donde se implementará el conjunto de escalado de máquinas virtuales.
- Una instancia de Azure Virtual Network para el conjunto de escalado de máquinas virtuales.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).



## <a name="deploy-virtual-machine-scale-set-with-existing-load-balancer"></a>Implementación de un conjunto de escalado de máquinas virtuales con un equilibrador de carga existente

En esta sección, creará un conjunto de escalado de máquinas virtuales en Azure Portal con un equilibrador de carga de Azure existente.

> [!NOTE]
> En los pasos siguientes se supone que se ha implementado previamente una red virtual denominada **myVNet** y un equilibrador de carga de Azure denominado **myLoadBalancer**.

1. En la parte superior izquierda de la pantalla, haga clic en **Crear un recurso** > **Proceso** > **Conjunto de escalado de máquinas virtuales** o busque **Conjunto de escalado de máquinas virtuales** en la búsqueda de Marketplace.

2. Seleccione **Crear**.

3. En **Creación de un conjunto de escalado de máquinas virtuales**, escriba o seleccione esta información en la pestaña **Conceptos básicos**:

    | Configuración                        | Value                                                                                                 |
    |--------------------------------|-------------------------------------------------------------------------------------------------------|
    | **Detalles del proyecto**            |                                                                                                       |
    | Subscription                   | Selección de su suscripción a Azure                                                                        |
    | Grupo de recursos                 | Seleccione Crear nuevo, escriba **myResourceGroup**, seleccione Aceptar o seleccione un grupo de recursos existente. |
    | **Detalles del conjunto de escalado**          |                                                                                                       |
    | Nombre del conjunto de escalado de máquinas virtuales | Escriba **myVMSS**.                                                                                      |
    | Region                         | Seleccione **Este de EE. UU. 2**.                                                                                    |
    | Zona de disponibilidad              | Seleccione **Ninguno**.                                                                                       |
    | **Detalles de instancia**           |                                                                                                       |
    | Imagen                          | Seleccione **Ubuntu Server 18.04 LTS**.                                                                    |
    | Instancia de Azure Spot            | Seleccione **No**.                                                                                         |
    | Size                           | Deje el valor predeterminado.                                                                                      |
    | **Cuenta de administrador**      |                                                                                                       |
    | Tipo de autenticación            | Seleccione **Contraseña**.                                                                                   |
    | Nombre de usuario                       | Escriba el nombre de usuario de administrador.        |
    | Contraseña                       | Escriba la contraseña del administrador.    |
    | Confirmar contraseña               | Escriba de nuevo la contraseña del administrador. |


    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-01.png" alt-text="Captura de pantalla que muestra la pestaña Aspectos básicos de Crear conjunto de escalado de máquinas virtuales." border="true":::

4. Seleccione la pestaña **Redes**.

5. Escriba o seleccione esta información en la pestaña **Redes**:

     Configuración                           | Value                                                    |
    |-----------------------------------|----------------------------------------------------------|
    | **Configuración de redes virtuales** |                                                          |
    | Virtual network                   | Seleccione **myVNet** o la red virtual existente.      |
    | **Equilibrio de carga**                |                                                          |
    | Usar un equilibrador de carga               | Seleccione **Sí**.                                           |
    | **Configuración de equilibrio de carga**       |                                                          |
    | Opciones de equilibrio de carga            | Seleccione el **equilibrador de carga de Azure**.                           |
    | Seleccionar un equilibrador de carga            | Seleccione **myLoadBalancer** o el equilibrador de carga existente. |
    | Seleccionar un grupo de back-end             | Seleccione **myBackendPool** o el grupo de back-end existente.  |

    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-02.png" alt-text="Captura de pantalla que muestra la pestaña Aspectos básicos de Crear conjunto de escalado de máquinas virtuales." border="true":::

6. Seleccione la pestaña **Administración**.

7. En la pestaña **Administración**, establezca **Diagnósticos de arranque** en **Desactivado**.

8. Seleccione el botón azul **Revisar y Crear**.

9. Revise la configuración y seleccione el botón **Crear**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, implementó un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer.  Para obtener más información sobre los conjuntos de escalado de máquinas virtuales y el equilibrador de carga, consulte:

- [¿Qué es Azure Load Balancer?](load-balancer-overview.md)
- [¿Qué son los conjuntos de escalado de máquinas virtuales?](../virtual-machine-scale-sets/overview.md)
