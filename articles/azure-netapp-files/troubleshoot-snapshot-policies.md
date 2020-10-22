---
title: Solución de problemas de directivas de instantáneas para Azure NetApp Files | Microsoft Docs
description: Aquí se describen los mensajes de error y propuestas que pueden ayudar a solucionar los problemas de administración de directivas de instantáneas para Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/23/2020
ms.author: b-juche
ms.openlocfilehash: 6ba8b18876bdae2754a6a772ce3909ff2f5a71b7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91651011"
---
# <a name="troubleshoot-snapshot-policies"></a>Solución de problemas de directivas de instantáneas

En este artículo se describen los escenarios de error que pueden producirse al administrar directivas de instantáneas de Azure NetApp Files. También proporciona soluciones que pueden ayudarle a corregir los problemas.

## <a name="error-conditions-and-resolutions"></a>Condiciones de error y soluciones 

|     Condición de error    |     Resolución    |
|-|-|
| La creación de directiva de instantáneas genera un error de nombre de directiva de instantáneas no válido. | Durante la creación de la directiva de instantáneas se produce un error si el nombre de la directiva de instantáneas no es válido. Las siguientes directrices se aplican a los nombres de directivas de instantáneas:  <ul><li> El nombre de la directiva de instantáneas no puede contener caracteres que no sean ASCII ni caracteres especiales. </li> <li> El nombre de la directiva de instantáneas debe comenzar por una letra o un número y solo puede contener letras, números, caracteres de subrayado ("_") y guiones ("-"). </li> <li> El nombre de la directiva de instantáneas debe tener entre 1 y 64 caracteres.  </li></ul> Revise el nombre de la directiva de instantáneas según las instrucciones.  |
| La creación de directiva de instantáneas genera un error de valores no válidos. | Azure NetApp Files no puede crear una directiva de instantáneas si escribe un valor no válido para un campo como `Number of snapshots to keep` o `Minute on the hour to take snapshot`. Los valores válidos son los siguientes:  <ul><li>El valor debe ser un número válido.</li> <li>El valor debe estar entre 0 y 59.</li></ul> Asegúrese de que se proporciona un valor válido para los campos. | 
| La creación de directiva de instantáneas genera el error `Total number of snapshots to keep exceeds 255`. | Cada volumen puede tener un [máximo de 255 instantáneas](azure-netapp-files-resource-limits.md). El máximo incluye la suma de todas las instantáneas por hora, diarias, semanales y mensuales. <br> Reduzca el valor de `Snapshots to keep` e inténtelo de nuevo. |
| La asignación de una directiva a un volumen genera el error `Total snapshot policy is over the max '255'`. | Cada volumen puede tener un [máximo de 255 instantáneas](azure-netapp-files-resource-limits.md). Cuando la suma de todas las instantáneas a petición, de cada hora, diarias, semanales y mensuales supera el máximo, se produce un error. <br> Reduzca el valor de `snapshots to keep` o elimine algunas instantáneas a petición e inténtelo de nuevo. | 

## <a name="next-steps"></a>Pasos siguientes  

* [Administración de directivas de instantánea](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies)
