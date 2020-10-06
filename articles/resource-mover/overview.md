---
title: ¿Qué es Azure Resource Mover?
description: Obtenga información sobre Azure Resource Mover.
author: rayne-wiselman
manager: evansma
ms.service: azure-migrate
ms.topic: overview
ms.date: 09/09/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 621c5e7b0061ccd76fd0109552107915b943511f
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653340"
---
# <a name="what-is-azure-resource-mover"></a>¿Qué es Azure Resource Mover?

En este artículo se proporciona una introducción al servicio Azure Resource Mover. Azure Resource Mover le ayuda a trasladar los recursos de Azure entre regiones de Azure.

Puede trasladar recursos a otras regiones de Azure para:

- **Alineación con el lanzamiento de una región**: mueva los recursos a una región de Azure que acaba de incorporar y que no estaba disponible previamente.
- **Alineación con los servicios o las características**: mueva recursos para aprovechar las ventajas de los servicios o las características que están disponibles en una región específica.
- **Respuesta a desarrollos empresariales**: mueva recursos a una región en respuesta a cambios empresariales, como fusiones o adquisiciones.
- **Alineación para proximidad**: mueva recursos a una región que sea local para la empresa.
- **Cumplimiento de requisitos de los datos**: traslade los recursos para la alineación con los requisitos de residencia de los datos o las necesidades de clasificación de los datos.
- **Respuesta a requisitos de implementación**: mueva recursos que se implementaron por error, o como respuesta a necesidades de capacidad.
- **Respuesta al desmantelamiento**: traslade los recursos por el desmantelamiento de una región.

> [!IMPORTANT]
> Azure Resource Mover se encuentra actualmente en versión preliminar pública.

## <a name="why-use-resource-mover"></a>¿Por qué usar Resource Mover?

Resource Mover proporciona:

- Un solo centro para trasladar recursos entre regiones.
- Menores tiempo de traslado y complejidad. Todo lo que necesita está en una única ubicación.
- Una experiencia sencilla y coherente para trasladar distintos tipos de recursos de Azure.
- Una manera fácil de identificar las dependencias entre los recursos que desee trasladar. Esto le ayuda a trasladar los recursos relacionados de forma conjunta, de modo que todo funciona según lo previsto en la región de destino, después del traslado.
- La limpieza automática de los recursos de la región de origen, si desea eliminarlos después del traslado.
- Pruebas: Puede probar un traslado y, a continuación, descartarlo si no desea completarlo.

## <a name="move-across-regions"></a>Traslado entre regiones

Para trasladar recursos entre regiones, seleccione los recursos que desea trasladar. Resource Mover valida esos recursos y resuelve las dependencias que tengan en otros recursos. Si hay dependencias, tiene un par de opciones:
- Trasladar los recursos dependientes a la región de destino.
- No trasladar los recursos dependientes, sino utilizar en su lugar recursos equivalentes en la región de destino.

Una vez que se resuelven todas las dependencias, Resource Mover le guía en un sencillo proceso de traslado.

1. En primer lugar, se ejecuta un traslado inicial.
2. Después del traslado inicial, puede decidir si desea confirmar y completar el traslado o descartar el traslado.
3. Una vez finalizado el traslado, puede decidir si desea eliminar los recursos de la ubicación de origen.

Puede trasladar los recursos entre regiones en el centro de Resource Mover o desde un grupo de recursos. [Más información](select-move-tool.md)

## <a name="what-resources-can-i-move-across-regions"></a>¿Qué recursos puedo trasladar entre regiones?

Actualmente, con Resource Mover puede trasladar los siguientes recursos entre regiones:

- Máquinas virtuales de Azure y discos asociados
- NIC
- Conjuntos de disponibilidad 
- Redes virtuales de Azure 
- Direcciones IP públicas
- Grupos de seguridad de red (NSG):
- Equilibradores de carga internos y públicos 
- Bases de datos de Azure SQL y grupos de bases de datos elásticas


## <a name="next-steps"></a>Pasos siguientes

[Más información](about-move-process.md) sobre los componentes de Resource Mover y el proceso de traslado.
