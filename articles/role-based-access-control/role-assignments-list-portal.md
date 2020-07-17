---
title: 'Enumeración de asignaciones de roles de Azure mediante Azure Portal: RBAC de Azure'
description: Obtenga información sobre cómo determinar a qué recursos tienen acceso los usuarios, grupos, entidades de servicio e identidades administradas mediante Azure Portal y el control de acceso basado en roles (RBAC) de Azure (RBAC de Azure).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 06/24/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f7d56ecc7fc6bd850fced33c2c1cf20902bb2df4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85361888"
---
# <a name="list-azure-role-assignments-using-the-azure-portal"></a>Enumeración de asignaciones de roles de Azure mediante Azure Portal

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] En este artículo se describe cómo enumerar las asignaciones de roles mediante Azure Portal.

> [!NOTE]
> Si su organización ha externalizado las funciones de administración a un proveedor de servicios que usa la [administración de recursos delegados de Azure](../lighthouse/concepts/azure-delegated-resource-management.md), las asignaciones de roles autorizadas por ese proveedor de servicios no se mostrarán aquí.

## <a name="list-role-assignments-for-a-user-or-group"></a>Lista de las asignaciones de rol de un usuario o grupo

La forma más fácil de ver los roles asignados a un usuario o grupo de una suscripción es usar el panel **Asignaciones de roles de Azure**.

1. En el menú de Azure Portal, seleccione **Todos los servicios**.

1. Seleccione **Azure Active Directory** y después seleccione **Usuarios** o **Grupos**.

1. Haga clic en el usuario o grupo del que desea enumerar las asignaciones de roles.

1. Haga clic en **Asignaciones de roles de Azure**.

    Verá una lista de los roles asignados al usuario o grupo seleccionado en varios ámbitos como los de grupo de administración, suscripción, grupo de recursos o recurso. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles de un usuario](./media/role-assignments-list-portal/azure-role-assignments-user.png)    

1. Para cambiar la suscripción, haga clic en la lista **Suscripciones**.

## <a name="list-owners-of-a-subscription"></a>Enumeración de los propietarios de una suscripción

Los usuarios que se han asignado al rol de [propietario](built-in-roles.md#owner) para una suscripción pueden administrar todo en esta. Siga estos pasos para mostrar los propietarios de una suscripción.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción de la que quiera mostrar los propietarios.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. Desplácese hasta la sección **Propietarios** para ver todos los usuarios a los que se ha asignado el rol de propietario para esta suscripción.

   ![Control de acceso a la suscripción: pestaña Asignaciones de roles](./media/role-assignments-list-portal/access-control-role-assignments-subscription.png)

## <a name="list-role-assignments-at-a-scope"></a>Lista de las asignaciones de roles en un ámbito

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

   ![Control de acceso: pestaña Asignaciones de roles](./media/role-assignments-list-portal/access-control-role-assignments.png)

   En la pestaña Asignaciones de roles puede ver quién tiene acceso a este ámbito. Observe que el ámbito de algunos roles es **este recurso**, mientras que el de otros es **(heredado)**  de otro ámbito. El acceso se asigna específicamente a este recurso o se hereda de una asignación en el ámbito principal.

## <a name="list-role-assignments-for-a-user-at-a-scope"></a>Lista de las asignaciones de rol de un usuario en un ámbito

Para mostrar el acceso de un usuario, grupo, entidad de servicio o identidad administra, puede mostrar sus asignaciones de roles. Siga estos pasos para mostrar las asignaciones de roles de un solo usuario, grupo, entidad de servicio o identidad administrada en un ámbito determinado.

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Comprobar acceso**.

    ![Control de acceso: pestaña Comprobar acceso](./media/role-assignments-list-portal/access-control-check-access.png)

1. En la lista **Buscar**, seleccione el tipo de entidad de seguridad cuyo acceso quiere comprobar.

1. En el cuadro de búsqueda, escriba una cadena para buscar nombres para mostrar, direcciones de correo electrónico o identificadores de objeto en el directorio.

    ![Lista de selección de Comprobar acceso](./media/role-assignments-list-portal/check-access-select.png)

1. Haga clic en la entidad de seguridad para abrir el panel **Asignaciones**.

    ![Panel de asignaciones](./media/role-assignments-list-portal/check-access-assignments.png)

    En este panel puede ver los roles asignados a la entidad de seguridad seleccionada y el ámbito. Si hay asignaciones denegadas en este ámbito o heredadas en este ámbito, se mostrarán.

## <a name="list-role-assignments-for-a-managed-identity"></a>Lista de asignaciones de roles para una identidad administrada

Puede mostrar las asignaciones de roles para las identidades administradas asignadas por el usuario y por el sistema en un ámbito determinado mediante la hoja **Control de acceso (IAM)** como se describió anteriormente. En esta sección se describe cómo mostrar asignaciones de roles solo para la identidad administrada.

### <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

1. En Azure Portal, abra una identidad administrada asignada por el sistema.

1. En el menú izquierdo, haga clic en **Identidad**.

    ![Identidad administrada asignada por el sistema](./media/shared/identity-system-assigned.png)

1. En **Permisos**, haga clic en **Asignaciones de roles de Azure**.

    Verá una lista de los roles asignados a la identidad administrada asignada por el sistema seleccionada en varios ámbitos como los de grupo de administración, suscripción, grupo de recursos o recurso. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el sistema](./media/shared/role-assignments-system-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripción**.

### <a name="user-assigned-managed-identity"></a>Identidad administrada asignada por el usuario

1. En Azure Portal, abra una identidad administrada asignada por el usuario.

1. Haga clic en **Asignaciones de roles de Azure**.

    Verá una lista de los roles asignados a la identidad administrada asignada por el usuario seleccionada en varios ámbitos como los de grupo de administración, suscripción, grupo de recursos o recurso. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el sistema](./media/shared/role-assignments-user-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripción**.

## <a name="list-number-of-role-assignments"></a>Enumeración del número de asignaciones de roles

Puede tener hasta **2000** asignaciones de roles en cada suscripción. Este límite incluye las asignaciones de roles en los ámbitos de suscripción, grupo de recursos y recurso. Para ayudarle a realizar un seguimiento de este límite, la pestaña **Asignaciones de roles** incluye un gráfico que muestra el número de asignaciones de roles de la suscripción actual.

![Cuadro de control de acceso: número de asignaciones de roles](./media/role-assignments-list-portal/access-control-role-assignments-chart.png)

Si está cerca del número máximo e intenta agregar más asignaciones de roles, verá una advertencia en el panel **Agregar asignación de roles**. Para saber cómo puede reducir el número de asignaciones de roles, vea [Solución de problemas de RBAC de Azure](troubleshooting.md#azure-role-assignments-limit).

![Advertencia de control de acceso - agregar asignación de roles](./media/role-assignments-list-portal/add-role-assignment-warning.png)

## <a name="download-role-assignments-preview"></a>Descarga de asignaciones de roles (versión preliminar)

Puede descargar las asignaciones de roles en un ámbito en formato CSV o JSON. Esto puede ser útil si necesita inspeccionar la lista en una hoja de cálculo o realizar un inventario al efectuar una suscripción.

> [!IMPORTANT]
> La descarga de las asignaciones de roles se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Al descargar asignaciones de roles, debe tener en cuenta los siguientes criterios:

- Si no tiene permisos para leer el directorio, como el rol de lectores de directorio, las columnas DisplayName, SignInName y ObjectType estarán en blanco.
- No se incluyen las asignaciones de roles cuya entidad de seguridad se haya eliminado.
- No se incluye el acceso concedido a los administradores clásicos.

Siga estos pasos para descargar asignaciones de roles en un ámbito.

1. En Azure Portal, haga clic en **Todos los servicios** y, a continuación, seleccione el ámbito del que quiere descargar las asignaciones de roles. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en **Descargar asignaciones de roles (versión preliminar)** para abrir el panel Descargar asignaciones de roles.

    ![Control de acceso: descargar asignaciones de roles](./media/role-assignments-list-portal/download-role-assignments.png)

1. Use las casillas para seleccionar las asignaciones de roles que quiere incluir en el archivo descargado.

    - **Heredadas**: incluye asignaciones de roles heredadas para el ámbito actual.
    - **En el ámbito actual**: incluye asignaciones de roles para el ámbito actual.
    - **Elementos secundarios**: incluye asignaciones de roles en niveles inferiores al ámbito actual. Esta casilla está deshabilitada para el ámbito del grupo de administración.

1. Seleccione el formato de archivo, que puede ser con valores separados por comas (CSV) o notación de objetos JavaScript (JSON).

1. Especifique el nombre de archivo.

1. Haga clic en **Iniciar** para iniciar la descarga.

    A continuación se muestran ejemplos de la salida para cada formato de archivo.

    ![Descargar asignaciones de roles como un CSV](./media/role-assignments-list-portal/download-role-assignments-csv.png)

    ![Descargar asignaciones de roles como un CSV](./media/role-assignments-list-portal/download-role-assignments-json.png)

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal](role-assignments-portal.md)
- [Solución de problemas de Azure RBAC](troubleshooting.md)
