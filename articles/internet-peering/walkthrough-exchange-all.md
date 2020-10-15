---
title: Tutorial del emparejamiento de Exchange
titleSuffix: Azure
description: Tutorial del emparejamiento de Exchange
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: ec83778d034cfc512582eddf79995412dad405c2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84710735"
---
# <a name="exchange-peering-walkthrough"></a>Tutorial del emparejamiento de Exchange

En esta sección se explican los pasos que debe seguir para configurar y administrar un emparejamiento de Exchange.

## <a name="create-an-exchange-peering"></a>Creación de un emparejamiento de Exchange
> [!div class="mx-imgBorder"]
> ![Estados de conexión y flujo de trabajo del emparejamiento de Exchange](./media/exchange-peering.png)

Se deben seguir los pasos siguientes para aprovisionar un emparejamiento de Exchange:
1. Revise la [directiva de emparejamiento](https://peering.azurewebsites.net/peering) de Microsoft para comprender los requisitos del emparejamiento de Exchange.
1. Busque el identificador de instalación de emparejamiento y la ubicación de emparejamiento de Microsoft en [PeeringDB](https://www.peeringdb.com/net/694).
1. Solicite el emparejamiento de Exchange para una ubicación de emparejamiento siguiendo las instrucciones del artículo [Creación y modificación de un emparejamiento de Exchange mediante PowerShell](howto-exchange-powershell.md) para obtener más detalles.
1. Después de enviar la solicitud de emparejamiento, Microsoft revisará la solicitud y se pondrá en contacto con usted si es necesario.
1. Una vez aprobado, el estado de la conexión cambia a Aprobado.
1. Configuración de la sesión BGP en el extremo y la notificación a Microsoft.
1. Aprovisionaremos la sesión de BGP con la directiva DENY ALL y realizaremos la validación de un extremo a otro.
1. Si la operación se realiza correctamente, recibirá una notificación que le indicará que el estado de la conexión de emparejamiento es Activo.
1. A continuación, se permitirá el tráfico a través del nuevo emparejamiento.

Tenga en cuenta que los estados de conexión no se deben confundir con los estados de sesión estándar de [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol).

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Conversión de un emparejamiento de Exchange heredado en un recurso de Azure
Se deben seguir los pasos siguientes para convertir un emparejamiento de Exchange heredado en un recurso de Azure:
1. Siga las instrucciones de [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure](howto-legacy-exchange-powershell.md).
1. Después de enviar la solicitud de conversión, Microsoft revisará la solicitud y se pondrá en contacto con usted si es necesario.
1. Una vez aprobada, verá el emparejamiento de Exchange con el estado de conexión Activo.

## <a name="deprovision-exchange-peering"></a>Desaprovisionamiento de emparejamiento de Exchange
Póngase en contacto con el [emparejamiento de Microsoft](mailto:peering@microsoft.com) para desaprovisionar el emparejamiento de Exchange.

Si se establece un emparejamiento de Exchange para el desaprovisionamiento, verá el estado de la conexión **PendingRemove**.

> [!NOTE]
> Si ejecuta el cmdlet de PowerShell para eliminar el emparejamiento de Exchange cuando el valor de ConnectionState es ProvisioningStarted o ProvisioningCompleted, se producirá un error en la operación.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información al respecto en [Requisitos previos para configurar el emparejamiento con Microsoft](prerequisites.md).
