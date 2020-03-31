---
title: SKU para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: SKU para SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aad4e3ff3df8b4aeecbbbee7883ba383b9fd0d9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617260"
---
# <a name="available-skus-for-hli"></a>SKU disponibles para HLI

El servicio de SAP HANA en Azure (instancias grandes) basado en sellos de la revisión 3 está disponible en varias configuraciones en las siguientes regiones de Azure:

- Oeste de EE. UU.
- Este de EE. UU.
- Este de Australia
- Sudeste de Australia
- Oeste de Europa
- Norte de Europa
- Japón Oriental
- Japón Occidental

El servicio de SAP HANA en Azure (instancias grandes) basado en sellos de la revisión 4 está disponible en varias configuraciones en las siguientes regiones de Azure:

- Oeste de EE. UU. 2
- Este de EE. UU.
- Centro-sur de EE. UU.
- Oeste de Europa
- Norte de Europa



[Lista de SKU de instancias grandes certificadas para SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure):

| Solución de SAP | CPU | Memoria | Storage | Disponibilidad |
| --- | --- | --- | --- | --- |
| Optimizado para OLAP: SAP BW, BW/4HANA<br /> o SAP HANA para cargas de trabajo de OLAP genérico | SAP HANA en Azure S72<br /> – 2 procesadores Intel® Xeon® E7-8890 v3<br /> 36 núcleos de CPU y 72 subprocesos de CPU |  768 GB |  3 TB | No disponible |
| --- | SAP HANA en Azure S144<br /> – 4 procesadores Intel® Xeon® E7-8890 v3<br /> 72 núcleos de CPU y 144 subprocesos de CPU |  1,5 TB |  6 TB | No disponible |
| --- | SAP HANA en Azure S192<br /> – 4 procesadores Intel® Xeon® E7-8890 v4<br /> 96 núcleos de CPU y 192 subprocesos de CPU |  2,0 TB |  8 TB | No disponible |
| --- | SAP HANA en Azure S224<br /> – 4 procesadores Intel® Xeon® Platinum 8276 (conocidos como Cascade lake)<br /> 112 núcleos de CPU y 224 subprocesos de CPU |  3,0 TB |  6,3 TB | Disponible en sellos de la revisión 3 y la revisión 4  |
| --- | SAP HANA en Azure S384<br /> – 8 procesadores Intel® Xeon® E7-8890 v4<br /> 192 núcleos de CPU y 384 subprocesos de CPU |  4,0 TB |  16 TB | Disponible en sellos de la revisión 4 |
| Optimizado para OLTP: SAP Business Suite<br /> en SAP HANA o S/4HANA (OLTP),<br /> OLTP genérico | SAP HANA en Azure S72m<br /> – 2 procesadores Intel® Xeon® E7-8890 v3<br /> 36 núcleos de CPU y 72 subprocesos de CPU |  1,5 TB |  6 TB | No disponible |
|---| SAP HANA en Azure S144m<br /> – 4 procesadores Intel® Xeon® E7-8890 v3<br /> 72 núcleos de CPU y 144 subprocesos de CPU |  3,0 TB |  12 TB | No disponible |
|---| SAP HANA en Azure S192m<br /> – 4 procesadores Intel® Xeon® E7-8890 v4<br /> 96 núcleos de CPU y 192 subprocesos de CPU  |  4,0 TB |  16 TB | No disponible |
| --- | SAP HANA en Azure S224m<br /> – 4 procesadores Intel® Xeon® Platinum 8276 (conocidos como Cascade lake)<br /> 112 núcleos de CPU y 224 subprocesos de CPU |  6,0 TB |  10,5 TB | Disponible en sellos de la revisión 3 y la revisión 4  |
|---| SAP HANA en Azure S384m<br /> – 8 procesadores Intel® Xeon® E7-8890 v4<br /> 192 núcleos de CPU y 384 subprocesos de CPU |  6,0 TB |  18 TB | Disponible en sellos de la revisión 4|
|---| SAP HANA en Azure S384xm<br /> – 8 procesadores Intel® Xeon® E7-8890 v4<br /> 192 núcleos de CPU y 384 subprocesos de CPU |  8,0 TB |  22 TB |  Disponible en sellos de la revisión 4 |
|---| SAP HANA en Azure S576m<br /> – 12 procesadores Intel® Xeon® E7-8890 v4<br /> 288 núcleos de CPU y 576 subprocesos de CPU |  12,0 TB |  28 TB | Disponible en sellos de la revisión 4|
|---| SAP HANA en Azure S768m<br /> – 16 procesadores Intel® Xeon® E7-8890 v4<br /> 384 núcleos de CPU y 768 subprocesos de CPU |  16,0 TB |  36 TB | Disponible en sellos de la revisión 4|
|---| SAP HANA en Azure S960m<br /> – 20 procesadores Intel® Xeon® E7-8890 v4<br /> 480 núcleos de CPU y 960 subprocesos de CPU |  20,0 TB |  46 TB | Disponible en sellos de la revisión 4|


En SAP HANA TDIv5, SAP permite tamaños específicos del cliente y proyectos específicos del cliente que podrían dar lugar a configuraciones de servidor que no se muestran como certificadas en:

- [Dispositivos certificados para SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [Plataformas IaaS certificadas para SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

En muchos casos, estas configuraciones de servidor específicas del cliente conllevan más memoria que las unidades de servidor certificadas con SAP. Cuando se trabaja con SAP, los clientes tienen la posibilidad de obtener soporte técnico de SAP y certificar sus configuraciones de servidor con el tamaño específico del cliente. Las siguientes SKU estándar de HANA (instancias grandes) están disponibles en Azure y en la lista de precios de Microsoft para dichos proyectos de tamaño específico del cliente TDIv5.

| SKU|CPU | Memoria | Storage | Disponibilidad |
| ---| --- | --- | --- | --- |
| S96 | SAP HANA en Azure S96<br /> – 2 procesadores Intel® Xeon® E7-8890 v4<br /> 48 núcleos de CPU y 96 subprocesos de CPU |  768 GB |  3 TB | Disponible en sellos de la revisión 3 y la revisión 4|


| SKU original que se puede <br /> ampliar en memoria | CPU | Memoria | Storage | Disponibilidad |
| --- | --- | --- | --- | --- |
| S192m se puede ampliar a | SAP HANA en Azure S192xm<br /> – 4 procesadores Intel® Xeon® E7-8890 v4<br /> 96 núcleos de CPU y 192 subprocesos de CPU |  6,0 TB |  16 TB | No disponible |
| S384xm se puede ampliar a | SAP HANA en Azure S384xxm<br /> – 8 procesadores Intel® Xeon® E7-8890 v4<br /> 192 núcleos de CPU y 384 subprocesos de CPU |  12,0 TB |  28 TB | Disponible en sellos de la revisión 4 |
| S576m se puede ampliar a | SAP HANA en Azure S576xm<br /> – 12 procesadores Intel® Xeon® E7-8890 v4<br /> 288 núcleos de CPU y 576 subprocesos de CPU |  18,0 TB |  41 TB | Disponible en sellos de la revisión 4|
| S768m se puede ampliar a | SAP HANA en Azure S768xm<br /> – 16 procesadores Intel® Xeon® E7-8890 v4<br /> 384 núcleos de CPU y 768 subprocesos de CPU |  24,0 TB |  56 TB | Disponible en sellos de la revisión 4 |

- Núcleos de CPU = suma de núcleos de CPU sin Hyper-Threading de la suma de procesadores de la unidad del servidor.
- Subprocesos de CPU = suma de suprocesos de proceso proporcionado por núcleos de CPU con Hyper-Threading de la suma de procesadores de la unidad del servidor. La mayoría de las unidades se configuran de forma predeterminada para usar la tecnología Hyper-Threading.
- Según las recomendaciones del proveedor, S768m, S768xm y S960m no están configuradas para ejecutar SAP HANA con la tecnología Hyper-Threading.


Las configuraciones específicas que se elijan dependen de la carga de trabajo, los recursos de CPU y la memoria que desee. La carga de trabajo de OLTP puede usar las SKU que estén optimizadas para la carga de trabajo OLAP. 

La base de hardware para las ofertas, excepto las unidades para proyectos de tamaño especifico del cliente, cuentan con la certificación de TDI para SAP HANA. Dos clases diferentes de hardware dividen las SKU en:

- S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224 y S224m, a las que se hace referencia como "clase Tipo I" de SKU.
- S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm y S960m, a los que se hace referencia como "clase Tipo II" de SKU.
- Si está interesado en otras SKU S224 de 4,5 TB a 9 TB con Optane, póngase en contacto con el equipo de su cuenta de Microsoft para más información. 


Una demarcación completa de HANA (instancias grandes) no está asignada exclusivamente para que la use un único cliente. Esto se aplica también a los bastidores de recursos de procesos y almacenamiento conectados mediante un tejido de red implementado en Azure. La infraestructura de HANA (instancias grandes), como Azure, implementa &quot;inquilinos&quot; de clientes diferentes que están aislados entre sí en los tres niveles siguientes:

- **Network** (Red): aislamiento a través de redes virtuales dentro de la demarcación de HANA (instancias grandes).
- **Almacenamiento**: aislamiento a través de máquinas virtuales de almacenamiento que tienen volúmenes de almacenamiento asignados y aíslan los volúmenes de almacenamiento entre inquilinos.
- **Proceso**: asignación dedicada de las unidades de servidor a un único inquilino. Sin particiones de hardware ni de software de las unidades de servidor. Sin uso compartido de una sola unidad de host o servidor entre los inquilinos. 

Las implementaciones de unidades de HANA (instancias grandes) entre varios inquilinos no son visibles entre sí. Las unidades de HANA (instancias grandes) implementadas en diferentes inquilinos tampoco se pueden comunicar directamente entre sí en el nivel de la demarcación de HANA (instancias grandes). Solo las unidades de HANA (instancias grandes) dentro de un mismo inquilino se pueden comunicar entre sí en el nivel de la demarcación de HANA (instancias grandes).

Se asigna un inquilino implementado en la demarcación de instancias grandes a una suscripción de Azure con fines de facturación. Para una red, se puede acceder desde redes virtuales de otras suscripciones de Azure dentro de la misma inscripción de Azure. Si implementa con otra suscripción de Azure en la misma región de Azure, también puede elegir pedir un inquilino de instancia grande de HANA independiente.

Existen diferencias notables entre ejecutar SAP HANA en HANA (instancias grandes) y ejecutar SAP HANA en máquinas virtuales implementadas en Azure:

- No hay ningún nivel de virtualización para SAP HANA en Azure (Instancias grandes). El rendimiento proviene del hardware de reconstrucción completa subyacente.
- A diferencia de Azure, el servidor de SAP HANA en Azure (Instancias grandes) está dedicado a un cliente específico. No hay ninguna posibilidad de que un host o unidad de servidor tenga particiones de hardware o software. Como resultado, se utiliza una unidad de HANA (instancias grandes) tal cual se asignó como un conjunto a un inquilino y después al usuario. Tras un reinicio o un apagado del servidor, no se implementa el sistema operativo ni SAP HANA en otro servidor de manera automática. (Para las SKU de clase de tipo I, la única excepción es si un servidor se encuentra en problemas y se tiene que volver a implementar en otro servidor).
- A diferencia de Azure, donde los tipos de procesador de host se seleccionan para lograr una relación precio/rendimiento óptima, los tipos de procesador elegidos para SAP HANA en Azure (instancias grandes) son los de mayor rendimiento en la línea de procesadores Intel E7v3 y E7v4.

**Pasos siguientes**
- Consulte [Ajuste de tamaño de HLI](hana-sizing.md).
