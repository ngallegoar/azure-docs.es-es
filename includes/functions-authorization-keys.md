---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 617608f382d3331e59ae92c9eb272347c736b768
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91828935"
---
Functions permite usar claves para dificultar el acceso a los puntos de conexión de función HTTP durante el desarrollo. A menos que el nivel de acceso HTTP en una función desencadenada por HTTP se establezca en `anonymous`, las solicitudes deben incluir una clave de acceso de API. 

Aunque las claves proporcionan un mecanismo de seguridad predeterminado, podría plantearse la posibilidad de opciones adicionales para proteger un punto de conexión HTTP en producción. Por ejemplo, por lo general, no es recomendable distribuir un secreto compartido en las aplicaciones públicas. Si se llama a la función desde un cliente público, puede que sea conveniente implementar otro mecanismo de seguridad. Para obtener más información, vea [Proteger un punto de conexión HTTP en producción](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#secure-an-http-endpoint-in-production).

Al renovar los valores de la clave de función, debe redistribuir manualmente los valores de clave actualizados a todos los clientes que llaman a la función.  

#### <a name="authorization-scopes-function-level"></a>Ámbitos de autorización (nivel de función)

Hay dos ámbitos de acceso para las claves de nivel de función:

* **Función**: estas claves se aplican solo a las funciones específicas en las que se definen. Cuando se usan como una clave de API, solo permiten el acceso a esa función.

* **Host**: las claves con un ámbito de host se pueden usar para acceder a todas las funciones dentro de la aplicación de función. Cuando se usan como una clave de API, permiten el acceso a cualquier función dentro de la aplicación de función. 

Para cada clave se usa un nombre fácilmente referenciable y hay una clave predeterminada (denominada "predeterminada") en el nivel de función y host. Las claves de función tienen prioridad sobre las claves de host. Si se definen dos claves con el mismo nombre, siempre se usa la clave de función.

#### <a name="master-key-admin-level"></a>Clave maestra (nivel de administrador) 

Cada aplicación de función también tiene una clave de host de nivel de administrador denominada `_master`. Además de proporcionar acceso de nivel de host a todas las funciones de la aplicación, la clave maestra también proporciona acceso administrativo a las API REST del entorno de ejecución. No se puede revocar esta clave. Al establecer un nivel de acceso `admin`, las solicitudes deben usar la clave maestra; cualquier otra clave da lugar a un error de acceso.

> [!CAUTION]  
> Debido a los permisos elevados de la aplicación de función otorgados por la clave maestra, no debe compartir esta clave con terceros ni distribuirla en aplicaciones cliente nativas. Tenga cuidado al elegir el nivel de acceso de administrador.
