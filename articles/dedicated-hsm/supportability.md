---
title: 'Compatibilidad: Azure Dedicated HSM | Microsoft Docs'
description: Opciones de compatibilidad y áreas de responsabilidad de Azure Dedicated HSM en distintos escenarios
services: dedicated-hsm
author: johndaw
manager: rkarlin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.custom: seodec18
ms.date: 03/27/2019
ms.author: mbaldwin
ms.openlocfilehash: d83d688707baf6098d63dfde9b4181eb04fb9729
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "70881007"
---
# <a name="azure-dedicated-hsm-supportability"></a>Compatibilidad de Azure Dedicated HSM

El servicio HSM dedicado de Azure proporciona un dispositivo físico para uso exclusivo del cliente, con un control administrativo y una responsabilidad en la administración completos. El dispositivo disponible es un [Gemalto SafeNet Luna 7 HSM modelo A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Microsoft no tendrá acceso administrativo después de que un cliente lo aprovisione, aparte de la conexión del puerto serie físico como rol de supervisión.  Sin acceso, Microsoft no puede tener ninguna responsabilidad en la administración del sistema o el mantenimiento continuo a nivel de software. En consecuencia, los clientes son responsables de las actividades operativas habituales.
Los clientes son completamente responsables de las aplicaciones que usan los HSM y deben contactar con Gemalto para obtener soporte técnico y consultoría. Debido a la extensión de la propiedad del cliente de la higiene operativa, no es posible que Microsoft ofrezca ningún tipo de garantía de alta disponibilidad para este servicio. Es responsabilidad del cliente asegurarse de que sus aplicaciones están configuradas correctamente para lograr una alta disponibilidad. Microsoft supervisará y mantendrá la conectividad de red y el buen estado de los dispositivos.

## <a name="getting-support"></a>Obtención de soporte técnico

La asistencia al cliente para Dedicated HSM es una iniciativa conjunta entre Microsoft y Gemalto. Microsoft resolverá las incidencias relacionadas con el hardware o la ruta de red, mientras que todo lo relacionado con el HSM real, como la configuración, software, firmware o desarrollo de aplicaciones lo resolverá Gemalto. Este modelo de asistencia garantiza la ruta más rápida para un soporte técnico eficaz. En caso de duda con una incidencia en particular, genere una solicitud de soporte técnico con Microsoft y nos aseguraremos de que se dirige adecuadamente. Microsoft se involucrará en todos los escenarios de soporte técnico y se esforzará por ofrecer la mejor experiencia de soporte técnico para nuestros clientes.

## <a name="gemalto-support"></a>Soporte técnico de Gemalto

Los clientes que usan el servicio Dedicated HSM recibirán soporte técnico de Gemalto en función de su Plan de soporte técnico. Esto solo requiere un proceso de registro mediante el portal de soporte técnico de Gemalto. Para ello, se proporcionará un identificador de cliente e instrucciones como parte de la involucración inicial con Microsoft para obtener acceso al servicio Dedicated HSM. Gemalto ofrece soporte técnico a través de su [portal de soporte técnico para clientes](https://supportportal.gemalto.com/csm/).
Un punto a destacar clave es que Gemalto proporcionará todo el software y la documentación necesarios para utilizar HSM (por ejemplo, software de acceso de cliente y los SDK) mediante descarga en el portal de asistencia al cliente.

### <a name="software-components"></a>Componentes de software

En la configuración de los dispositivos HSM se utilizan diversos componentes de software:

* Software de cliente
* SDK
* Herramientas

### <a name="guidance"></a>Guía

Gemalto ofrece guías de administración y configuración a través del [portal de soporte técnico para clientes](https://supportportal.gemalto.com/csm/). Tras iniciar sesión con un Id. de cliente válido, estos documentos estarán disponibles para descargarse. Gemalto también proporciona una serie de guías de integración para ayudar a los clientes con distintos escenarios e integraciones de software. Para más información, consulte el [Sitio de asociados de Gemalto para Microsoft](https://safenet.gemalto.com/partners/microsoft/).

### <a name="support"></a>Soporte técnico

Cualquier problema a nivel de software o cuestión en relación con el uso de los HSM como parte del servicio HSM dedicado debe remitirse directamente al soporte técnico de Gemalto. Gemalto se hará cargo de todos los componentes de software indicados anteriormente y las configuraciones de HSM personalizadas posteriores al aprovisionamiento. Para más información, consulte el [portal de soporte técnico para clientes de Gemalto](https://supportportal.gemalto.com/csm/).

### <a name="consulting-services"></a>Servicios de consultoría

Para recibir asistencia con el diseño, desarrollo e implementación de aplicaciones personalizadas que usan HSM, póngase en contacto con su representante de cuenta de Gemalto.

## <a name="microsoft-support"></a>Soporte técnico de Microsoft

Microsoft garantizará que los dispositivos HMS físicos sean accesibles a través de una red y se encuentren en estado operativo para el uso exclusivo de un único cliente. Los clientes son responsables de la configuración y administración del dispositivo. Entre las responsabilidades de Microsoft están:

* Garantizar que el dispositivo tenga energía y esté refrigerado.
* Mantener el estado operativo del HSM (por ejemplo, en casos de avería/reparación).
* Garantizar que el dispositivo esté accesible a través de la red.

A Microsoft se le deben notificar problemas como los siguientes:

* Errores de componentes
* Problemas de todo el dispositivo
* Problemas de acceso a redes
* Problemas de aprovisionamiento y desaprovisionamiento

Microsoft tiene un acceso de puerto serie físico al dispositivo mediante un rol de supervisión (es decir, un rol no administrativo) que permite obtener telemetría de estado básica.  Esto permitirá a Microsoft notificar automáticamente problemas al cliente a menos que el cliente restrinja este permiso. 

### <a name="provisioning-and-decommissioning"></a>Aprovisionamiento y retirada de dispositivos

Después de que un cliente apruebe el registro del servicio HSM dedicado, podrá crear recursos HSM (actualmente mediante PowerShell o la interfaz de línea de comandos, no en Azure Portal). El recurso pasa por un proceso de asignación que asigna un dispositivo físico en una región determinada a la red virtual (VNet) predefinida de un cliente. Una vez que el dispositivo es visible en una red virtual, el cliente puede acceder a este y configurarlo aún más en función de sus requisitos. Los clientes acceden a sus HMS dedicados mediante las herramientas y el software cliente de Gemalto. Microsoft ofrece asistencia para el proceso de creación de recursos. Gemalto ofrece asistencia desde el proceso de configuración personalizada en adelante. (consulte la información sobre el soporte técnico de Gemalto arriba). Cuando un cliente termina de usar un HSM, debe restablecerse (o inicializar sus datos) para garantizar que los datos no se conserven. El proceso de restablecer el dispositivo elimina todas las configuraciones y los datos personalizados. Microsoft desasigna el dispositivo y lo devuelve al grupo en perfecto estado. Esto significa que cuando el dispositivo se devuelve al grupo, no presentará indicios de actividades anteriores de clientes. 

### <a name="hardware-issues"></a>Problemas de hardware

El dispositivo HSM tiene fuentes de alimentación y ventiladores redundantes y reemplazables.  Sin embargo, el desmontaje del ventilador de la unidad todavía producirá un evento de manipulación. De producirse un problema en un componente, Microsoft usará el proceso más adecuado para solucionar el problema a nivel de componente de manera que la interrupción y el riesgo para la disponibilidad del servicio para los clientes sean mínimos.
Con cualquier otro problema más grave del dispositivo, este se reemplazará por uno nuevo del grupo disponible. El cliente solo tendrá que incluir el nuevo dispositivo en el par de alta disponibilidad existente para que se sincronice y vuelva a un estado completamente operativo. Del dispositivo con el problema se retirarán todos sus dispositivos que contienen datos y se destruirán in situ en el centro de datos. Solo el chasis se devolverá a Gemalto para su reciclaje.


### <a name="networking-issues"></a>Problemas de red

Si los clientes experimentan problemas de acceso de red al dispositivo HSM, deben ponerse en contacto con el soporte técnico de Microsoft. El acceso a la red se puede probar de manera sencilla utilizando SSH para conectarse al dispositivo HMS. Si esto no funciona, póngase en contacto con el soporte técnico de Microsoft.

## <a name="service-level-expectations-for-support"></a>Expectativas de nivel de servicio de soporte técnico

Para los niveles de servicio de soporte técnico de Microsoft, consulte el [plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
Para los niveles de servicio de soporte técnico de Gemalto, consulte la [información esencial de soporte técnico de Gemalto](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Pasos siguientes

Se recomienda que todos los conceptos clave, como la alta disponibilidad y la seguridad, se entiendan bien antes de aprovisionar los dispositivos o diseñar e implementar la aplicación.

* [Arquitectura de implementación](deployment-architecture.md)
* [Alta disponibilidad](high-availability.md)
* [Seguridad física](physical-security.md)
* [Redes](networking.md)

