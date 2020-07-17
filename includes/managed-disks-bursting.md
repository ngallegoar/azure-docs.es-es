---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 04/27/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 2af8b429b6addf6da32b34773525c51a36624e78
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85242138"
---
En Azure, ofrecemos la posibilidad de aumentar las IOPS de almacenamiento en disco y el rendimiento de MB/s, lo que se conoce como expansión, tanto en máquinas virtuales como en discos. La expansión es útil en muchos escenarios, como el control de tráfico de disco inesperado o el procesamiento de trabajos por lotes. Puede aprovechar de forma eficaz la expansión de nivel de VM y disco para lograr una excelente base de referencia y expandir el rendimiento en la VM y el disco. De este modo, puede lograr un excelente rendimiento de base de referencia y expandir el rendimiento tanto en la VM como en el disco. 

Tenga en cuenta que la expansión de discos y máquinas virtuales son independientes entre sí. Si tiene un disco en expansión, no necesita una máquina virtual en expansión para permitir la expansión de disco. Si tiene una máquina virtual en expansión, no necesita un disco en expansión para permitir la expansión de la máquina virtual. 