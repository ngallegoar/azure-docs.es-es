---
title: Habilitar una conexión de explorador en máquinas virtuales de Azure DevTest Labs | Microsoft Docs
description: Ahora, DevTest Labs se integra con Azure Bastion, lo que le permite, como propietario del laboratorio, habilitar el acceso a todas las máquinas virtuales de laboratorio a través de un explorador.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2019
ms.author: takamath
ms.openlocfilehash: 2ddc56c60c547bd4ce48d620a83fb79246762bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "69641984"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Habilitar una conexión del explorador en máquinas virtuales de laboratorio 

DevTest Labs se integra con [Azure Bastion](https://docs.microsoft.com/azure/bastion/), lo que le permite conectarse a sus máquinas virtuales a través de un explorador. En primer lugar, hay que habilitar una conexión del explorador en las máquinas virtuales de laboratorio en cuestión.

Como propietario de un laboratorio, puede habilitar el acceso a todas las máquinas virtuales de laboratorio a través de un explorador. No es necesario un cliente, agente o componente de software adicional. Azure Bastion proporciona una conexión RDP/SSH segura e ininterrumpida a las máquinas virtuales directamente en Azure Portal a través de SSL. Cuando se conecta a través de Azure Bastion, las máquinas virtuales no necesitan una dirección IP pública. Para más información, vea [¿Qué es Azure Bastion?](../bastion/bastion-overview.md)

> [!NOTE]
> La capacidad de habilitar una conexión del explorador en máquinas virtuales de laboratorio está en fase de versión preliminar.

En este artículo se explica cómo habilitar una conexión del explorador en máquinas virtuales de laboratorio.

## <a name="prerequisites"></a>Prerrequisitos 
Implemente un host de Bastion en la red virtual del laboratorio existente **O** conecte el laboratorio a una red virtual configurada con Bastion. 

Para saber cómo implementar un host de Bastion en una red virtual, vea [Creación de un host de Azure Bastion (versión preliminar)](../bastion/bastion-create-host-portal.md). Al crear el host de Bastion, seleccione la red virtual del laboratorio. 

Para saber cómo conectar el laboratorio con una red virtual configurada de Bastion, vea [Configuración de una red virtual en Azure DevTest Labs](devtest-lab-configure-vnet.md). Seleccione la red virtual que tenga implementado el host de Bastion y **AzureBastionSubnet**. Estos son los pasos detallados: 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Todos los servicios** en el menú de navegación izquierdo. 
1. Seleccione **DevTest Labs** en la lista. 
1. En la lista de laboratorios, seleccione el *suyo*. 

    > [!NOTE]
    > Azure Bastion se encuentra actualmente en fase de versión preliminar y se limita a las siguientes regiones: Oeste de EE. UU., Este de EE. UU., Oeste de Europa, Centro-sur de EE. UU., Este de Australia y Japón Oriental. Por lo tanto, cree un laboratorio en una de estas regiones si el suyo no está en una de ellas. 
1. Seleccione **Configuración y directivas** en la sección **Configuración** en el menú de la izquierda. 
1. Seleccione **Redes virtuales**.
1. Seleccione **Agregar** en la barra de herramientas. 
1. Seleccione la **red virtual** que tenga implementado el host de Bastion. 
1. Seleccione la subred **AzureBastionSubnet**. 

    ![Subnet](./media/enable-browser-connection-lab-virtual-machines/subnet.png)
1. Seleccione la opción **Usar en la creación de máquinas virtuales**. 
1. Seleccione **Guardar** en la barra de herramientas. 
1. Si tiene una red virtual anterior del laboratorio, quítela seleccionando * *...* y, después, **Quitar**. 

## <a name="enable-browser-connection"></a>Habilitar una conexión del explorador 

Una vez que tenga configurada una red virtual de Bastion en el laboratorio, como propietario del laboratorio, puede habilitar una conexión del explorador en máquinas virtuales del laboratorio.

Haga lo siguiente para habilitar una conexión del explorador en las máquinas virtuales de laboratorio:

1. En Azure Portal, vaya a *su laboratorio*.
1. Seleccione **Configuration and policies** (Directivas y configuración).
1. En **Configuración**, seleccione **Browser connect (Preview)** (Conexión de explorador [versión preliminar]).

![Habilitar una conexión del explorador](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Pasos siguientes
Vea el siguiente artículo para saber cómo conectarse a máquinas virtuales con un explorador: [Conexión a las máquinas virtuales a través de un explorador](connect-virtual-machine-through-browser.md)