---
title: Movimiento de recursos de Azure a otra región
description: Proporciona información general sobre cómo mover recursos de Azure entre regiones de Azure.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: raynew
ms.openlocfilehash: 22d8bcee96b4ac52641d4f0841267195f44fe15a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75476594"
---
# <a name="moving-azure-resources-across-regions"></a>Movimiento de recursos de Azure entre regiones

En este artículo se proporciona información sobre cómo mover recursos de Azure entre regiones de Azure.

Las geografías, las regiones y las zonas de disponibilidad de Azure forman la base de la infraestructura global de Azure. Las [geografías](https://azure.microsoft.com/global-infrastructure/geographies/) de Azure suelen contener dos o más [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/). Una región es un área dentro de una geografía que contiene zonas de disponibilidad y varios centros de datos. 

Después de implementar recursos en una región específica de Azure, hay una serie de motivos por los que podría querer mover recursos a una región diferente.

- **Alineación con el lanzamiento de una región**: mueva los recursos a una región de Azure que acaba de incorporar y que no estaba disponible previamente.
- **Alineación con los servicios o las características**: mueva recursos para aprovechar las ventajas de los servicios o las características que están disponibles en una región específica.
- **Respuesta a desarrollos empresariales**: mueva recursos a una región en respuesta a cambios empresariales, como fusiones o adquisiciones.
- **Alineación para proximidad**: mueva recursos a una región que sea local para la empresa.
- **Cumplimiento de requisitos de los datos**: mueva recursos para la alineación con los requisitos de residencia de los datos o las necesidades de clasificación de los datos. [Más información](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf).
- **Respuesta a requisitos de implementación**: mueva recursos que se implementaron por error, o como respuesta a necesidades de capacidad. 
- **Respuesta al desmantelamiento**: mueva recursos debido al desmantelamiento de regiones.

## <a name="move-process"></a>Proceso de movimiento

El proceso de movimiento real depende de los recursos que se van a mover. Sin embargo, hay algunos pasos clave que son comunes:

- **Verificación de los requisitos previos**: los requisitos previos son asegurarse de que los recursos que necesita están disponibles en la región de destino, comprobar que tiene una cuota suficiente y comprobar que la suscripción puede acceder a la región de destino.
- **Análisis de dependencias**: los recursos pueden tener dependencias de otros recursos. Antes del movimiento, averigüe las dependencias para que los recursos movidos sigan funcionando según lo esperado después del movimiento.
- **Preparación del movimiento**: estos son los pasos que debe realizar en la región principal antes del movimiento. Por ejemplo, puede que necesite exportar una plantilla de Azure Resource Manager o empezar a replicar recursos desde el origen en el destino.
- **Movimiento de los recursos**: la forma de mover los recursos dependerá de lo que sean. Es posible que deba implementar una plantilla en la región de destino o conmutar por error los recursos al destino.
- **Descarte de recursos de destino**: después de mover los recursos, es posible que desee echar un vistazo a los recursos que se encuentran en la región de destino y decidir si hay alguno que no necesita.
- **Confirmación del movimiento**: después de comprobar los recursos en la región de destino, algunos recursos pueden requerir una acción de confirmación final. Por ejemplo, en una región de destino que ahora es la región primaria, es posible que tenga que configurar la recuperación ante desastres en una nueva región secundaria. 
- **Limpieza del origen**: por último, después de que todo esté activo y en funcionamiento en la nueva región, puede limpiar y retirar los recursos que creó para el movimiento y los recursos de la región primaria.



## <a name="next-steps"></a>Pasos siguientes

Para una lista de qué recursos son compatibles con el movimiento entre regiones, consulte [Compatibilidad con la operación de movimiento para recursos](region-move-support.md).
