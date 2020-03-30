---
title: Alta disponibilidad de Azure Dedicated HSM | Microsoft Docs
description: Consideraciones básicas y ejemplos de alta disponibilidad de Azure Dedicated HSM
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: mbaldwin
ms.openlocfilehash: 536ef62acad900090924598edfa45450b2a8c951
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70882253"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Alta disponibilidad de Azure Dedicated HSM

Azure Dedicated HSM está respaldado por los centros de datos de alta disponibilidad de Microsoft. Sin embargo, todos los centros de datos de alta disponibilidad son vulnerables a errores localizados y, en circunstancias extremas, a errores a nivel regional. Microsoft implementa dispositivos HSM en distintos centros de datos dentro de una región para garantizar que el aprovisionamiento de varios dispositivos no provoque que compartan un único bastidor. Es posible lograr un nivel adicional de alta disponibilidad mediante el emparejamiento de estos HSM entre los centros de datos de una región con la característica de Grupo de alta disponibilidad de Gemalto. También es posible emparejar dispositivos entre regiones para solucionar una conmutación por error regional en una situación de recuperación ante desastres. Con esta configuración de alta disponibilidad de varios niveles, cualquier error del dispositivo se solucionará automáticamente para mantener las aplicaciones en funcionamiento. Además, todos los centros de datos tienen componentes y dispositivos de repuesto in situ para poder reemplazar cualquier dispositivo con errores de manera oportuna.

## <a name="high-availability-example"></a>Ejemplo de alta disponibilidad

Para obtener información sobre cómo configurar los dispositivos HSM para lograr alta disponibilidad a nivel de software, consulte la guía de administración de Gemalto Luna Network HSM. Este documento está disponible en la [página de Gemalto HSM](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).

En el siguiente diagrama se muestra una arquitectura de alta disponibilidad. Utiliza varios dispositivos de una región y varios dispositivos emparejados de otra región distinta. Esta arquitectura emplea un mínimo de cuatro dispositivos HSM y componentes de red virtual.

![Diagrama de alta disponibilidad](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Pasos siguientes

Se recomienda que todos los conceptos clave del servicio, como la alta disponibilidad y la seguridad, se entiendan bien antes de aprovisionar los dispositivos o diseñar e implementar la aplicación.
Más temas de nivel de conceptos:

* [Arquitectura de implementación](deployment-architecture.md)
* [Seguridad física](physical-security.md)
* [Redes](networking.md)
* [Compatibilidad](supportability.md)
* [Supervisión](monitoring.md)

Para obtener detalles concretos sobre la configuración de los dispositivos HSM para lograr alta disponibilidad, visite el portal de soporte técnico para clientes de Gemalto para consultar las guías de administrador y la sección 6.
