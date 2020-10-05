---
title: Movimiento de recursos de Azure a otra región
description: Proporciona información general sobre cómo mover recursos de Azure entre regiones de Azure.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 7a71502ec361004079e0962d8bc6433316a4ba81
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007645"
---
# <a name="moving-azure-resources-across-regions"></a>Movimiento de recursos de Azure entre regiones

En este artículo se proporciona información sobre cómo mover recursos de Azure entre regiones de Azure.

Las geografías, las regiones y las zonas de disponibilidad de Azure conforman la base de la infraestructura global de Azure. Las [geografías](https://azure.microsoft.com/global-infrastructure/geographies/) de Azure suelen contener dos o más [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/). Una región es un área dentro de una geografía que contiene zonas de disponibilidad y varios centros de datos. 

Después de implementar recursos en una región específica de Azure, hay una serie de motivos por los que podría querer mover recursos a una región diferente.

- **Alineación con el lanzamiento de una región**: mueva los recursos a una región de Azure que acaba de incorporar y que no estaba disponible previamente.
- **Alineación con los servicios o las características**: mueva recursos para aprovechar las ventajas de los servicios o las características que están disponibles en una región específica.
- **Respuesta a desarrollos empresariales**: mueva recursos a una región en respuesta a cambios empresariales, como fusiones o adquisiciones.
- **Alineación para proximidad**: mueva recursos a una región que sea local para la empresa.
- **Cumplimiento de requisitos de los datos**: mueva recursos para la alineación con los requisitos de residencia de los datos o las necesidades de clasificación de los datos. [Más información](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf).
- **Respuesta a requisitos de implementación**: mueva recursos que se implementaron por error, o como respuesta a necesidades de capacidad. 
- **Respuesta al desmantelamiento**: mueva recursos debido al desmantelamiento de regiones.

## <a name="move-resources-with-resource-mover"></a>Traslado de recursos con Resource Mover

Puede trasladar recursos a una región diferente con [Azure Resource Mover](../../resource-mover/overview.md). Resource Mover proporciona:

- Un solo centro para trasladar recursos entre regiones.
- Tiempos y complejidad de traslado menores. Todo lo que necesita está en una única ubicación.
- Una experiencia sencilla y coherente para trasladar distintos tipos de recursos de Azure.
- Una manera fácil de identificar las dependencias entre los recursos que quiera trasladar. Esto le ayuda a trasladar los recursos relacionados de forma conjunta, de modo que todo funciona según lo previsto en la región de destino después del traslado.
- Limpieza automática de los recursos de la región de origen, si quiere eliminarlos después del traslado.
- Pruebas. Puede probar un traslado y, a continuación, descartarlo si no quiere completarlo.

Puede trasladar recursos a otra región con un par de métodos diferentes:

- **Comience a trasladar recursos de un grupo de recursos**: Con este método, se inicia el traslado de región desde un grupo de recursos. Después de seleccionar los recursos que quiere trasladar, el proceso continúa en el centro de Resource Mover para comprobar las dependencias de los recursos y coordinar el proceso de traslado. [Más información](../../resource-mover/move-region-within-resource-group.md).
- **Comience a trasladar recursos directamente desde el centro de Resource Mover**: Con este método, se inicia el proceso de traslado de la región directamente en el centro. [Más información](../../resource-mover/tutorial-move-region-virtual-machines.md).


## <a name="support-for-region-move"></a>Compatibilidad con el traslado de región

Actualmente, puede usar Resource Mover para trasladar los siguientes recursos a otra región:

- Máquinas virtuales de Azure y discos asociados
- NIC
- Conjuntos de disponibilidad
- Redes virtuales de Azure
- Direcciones IP públicas
- Grupos de seguridad de red (NSG)
- Equilibradores de carga internos y públicos
- Bases de datos de Azure SQL y grupo de bases de datos elásticas

## <a name="region-move-process"></a>Proceso de traslado de región

El proceso real para trasladar recursos entre regiones depende de los recursos que se van a trasladar. Sin embargo, hay algunos pasos clave que son comunes:

1. **Verificación de los requisitos previos**: los requisitos previos son asegurarse de que los recursos que necesita están disponibles en la región de destino, comprobar que tiene una cuota suficiente y comprobar que la suscripción puede acceder a la región de destino.
2. **Análisis de dependencias**: los recursos pueden tener dependencias de otros recursos. Antes del movimiento, averigüe las dependencias para que los recursos movidos sigan funcionando según lo esperado después del movimiento.
3. **Preparación del movimiento**: estos son los pasos que debe realizar en la región principal antes del movimiento. Por ejemplo, puede que necesite exportar una plantilla de Azure Resource Manager o empezar a replicar recursos desde el origen en el destino.
4. **Movimiento de los recursos**: la forma de mover los recursos dependerá de lo que sean. Es posible que deba implementar una plantilla en la región de destino o conmutar por error los recursos al destino.
5. **Descarte de recursos de destino**: después de mover los recursos, es posible que desee echar un vistazo a los recursos que se encuentran en la región de destino y decidir si hay alguno que no necesita.
6. **Confirmación del movimiento**: después de comprobar los recursos en la región de destino, algunos recursos pueden requerir una acción de confirmación final. Por ejemplo, en una región de destino que ahora es la región primaria, es posible que tenga que configurar la recuperación ante desastres en una nueva región secundaria. 
7. **Limpieza del origen**: por último, después de que todo esté activo y en funcionamiento en la nueva región, puede limpiar y retirar los recursos que creó para el movimiento y los recursos de la región primaria.



## <a name="next-steps"></a>Pasos siguientes

[Más información](../../resource-mover/about-move-process.md) sobre el proceso de traslado en Resource Mover.
