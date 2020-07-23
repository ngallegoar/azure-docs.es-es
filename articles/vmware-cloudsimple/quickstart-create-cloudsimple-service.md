---
title: 'Inicio rápido: Creación del servicio VMware CloudSimple'
titleSuffix: Azure VMware Solution by CloudSimple
description: Aprenda a crear el servicio CloudSimple, a adquirir y a reservar nodos
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 14df0f131aaef8a4c24e2d1eb242a9b440e7c7b0
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507597"
---
# <a name="quickstart---create-azure-vmware-solution-by-cloudsimple-service"></a>Inicio rápido: creación de un servicio de Azure VMware Solution by CloudSimple

Para empezar, cree una solución Azure VMware Solution by CloudSimple en Azure Portal.

## <a name="vmware-solution-by-cloudsimple---service-overview"></a>VMware Solution by CloudSimple: información general del servicio

El servicio CloudSimple le permite consumir Azure VMware Solution by CloudSimple.  Al crear el servicio, puede aprovisionar nodos y reservarlos, y crear nubes privadas.  El servicio CloudSimple se agrega en cada región de Azure donde está disponible.  El servicio define la red perimetral de Azure VMware Solution by CloudSimple.  Esta red perimetral se usa en servicios que incluyen VPN, ExpressRoute y conectividad de Internet a las nubes privadas.

Para agregar el servicio CloudSimple, debe crear una subred de puerta de enlace. La subred de puerta de enlace se usa al crear la red perimetral y requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No puede superponerse con ninguno de los espacios de direcciones de red local ni con el espacio de direcciones de red virtual de Azure.

## <a name="before-you-begin"></a>Antes de empezar

Asigne un bloque CIDR /28 para la subred de puerta de enlace.  Se requiere una subred de puerta de enlace por cada servicio CloudSimple y es única para la región en la que se crea. Los servicios de red perimetral de Azure VMware Solution by CloudSimple usan la subred de la puerta de enlace, que requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No se debe superponer con otras redes que se comuniquen con el entorno de CloudSimple.  Las redes que se comunican con CloudSimple incluyen redes locales y redes virtuales de Azure.

Consulte [Requisitos previos de redes avanzadas](cloudsimple-network-checklist.md). 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-the-service"></a>Creación del servicio

1. Seleccione **Todos los servicios**.
2. Busque **Servicio CloudSimple**.

    ![Búsqueda del servicio CloudSimple](media/create-cloudsimple-service-search.png)

3. Seleccione **Servicios CloudSimple**.
4. Haga clic en **Agregar** para crear un servicio.

    ![Adición del servicio CloudSimple](media/create-cloudsimple-service-add.png)

5. Seleccione la suscripción en la que quiera crear el servicio CloudSimple.
6. Seleccione el grupo de recursos del servicio. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.
7. Escriba un nombre para identificar el servicio.
8. Escriba el CIDR de la puerta de enlace de servicio. Especifique una subred /28 que no se superponga con ninguna de sus subredes locales, las subredes de Azure ni las subredes planeadas de CloudSimple. No puede cambiar el CIDR después de crear el servicio.

    ![Creación del servicio CloudSimple](media/create-cloudsimple-service.png)

9. Haga clic en **OK**.

El servicio se crea y se agrega a la lista de servicios.

## <a name="provision-nodes"></a>Aprovisionamiento de nodos

Para configurar la capacidad de pago por uso de un entorno de nube privada de CloudSimple, aprovisione primero los nodos en Azure Portal.

1. Seleccione **Todos los servicios**.
2. Busque **Nodos CloudSimple**.

    ![Búsqueda de los nodos CloudSimple](media/create-cloudsimple-node-search.png)

3. Seleccione **Nodos CloudSimple**.
4. Haga clic en **Agregar** para crear los nodos.

    ![Adición de los nodos CloudSimple](media/create-cloudsimple-node-add.png)

5. Seleccione la suscripción en la que quiera aprovisionar los nodos CloudSimple.
6. Seleccione el grupo de recursos de los nodos. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.
7. Escriba el prefijo para identificar los nodos.
8. Seleccione la ubicación de los recursos de los nodos.
9. Seleccione la ubicación dedicada en la que hospedar los recursos de los nodos.
10. Seleccione el [tipo de nodo](cloudsimple-node.md).
11. Seleccione el número de nodos que quiere aprovisionar.
12. Seleccione **Revisar + crear**.
13. Revise la configuración. Para modificar la configuración, haga clic en **Anterior**.
14. Seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de la nube privada y configuración del entorno](quickstart-create-private-cloud.md)
* Más información sobre el [servicio CloudSimple](./cloudsimple-service.md)
