---
title: Notas de la versión de Microsoft Azure StorSimple Virtual Array Update 1.2 | Microsoft Docs
description: Se describen los problemas críticos por resolver y las soluciones para StorSimple Virtual Array que ejecuta Update 1.2.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: 1118dfcec67a49365f1f6e5e522e98b97694d052
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94960202"
---
# <a name="storsimple-virtual-array-update-12-release-notes"></a>Notas de la versión de StorSimple Virtual Array Update 1.2

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de las actualizaciones de la matriz virtual de Microsoft Azure StorSimple.

Las notas de la versión se actualizan continuamente. A medida que se descubren problemas críticos que requieren una solución alternativa, se van agregando. Antes de implementar la matriz virtual de StorSimple, le recomendamos que lea detenidamente la información que encontrará en las notas de la versión.

Update 1.2 se corresponde con la versión del software **10.0.10311.0**.

> [!IMPORTANT]
> - Las actualizaciones causan interrupciones y reinician el dispositivo. Si hay procesos de E/S en curso, el dispositivo incurrirá en tiempo de inactividad. Para instrucciones detalladas con los paquetes que se usan para aplicar esta actualización, vaya a [Descargar Update 1.2](#download-update-12).
> - Update 1.2 solo está disponible a través de Azure Portal si el dispositivo ejecuta Update 1.0 o 1.1.

## <a name="whats-new-in-update-12"></a>Novedades de la actualización 1.2

Esta actualización contiene las siguientes correcciones de errores:

- Resistencia mejorada al procesar los archivos eliminados.
- Control de excepciones mejorado en la ruta de acceso de inicio del código, lo que reduce los errores en las copias de seguridad, la restauración, las lecturas en la nube y la recuperación de espacio automatizada.

## <a name="download-update-12"></a>Descarga de Update 1.2

Para descargar esta actualización, vaya al servidor de [Catálogo de Microsoft Update](https://www.catalog.update.microsoft.com/Home.aspx) y descargue el paquete KB4502035. Este paquete contiene los siguientes paquetes:

 - **KB4493446** que contiene las actualizaciones acumulativas de Windows desde 2012 R2 hasta abril de 2019. Para más información sobre qué se incluye en este paquete acumulativo, vaya a [9 de abril de 2019, KB4493446 (paquete acumulativo mensual)](https://support.microsoft.com/help/4493446/windows-8-1-update-kb4493446).
 - **KB3011067** que es un archivo de paquete independiente de Microsoft Update WindowsTH-KB3011067-x64. Este archivo se utiliza para actualizar el software del dispositivo.

Descargue KB4502035 y siga estas instrucciones para [aplicar la actualización a través de la interfaz de usuario local](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="issues-fixed-in-update-12"></a>Problemas corregidos en la actualización 1.2

En la tabla siguiente se proporciona un resumen de los problemas corregidos en esta versión.

| No. | Característica | Problema |
| --- | --- | --- |
| 1 |Eliminación| En las versiones anteriores del software, hubo un problema cuando el uso del dispositivo no cambiaba ni siquiera cuando se eliminaban archivos. Este problema está corregido en esta versión. La organización en niveles de la ruta de acceso del código se hizo más resistente al procesar los archivos eliminados.|
| 2 |Control de excepciones| En las versiones anteriores del software, hubo un problema tras el reinicio del sistema que podría conducir a errores en las copias de seguridad, la restauración, la lectura desde la nube y la recuperación automatizada del espacio. Esta versión contiene cambios sobre cómo se controlaron las excepciones en la ruta de acceso de inicio.|

## <a name="known-issues-in-update-12"></a>Problemas conocidos de la actualización 1.2

No se notificaron nuevos problemas de la versión en Update 1.2. Todos los problemas notificados de la versión provienen de las versiones anteriores. Para ver el resumen de los problemas conocidos incluidos desde las versiones anteriores, vaya a [Problemas conocidos de Update 1.1](storsimple-virtual-array-update-11-release-notes.md#known-issues-in-update-11).

## <a name="next-steps"></a>Pasos siguientes

Descargar KB4502035 y [aplicar la actualización a través de la interfaz de usuario local](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="references"></a>Referencias

¿Busca una nota de la versión anterior? Vaya a:
* [Notas de la versión de StorSimple Virtual Array Update 1.1](storsimple-virtual-array-update-11-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 1.0](storsimple-virtual-array-update-1-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.6](storsimple-virtual-array-update-06-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.5](storsimple-virtual-array-update-05-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.4](storsimple-virtual-array-update-04-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.3](storsimple-ova-update-03-release-notes.md)
* [Notas de la versión de la matriz virtual de StorSimple Update 0.1 y 0.2](storsimple-ova-update-01-release-notes.md)
* [Notas de la versión de disponibilidad general de la matriz Virtual de StorSimple](./storsimple-virtual-array-update-06-release-notes.md)