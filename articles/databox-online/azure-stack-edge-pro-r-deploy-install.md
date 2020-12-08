---
title: 'Tutorial: Instalación del dispositivo físico Azure Stack Edge Pro R | Microsoft Docs'
description: El segundo tutorial sobre la instalación de Azure Stack Edge Pro R explica cómo cablear el dispositivo físico a la alimentación y la red.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/18/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Azure Stack Edge Pro R in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: b67da2607d206424f69f53645eda148495ea58ec
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464593"
---
# <a name="tutorial-install-azure-stack-edge-pro-r"></a>Tutorial: Instalación de Azure Stack Edge Pro R

Este tutorial describe cómo instalar un dispositivo físico Azure Stack Edge Pro R. El procedimiento de instalación requiere el cableado del dispositivo.

La instalación puede tardar alrededor de 30 minutos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Inspección del dispositivo
> * Cableado del dispositivo

## <a name="prerequisites"></a>Requisitos previos

Los requisitos previos para instalar un dispositivo físico son los siguientes:

### <a name="for-the-azure-stack-edge-resource"></a>Para el recurso de Azure Stack Edge

Antes de comenzar, asegúrese de que:

* Ha completado todos los pasos del tutorial [Preparación de la implementación de Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-prep.md).
    * Ha creado un recurso de Azure Stack Edge para implementar el dispositivo.
    * Ha generado la clave de activación para activar el dispositivo con el recurso de Azure Stack Edge.

 
### <a name="for-the-azure-stack-edge-pro-r-physical-device"></a>Para el dispositivo físico Azure Stack Edge Pro R

Antes de implementar un dispositivo:

- Asegúrese de colocar el dispositivo de forma segura sobre una superficie de trabajo plana, estable y nivelada.
- Compruebe que el sitio donde desea realizar la instalación tiene:
    - Corriente alterna estándar de una fuente independiente

        O
    - Una unidad de distribución de energía (PDU) de bastidor. El dispositivo se suministra con un sistema de alimentación ininterrumpida.
    

### <a name="for-the-network-in-the-datacenter"></a>Para la red del centro de datos

Antes de empezar:

- Revise los requisitos de red para implementar Azure Stack Edge Pro R y configure la red del centro de datos según dichos requisitos. Para más información, consulte [Requisitos de red de Azure Stack Edge Pro R](azure-stack-edge-pro-r-system-requirements.md#networking-port-requirements).

- Asegúrese de que el ancho de banda mínimo de Internet sea de 20 Mbps para que el dispositivo funcione de forma óptima.


## <a name="inspect-the-device"></a>Inspección del dispositivo

Este dispositivo se suministra como una única unidad. Realice los pasos siguientes para desempaquetar el dispositivo.

1. Coloque la caja en una superficie plana y nivelada.
2. Compruebe si la caja del dispositivo tiene daños. Abra la caja e inspeccione el dispositivo. Si la caja o el dispositivo parecen dañados, póngase en contacto con el servicio de soporte técnico de Microsoft para que le ayude a determinar si el dispositivo está en buen estado.
3. Una vez abierta la caja, asegúrese de que dispone de los siguientes elementos:
    - Un dispositivo contenedor Azure Stack Edge Pro R
    - Un sistema de alimentación ininterrumpida
    - Dos cables de alimentación corta para conectar el dispositivo al sistema de alimentación ininterrumpida
    - Un cable de alimentación para conectar el sistema de alimentación ininterrumpida a la fuente de alimentación

Si no ha recibido todos los elementos enumerados aquí, póngase en contacto con el equipo de soporte técnico de Azure Stack Edge Pro R. El siguiente paso consiste en cablear el dispositivo.


## <a name="cable-the-device"></a>Cableado del dispositivo

En los siguientes procedimientos se describe cómo cablear el dispositivo Azure Stack Edge Pro R a la alimentación y la red.

Antes de empezar el cableado del dispositivo, necesita lo siguiente:

- El dispositivo físico Azure Stack Edge Pro R en el sitio de instalación.
- Un cable de alimentación.
- Al menos un cable de red RJ-45 de 1-GbE cable para conectarse a la interfaz de administración de red. Hay dos interfaces de red de 1-GbE, uno de administración y otro de datos, en el dispositivo.
- Un cable de cobre SFP+ de 10/25 GbE para cada interfaz de red de datos que se vaya a configurar. Al menos una interfaz de red de datos de los puertos 3 o 4 debe estar conectada a Internet (con conectividad para Azure).  
- Acceso a una unidad de distribución de energía (recomendado).

> [!NOTE]
> - Si va a conectar solo una interfaz de red de datos, es recomendable que use una interfaz de red de 25/10 GbE, como los puertos 3 o 4, para enviar datos a Azure. 
> - Para obtener el mejor rendimiento y controlar grandes volúmenes de datos, considere la posibilidad de conectar todos los puertos de datos.
> - El dispositivo Azure Stack Edge Pro R debe estar conectado a la red del centro de datos para que pueda realizar la ingesta de datos desde servidores de origen de datos.

En el dispositivo Azure Stack Edge Pro R:

- El panel frontal tiene unidades de disco y un botón de encendido.

    - Hay 8 ranuras de disco en la parte frontal del dispositivo.
    - El dispositivo también incorpora 2 discos SATA M.2 que sirven como discos del sistema operativo. 

- El backplane incluye fuentes de alimentación (PSU) redundantes.
- El backplane tiene cuatro interfaces de red:

    - Dos interfaces de 1 Gbps.
    - Dos interfaces de 25 Gbps que también sirven como interfaces de 10 Gbps.
    - Un controlador de administración de placa base (BMC).

<!--- The back plane has two network cards corresponding to the 4 ports:

    - QLogic FastLinQ 41264
    - QLogic FastLinQ 41262

For a full list of supported cables, switches, and transceivers for these network cards, go to [Cavium FastlinQ 41000 Series Interoperability Matrix](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).-->
 
Realice los pasos siguientes para realizar el cableado de los cables de alimentación y de red del dispositivo.

1. Identifique los distintos puertos del backplane del dispositivo.

    ![Backplane de un dispositivo cableado](./media/azure-stack-edge-pro-r-deploy-install/backplane-cabled.png)

2. Busque las ranuras de los discos y el botón de encendido en el frontal del dispositivo.

    ![Plano delantero de un dispositivo](./media/azure-stack-edge-pro-r-deploy-install/device-front-plane-labeled-1.png)

3. Conecte un extremo del cable de alimentación al sistema de alimentación ininterrumpida. Conecte el otro extremo del cable de alimentación a la unidad de distribución de energía de bastidor. 
5. Presione el botón de encendido para encender el dispositivo.
6. Conecte el PUERTO 1 de la interfaz de red de 1-GbE al equipo que se usa para configurar el dispositivo físico. El PUERTO 1 es la interfaz de administración dedicada.
7. Conecte uno o varios de los puertos 2, 3 o 4 a la red del centro de datos o a Internet.

    - Si conecta el PUERTO 2, utilice el cable de red RJ-45.
    - Para las interfaces de red de 10/25 GbE, use cables de cobre SFP+.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información sobre varios temas relacionados con Azure Stack Edge Pro R. Por ejemplo:

> [!div class="checklist"]
> * Desempaquetado del dispositivo
> * Cableado del dispositivo

Continúe con el siguiente tutorial para aprender a conectar el dispositivo.

> [!div class="nextstepaction"]
> [Conexión a Azure Stack Edge Pro R](./azure-stack-edge-pro-r-deploy-connect.md)
