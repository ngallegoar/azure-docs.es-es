---
title: Uso de un panel para un laboratorio educativo en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo usar un panel para un laboratorio educativo en Azure Lab Services.
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
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: 363e5705e9ccf4aa207ff7e5cafb615bc01bc7d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538947"
---
# <a name="dashboard-for-classroom-labs"></a>Panel para laboratorios educativos
En este artículo se describe la vista de panel de un laboratorio educativo en Azure Lab Services. 

![Panel](../media/use-dashboard/dashboard.png)

## <a name="costs-and-billing-tile"></a>Icono de costos y facturación
En este icono se proporcionan los siguientes detalles de la estimación de costos:

| Configuración | Value | 
| ------- | ----- | 
| Horas de cuota | El número máximo de horas que un usuario puede usar la máquina virtual fuera de las horas programadas. |
| Horas programadas | Horas en las que se incurrirá en función de la programación establecida en el laboratorio. Este valor solo está disponible si se ha establecido una fecha en formato desde/hasta en todos los eventos de programación. |
| Horas/usuario | La suma de las horas de cuota y las horas programadas. |
| Número máximo de usuarios | Número máximo de usuarios del laboratorio en función de todas las máquinas virtuales que se van a notificar. |
| Horas x usuarios | Horas/usuario multiplicado por el número de usuarios. |
| Cuota ajustada | La suma de las horas de cuota agregadas a usuarios concretos. |
| Total de horas * $/hora | El costo por hora en función del tamaño de la máquina virtual seleccionada. Esto se basa en el precio de pago por uso normal. |
| Costo total estimado | Este es el precio máximo para este laboratorio basado en la configuración actual. |

## <a name="template-tile"></a>Icono de plantilla
En este icono verá la siguiente información:

- Fecha en la que se ha creado la plantilla. 
- Fecha en la que se ha publicado por última vez la plantilla. 

También tiene un vínculo para ir a la página **Plantilla**, donde puede [administrar la máquina virtual de la plantilla](how-to-create-manage-template.md) para la clase. 

## <a name="virtual-machine-pool-tile"></a>Icono de grupo de máquinas virtuales

En este icono verá la siguiente información:

- Número de máquinas virtuales que se asignan a los alumnos (usuarios).
- Número de máquinas virtuales que todavía no se han asignado a los alumnos.

También tiene un vínculo para ir a la página **Grupo de máquinas virtuales**, donde puede [administrar el grupo de máquinas virtuales](how-to-set-virtual-machine-passwords.md) en el laboratorio. 

## <a name="users-tile"></a>Icono de usuarios

En este icono verá la siguiente información:

- Número de usuarios registrados en la clase.
- Número de usuarios que se han agregado al laboratorio pero no se han registrado en la clase. 

También tiene un vínculo para ir a la página **Usuarios**, donde puede [administrar los usuarios](how-to-configure-student-usage.md) del laboratorio. 

## <a name="schedules-tile"></a>Icono de programaciones
En el icono se ven los eventos programados actuales del laboratorio. También tiene un vínculo para navegar a la página **Programación**, en la que puede [crear y administrar programaciones](how-to-create-schedules.md). En el icono solo se muestran los detalles de dos eventos programados y el número de eventos programados restantes del laboratorio. 

![Eventos programados](../media/use-dashboard/scheduled-events.png)

