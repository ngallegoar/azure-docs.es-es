---
title: Configuración del apagado automático de las VM en Azure Lab Services
description: En este artículo se describe cómo configurar el apagado automático de las VM en la cuenta de laboratorio.
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
ms.openlocfilehash: afe74e5c3aec2519ec22eee9573ae7b47c1c73f8
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588214"
---
# <a name="configure-automatic-shutdown-of-vms-on-disconnect-setting-for-a-lab-account"></a>Configuración del apagado automático de las VM al desconectarse de una cuenta de laboratorio
El apagado automático de las máquinas virtuales de laboratorio Windows (plantilla o alumno) se habilitará o deshabilitará después de cerrar la conexión a Escritorio remoto. También puede especificar cuánto tiempo debe esperar Lab Services para que el usuario se vuelva a conectar antes de que se apague automáticamente.

## <a name="enable-automatic-shutdown"></a>Habilitación del apagado automático

1. En la página **Cuenta de laboratorio**, seleccione **Configuración del laboratorio** en el menú de la izquierda.
2. Seleccione la opción **Apagar automáticamente las máquinas virtuales cuando los usuarios se desconecten**.
3. Especifique cuánto tiempo debe esperar Lab Services para que el usuario se vuelva a conectar antes de apagar automáticamente las VM.

    ![Configuración del apagado automático en la cuenta de laboratorio](../media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

    Esta configuración se aplica a todos los laboratorios creados en la cuenta de laboratorio. Un creador de laboratorio (formador) puede invalidar esta configuración en el nivel de laboratorio. El cambio que se realice en esta configuración en la cuenta de laboratorio solo afectará a los laboratorios creados después.

    Para deshabilitar esta configuración, desactive la casilla de verificación de la opción **Apagar automáticamente las máquinas virtuales cuando los usuarios se desconecten** de esta página. 

## <a name="next-steps"></a>Pasos siguientes
Para información sobre cómo los propietarios de laboratorio pueden configurar o anular esta configuración en el nivel de laboratorio, consulte [este artículo](how-to-enable-shutdown-disconnect.md).
