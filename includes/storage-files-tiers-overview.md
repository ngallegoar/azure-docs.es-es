---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7d0286b63703c165dda6cd12bb625fc64272aac1
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011783"
---
Azure Files ofrece cuatro niveles diferentes de almacenamiento: Premium, Optimizado para transacciones, Frecuente y Esporádico, con el fin de que pueda adaptar sus recursos compartidos a los requisitos de rendimiento y precio de su escenario:

- **Premium**: Los recursos compartidos de archivos Premium están respaldados por unidades de estado sólido (SSD) y se implementan en el tipo de **cuenta de almacenamiento de FileStorage**. Los recursos compartidos de archivos Prémium proporcionan un alto rendimiento y una baja latencia de forma coherente en menos de 10 milisegundos en la mayoría de las operaciones de E/S para las cargas de trabajo con un uso intensivo de E/S, Los recursos compartidos de archivos Premium son adecuados para una amplia variedad de cargas de trabajo como bases de datos, hospedaje de sitios web y entornos de desarrollo. Los recursos compartidos de archivos Premium se pueden usar con los protocolos Bloque de mensajes del servidor (SMB) y Network File System (NFS).
- **Optimizado para transacciones**: Los recursos compartidos de archivos con el nivel Optimizado para transacciones permiten cargas de trabajo con muchas transacciones que no necesitan la latencia que ofrecen los recursos compartidos de archivos Premium. Los recursos compartidos de archivos con el nivel Optimizado para transacciones se ofrecen en el hardware de almacenamiento estándar con el respaldo de las unidades de disco duro (HDD) y se implementan en el tipo de **cuenta de almacenamiento de uso general versión 2 (GPv2)** . A los optimizados para transacciones se les ha llamado históricamente "estándar"; sin embargo, realmente es el tipo de soporte físico de almacenamiento, en lugar del propio nivel (tanto el acceso frecuente como el esporádico también son niveles "estándar", ya que se encuentran en el hardware de almacenamiento estándar).
- **Acceso frecuente**: Los recursos compartidos de nivel de acceso frecuente ofrecen almacenamiento optimizado para escenarios de uso compartido de archivos de uso general, como los recursos compartidos de los equipos y Azure File Sync. Los recursos compartidos de nivel de acceso frecuente se ofrecen en el hardware de almacenamiento estándar que usa unidades de disco duro (HDD) y se implementan en el tipo de **cuenta de almacenamiento de uso general versión 2 (GPv2)** .
- **Acceso esporádico**: Los recursos compartidos de archivos de acceso esporádico ofrecen un almacenamiento económico optimizado para escenarios de almacenamiento de archivo en línea. Azure File Sync también puede servir perfectamente para cargas de trabajo con menor renovación. Los recursos compartidos de nivel de acceso esporádico se ofrecen en el hardware de almacenamiento estándar que usa unidades de disco duro (HDD) y se implementan en el tipo de **cuenta de almacenamiento de uso general versión 2 (GPv2)** .

Los recursos compartidos de archivos Premium solo están disponibles en un modelo de facturación aprovisionada. Para más información sobre el modelo de facturación aprovisionado para recursos compartidos de archivos Premium, consulte [Planeamiento de una implementación de Azure Files](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). Los recursos compartidos de archivos estándar, que incluyen los optimizados para las transacciones, los de nivel de acceso frecuente y los de nivel de acceso esporádico, están disponibles en un modelo de pago por uso.

Los recursos compartidos de archivos de acceso frecuente y esporádico están disponibles en todas las regiones públicas y de Azure Government de Azure. Los recursos compartidos de archivos optimizados para transacciones están disponibles en todas las regiones de Azure, incluidas las regiones de Azure China y Azure Germany.

> [!Important]  
> Los recursos compartidos de archivos se pueden mover entre niveles dentro de los tipos de cuenta de almacenamiento GPv2 (transacción optimizada, nivel de acceso frecuente y nivel de acceso esporádico). Los movimientos de recursos compartidos entre niveles incurren en transacciones: el traslado de un nivel de acceso frecuente a un nivel de acceso esporádico incurrirá en el cargo de transacción de escritura del nivel de acceso esporádico, mientras que si dicho traslado se realiza de un nivel esporádico a un nivel frecuente, todos los archivos incurrirán en el cargo de transacción de lectura del nivel de acceso esporádico.