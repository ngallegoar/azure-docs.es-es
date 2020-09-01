---
title: 'Referencia: Retirada de la imagen de Data Science Virtual Machine'
titleSuffix: Azure Data Science Virtual Machine
description: Detalles sobre las retiradas que afectan a Azure Data Science Virtual Machine
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 07/17/2020
ms.topic: reference
ms.openlocfilehash: ac56e2e69cf62c8afae73e0a7616e7e7f80c0493
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816327"
---
# <a name="reference-retirements-of-dsvm-images"></a>Referencia: Retiradas de imágenes de DSVM

A continuación, se describe la retirada (desuso) de imágenes y las sugerencias para abordar las próximas retiradas en Azure Data Science Virtual Machine.

## <a name="why-we-retire-dsvm-images"></a>Por qué se retiran imágenes de DSVM

Actualmente, Data Science Virtual Machine tiene dos ediciones:

* Ubuntu
* Windows Server

La imagen de DSVM de estas ediciones se basa en una versión específica del sistema operativo, por ejemplo, Ubuntu 18.04 LTS. Con el tiempo, la ventana de mantenimiento de la _versión_ del sistema operativo finalizará o las herramientas de ciencia de datos ya no admitirán versiones anteriores del sistema operativo. Para mantener actualizada la imagen de DSVM con los sistemas operativos y las herramientas de ciencia de datos más recientes, se ha lanzado una nueva imagen de DSVM basada en las versiones más recientes del sistema operativo.

## <a name="how-we-retire-dsvm-images"></a>Cómo se retiran las imágenes de DSVM

Se emite un _anuncio de retirada_ en el que los usuarios de DSVM existentes reciben una notificación (por correo electrónico) de una próxima retirada de una imagen de DSVM. En el anuncio de retirada se detallará la _fecha de retirada_ (después de esta fecha, la imagen no está disponible) y las recomendaciones de mitigación (por ejemplo, actualizar a una imagen de DSVM más reciente).

En la fecha del _anuncio_, se ocultará la imagen en el marketplace por las siguientes razones:

1. Impedir que los nuevos usuarios aprovisionen sin querer una imagen de DSVM retirada.
2. Los usuarios existentes puedan usar las implementaciones de ARM hasta la fecha de retirada.

La imagen oculta recibirá revisiones del sistema operativo hasta la _fecha de retirada_, pero __no__ recibirá actualizaciones de las herramientas y los marcos de ciencia de datos. En la _fecha de retirada_, la imagen no estará disponible para las implementaciones de ARM.

Cualquier imagen de DSVM aprovisionada en la suscripción seguirá funcionando después de la fecha de retirada. Sin embargo, se recomienda actualizar a la imagen de DSVM más reciente para tener los sistemas operativos y las herramientas de ciencia de datos más actuales.

## <a name="impact"></a>Impacto

Las imágenes aprovisionadas de DSVM existentes en la suscripción seguirán funcionando después de la fecha de retirada. Sin embargo, se recomienda que los usuarios actualicen su imagen de DSVM a la versión más reciente mediante Azure Portal o la plantilla de ARM.

> [!WARNING]
> Las imágenes de DSVM retiradas aprovisionadas con Virtual Machine Scale Sets no podrán escalarse verticalmente después de la fecha de retirada.
>
> Las plantillas de ARM que no se han actualizado con los nuevos detalles de la imagen de DSVM no se implementarán después de la fecha de retirada.

## <a name="mitigating-upcoming-retirements"></a>Mitigación de futuras retiradas

En esta sección, se describe la mitigación de futuras retiradas.

### <a name="upgrade-windows-2016-dsvm"></a>Actualización de DSVM de Windows 2016

Para migrar a un disco de datos de la versión de DSVM Windows 2016 existente a Windows 2019, realice los pasos siguientes:

1. Cree una instancia de DSVM de Windows 2019 siguiendo las instrucciones que se muestran [aquí](./provision-vm.md#create-your-dsvm).
1. Desasocie los discos de datos existentes de la imagen de Windows 2016 según [estas instrucciones](../../virtual-machines/windows/detach-disk.md).
1. Asocie el disco del paso anterior a la imagen de Windows 2019 según [estas instrucciones](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm).

### <a name="upgrade-ubuntu-1604-dsvm"></a>Actualización de DSVM de Ubuntu 16.04

Se recomienda actualizar las instancias de DSVM de Ubuntu 16.04 existentes a la [edición de Ubuntu 18.04](./dsvm-ubuntu-intro.md).
