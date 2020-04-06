---
title: Habilitación de la virtualización anidada en una plantilla de máquina virtual en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear una plantilla de máquina virtual con varias máquinas virtuales dentro.  En otras palabras, habilite la virtualización anidada en una plantilla de máquina virtual en Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 59b32834369f76d39bb4a253dad4ec541e7ef999
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502014"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Habilitación de la virtualización anidada en una plantilla de máquina virtual en Azure Lab Services

Actualmente, Azure Lab Services le permite configurar una máquina virtual de una plantilla en un laboratorio y realizar una copia única disponible para cada uno de los usuarios. Si usted es un profesor que hace clases de redes, seguridad o TI, puede que necesite proporcionar a cada uno de sus alumnos un entorno en el que varias máquinas virtuales puedan comunicarse entre sí a través de una red.

La virtualización anidada le permite crear un entorno de varias máquinas virtuales dentro de la máquina virtual de plantilla de un laboratorio. La publicación de la plantilla proporcionará a cada usuario del laboratorio una máquina virtual configurada con varias máquinas virtuales dentro.  En este artículo se explica cómo configurar la virtualización anidada en una máquina de pantilla en Azure Lab Services.

## <a name="what-is-nested-virtualization"></a>¿Qué es la virtualización anidada?

La virtualización anidada le permite crear máquinas virtuales dentro de una máquina virtual. La virtualización anidada se realiza a través de Hyper-V y solo está disponible en las máquinas virtuales de Windows.

Para más información sobre la virtualización anidada, vea los siguientes artículos:

- [Virtualización anidada en Azure](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Habilitación de la virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md)

## <a name="considerations"></a>Consideraciones

Antes de configurar un laboratorio con virtualización anidada, estas son algunas cosas que debe tener en cuenta.

- Al crear un laboratorio, seleccione los tamaños **Medio (virtualización anidada)** o **Grande (virtualización anidada)** como tamaño de la máquina virtual. Estos tamaños de máquina virtual admiten la virtualización anidada.
- Elija un tamaño que proporcione un buen rendimiento para las máquinas virtuales de host y de cliente.  Recuerde que, al usar la virtualización, el tamaño que elija debe ser adecuado no solo para una máquina, sino también para el host y las máquinas cliente que deban ejecutarse simultáneamente.
- Las máquinas virtuales de cliente no tendrán acceso a los recursos de Azure como, por ejemplo, a los servidores DNS de la red virtual de Azure.
- La máquina virtual del host requiere configuración para permitir que la máquina cliente tenga conectividad a Internet.
- Las máquinas virtuales de cliente tienen licencia como máquinas independientes. Consulte [Licencias de Microsoft](https://www.microsoft.com/licensing/default) para más información sobre las licencias de los productos y sistemas operativos de Microsoft. Compruebe los contratos de licencia de cualquier otro software que se esté usando antes de configurar la plantilla de máquina virtual.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Habilitación de la virtualización anidada en una máquina virtual de plantilla

En este artículo se supone que ha creado una cuenta de laboratorio y un laboratorio.  Para más información sobre la creación de una nueva cuenta de laboratorio, consulte el [tutorial para configurar una cuenta de laboratorio](tutorial-setup-lab-account.md). Para más información sobre cómo crear un laboratorio, consulte el [tutorial Configuración de un laboratorio educativo](tutorial-setup-classroom-lab.md).

>[!IMPORTANT]
>Seleccione **Grande (virtualización anidada)** o **Medio (virtualización anidada)** como tamaño de la máquina virtual al crear el laboratorio.  De lo contrario, la virtualización anidada no funcionará.  

Para conectarse a la máquina de plantilla, consulte [Creación y administración de plantillas educativas en Azure Lab Services](how-to-create-manage-template.md).

### <a name="using-script-to-enable-nested-virtualization"></a>Uso de un script para habilitar la virtualización anidada

Si quiere usar la configuración automatizada para la virtualización anidada con Windows Server 2016 o Windows Server 2019, consulte [Habilitación de la virtualización anidada en una máquina virtual de plantilla en Azure Lab Services mediante un script](how-to-enable-nested-virtualization-template-vm-using-script.md). Usará scripts de [Hyper-V de Lab Services](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV) para instalar el rol de Hyper-V.  Los scripts también configurarán las funciones de red para que las máquinas virtuales de Hyper-V puedan acceder a Internet.

### <a name="using-windows-tools-to-enable-nested-virtualization"></a>Uso de las herramientas de Windows para habilitar la virtualización anidada

Si quiere configurar la virtualización anidada para Windows Server 2016 o Windows Server 2019 con los roles y herramientas administrativas de Windows, consulte [Habilitación de la virtualización anidada en una máquina virtual de plantilla en Azure Lab Services de forma manual](how-to-enable-nested-virtualization-template-vm-ui.md).  Las instrucciones también explican cómo configurar las funciones de red para que las máquinas virtuales de Hyper-V puedan acceder a Internet.
