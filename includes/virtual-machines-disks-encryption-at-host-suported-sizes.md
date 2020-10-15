---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e5a811620de8336abd3e0df6d72db761ce18b2b6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86230997"
---
Toda la última generación de tamaños de máquina virtual admite el cifrado en el host:

|Tipo  |No compatible  |Compatible  |
|---------|---------|---------|
|Uso general     | Dv3, Dav4, Dv2, Av2        | B, DSv2, Dsv3, DC, DCv2, Dasv4        |
|Proceso optimizado     |         | Fsv2        |
|Memoria optimizada     | Ev3, Eav4        | DSv2, Esv3, M, Mv2, Easv4        |
|Almacenamiento optimizado     |         | Ls, Lsv2 (discos NVMe no cifrados)        |
|GPU     | NC, NV        | NCv2, NCv3, ND, NVv3, NVv4, NDv2 (versión preliminar)        |
|Informática de alto rendimiento     | H        | HB, HC, HBv2        |
|Generaciones anteriores     | F, A, D, L, G        | DS, GS, Fs, NVv2        |

La actualización del tamaño de la máquina virtual producirá una validación para comprobar si el nuevo tamaño de máquina virtual admite la característica EncryptionAtHost.
