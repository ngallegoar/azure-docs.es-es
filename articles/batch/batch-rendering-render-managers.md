---
title: Compatibilidad con el administrador de representación
description: Uso de la integración del administrador de representación de Azure Batch. Obtenga información sobre la compatibilidad integrada o los complementos para administradores de representación más populares.
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: how-to
ms.openlocfilehash: c44cbf86d8bf2fe83a6dc91dee1c4f58eec156c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83726458"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Uso de Azure Batch con administradores de granja de representación

Si usa una granja de representaciones locales existente, es muy probable que un administrador de representación controle la capacidad de la granja de representaciones y represente los trabajos.

Azure proporciona compatibilidad integrada o complementos para administradores de representación populares. Después, puede agregar y quitar máquinas virtuales de Azure, incluidas las máquinas virtuales con las licencias de aplicación de pago por uso y las máquinas virtuales de prioridad baja.

Se admiten los siguientes administradores de representación:

* [¡PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](https://www.royalrender.de/)
* [Fecha límite Thinkbox](https://deadline.thinkboxsoftware.com/)

## <a name="azure-render-hub"></a>Azure Render Hub

Azure Render Hub simplifica la creación y administración de granjas de representación de Azure.  Render Hub tiene compatibilidad nativa con PipelineFx Qube y Deadline 10.  Para más información e instrucciones detalladas, consulte [el repositorio de GitHub](https://github.com/Azure/azure-render-hub).

## <a name="using-azure-with-pipelinefx-qube"></a>Uso de Azure con PipelineFX Qube

Azure Render Hub es compatible con los administradores de representación más conocidos, incluido Deadline.  Para obtener instrucciones sobre la implementación y el uso de Render Hub, consulte [el repositorio de GitHub](https://github.com/Azure/azure-render-hub).

Los scripts y las instrucciones para habilitar las máquinas virtuales de grupos de Azure Batch que se usarán como trabajos de Qube también están disponibles en [el repositorio de GitHub](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Uso de Azure con Royal Render

Render Royal tiene incorporada la integración de Azure y Azure Batch, lo que permite ampliar una granja de representaciones con las máquinas virtuales basadas en Azure. Para un resumen, consulte [los archivos de ayuda](https://www.royalrender.de/help8/index.html?Cloudrendering.html).

Para un ejemplo de un cliente de Royal Render que usa la integración de Azure, consulte la [historia de cliente Jellyfish Pictures](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Uso de Azure con Thinkbox Deadline

Azure Render Hub es compatible con los administradores de representación más conocidos, incluido Deadline.  Para obtener instrucciones sobre la implementación y el uso de Render Hub, consulte [el repositorio de GitHub](https://github.com/Azure/azure-render-hub).

## <a name="next-steps"></a>Pasos siguientes

Pruebe la integración de Azure Batch para el administrador de representación, mediante el complemento y las instrucciones apropiados en GitHub, si procede.
