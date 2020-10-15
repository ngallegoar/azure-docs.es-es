---
title: 'Opciones de supervisión: Azure Dedicated HSM | Microsoft Docs'
description: Introducción a las opciones de supervisión de Azure Dedicated HSM y responsabilidades de supervisión
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
ms.openlocfilehash: 3fde577a6b0efb7584e1c9efd57c95583ebe4ec9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "70881423"
---
# <a name="azure-dedicated-hsm-monitoring"></a>Supervisión de HSM dedicado de Azure

El servicio HSM dedicado de Azure proporciona un dispositivo físico para uso exclusivo del cliente, con un control administrativo y una responsabilidad en la administración completos. El dispositivo disponible es un [Gemalto SafeNet Luna 7 HSM modelo A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).  Microsoft no tendrá acceso administrativo después de que un cliente lo aprovisione, aparte de la conexión del puerto serie físico como rol de supervisión. Como resultado, los clientes son responsables de las actividades operativas típicas, incluida la supervisión integral y el análisis de registros.
Los clientes son completamente responsables de las aplicaciones que usan los HSM y deben contactar con Gemalto para obtener soporte técnico y asesoramiento. Debido a la extensión de la propiedad del cliente de la higiene operativa, no es posible que Microsoft ofrezca ningún tipo de garantía de alta disponibilidad para este servicio. Es responsabilidad del cliente asegurarse de que sus aplicaciones estén configuradas correctamente para lograr una alta disponibilidad. Microsoft supervisará y mantendrá la conectividad de red y el buen estado de los dispositivos.

## <a name="microsoft-monitoring"></a>Supervisión de Microsoft

El dispositivo Gemalto SafeNet en uso tiene establecido de forma predeterminada SNMP y el puerto serie como opciones para supervisar el dispositivo. Microsoft ha usado la conexión de puerto serie como medio físico para conectarse al dispositivo a fin de recuperar datos de telemetría básicos sobre el estado del dispositivo. Esto incluye elementos como la temperatura y el estado de componentes como fuentes de alimentación y ventiladores.
Para lograr esto, Microsoft usa un rol de "supervisor" no administrativo configurado en el dispositivo Gemalto. Esta función brinda la capacidad de recuperar los datos de telemetría, pero no otorga ningún acceso al dispositivo en términos de tareas administrativas ni para ver de forma alguna la información criptográfica. Nuestros clientes pueden estar seguros de que su dispositivo es realmente suyo para la administración y el uso con fines de almacenamiento de claves criptográficas confidenciales. En caso de que algún cliente no esté satisfecho con este acceso mínimo para la supervisión del estado básico, tiene la opción de deshabilitar la cuenta de supervisión. La consecuencia obvia que se deriva de ello es que Microsoft no tendrá ninguna información y, por tanto, no podrá ofrecer notificaciones proactivas sobre los problemas de estado del dispositivo. En esta situación, el cliente es responsable del mantenimiento del dispositivo.
La propia función de supervisión está configurada para sondear el dispositivo cada diez minutos para reunir los datos de estado. Dada la naturaleza propensa a errores de las comunicaciones en serie, solo se generará una alerta después de varios indicadores de estado negativos durante un período de una hora. Esta alerta conducirá en última instancia a una comunicación proactiva al cliente notificando el problema.
Según la naturaleza del problema, se tomaría el curso de acción adecuado para reducir el impacto y garantizar la corrección de bajo riesgo. Por ejemplo, un corte de suministro de alimentación es un procedimiento de intercambio directo sin un evento de manipulación resultante, por lo que se puede realizar con bajo impacto y con un riesgo mínimo para la operación. Otros procedimientos pueden requerir un dispositivo posicionado en cero y desaprovisionado para minimizar los riesgos de seguridad para el cliente. En esta situación, un cliente aprovisionaría un dispositivo alternativo y se volvería a unir a un emparejamiento de alta disponibilidad, lo que provocaría la sincronización del dispositivo. El funcionamiento normal se reanudaría en un tiempo mínimo, con una interrupción mínima y el menor riesgo de seguridad.  

## <a name="customer-monitoring"></a>Supervisión de clientes

Una propuesta de valor del servicio HSM dedicado es el control que el cliente obtiene del dispositivo, especialmente si se considera que es un dispositivo en la nube. Una consecuencia de este control es la responsabilidad de supervisar y administrar el estado del dispositivo. El dispositivo Gemalto SafeNet incluye instrucciones para la implementación de SNMP y Syslog. Se recomienda que los clientes del servicio HSM dedicado utilicen esto incluso cuando la cuenta de supervisión de Microsoft permanezca activa, y deberían considerarlo obligatorio si deshabilitan la cuenta de supervisión de Microsoft.
Cualquiera de estas técnicas disponibles permitiría a un cliente identificar problemas y llamar al soporte técnico de Microsoft para iniciar el trabajo de corrección adecuado.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda que todos los conceptos clave del servicio, como la alta disponibilidad y la seguridad, por ejemplo, se entiendan bien antes de aprovisionar los dispositivos o diseñar e implementar la aplicación. Más temas de nivel de conceptos:

* [Alta disponibilidad](high-availability.md)
* [Seguridad física](physical-security.md)
* [Redes](networking.md)
* [Compatibilidad](supportability.md)
