---
title: Responsabilidad compartida en la nube - Microsoft Azure
description: Información sobre el modelo de responsabilidad compartida, las tareas de seguridad que administra el proveedor de nube y las tareas que administra el usuario.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: na
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2019
ms.author: terrylan
ms.openlocfilehash: 8f16105d6bda1798828bc423ec8a158d49e0cf2b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "72518148"
---
# <a name="shared-responsibility-in-the-cloud"></a>Responsabilidad compartida en la nube

A medida que considera y evalúa los servicios en la nube pública, es fundamental que comprenda el modelo de responsabilidad compartida y qué tareas de seguridad administra el proveedor de servicios en la nube y cuáles administra usted. Las responsabilidades de la carga de trabajo varían en función de si la carga de trabajo está hospedada en una implementación de software como servicio (SaaS), plataforma como servicio (PaaS), infraestructura como servicio (IaaS) o bien en un centro de datos local.

## <a name="division-of-responsibility"></a>División de responsabilidad
En un centro de datos local, usted es el propietario de toda la pila. A medida que se traslada a la nube, algunas responsabilidades se transfieren a Microsoft. En el diagrama siguiente se muestran las áreas de responsabilidad entre usted y Microsoft, según el tipo de implementación de la pila.

![Zonas de responsabilidad](./media/shared-responsibility/shared-responsibility.png)

Para todos los tipos de implementación de nube, es propietario de los datos y las identidades. Asimismo, es responsable de proteger la seguridad de los datos y las identidades, los recursos locales y los componentes en la nube que controla (que varía según el tipo de servicio).

Con independencia del tipo de implementación, siempre conserva las siguientes responsabilidades:

- data
- Puntos de conexión
- Cuenta
- Administración de acceso

## <a name="cloud-security-advantages"></a>Ventajas de seguridad en la nube
La nube ofrece importantes ventajas para solucionar los desafíos de seguridad de la información de larga duración. En un entorno local, las organizaciones probablemente tengan responsabilidades inadecuadas y recursos limitados disponibles para invertir en seguridad, de tal manera que se crea un entorno donde los atacantes pueden aprovechar vulnerabilidades a todos los niveles.

En el diagrama siguiente se muestra un enfoque tradicional, en el que no se pueden satisfacer muchas responsabilidades de seguridad debido a los limitados recursos. En el enfoque habilitado para la nube, puede trasladar las responsabilidades de seguridad del día a día a su proveedor de servicios en la nube y reasignar sus recursos.

![Ventajas de seguridad de la era en la nube](./media/shared-responsibility/cloud-enabled-security.png)

En el enfoque habilitado para la nube, también puede aprovechar las funcionalidades de seguridad basadas en la nube para conseguir mayor eficacia y usar la inteligencia en la nube para mejorar la detección de amenazas y el tiempo de respuesta. Con la transferencia de responsabilidades al proveedor de nube, las organizaciones pueden obtener más cobertura de seguridad, lo que les permite reasignar recursos de seguridad y presupuestos a otras prioridades empresariales.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre la división de la responsabilidad entre usted y Microsoft en una implementación SaaS, PaaS e IaaS, consulte [Shared responsibilities for cloud computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91) (Responsabilidades compartidas de la informática en la nube).
