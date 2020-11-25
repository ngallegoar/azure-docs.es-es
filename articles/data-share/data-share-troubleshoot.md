---
title: Solución de problemas de Azure Data Share
description: Obtenga información sobre cómo solucionar problemas con invitaciones y errores al crear o recibir recursos compartidos de datos con Azure Data Share.
services: data-share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: troubleshooting
ms.date: 10/15/2020
ms.openlocfilehash: e29c640494a18bb3be2125a5b53b4f943521fe6c
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94579154"
---
# <a name="troubleshoot-common-issues-in-azure-data-share"></a>Solución de problemas habituales de Azure Data Share 

En este artículo se muestra cómo solucionar problemas habituales de Azure Data Share. 

## <a name="azure-data-share-invitations"></a>Invitaciones de Azure Data Share 

En algunos casos, si un usuario nuevo hace clic en **Accept Invitation** (Aceptar invitación) en la invitación de correo electrónico que se envió, pueden aparecer una lista vacía de invitaciones. 

![No hay invitaciones](media/no-invites.png)

Esto puede deberse a los siguientes motivos:

* **El servicio Azure Data Share no está registrado como proveedor de recursos de ninguna suscripción de Azure en el inquilino de Azure.** Tendrá este problema si no hay ningún recurso compartido de datos en su inquilino de Azure. Cuando se crea un recurso de Azure Data Share, se registra automáticamente el proveedor de recursos en la suscripción de Azure. También puede registrar manualmente el servicio Data Share si sigue estos pasos. Para completar estos pasos, debe tener el rol Colaborador de Azure.

    1. En Azure Portal, vaya a **Suscripciones**
    1. Seleccione la suscripción de Azure que quiere usar para crear un recurso de Azure Data Share
    1. Haga clic en **Proveedores de recursos**
    1. Busque **Microsoft.DataShare**
    1. Haga clic en **Registrar**. 

    Para completar estos pasos, debe tener el [rol Colaborador de Azure](../role-based-access-control/built-in-roles.md#contributor) para la suscripción de Azure. 

* **La invitación se envía a su alias de correo electrónico en lugar de a su correo electrónico de inicio de sesión de Azure.** Si ha registrado el servicio de Azure Data Share o ya ha creado un recurso compartido de datos en el inquilino de Azure, pero sigue sin poder ver la invitación, es posible que el proveedor haya escrito su alias de correo electrónico como destinatario en lugar de su dirección de correo electrónico de inicio de sesión de Azure. Póngase en contacto con el proveedor de datos y asegúrese de que ha enviado la invitación a su dirección de correo electrónico de inicio de sesión de Azure y no a su alias de correo electrónico.

* **Ya se aceptó la invitación.** El vínculo del correo electrónico le lleva a la página Invitación de recurso compartido de datos en Azure Portal, que solo muestra las invitaciones pendientes. Si ha aceptado la invitación, ya no aparecerá en la página Invitación de recurso compartido de datos. Continúe con el recurso compartido de datos que usó para aceptar la invitación para ver los recursos compartidos recibidos y configurar la opción del clúster de Azure Data Explorer.

## <a name="error-when-creating-or-receiving-a-new-share"></a>Error al crear o recibir un nuevo recurso compartido

"No se pudo agregar conjuntos de datos"

"No se pudo asignar conjuntos de datos"

"No se puede conceder acceso de x para x al recurso de Data Share"

"No tiene los permisos apropiados para x"

"No hemos podido agregar permisos de escritura para la cuenta de Azure Data Share en uno o más de los recursos seleccionados"

Si recibe un error similar a los anteriores al crear un nuevo recurso compartido, agregar conjuntos de datos o asignar conjuntos de datos, podría deberse a la falta de permisos en el almacén de datos de Azure. Consulte [Roles y requisitos](concepts-roles-permissions.md) para obtener los permisos necesarios. 

Necesita permiso de escritura para compartir o recibir datos de un almacén de datos de Azure, que normalmente existe en el rol **Colaborador**. 

Si esta es la primera vez que va a compartir o recibir datos del almacén de datos de Azure, también necesita el permiso *Microsoft.Authorization/role assignments/write*, que normalmente existe en el rol **Propietario**. Aunque haya creado el recurso del almacén de datos de Azure, ello NO le convierte automáticamente el propietario del recurso. Con el permiso adecuado, el servicio de Azure Data Share concede automáticamente a la identidad administrada del recurso compartido de datos acceso al almacén de datos. Ese proceso podría tardar unos minutos en surtir efecto. Si experimenta un error debido a este retraso, vuelva a intentarlo en unos minutos.

El uso compartido basado en SQL requiere permisos adicionales. Vea [Compartir desde orígenes de SQL](how-to-share-from-sql.md) para obtener una lista detallada de los requisitos previos.

## <a name="snapshot-failed"></a>La instantánea generó un error
La instantánea podría generar un error debido a diversos motivos. Puede encontrar un mensaje de error detallado si hace clic en la hora de inicio de la instantánea y, a continuación, en el estado de cada conjunto de datos. Estos son los motivos comunes por los que se produce un error en la instantánea:

* Data Share no tiene permiso para leer desde el almacén de datos de origen ni para escribir en el almacén de datos de destino. Vea [Roles y requisitos](concepts-roles-permissions.md) para conocer los requisitos de permisos detallados. Si es la primera vez que toma una instantánea, podría llevar unos minutos conceder al recurso compartido de datos el acceso al almacén de datos de Azure. Espere y vuelva a intentarlo.
* El firewall bloquea la conexión de Data Share al almacén de datos de origen o de destino.
* Se elimina el conjunto de datos compartido o el almacén de datos de origen o de destino.

Otras causas de errores de instantánea en el caso de los orígenes SQL son las siguientes. 

* El script SQL de origen o destino para conceder permiso para Data Share no se ejecuta. O bien para Azure SQL Database o Azure Synapse Analytics (anteriormente Azure SQL DW), se ejecuta mediante la autenticación de SQL en lugar de la autenticación de Azure Active Directory.  
* El almacén de datos SQL de origen o de destino está en pausa.
* Los tipos de datos SQL no son compatibles con el proceso de instantánea o el almacén de datos de destino. Vea [Compartir desde orígenes de SQL](how-to-share-from-sql.md#supported-data-types) para obtener más información.
* El almacén de datos SQL de origen o de destino está bloqueado por otros procesos. Azure Data Share no aplica bloqueos al almacén de datos SQL de origen y de destino. Sin embargo, los bloqueos existentes en el almacén de datos SQL de origen y de destino provocarán errores de instantánea.
* Una restricción de clave externa hace referencia a la tabla SQL de destino. Durante la instantánea, si existe una tabla de destino con el mismo nombre, Azure Data Share descarta la tabla y crea una nueva tabla. Si una restricción de clave externa hace referencia a la tabla SQL de destino, la taba no se puede descartar.
* Se genera el archivo CSV de destino, pero los datos no se pueden leer en Excel. Esto puede ocurrir cuando la tabla SQL de origen contiene datos con caracteres que no son ingleses. En Excel, seleccione la pestaña "Obtener datos" y elija el archivo CSV, seleccione el origen del archivo como 65001: Unicode (UTF-8) y cargue los datos.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md). 

Para información sobre cómo recibir los datos, continúe con el tutorial [Aceptación y recepción de datos con Azure Data Share](subscribe-to-data-share.md).