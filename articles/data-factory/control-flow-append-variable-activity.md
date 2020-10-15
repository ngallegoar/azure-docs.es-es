---
title: Actividad de anexión de variables en Azure Data Factory
description: Aprenda a configurar la actividad de anexión de variables para agregar un valor a una variable de matriz existente definida en una canalización de Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.date: 10/09/2018
ms.openlocfilehash: 5d74cd0fcd524f00d79eb3fbab386c602a413766
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81414207"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Actividad de anexión de variables en Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
Use la actividad de anexión de variables para agregar un valor a una variable de matriz existente definida en una canalización de Data Factory.

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | Descripción | Obligatorio
-------- | ----------- | --------
name | Nombre de la actividad en la canalización | Sí
description | Texto que describe para qué se usa la actividad | no
type | Tipo de actividad es AppendVariable | sí
value | Cadena literal o valor del objeto de expresión usado para anexar a la variable especificada | sí
variableName | Nombre de la variable que se va a modificar mediante la actividad; la variable debe ser de tipo 'Array' | sí

## <a name="next-steps"></a>Pasos siguientes
Obtenga información acerca de una actividad de flujo de control relacionada compatible con Data Factory: 

- [Establecimiento de la actividad de variable](control-flow-set-variable-activity.md)
