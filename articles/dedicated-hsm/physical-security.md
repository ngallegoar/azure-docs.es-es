---
title: Seguridad física de dispositivos HSM dedicados de Azure | Microsoft Docs
description: Información sobre la seguridad física de los dispositivos HSM dedicados de Azure en centros de datos
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: ede1af4625d06af4e280eda86d09ae1db3dfdfd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70881029"
---
# <a name="azure-dedicated-hsm-physical-security"></a>Seguridad física de HSM dedicado de Azure

HSM dedicado de Azure le ayuda a cumplir los requisitos de seguridad avanzada para el almacenamiento de claves. Se administra siguiendo los estrictos procedimientos de seguridad siguientes a lo largo de su ciclo de vida completo para satisfacer las necesidades de los clientes.

## <a name="security-through-procurement"></a>Seguridad en las adquisiciones

Microsoft sigue un proceso de compra segura. Se administra la cadena de custodia y se asegura que el dispositivo específico pedido y enviado es el que llega a nuestros centros de datos. Los dispositivos se encuentran en el fondo plástico para evitar la manipulación. Se almacenan en un área de almacenamiento seguro hasta que se inician en la galería de datos del centro de datos.  Los bastidores que contienen los dispositivos HSM se consideran de alta repercusión empresarial. Los dispositivos están bloqueados y bajo vigilancia por vídeo en todo momento y circunstancia.

## <a name="security-through-deployment"></a>Seguridad en la implementación

Los dispositivos HSM se instalan en bastidores junto con los componentes de red asociados. Una vez instalados, deben configurarse antes de que estén disponibles como parte del servicio HSM dedicado de Azure. Esta actividad de configuración es realizada por empleados de Microsoft que han llevado a cabo una comprobación en segundo plano. La administración "Just-In-Time" (JIT) se usa para limitar el acceso solo a los empleados adecuados y únicamente durante el tiempo en que se necesita acceso. Los procedimientos y los sistemas que se usan también garantizan que se registra toda la actividad relacionada con los dispositivos HSM.

## <a name="security-in-operations"></a>Seguridad en las operaciones

Los HSM son dispositivos de hardware (el HSM real es una tarjeta PCI en la aplicación) por lo que es posible que surjan problemas en los componentes. Algunos problemas posibles incluyen, entre otros, errores de fuente de alimentación y de ventilación. Este tipo de evento requerirá actividades break-fix o de mantenimiento para reemplazar los componentes intercambiables.

### <a name="component-replacement"></a>Reemplazo de los componentes

Después de aprovisionar un dispositivo y con la administración del cliente, la fuente de alimentación intercambiable en caliente es el único componente que se reemplazaría. Este componente está fuera del límite de seguridad y no provoca un evento de manipulación. Se usa un sistema de vales para autorizar a un ingeniero de Microsoft a acceder a la parte trasera del bastidor HBI. Cuando se procesa el vale, se emite una clave temporal física. Esta clave concede al ingeniero acceso al dispositivo y le permite intercambiar el componente afectado. Cualquier otro acceso (que cause un evento de manipulación) se terminaría cuando un dispositivo no se asignara a un cliente, con lo que se minimiza el riesgo de seguridad y de disponibilidad.  

### <a name="device-replacement"></a>Reemplazo del dispositivo

Si se produce un error total del dispositivo, se sigue un proceso similar al usado durante un error de un componente. Si un cliente no es capaz de restablecer el dispositivo o esta está en un estado desconocido, se quitarán los datos de los dispositivos y se colocarán en un contenedor para la destrucción en bastidor. Los dispositivos del contenedor se destruirán de forma segura y controlada. Ningún dato de los dispositivos que procedan de un bastidor HBI abandonará un centro de datos de Microsoft.

### <a name="other-rack-access-activities"></a>Otras actividades de acceso del bastidor

Si un ingeniero de Microsoft debe acceder al bastidor usado por los dispositivos HSM (por ejemplo, para el mantenimiento de dispositivos de red), los procedimientos de seguridad estándar se usarán para tener acceso al bastidor HBI seguro. Todos los accesos se someterán a vigilancia por vídeo. Los dispositivos HSM Se validan con [FIPS 140-2 nivel 3](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf) por lo que cualquier acceso no autorizado a los dispositivos HSM se señalará al cliente y se inicializarán los datos.

## <a name="logical-level-security-considerations"></a>Consideraciones sobre la seguridad con respecto a la lógica

Los HSM se aprovisionan en una red virtual creada por el cliente. Se trata de un espacio de direcciones de IUP privado de un cliente.  Esta configuración proporciona un valioso aislamiento de nivel de red lógica y garantiza el acceso únicamente a los clientes. Esto implica que todos los controles de seguridad de nivel lógico son responsabilidad del cliente.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda que todos los conceptos clave del servicio, como la alta disponibilidad, la seguridad y la compatibilidad, por ejemplo, se entiendan bien antes de aprovisionar los dispositivos o diseñar e implementar la aplicación.

* [Alta disponibilidad](high-availability.md)
* [Redes](networking.md)
* [Compatibilidad](supportability.md)
* [Supervisión](monitoring.md)
* [Arquitectura de implementación](deployment-architecture.md)