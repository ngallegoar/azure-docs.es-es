---
title: Configuración de cuentas de laboratorio en Azure Lab Services | Microsoft Docs
description: En este artículo se describe cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio en Azure Lab Services.
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
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: fa9dba62b3b58687ec6a2bfc29e8722f7016b679
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79237264"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Configuración de cuentas de laboratorio en Azure Lab Services 
En Azure Lab Services, una cuenta de laboratorio es un contenedor para tipos de laboratorios administrados, como los laboratorios de clase. Los administradores configuran una cuenta de laboratorio con Azure Lab Services y proporcionan acceso a los propietarios del laboratorio que pueden crear laboratorios en la cuenta. 

Este artículo muestra cómo realizar las siguientes tareas: 

- Especificación de un intervalo de direcciones para las máquinas virtuales del laboratorio
- Configuración del apagado automático de las máquinas virtuales al desconectarse

## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Especificación de un intervalo de direcciones para las máquinas virtuales del laboratorio
El procedimiento siguiente tiene pasos para especificar un intervalo de direcciones para las máquinas virtuales del laboratorio. Si actualiza el intervalo que ha especificado antes, el intervalo de direcciones modificado se aplica solo a las máquinas virtuales que se crean después de realizar el cambio. 

Estas son algunas restricciones al especificar el intervalo de direcciones que debe tener en cuenta. 

- El prefijo debe ser menor o igual a 23. 
- Si una red virtual se empareja con la cuenta de laboratorio, el intervalo de direcciones proporcionado no puede coincidir con el de la red virtual emparejada.

1. En la página **Cuenta de laboratorio**, seleccione **Configuración del laboratorio** en el menú de la izquierda.
2. Para el campo **Intervalo de direcciones**, especifique el intervalo de direcciones para las máquinas virtuales que se van a crear en el laboratorio. Dicho rango debe estar en la notación de enrutamiento de interdominios sin clases (CIDR) (por ejemplo: 10.20.0.0/23). Las máquinas virtuales del laboratorio se crearán en este rango de direcciones.
3. Seleccione **Guardar** en la barra de herramientas. 

    ![Configuración del intervalo de direcciones](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)


## <a name="automatic-shutdown-of-vms-on-disconnect"></a>Apagado automático de las máquinas virtuales al desconectarse
El apagado automático de las máquinas virtuales de laboratorio Windows (plantilla o alumno) se habilitará o deshabilitará después de cerrar la conexión a Escritorio remoto. También puede especificar cuánto tiempo deben esperar las máquinas virtuales para que el usuario se vuelva a conectar antes de que se apaguen automáticamente.

![Configuración del apagado automático en la cuenta de laboratorio](../media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

Esta configuración se aplica a todos los laboratorios creados en la cuenta de laboratorio. Un propietario de laboratorio puede invalidar esta configuración en el nivel de laboratorio. El cambio que se realice en esta configuración en la cuenta de laboratorio solo afectará a los laboratorios creados después.

Para información sobre cómo pueden configurar los propietarios de laboratorio esta opción en el nivel de laboratorio, consulte [este artículo.](how-to-enable-shutdown-disconnect.md)

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Permiso para que el creador del laboratorio seleccione su ubicación](allow-lab-creator-pick-lab-location.md)
- [Conexión de la red del laboratorio con una red virtual del mismo nivel en Azure Lab Services](how-to-connect-peer-virtual-network.md)
- [Asociación o desasociación de una galería de imágenes compartidas en Azure Lab Services](how-to-attach-detach-shared-image-gallery.md)
- [Cómo agregar un usuario como propietario de un laboratorio de clase en Azure Lab Services](how-to-add-user-lab-owner.md)
- [Configuración del firewall para Azure Lab Services](how-to-configure-firewall-settings.md)
