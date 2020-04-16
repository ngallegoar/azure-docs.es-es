---
title: 'Referencia: Desuso de la imagen de Data Science Virtual Machine'
titleSuffix: Azure Data Science Virtual Machine
description: Detalles sobre el desuso que afecta a Azure Data Science Virtual Machine
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 04/03/2020
ms.topic: reference
ms.openlocfilehash: 6004ae0c1fd2fa25a07ab84776ab74789f6e7da9
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754770"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referencia: Desuso de las imágenes de DSVM

A continuación se describen sugerencias para tratar con las versiones que próximamente caerán en desuso de Azure Data Science Virtual Machine.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Migración de discos de datos

Dejaremos de admitir la imagen de DSVM Windows 2012 el 31 de diciembre de 2019. Para migrar a un disco de datos de la versión de DSVM Windows 2012 existente a Windows 2016, realice los pasos siguientes:

1. Cree una nueva instancia de DSVM Windows 2016 siguiendo las instrucciones que se muestran [aquí](./provision-vm.md#create-your-dsvm).
1. Desasocie los discos de datos existentes de la imagen de Windows 2012 según [estas instrucciones](../../virtual-machines/windows/detach-disk.md).
1. Conecte el disco del paso anterior a la imagen de Windows 2016 según [estas instrucciones](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm).

## <a name="centos"></a>CentOS

Los usuarios nuevos deben usar las imágenes de Ubuntu o Windows más recientes. CentOS seguirá estando disponible para su uso con las plantillas de solución existentes.