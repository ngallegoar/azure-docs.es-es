---
title: Requisitos del sistema de Microsoft Azure Data Box Disk | Microsoft Docs
description: Obtenga más información sobre los requisitos de software y red de Azure Data Box Disk
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 09/04/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: fb2fd89664517e44cf5128a5c82e583f03087061
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79214265"
---
::: zone target="docs"

# <a name="azure-data-box-disk-system-requirements"></a>Requisitos del sistema de Azure Data Box Disk

En este artículo se describen los requisitos del sistema importantes de la solución Microsoft Azure Data Box Disk y de los clientes de que se conectan a Data Box Disk. Es recomendable que revise cuidadosamente la siguiente información antes de implementar Data Box Disk y que luego la consulte según sea necesario durante la implementación y el funcionamiento posterior.

Los requisitos del sistema incluyen las plataformas admitidas para los clientes que se conectan a discos, cuentas de almacenamiento admitidas y tipos de almacenamiento.

::: zone-end

::: zone target="chromeless"

## <a name="review-prerequisites"></a>Revisar los requisitos previos

1. Tiene que haber realizado el pedido de Data Box Disk mediante el [Tutorial: Pedido de Azure Data Box Disk](data-box-disk-deploy-ordered.md). Ha recibido los discos y un cable de conexión por disco.
2. Tiene un equipo cliente disponible desde el que puede copiar los datos. El equipo cliente debe:

    - Ejecutar un sistema operativo admitido.
    - Tener instalado otro software necesario.

::: zone-end

## <a name="supported-operating-systems-for-clients"></a>Sistemas operativos compatibles para clientes

Aquí se proporciona una lista de los sistemas operativos compatibles para la operación de desbloqueo de discos y copia de datos a través de los clientes conectados a Data Box Disk.

| **Sistema operativo** | **Versiones probadas** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows (64 bits) |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Otro software necesario para los clientes Windows

Para el cliente Windows, también se debe instalar lo siguiente.

| **Software**| **Versión** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Otro software necesario para los clientes Linux

Para el cliente Linux, el Conjunto de herramientas de Data Box Disk instala el siguiente software necesario:

- dislocker
- OpenSSL

## <a name="supported-connection"></a>Conexión admitida

El equipo cliente que contiene los datos debe tener un puerto USB 3.0 o de una versión posterior. Los discos se conectan a este cliente mediante el cable proporcionado.

## <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

Aquí se proporciona una lista de los tipos de almacenamiento compatibles para Data Box Disk.

| **Cuenta de almacenamiento** | **Notas** |
| --- | --- |
| Clásico | Estándar |
| Uso general  |Estándar; se admiten V1 y V2. Se admiten niveles frecuentes y esporádicos. |
| Cuenta de Blob Storage | |

>[!NOTE]
> No se admiten las cuentas de Azure Data Lake Storage Gen 2.


## <a name="supported-storage-types-for-upload"></a>Tipos de almacenamiento admitidos para la carga

A continuación se muestra una lista de los tipos de almacenamiento admitidos para cargar en Azure mediante Data Box Disk.

| **Formato de archivo** | **Notas** |
| --- | --- |
| Blob en bloques de Azure | |
| Blob en páginas de Azure  | |
| Archivos de Azure  | |
| Managed Disks | |

::: zone target="docs"

## <a name="next-step"></a>Paso siguiente

* [Implementación de Azure Data Box Disk](data-box-disk-deploy-ordered.md)

::: zone-end

