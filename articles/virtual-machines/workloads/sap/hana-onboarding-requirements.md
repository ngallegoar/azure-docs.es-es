---
title: Requisitos de incorporación de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Requisitos de incorporación de SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/31/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 141a75a75a214ff4a6f136df7570d6e81f7f4e82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617004"
---
# <a name="onboarding-requirements"></a>Requisitos de incorporación

En esta lista se recopilan los requisitos para ejecutar SAP HANA en Azure (instancias grandes).

**Microsoft Azure**

- Una suscripción de Azure que se pueda vincular a SAP HANA en Azure (Instancias grandes).
- Contrato de soporte técnico Premier de Microsoft. Para obtener información específica relacionada con la ejecución de SAP en Azure, consulte [SAP Support Note #2015553 – SAP on Microsoft Azure: Support Prerequisites (Nota de soporte técnico 2015553 de SAP en Microsoft Azure: requisitos previos de soporte técnico)](https://launchpad.support.sap.com/#/notes/2015553). Si usa unidades de HANA (instancias grandes) con 384 o más CPU, también necesita extender el contrato de soporte técnico Premier para incluir Respuesta rápida de Azure.
- Conocimiento de las SKU de HANA (instancias grandes) que necesita después de realizar un ajuste de tamaño con SAP.

**Conectividad de red**

- ExpressRoute entre el entorno local y el de Azure: asegúrese de pedir a su ISP una conexión de 1 Gbps como mínimo para conectar el centro de datos local a Azure. La conectividad entre las unidades de instancia grande de HANA y Azure también usa la tecnología ExpressRoute. Esta conexión de ExpressRoute entre las unidades de instancia grande de HANA y Azure está incluida en el precio de las unidades de instancia grande de HANA, incluido el precio de las entradas y salidas de este circuito específico de ExpressRoute. Por lo tanto, como cliente, no debe pagar costos adicionales más allá de su vínculo de ExpressRoute entre el entorno local y el de Azure.

**Sistema operativo**

- Licencias para SUSE Linux Enterprise Server 12 for SAP Applications.

   > [!NOTE] 
   > El sistema operativo entregado por Microsoft no está registrado con SUSE. No está conectado a una instancia de la herramienta de administración de suscripciones.

- SUSE Linux Subscription Management Tool implementada en Azure en una máquina virtual. Esta herramienta proporciona la funcionalidad para que SAP HANA en Azure (instancias grandes) sea registrado y actualizado respectivamente por SUSE. (No hay ningún acceso a internet en el centro de datos de HANA [instancias grandes]). 
- Licencias para Red Hat Enterprise Linux 6.7 o 7.x para SAP HANA.

   > [!NOTE]
   > El sistema operativo entregado por Microsoft no está registrado con Red Hat. No está conectado a una instancia del administrador de suscripciones de Red Hat.

- Red Hat Subscription Manager implementado en Azure en una máquina virtual. Red Hat Subscription Manager proporciona la funcionalidad para que SAP HANA en Azure (instancias grandes) sea registrado y actualizado respectivamente por Red Hat. (No hay ningún acceso directo a Internet desde el inquilino implementado en la demarcación de instancias grandes de Azure).
- SAP requiere que también disponga de un contrato de soporte con el proveedor de Linux. Este requisito no se elimina con la solución de HANA (instancias grandes) ni por el hecho de ejecutar Linux en Azure. A diferencia de lo que ocurre con algunas de las imágenes de la galería de Azure para Linux, la tarifa del servicio *no* se incluye en la oferta de la solución HANA (instancias grandes). Es su responsabilidad cumplir los requisitos de SAP en relación con los contratos de soporte técnico con el distribuidor de Linux. 
   - Para SUSE Linux, busque los requisitos de los contratos de soporte técnico en [SAP Note # 1984787 - SUSE Linux Enterprise Server 12: Installation notes](https://launchpad.support.sap.com/#/notes/1984787) (Nota de SAP 1984787: SUSE Linux Enterprise Server 12) y [SAP Note #1056161 - SUSE priority support for SAP applications](https://launchpad.support.sap.com/#/notes/1056161) (Nota de SAP 1056161: soporte técnico prioritario de SUSE para aplicaciones de SAP).
   - Para Red Hat Linux, necesita tener los niveles de suscripción adecuados que incluyan soporte técnico y actualizaciones del servicio de los sistemas operativos de HANA (instancias grandes). Red Hat recomienda la suscripción a Red Hat Enterprise Linux para soluciones de SAP. Consulte https://access.redhat.com/solutions/3082481. 

Para la matriz de compatibilidad de las distintas versiones de SAP HANA con las diferentes versiones de Linux, consulte la [Nota de SAP 2235581](https://launchpad.support.sap.com/#/notes/2235581).

Para la matriz de compatibilidad del sistema operativo y de las versiones de firmware HLI o de controlador, consulte [OS Upgrade for HLI](os-upgrade-hana-large-instance.md) (Actualización del sistema operativo para HLI).


> [!IMPORTANT] 
> Para las unidades de tipo II, en este momento solo se admite la versión del sistema operativo SLES 12 SP2. 


**Base de datos**

- Las licencias y los componentes de instalación de software para SAP HANA (Platform Edition o Enterprise Edition).

**Aplicaciones**

- Licencias y componentes de instalación de software para todas las aplicaciones de SAP que se conecten a SAP HANA y contratos de soporte de SAP relacionados.
- Licencias y componentes de instalación de software para aplicaciones que no sean de SAP y que se usan con el entorno de SAP HANA en Azure (instancias grandes) y los contratos de soporte técnico relacionados.

**Habilidades**

- Experiencia y conocimientos sobre IaaS de Azure y sus componentes.
- Experiencia y conocimientos sobre cómo implementar una carga de trabajo SAP en Azure.
- Personal certificado para la instalación de SAP HANA.
- Conocimientos de la arquitectura de SAP para diseñar la alta disponibilidad y la recuperación ante desastres de SAP HANA.

**SAP**

- Se espera que sea un cliente de SAP y que tenga un contrato de soporte técnico con SAP.
- Especialmente para las implementaciones de las SKU de clase de tipo II de HANA (instancias grandes), consulte con SAP las versiones de SAP HANA y las configuraciones posibles en hardware de escalado vertical de tamaño grande.

**Pasos siguientes**
- Consulte [Arquitectura de SAP HANA (instancias grandes) en Azure](hana-architecture.md).