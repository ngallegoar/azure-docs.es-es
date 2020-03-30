---
title: 'Conceptos: Acceso e identidad en Azure Kubernetes Service (AKS)'
description: Este artículo contiene más información sobre el acceso y la identidad en Azure Kubernetes Service (AKS), incluida la integración de Azure Active Directory, el control de acceso basado en rol de Kubernetes y los roles y enlaces.
services: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.openlocfilehash: e4945535417f7d8d33308121267ba97e1f835e13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229664"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Opciones de acceso e identidad en Azure Kubernetes Service (AKS)

Hay diferentes maneras de autenticarse y proteger los clústeres de Kubernetes. Con el control de acceso basado en rol (RBAC), puede conceder a los usuarios o grupos acceso solo a los recursos que necesitan. Con Azure Kubernetes Service (AKS), puede mejorar aún más la seguridad y la estructura de permisos mediante el uso de Azure Active Directory. Estos enfoques le ayudan a proteger las cargas de trabajo de sus aplicaciones y los datos del cliente.

En este artículo se presentan los conceptos básicos para ayudarle a autenticarse y a asignar permisos en AKS:

- [Cuentas de servicio de Kubernetes](#kubernetes-service-accounts)
- [Integración de Azure Active Directory](#azure-active-directory-integration)
- [Controles de acceso basado en rol (RBAC)](#role-based-access-controls-rbac)
- [Roles y ClusterRoles](#roles-and-clusterroles)
- [RoleBindings y ClusterRoleBindings](#rolebindings-and-clusterrolebindings)

## <a name="kubernetes-service-accounts"></a>Cuentas de servicio de Kubernetes

Uno de los tipos de usuario principales de Kubernetes es una *cuenta de servicio*. Una cuenta de servicio existe en la API de Kubernetes y está administrada por esta. Las credenciales de las cuentas de servicio se almacenan como secretos de Kubernetes que usan los pods autorizados para comunicarse con el servidor de API. La mayoría de las solicitudes de API proporcionan un token de autenticación para una cuenta de servicio o una cuenta de usuario normal.

Las cuentas de usuario normales permiten un acceso más tradicional para administradores o desarrolladores humanos, no solo a servicios y procesos. Kubernetes por sí solo no proporciona una solución de administración de identidades donde se almacenan contraseñas y cuentas de usuario normales. En su lugar, se pueden integrar soluciones de identidad externas en Kubernetes. Para los clústeres de AKS, esta solución de identidades integrada es Azure Active Directory.

Para más información sobre las opciones de identidad en Kubernetes, consulte la [autenticación Kubernetes][kubernetes-authentication].

## <a name="azure-active-directory-integration"></a>Integración de Azure Active Directory

Puede mejorar la seguridad de los clústeres de AKS con la integración de Azure Active Directory (AD). Compilada después de varias décadas de administración de identidades empresariales, Azure AD es un servicio multiinquilino de administración de identidades y de directorios basado en la nube que combina los servicios de directorio principales, la administración del acceso de las aplicaciones y la protección de identidades. Con Azure AD, puede integrar identidades locales en clústeres de AKS para proporcionar un único origen para la seguridad y administración de cuentas.

![Integración de Azure Active Directory con clústeres de AKS](media/concepts-identity/aad-integration.png)

Con los clústeres de AKS integrados en Azure AD, puede conceder a los usuarios o grupos acceso a los recursos de Kubernetes de un espacio de nombres o del clúster. Para obtener un contexto de configuración de `kubectl`, el usuario puede ejecutar el comando [az aks get-credentials][az-aks-get-credentials]. Cuando un usuario interactúa con el clúster de AKS con `kubectl`, se le pide que inicie sesión con sus credenciales de Azure AD. Este enfoque proporciona un único origen para la administración de cuentas de usuario y de las credenciales de contraseña. El usuario solo puede acceder a los recursos como defina el administrador de clústeres.

La autenticación de Azure AD en los clústeres de AKS usa OpenID Connect, una capa de identidad que se basa en el protocolo OAuth 2.0. OAuth 2.0 define los mecanismos para obtener y utilizar tokens de acceso para acceder a recursos protegidos, y OpenID Connect implementa la autenticación como una extensión del proceso de autorización de OAuth 2.0. Puede encontrar más información sobre OpenID Connect en la [documentación de OpenID Connect][openid-connect]. Para comprobar los tokens de autenticación obtenidos de Azure AD a través de OpenID Connect, los clústeres de AKS usan la autenticación de token de webhook de Kubernetes. Para más información, consulte la [documentación de autenticación de token de webhook][webhook-token-docs].

## <a name="role-based-access-controls-rbac"></a>Controles de acceso basado en rol (RBAC)

Para proporcionar el filtrado pormenorizado de las acciones que los usuarios pueden realizar, Kubernetes utiliza controles de acceso basado en rol (RBAC). Este mecanismo de control le permite asignar a usuarios o grupos de usuarios, permiso para realizar acciones como crear o modificar recursos, o ver registros de cargas de trabajo de aplicaciones en ejecución. Estos permisos se pueden limitar a un único espacio de nombres o conceder en todo el clúster de AKS. Con el control de acceso basado en rol de Kubernetes, puede crear *roles* para definir permisos y, después, asignar esos roles a usuarios mediante *enlaces de rol*.

Para obtener más información, consulte [Uso de la autorización de RBAC][kubernetes-rbac].

### <a name="azure-role-based-access-controls-rbac"></a>Controles de acceso basado en rol (RBAC) de Azure
Un mecanismo adicional para controlar el acceso a los recursos es el control de acceso de basado en rol (RBAC) de Azure. El control de acceso basado en rol de Kubernetes está diseñado para trabajar con los recursos del clúster de AKS y el de Azure está diseñado para trabajar con los recursos de su suscripción de Azure. Con el control de acceso basado en rol de Azure, puede crear una *definición de rol* que describe los permisos que se aplicarán. A continuación, se asigna esta definición de rol a un usuario o grupo para un *ámbito* determinado que puede ser un recurso individual, un grupo de recursos o en toda la suscripción.

Para más información, consulte [¿Qué es el control de acceso basado en rol de Azure?][azure-rbac]

## <a name="roles-and-clusterroles"></a>Roles y ClusterRoles

Antes de asignar permisos a los usuarios con el control de acceso basado en rol de Kubernetes, defina primero esos permisos como un *rol*. Los roles de Kubernetes *conceden* permisos. No hay ningún concepto de *denegación* de permiso.

Los roles se usan para conceder permisos en un espacio de nombres. Si necesita conceder permisos en todo el clúster o para recursos de clúster fuera de un espacio de nombres determinado, puede usar en su lugar *ClusterRoles*.

Un ClusterRole funciona de la misma manera para conceder permisos a los recursos, pero se puede aplicar a los recursos de todo el clúster, no solo a los de un espacio de nombres específico.

## <a name="rolebindings-and-clusterrolebindings"></a>RoleBindings y ClusterRoleBindings

Una vez que se definen roles para conceder permisos a los recursos, asigne esos permisos de RBAC de Kubernetes con un *RoleBinding*. Si el clúster de AKS se integra con Azure Active Directory, los enlaces son la solución para conceder permisos a esos usuarios de Azure AD para que realicen acciones en el clúster.

Los enlaces de roles se usan para asignar roles para un espacio de nombres determinado. Este enfoque le permite separar lógicamente un único clúster de AKS, con usuarios que solo pueden acceder a los recursos de la aplicación en su espacio de nombres asignado. Si necesita enlazar roles en todo el clúster o para recursos de clúster fuera de un espacio de nombres determinado, puede usar en su lugar *ClusterRoleBindings*.

Un ClusterRoleBinding funciona de la misma manera para enlazar roles con usuarios, pero se puede aplicar a los recursos de todo el clúster, no solo a los de un espacio de nombres específico. Este enfoque le permite conceder acceso para los administradores o ingenieros de soporte técnico a todos los recursos del clúster de AKS.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con Azure AD y el RBAC de Kubernetes, consulte [Integración de Azure Active Directory con AKS][aks-aad].

Para conocer los procedimientos recomendados asociados, consulte [Procedimientos recomendados para la autenticación y la autorización en AKS][operator-best-practices-identity].

Para obtener más información sobre los conceptos básicos de Kubernetes y AKS, consulte los artículos siguientes:

- [Clústeres y cargas de trabajo de Kubernetes/AKS][aks-concepts-clusters-workloads]
- [Seguridad de Kubernetes/AKS][aks-concepts-security]
- [Redes virtuales de Kubernetes/AKS][aks-concepts-network]
- [Almacenamiento de Kubernetes/AKS][aks-concepts-storage]
- [Escala de Kubernetes/AKS][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v2-protocols-oidc.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: azure-ad-integration-cli.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
