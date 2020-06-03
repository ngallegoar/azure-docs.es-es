---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 926434d7110877e234888682cb6c946afe3ae685
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648963"
---
Functions permite usar claves para dificultar el acceso a los puntos de conexión de función HTTP durante el desarrollo. A menos que el nivel de acceso HTTP en una función desencadenada por HTTP se establezca en `anonymous`, las solicitudes deben incluir una clave de acceso de API. 

#### <a name="authorization-scopes-function-level"></a>Ámbitos de autorización (nivel de función)

Hay dos ámbitos de acceso para las claves de nivel de función:

* **Función**: estas claves se aplican solo a las funciones específicas en las que se definen. Cuando se usan como una clave de API, solo permiten el acceso a esa función.

* **Host**: las claves con un ámbito de host se pueden usar para acceder a todas las funciones dentro de la aplicación de función. Cuando se usan como una clave de API, permiten el acceso a cualquier función dentro de la aplicación de función. 

Para cada clave se usa un nombre fácilmente referenciable y hay una clave predeterminada (denominada "predeterminada") en el nivel de función y host. Las claves de función tienen prioridad sobre las claves de host. Si se definen dos claves con el mismo nombre, siempre se usa la clave de función.

#### <a name="master-key-admin-level"></a>Clave maestra (nivel de administrador) 

Cada aplicación de función también tiene una clave de host de nivel de administrador denominada `_master`. Además de proporcionar acceso de nivel de host a todas las funciones de la aplicación, la clave maestra también proporciona acceso administrativo a las API REST del entorno de ejecución. No se puede revocar esta clave. Al establecer un nivel de acceso `admin`, las solicitudes deben usar la clave maestra; cualquier otra clave da lugar a un error de acceso.

> [!CAUTION]  
> Debido a los permisos elevados de la aplicación de función otorgados por la clave maestra, no debe compartir esta clave con terceros ni distribuirla en aplicaciones cliente nativas. Tenga cuidado al elegir el nivel de acceso de administrador.
