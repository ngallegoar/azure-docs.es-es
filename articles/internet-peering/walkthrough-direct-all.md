---
title: Tutorial del emparejamiento directo
titleSuffix: Azure
description: Tutorial del emparejamiento directo
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e27fa26514d27d68aecdf9e28b36e2747dc8ffe2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84710769"
---
# <a name="direct-peering-walkthrough"></a>Tutorial del emparejamiento directo

En esta sección se explican los pasos que debe seguir para configurar y administrar un emparejamiento directo.

## <a name="create-a-direct-peering"></a>Creación de un emparejamiento directo
> [!div class="mx-imgBorder"]
> ![Estados de conexión y flujo de trabajo del emparejamiento directo](./media/direct-peering.png)

Se deben seguir los pasos siguientes para aprovisionar un emparejamiento directo:
1. Revise la [directiva de emparejamiento](https://peering.azurewebsites.net/peering) de Microsoft para comprender los requisitos del emparejamiento directo.
1. Siga las instrucciones de [Creación o modificación de un emparejamiento directo](howto-direct-powershell.md) para enviar una solicitud de emparejamiento.
1. Después de enviar una solicitud de emparejamiento, Microsoft se pondrá en contacto con su dirección de correo electrónico registrada para proporcionar la carta de autorización (LOA) u otra información.
1. Una vez aprobada la solicitud de emparejamiento, el estado de la conexión cambia a ProvisioningStarted.
1. Necesita:
    1. Completar la conexión según la LOA.
    1. (Opcionalmente) realizar una prueba de vínculo con 169.254.0.0/16.
    1. Configurar la sesión de BGP y, a continuación, notificárnoslo.
1. Microsoft aprovisiona la sesión de BGP con la directiva DENY ALL y realiza la validación de un extremo a otro.
1. Si la operación se realiza correctamente, recibirá una notificación que le indicará que el estado de la conexión de emparejamiento es Activo.
1. A continuación, se permitirá el tráfico a través del nuevo emparejamiento.

Tenga en cuenta que los estados de conexión no se deben confundir con los estados de sesión estándar de [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol).

## <a name="convert-a-legacy-direct-peering-to-azure-resource"></a>Conversión de un emparejamiento directo heredado en un recurso de Azure
Se deben seguir los pasos siguientes para convertir un emparejamiento directo heredado en un recurso de Azure:
1. Siga las instrucciones de [Conversión de un emparejamiento directo heredado en un recurso de Azure](howto-legacy-direct-powershell.md).
1. Después de enviar la solicitud de conversión, Microsoft revisará la solicitud y se pondrá en contacto con usted si es necesario.
1. Una vez aprobada, verá el emparejamiento directo con el estado de conexión Activo.

## <a name="deprovision-direct-peering"></a>Desaprovisionamiento del emparejamiento directo
Póngase en contacto con el equipo de [emparejamiento de Microsoft](mailto:peering@microsoft.com) para desaprovisionar el emparejamiento directo.

Si se establece un emparejamiento directo para el desaprovisionamiento, verá el estado de la conexión **PendingRemove**.

> [!NOTE]
> Si ejecuta el cmdlet de PowerShell para eliminar el emparejamiento directo cuando el valor de ConnectionState es ProvisioningStarted o ProvisioningCompleted, se producirá un error en la operación.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información al respecto en [Requisitos previos para configurar el emparejamiento con Microsoft](prerequisites.md).
