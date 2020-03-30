---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 86d03e980a233a60e4f3a6541462264dae0862af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74005587"
---
Este artículo proporciona instrucciones para crear aplicaciones de alto rendimiento con Azure Premium Storage. Puede usar las instrucciones proporcionadas en este documento junto con los procedimientos recomendados de rendimiento aplicables a las tecnologías usadas por la aplicación. Para ilustrar las directrices, hemos usado SQL Server en Premium Storage como ejemplo en este documento.

Si bien en este artículo se tratan los escenarios de rendimiento de la capa de almacenamiento, deberá optimizar la capa de la aplicación. Por ejemplo, si hospeda una granja de SharePoint en Azure Premium Storage, puede usar los ejemplos de SQL Server de este artículo para optimizar el servidor de bases de datos. Además, optimice el servidor web y el servidor de aplicaciones de la granja de SharePoint para obtener el máximo rendimiento.

Este artículo le ayudará a responder a las siguientes preguntas habituales acerca de cómo optimizar el rendimiento de las aplicaciones en Azure Premium Storage:

* ¿Cómo medir el rendimiento de las aplicaciones?  
* ¿Por qué no se ve el alto rendimiento esperado?  
* ¿Qué factores influyen en el rendimiento de las aplicaciones en Premium Storage?  
* ¿Cómo influyen estos factores en el rendimiento de las aplicaciones en Premium Storage?  
* ¿Cómo puede optimizar para IOPS, el ancho de banda y la latencia?  

Proporcionamos estas directrices específicamente para Premium Storage porque las cargas de trabajo que se ejecutan en Premium Storage dependen mucho del rendimiento. Se proporcionan ejemplos donde corresponda. También puede aplicar algunas de estas instrucciones a las aplicaciones que se ejecutan en máquinas virtuales de IaaS con discos de Standard Storage.