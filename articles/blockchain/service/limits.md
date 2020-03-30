---
title: Límites de Azure Blockchain Service
description: Información general sobre los límites funcionales y de servicio en Azure Blockchain Service
ms.date: 11/22/2019
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: f4001ee520f3f3136d1bac5ca047c80526fc92e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455645"
---
# <a name="limits-in-azure-blockchain-service"></a>Límites en Azure Blockchain Service

Azure Blockchain Service tiene límites funcionales y de servicio, como el número de nodos que puede tener un miembro, restricciones de consorcio y cantidades de almacenamiento.

## <a name="pricing-tier"></a>Plan de tarifa

Los límites máximos de los nodos de validación y transacciones dependen de si aprovisiona Azure Blockchain Service en el plan de tarifa Básico o Estándar.

| Plan de tarifa | Cantidad máxima de nodos de transacción | Cantidad máxima de nodos de validación |
|:---|:---:|:---:|
| Básica | 10 | 1 |
| Estándar | 10 | 2 |

Después de la creación de un miembro, no se puede cambiar el plan de tarifa de Básico a Estándar.

## <a name="storage-capacity"></a>Capacidad de almacenamiento

La cantidad máxima de almacenamiento que se puede usar por nodo para los datos de libro de contabilidad y registros es de 1,8 terabyte.

El tamaño de almacenamiento del libro de contabilidad y del registro no se puede reducir.

## <a name="consortium-limits"></a>Límites del consorcio

* **Los nombres de miembro y de consorcio deben ser únicos** en Azure Blockchain Service.

* **Los nombres de miembro y de consorcio no se pueden cambiar**

* **Todos los miembros de un consorcio deben estar en el mismo plan de tarifa**

* **Todos los miembros que participan en un consorcio deben residir en la misma región**

    El primer miembro creado en un consorcio dicta la región. Los miembros invitados al consorcio deben residir en la misma región que el primer miembro. Limitar todos los miembros a la misma región ayuda a garantizar que el consenso de red no se ve afectado.

* **Un consorcio debe tener al menos un administrador**

    Si un consorcio solo tiene un administrador, este no se puede quitar del consorcio ni eliminar a su miembro hasta que se agregue otro administrador o se promocione en el consorcio.

* **Los miembros quitados del consorcio no se pueden volver a agregar**

    Tienen que recibir una nueva invitación para unirse al consorcio y crear un nuevo miembro. Los recursos de los miembros existentes no se eliminan para conservar el historial de transacciones.

* **Todos los miembros de un consorcio deben usar la misma versión de libro de contabilidad**

    Para obtener más información sobre la aplicación de revisiones, actualizaciones y versiones del libro de contabilidad disponibles en Azure Blockchain Service, vea [Aplicación de revisiones, actualizaciones y versiones](ledger-versions.md).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las directivas relacionadas con la revisión y las actualizaciones de sistemas: [Revisiones, actualizaciones y versiones](ledger-versions.md).
