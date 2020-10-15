---
title: Historial de versiones de Azure Site Recovery Deployment Planner
description: Correcciones de distintas versiones de Site Recovery Deployment Planner y limitaciones conocidas y limitaciones conocidas junto con sus fechas de lanzamiento.
services: site-recovery
author: Daya-Patil
manager: carmonm
ms.topic: article
ms.service: site-recovery
ms.date: 6/4/2020
ms.author: dapatil
ms.openlocfilehash: feb4f6a24653aca8da825af90341c8016255e8b6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86133810"
---
# <a name="azure-site-recovery-deployment-planner-version-history"></a>Historial de versiones de Azure Site Recovery Deployment Planner

En este artículo se proporciona el historial de todas las versiones de Azure Site Recovery Deployment Planner, junto con las correcciones y limitaciones conocidas de cada una de ellas, así como sus fechas de lanzamiento.

## <a name="version-252"></a>Versión 2.52

**Fecha de lanzamiento: 4 de junio de 2020**

**Correcciones:**

- Se agregó compatibilidad para vCenter 7.0.
- Se agregó compatibilidad para los siguientes sistemas operativos:

    - SUSE Linux Enterprise 15 (con todas las versiones secundarias)
    - Red Hat Enterprise Linux 8 (con todas las versiones secundarias)


## <a name="version-251"></a>Versión 2.51

**Fecha de lanzamiento: 22 de agosto de 2019**

**Correcciones:**

- Se ha corregido el problema de la recomendación de costos con Deployment Planner, versión 2.5

## <a name="version-25"></a>Versión 2.5

**Fecha de lanzamiento: 29 de julio de 2019**

**Correcciones:**

- En el caso de máquinas virtuales de VMware y máquinas físicas, la recomendación se actualiza para basarse en la replicación en Managed Disks.
- Se ha agregado compatibilidad con Windows 10 (x64), Windows 8.1 (x64), Windows 8 (x64) y Windows 7 (x64) SP1 o posterior.

## <a name="version-24"></a>Versión 2.4

**Fecha de lanzamiento: 17 de abril de 2019**

**Correcciones:**

- Se mejoró la compatibilidad de sistemas operativos, específicamente al control de errores de localización.
- Se agregaron VM de hasta 20 Mbps de frecuencia de cambio de datos (renovación) a la lista de compatibilidad.
- Se mejoraron los mensajes de error.
- Se agregó compatibilidad para vCenter 6.7.
- Se agregó compatibilidad para la estación de trabajo de Windows Server 2019 y Red Hat Enterprise Linux (RHEL).



## <a name="version-23"></a>Versión 2.3

**Fecha de lanzamiento: 3 de diciembre de 2018**

**Correcciones:**

- Se corrigió un problema que impedía que Deployment Planner generara un informe con la ubicación de destino y la suscripción proporcionadas.

## <a name="version-22"></a>Versión 2.2 

**Fecha de lanzamiento: 25 de abril de 2018**

**Correcciones:**

- Operaciones GetVMList:
  - Se corrigió un problema que provocaba que GetVMList produjera un error si la carpeta especificada no existía. Ahora crea el directorio predeterminado, o crea el directorio especificado en el parámetro outputfile.
  - Se agregaron motivos más detallados del error para GetVMList.
- Se agregó información de tipo de VM, como una columna en la hoja de VM compatibles del informe de Deployment Planner.
- Recuperación ante desastres de Hyper-V a Azure:
  - Se excluyeron de la generación de perfiles las VM con VHD compartidos y discos de acceso directo. La operación Startprofiling muestra la lista de VM excluidas en la consola.
  - Se agregaron VM con más de 64 discos a la lista de VM incompatibles.
  - Se actualizó la replicación inicial (IR) y el factor de compresión de replicación diferencial (DR).
  - Se agregó compatibilidad limitada para el almacenamiento SMB.

## <a name="version-21"></a>Versión 2.1

**Fecha de lanzamiento: 3 de enero de 2018**

**Correcciones:**

- Se actualizó el informe de Excel.
- Se corrigieron errores en la operación GetThroughput.
- Se agregó la opción para limitar el número de VM para generar perfiles o generar el informe. El límite predeterminado es de 1000 VM.
- Recuperación ante desastres de VMware a Azure:
  - Se corrigió un problema por el que VM de Windows Server 2016 iban a la tabla incompatible. 
  - Se actualizaron los mensajes de compatibilidad para VM Windows de Extensible Firmware Interface (EFI).
- Se actualizó el límite de renovación de datos de VMware a Azure y de Hyper-V a Azure por VM. 
- Se mejoró la confiabilidad de análisis de archivos de lista de VM.

## <a name="version-201"></a>Versión 2.0.1

**Fecha de lanzamiento: 7 de diciembre de 2017**

**Correcciones:**

- Se agregó la recomendación de optimizar el ancho de banda de red.

## <a name="version-20"></a>Versión 2.0

**Fecha de lanzamiento: 28 de noviembre de 2017**

**Correcciones:**

- Se agregó compatibilidad para la recuperación ante desastres de Hyper-V a Azure.
- Se agregó la calculadora de costos.
- Se agregó la comprobación de la versión de sistema operativo para la recuperación ante desastres de VMware a Azure para determinar si la VM es compatible o incompatible para la protección. La herramienta usa la cadena de versión del sistema operativo devuelto por el servidor vCenter para esa VM. Es la versión de sistema operativo invitado que el usuario ha seleccionado al crear la VM en VMware.

**Limitaciones conocidas:**

- Para la recuperación ante desastres de Hyper-V a Azure, no se admiten VM con nombres que contengan caracteres como: `,`, `"`, `[`, `]` y ``` ` ```. Si se generan perfiles, la generación de informes producirá un error o tendrá un resultado incorrecto.
- Para la recuperación ante desastres de VMware a Azure, no se admiten VM con nombres que contengan comas. Si se generan perfiles, la generación de informes produce un error o tiene un resultado incorrecto.

## <a name="version-131"></a>Versión 1.3.1

**Fecha de lanzamiento: 19 julio de 2017** 

**Correcciones:**

- Se agregó compatibilidad con discos de gran tamaño (> 1TB) en la generación de informes. Ahora puede usar Deployment Planner para planear la replicación de máquinas virtuales que tengan tamaños de disco superiores a 1 TB (hasta 4095 GB).
Para más información, consulte [Compatibilidad con discos de gran tamaño en Azure Site Recovery](https://azure.microsoft.com/blog/azure-site-recovery-large-disks/)

## <a name="version-13"></a>Versión 1.3

**Fecha de lanzamiento: 9 de mayo de 2017**

**Correcciones:**

- Se agregó compatibilidad con discos administrados en la generación de informes. El número de VM que se pueden colocar en una cuenta de almacenamiento individual se calcula en función de si el disco administrado está seleccionado para la conmutación por error o la conmutación por error de prueba.

## <a name="version-12"></a>Versión 1.2

**Fecha de lanzamiento: 7 de abril de 2017**

**Correcciones:**

- Se agregaron comprobaciones del tipo de arranque (BIOS o EFI) para cada VM, a fin de determinar si la VM es compatible o incompatible a efectos de protección.
- Se agregó la información del tipo de sistema operativo para cada máquina virtual en las hojas de cálculo de VM compatibles y máquina virtuales incompatibles.
- Se agregó compatibilidad para la operación GetThroughput en las regiones de Microsoft Azure US Government y China.
- Se han agregado algunas comprobaciones más de requisitos previos para el servidor vCenter y ESXi.
- Se corrigió un error de informe incorrecto generado cuando la configuración local no está establecida en inglés.

## <a name="version-11"></a>Versión 1.1

**Fecha de lanzamiento: 9 de marzo de 2017**

**Correcciones:**

- Se corrigió un problema que impedía la generación de perfiles de VM cuando había dos VM o más con el mismo nombre o dirección IP entre los distintos hosts ESXi de vCenter.
- Se corrigió un problema que provocaba que la copia y la búsqueda estuvieran deshabilitas en las hojas de cálculo de VM compatibles y VM no compatibles.

## <a name="version-10"></a>Versión 1.0

**Fecha de lanzamiento: 23 de febrero de 2017**

**Limitaciones conocidas:**

- Se admite solo para los escenarios de recuperación ante desastres de VMware a Azure. En los escenarios de recuperación ante desastres de Hyper-V a Azure, use la [herramienta de planeamiento de la capacidad de Hyper-V](./hyper-v-deployment-planner-overview.md).
- No se admite la operación GetThroughput en las regiones de Microsoft Azure US Government y China.
- La herramienta no puede generar perfiles de máquinas virtuales si el servidor vCenter tiene dos, o más, máquinas virtuales con el mismo nombre o dirección IP entre los distintos hosts de ESXi.
En esta versión, la herramienta omite la generación de perfiles de los nombres o direcciones IP de máquinas virtuales duplicados de VMListFile. La solución alternativa consiste en generar perfiles de las máquinas virtuales mediante un host de ESXi, en lugar del servidor vCenter. Asegúrese de ejecutar una instancia para cada host de ESXi.
