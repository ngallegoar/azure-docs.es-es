---
title: Establecimiento de roles de Azure para el acceso administrativo de Azure
titleSuffix: Azure Cognitive Search
description: Control de acceso basado en rol (RBAC) de Azure Portal para controlar y delegar tareas administrativas para la administración de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: 2f9f979e5871a4888978ff14362a7fb0082917d5
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151189"
---
# <a name="set-azure-roles-for-administrative-access-to-azure-cognitive-search"></a>Configuración de roles de Azure para el acceso administrativo a Azure Cognitive Search

Azure proporciona un [modelo de autorización global basado en roles](../role-based-access-control/role-assignments-portal.md) para todos los servicios que se administran del portal o de las API de Resource Manager. Los roles de Propietario, Colaborador y Lector determinan el nivel de *administración de servicio* de los usuarios, grupos y entidades de seguridad de Active Directory asignados a cada rol. 

> [!Note]
> No hay control de acceso basado en rol (RBAC) para proteger el contenido del servicio. Usará una clave de API de administrador o una clave de API de consulta para las solicitudes autenticadas en el propio servicio. Para tener acceso basado en la identidad a través de los resultados de búsqueda, puede crear filtros de seguridad para recortar los resultados según la identidad y quitar los documentos para los que el solicitante no debe tener acceso. Para más información, consulte [Filtros de seguridad](search-security-trimming-for-azure-search.md).

## <a name="management-tasks-by-role"></a>Tareas de administración por rol

Para Azure Cognitive Search, los roles están asociados a los niveles de permisos que admiten las siguientes tareas de administración:

| Role | Tarea |
| --- | --- |
| Propietario |Crear o eliminar el servicio o cualquier objeto del servicio, entre los que se incluyen claves de API, índices, indexadores, orígenes de datos de indexadores y programas de indexadores.<p>Ver el estado del servicio, incluidos el tamaño de almacenamiento y los recuentos.<p>Agregar o eliminar miembros del rol (solo un propietario puede administrar la pertenencia a roles).<p>Los administradores de suscripciones y los propietarios de servicios tienen pertenencia automática al rol Propietario. |
| Colaborador | El mismo nivel de acceso que el rol de Propietario, menos la administración de roles de Azure. Por ejemplo, un usuario con el rol Colaborador puede crear o eliminar objetos, o ver y regenerar [claves de API](search-security-api-keys.md), pero no puede modificar pertenencias a roles.<br><br>[Colaborador del servicio Search](../role-based-access-control/built-in-roles.md#search-service-contributor) equivale al rol integrado Colaborador genérico. |
| Lector |Vea elementos esenciales del servicio, como el punto de conexión del servicio, la suscripción, el grupo de recursos, la región, el nivel y la capacidad. También puede ver las métricas del servicio, como el promedio de consultas por segundo, en la pestaña Supervisión. Los miembros de este rol no pueden ver información del índice, indexador, origen de datos o conjunto de aptitudes. Esto incluye los datos de uso de esos objetos, como el número de índices que hay en el servicio. |

Los roles no otorgan derechos de acceso al punto de conexión de servicio. Las operaciones del servicio Search, como la administración de índices, el rellenado del índice y las consultas en datos de búsqueda, se controlan mediante claves de API, no a través de roles. Para más información, consulte [Administración de claves de API](search-security-api-keys.md).

## <a name="permissions-table"></a>Tabla de permisos

En la tabla siguiente se resumen las operaciones permitidas en Azure Cognitive Search y la clave que desbloquea el acceso a una operación determinada.

Los permisos de RBAC se aplican a las operaciones del portal y a la administración de servicios (crear, eliminar o cambiar un servicio o sus claves de API). Las claves de API se crean después de que un servicio exista y se aplican a las operaciones de contenido del servicio. Además, para las operaciones relacionadas con contenido del portal, como la creación o eliminación de objetos, los usuarios con los roles Propietario o Colaborador de RBAC interactúan con el servicio con una clave de API de administrador.

| Operación | Controlado por |
|-----------|-------------------------|
| Crear un servicio | Permisos de RBAC: Propietario o Colaborador |
| Escalar un servicio | Permisos de RBAC: Propietario o Colaborador|
| Eliminar un servicio | Permisos de RBAC: Propietario o Colaborador |
| Administrar claves de administrador o de consulta | Permisos de RBAC: Propietario o Colaborador|
| Ver la información del servicio en el portal o en una API de administración | Permisos de RBAC: Propietario, Colaborador o Lector  |
| Ver métricas e información del objeto en el portal o en una API de administración | Permisos de RBAC: Propietario o Colaborador |
| Crear, modificar y eliminar objetos en el servicio: <br>Índices y partes de componentes (incluidas las definiciones del analizador, los perfiles de puntuación y las opciones de CORS), indexadores, orígenes de datos, sinónimos, proveedores de sugerencias | Clave de administrador si se usa una API, roles Propietario o Colaborador de RBAC si se usa el portal |
| Consultar un índice | Clave de administrador o de consulta si se usa una API, roles Propietario o Colaborador de RBAC si se usa el portal |
| Consultar la información del sistema sobre los objetos, como devolver estadísticas, recuentos y listas de objetos | Clave de administrador si se usa una API, roles Propietario o Colaborador de RBAC si se usa el portal |

## <a name="next-steps"></a>Pasos siguientes

+ [Administración mediante PowerShell](search-manage-powershell.md) 
+ [Consideraciones sobre el rendimiento y la optimización de Azure Cognitive Search](search-performance-optimization.md)
+ [Introducción al control de acceso basado en roles en Azure Portal](../role-based-access-control/overview.md)
