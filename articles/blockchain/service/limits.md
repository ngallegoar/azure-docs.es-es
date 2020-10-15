---
title: Límites de Azure Blockchain Service
description: Información general sobre los límites funcionales y de servicio en Azure Blockchain Service
ms.date: 04/02/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: 71e1bebf10fa0142870d03977182472da1ad031f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80676517"
---
# <a name="limits-in-azure-blockchain-service"></a>Límites en Azure Blockchain Service

Azure Blockchain Service tiene límites funcionales y de servicio, como el número de nodos que puede tener un miembro, restricciones de consorcio y cantidades de almacenamiento.

## <a name="pricing-tier"></a>Plan de tarifa

Los límites máximos de los nodos de validación y transacciones dependen de si aprovisiona Azure Blockchain Service en el plan de tarifa básico o estándar.

| Plan de tarifa | Cantidad máxima de nodos de transacción | Cantidad máxima de nodos de validación |
|:---|:---:|:---:|
| Básica | 10 | 1 |
| Estándar | 10 | 2 |

Su red de consorcio debe tener un mínimo de dos nodos de nivel estándar de Azure Blockchain Service. Los nodos de nivel estándar incluyen dos nodos de validación. Se requieren cuatro nodos de validación para cumplir con el [consenso Istanbul Byzantine Fault Tolerance](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus).

Use el nivel básico para desarrollo, pruebas y pruebas de concepto. Use el nivel estándar para las implementaciones de nivel de producción. También debe usar el nivel *Estándar* si va a utilizar Blockchain Data Manager o va a enviar un gran volumen de transacciones privadas.

Después de la creación de un miembro, no se puede cambiar el plan de tarifa entre básico y estándar.

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

## <a name="performance"></a>Rendimiento

No utilice la función de gas *eth.estimate* para cada envío de transacción. La función *eth.estimate* consume mucha memoria. Al llamar varias veces a la función, se reducen drásticamente las transacciones por segundo.

Si es posible, use un valor de gas conservador para el envío de transacciones y minimice el uso de *eth.estimate*.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las directivas relacionadas con la revisión y las actualizaciones de sistemas: [Revisiones, actualizaciones y versiones](ledger-versions.md).
