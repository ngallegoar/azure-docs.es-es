---
title: Certificación de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Certificación de SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2a02f0e1b05b9de8105126d1c9e4e3f79057285f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77617225"
---
# <a name="certification"></a>Certificación

Además de la certificación NetWeaver, SAP requiere una certificación especial de SAP HANA para admitir SAP HANA en determinadas infraestructuras, como IaaS de Azure.

La nota fundamental de SAP sobre NetWeaver y, hasta cierto punto, sobre la certificación de SAP HANA, es [Nota de SAP 1928533: Aplicaciones de SAP en Azure: Supported products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533). (Nota de SAP 1928533: Aplicaciones de SAP en Azure: productos y tipos de máquina virtual de Azure admitidos)

Los registros de certificación de unidades de SAP HANA en Azure (instancias grandes) pueden encontrarse en el sitio de [plataformas IaaS certificadas para SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). 

Los tipos de SAP HANA en Azure (instancias grandes) mencionados en el sitio de plataformas IaaS certificadas para SAP HANA ofrecen a los clientes de Microsoft y SAP la posibilidad de implementar cargas de trabajo de gran tamaño de SAP Business Suite, SAP BW, S/4 HANA, BW/4HANA u otras cargas de trabajo de SAP HANA en Azure. Esta solución se basa en la demarcación de hardware dedicado con certificación de SAP HANA ([Integración de centro de datos adaptado para SAP HANA o TDI](https://scn.sap.com/docs/DOC-63140)). Si ejecuta una solución configurada según SAP HANA TDI, todas las aplicaciones basadas en SAP HANA (por ejemplo, SAP Business Suite en SAP HANA, SAP BW en SAP HANA, S4/HANA y BW4/HANA) trabajan en la infraestructura de hardware.

Comparada con la ejecución de SAP HANA en máquinas virtuales, esta solución tiene una ventaja. Proporciona volúmenes de memoria mucho mayores. Para habilitar esta solución, debe entender los siguientes aspectos fundamentales:

- El nivel de aplicación de SAP y las aplicaciones que no son de SAP se ejecutan en máquinas virtuales que se hospedan en las demarcaciones de hardware de Azure habituales.
- La infraestructura local, los centros de datos y las implementaciones de aplicaciones del cliente se conectan a la plataforma en la nube mediante ExpressRoute (recomendado) o una red privada virtual (VPN). Active Directory y DNS también se extienden a Azure.
- La instancia de base de datos de SAP HANA para cargas de trabajo de HANA se ejecuta en SAP HANA en Azure (Instancias grandes). La demarcación de instancias grandes está conectada a las redes de Azure, por lo que el software que se ejecuta en las máquinas virtuales puede interactuar con la instancia de HANA que se ejecuta en HANA (instancias grandes).
- El hardware de SAP HANA en Azure (instancias grandes) es hardware dedicado que se proporciona como IaaS con SUSE Linux Enterprise Server o Red Hat Enterprise Linux preinstalado. Al igual que con las máquinas virtuales, es responsabilidad suya llevar a cabo las actualizaciones y el mantenimiento del sistema operativo.
- La instalación de HANA o los componentes adicionales necesarios para ejecutar SAP HANA en las unidades de HANA (instancias grandes) es su responsabilidad. Todas las operaciones relacionadas correspondientes y la administración de SAP HANA en Azure también son responsabilidad suya.
- Además de las soluciones descritas aquí, puede instalar otros componentes en la suscripción de Azure conectada a SAP HANA en Azure (Instancias grandes). Por ejemplo, los componentes que permiten la comunicación, directa o no, con la base de datos de SAP HANA, como los servidores de salto, servidores RDP, SAP HANA Studio, SAP Data Services para escenarios de BI de SAP o soluciones de supervisión de red.
- Como en Azure, HANA (instancias grandes) ofrece funcionalidades auxiliares de alta disponibilidad y recuperación ante desastres.

**Pasos siguientes**
- Consulte [SKU disponibles para HLI](hana-available-skus.md). 