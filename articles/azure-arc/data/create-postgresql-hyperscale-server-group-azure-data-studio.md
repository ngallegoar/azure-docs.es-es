---
title: Creación de Hiperescala de PostgreSQL habilitada para Azure Arc mediante Azure Data Studio
description: Creación de Hiperescala de PostgreSQL habilitada para Azure Arc mediante Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: e2007d8f0c558d35c0507b6e12bce6d6777fad52
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92310898"
---
# <a name="create-azure-arc-enabled-postgresql-hyperscale-using-azure-data-studio"></a>Creación de Hiperescala de PostgreSQL habilitada para Azure Arc mediante Azure Data Studio

En este documento se le guía por los pasos para usar Azure Data Studio a fin de aprovisionar grupos de servidores Hiperescala de PostgreSQL habilitados para Azure Arc.

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connect-to-the-azure-arc-data-controller"></a>Conexión al controlador de datos de Azure Arc

Antes de poder crear una instancia, inicie sesión en el controlador de datos de Azure Arc si todavía no lo ha hecho.

```console
azdata login
```

Después, se le solicitará el espacio de nombres en el que se ha creado el controlador de datos, el nombre de usuario y la contraseña para iniciar sesión en el controlador.

> Si tiene que validar el espacio de nombres, puede ejecutar ```kubectl get pods -A``` para obtener una lista de todos los espacios de nombres del clúster.

```console
Username: arcadmin
Password:
Namespace: arc
Logged in successfully to `https://10.0.0.4:30080` in namespace `arc`. Setting active context to `arc`
```

## <a name="preliminary-and-temporary-step-for-openshift-users-only"></a>Paso preliminar y temporal solo para usuarios de OpenShift

Implemente este paso antes de pasar al siguiente. Para implementar el grupo de servidores Hiperescala de PostgreSQL en Red Hat OpenShift en un proyecto distinto del predeterminado, debe ejecutar los comandos siguientes en el clúster para actualizar las restricciones de seguridad. Este comando concede los privilegios necesarios a las cuentas de servicio que ejecutarán el grupo de servidores Hiperescala de PostgreSQL. La restricción de contexto de seguridad (SCC) **_arc-data-scc_** es la que ha agregado al implementar el controlador de datos de Azure Arc.

```console
oc adm policy add-scc-to-user arc-data-scc -z <server-group-name> -n <namespace name>
```

_**Server-group-name** es el nombre del grupo de servidores que se va a implementar en el siguiente paso._
   
Para obtener más detalles sobre las SCC en OpenShift, consulte la [documentación de OpenShift](https://docs.openshift.com/container-platform/4.2/authentication/managing-security-context-constraints.html).
Ahora puede implementar el paso siguiente.

## <a name="create-an-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Creación de un grupo de servidores Hiperescala de PostgreSQL habilitado para Azure Arc

1. Inicio de Azure Data Studio
1. En la pestaña Conexiones, haga clic en los tres puntos de la parte superior izquierda y elija "Nueva implementación".
1. En las opciones de implementación, seleccione **Grupo de servidores Hiperescala de PostgreSQL: Azure Arc**
    >[!NOTE]
    > Es posible que se le pida que instale [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] si no lo está actualmente.
1. Acepte los términos de privacidad y de licencia, y haga clic en **Seleccionar** en la parte inferior.
1. En la hoja Implementar grupo de servidores Hiperescala de PostgreSQL: Azure Arc, escriba la información siguiente:
   - Escriba un nombre para el grupo de servidores
   - Escriba y confirme una contraseña para el usuario administrador _postgres_ del grupo de servidores
   - Seleccione la clase de almacenamiento adecuada para los datos
   - Seleccione la clase de almacenamiento adecuada para los registros
   - Seleccione la clase de almacenamiento adecuada para las copias de seguridad
   - Seleccione el número de nodos de trabajo que quiere aprovisionar
1. Haga clic en el botón **Implementar**

Esto inicia la creación del grupo de servidores Hiperescala de PostgreSQL habilitado para Azure Arc en el controlador de datos.

Tras unos minutos, la creación se habrá completado de forma correcta.

## <a name="next-steps"></a>Pasos siguientes
- [Administración del grupo de servidores mediante Azure Data Studio](manage-postgresql-hyperscale-server-group-with-azure-data-studio.md)
- [Supervisión del grupo de servidores](monitor-grafana-kibana.md)
- Lea los conceptos y las guías paso a paso de Hiperescala de Azure Database for PostgreSQL para distribuir los datos entre varios nodos de Hiperescala de PostgreSQL y poder beneficiarse de toda la eficacia de esta opción. :
    * [Nodos y tablas](../../postgresql/concepts-hyperscale-nodes.md)
    * [Determinar el tipo de aplicación](../../postgresql/concepts-hyperscale-app-type.md)
    * [Elección de una columna de distribución](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Colocación de tabla](../../postgresql/concepts-hyperscale-colocation.md)
    * [Distribución y modificación de tablas](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Diseño de una base de datos multiinquilino](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Diseño de un panel de análisis en tiempo real](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

    > \* En los documentos anteriores, omita las secciones **Inicio de sesión en Azure Portal** y **Creación de una instancia de Azure Database for PostgreSQL: Hiperescala (Citus)** . Implemente los pasos restantes en la implementación de Azure Arc. Esas secciones son específicas de Hiperescala (Citus) de Azure Database for PostgreSQL que se ofrece como un servicio PaaS en la nube de Azure, pero las demás partes de los documentos se aplican directamente a Hiperescala de PostgreSQL habilitada para Azure Arc.

- [Escalado del grupo de servidores Hiperescala de Azure Database for PostgreSQL](scale-out-postgresql-hyperscale-server-group.md)
- [Conceptos de almacenamiento de Kubernetes y configuración de almacenamiento](storage-configuration.md)
- [Modelo de recursos de Kubernetes](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)

