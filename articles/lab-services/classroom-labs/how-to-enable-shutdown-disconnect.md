---
title: Habilitación del apagado de las máquinas virtuales al desconectar Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo habilitar o deshabilitar el apagado automático de las máquinas virtuales cuando se desconecta una conexión a escritorio remoto.
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
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: b0c7f5daa6bcd9ab5cb8f4d7b1a513a15cd1c708
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83591716"
---
# <a name="enable-automatic-shutdown-of-vms-on-disconnect"></a>Habilitación del apagado automático de las máquinas virtuales al desconectarse
En este artículo se muestra cómo habilitar o deshabilitar el apagado automático de máquinas virtuales de laboratorio **Windows 10** (plantilla o alumno) después de cerrar una conexión de escritorio remoto. También puede especificar cuánto tiempo deben esperar las máquinas virtuales para que el usuario se vuelva a conectar antes de que se apaguen automáticamente.

Un administrador de cuentas de laboratorio puede configurar esta opción para la cuenta de laboratorio en la que crea laboratorios. Para obtener más información, consulte [Configurar el apagado automático de las máquinas virtuales al desconectarse de una cuenta de laboratorio](how-to-configure-lab-accounts.md). Como propietario de un laboratorio, puede invalidar la configuración al crear un laboratorio o después de su creación. 

## <a name="configure-when-creating-a-lab"></a>Configuración al crear un laboratorio
En la página del paso 3 del Asistente para creación del laboratorio, puede habilitar o deshabilitar esta característica y también especificar cuánto tiempo debe esperar la máquina virtual para que el usuario se vuelva a conectar antes de cerrarse automáticamente. 

![Configuración al crear el laboratorio](../media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

## <a name="configure-after-the-lab-is-created"></a>Configuración después de crear el laboratorio
Puede configurar esta opción en la página **Configuración** tal y como se muestra en la siguiente imagen: 

![Configuración después de crear el laboratorio](../media/how-to-enable-shutdown-disconnect/configure-lab-automatic-shutdown.png)

> [!WARNING]
> Si apaga el sistema operativo Windows (SO) en una máquina virtual antes de desconectar en ella una sesión RDP, la característica de apagado automático no funcionará correctamente.  

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Panel para laboratorios de clase](use-dashboard.md)