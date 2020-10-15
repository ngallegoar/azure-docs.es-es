---
title: Control de acceso a libros de Azure Monitor
description: Simplifique la creación de informes complejos con libros parametrizados creados previamente y personalizados con control de acceso basado en roles
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 9c0b5dd72cf82649e7f659d23606d23aea62ac82
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932556"
---
# <a name="access-control"></a>Control de acceso

El control de acceso en los libros hace referencia a dos cosas:

* Acceso necesario para leer los datos de un libro. Este acceso está controlado por los [roles estándar de Azure](../../role-based-access-control/overview.md) en los recursos usados en el libro. Los libros no especifican ni configuran el acceso a esos recursos. Normalmente, los usuarios obtendrán este acceso a esos recursos mediante el rol de [Lector de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-reader) en esos recursos.

* Acceso necesario para guardar libros

    - Guardar los libros `("My")` privados no requiere ningún privilegio adicional. Todos los usuarios pueden guardar libros privados y solo ellos pueden verlos.
    - Guardar los libros compartidos requiere privilegios de escritura en un grupo de recursos para guardar el libro. Estos privilegios se especifican normalmente mediante el rol de [Colaborador de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-contributor), pero también se pueden establecer mediante el rol *Colaborador de libros*.
    
## <a name="standard-roles-with-workbook-related-privileges"></a>Roles estándar con privilegios relacionados con el libro

El [lector de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-reader) incluye privilegios estándar/de lectura que se utilizarían en las herramientas de supervisión (incluidos los libros) para leer los datos de los recursos.

El [colaborador de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-contributor) incluye los privilegios de `/write` generales utilizados por diversas herramientas de supervisión para guardar elementos (incluido el privilegio `workbooks/write` para guardar libros compartidos).
"Colaborador de libros" agrega privilegios de "libros y escritura" a un objeto para guardar libros compartidos.
No se requiere ningún privilegio especial para que los usuarios guarden libros privados que solo ellos pueden ver.

Para un control de acceso basado en roles personalizado:

Agregue `microsoft.insights/workbooks/write` para guardar los libros compartidos. Para obtener más información, vea el rol [Colaborador de libro](../../role-based-access-control/built-in-roles.md#monitoring-contributor).

## <a name="next-steps"></a>Pasos siguientes

* [Comience](workbooks-visualizations.md) a aprender más sobre las muchas opciones de visualizaciones enriquecidas de los libros.
