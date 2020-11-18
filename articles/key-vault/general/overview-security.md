---
title: Seguridad de Azure Key Vault
description: Administración de los permisos de acceso para Azure Key Vault, claves y secretos. Trata sobre el modelo de autenticación y autorización de Key Vault y cómo proteger un almacén de claves.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: mbaldwin
ms.openlocfilehash: dc08df7390285f9b6e4701bb1ca5c4227b19f1da
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445037"
---
# <a name="azure-key-vault-security"></a>Seguridad de Azure Key Vault

Puede usar Azure Key Vault para proteger las claves de cifrado y los secretos, como certificados, cadenas de conexión y contraseñas en la nube. Cuando almacena datos confidenciales y críticos para la empresa, debe tomar medidas para maximizar la seguridad de los almacenes y de los datos almacenados en estos.

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

Cuando se crea un almacén de claves en una suscripción de Azure, se asocia automáticamente al inquilino de Azure AD de dicha suscripción. Cualquier persona que intente administrar o recuperar el contenido desde un almacén debe autenticarse con Azure AD.

- La autenticación establece la identidad del llamador.
- La autorización determina las operaciones que puede realizar quien envía la llamada. La autorización en Key Vault usa una combinación de [control de acceso basado en roles de Azure ](../../role-based-access-control/overview.md) (Azure RBAC) y directivas de acceso de Azure Key Vault.

### <a name="access-model-overview"></a>Introducción al modelo acceso

El acceso a los almacenes se realiza a través de dos interfaces o planos. Estos son el plano de administración y el plano de datos.

- El *plano de administración* es donde se administra Key Vault y es la interfaz utilizada para crear y eliminar almacenes. También puede leer las propiedades del almacén de claves y administrar las directivas de acceso.
- El *plano de datos* es donde se trabaja con los datos almacenados en un almacén de claves. Puede agregar, eliminar y modificar claves, secretos y certificados.

Para obtener acceso a un almacén de claves en cualquier plano, quienes realizan la llamada (usuarios o aplicaciones) deben estar autenticados y autorizados. Ambos planos usan Azure Active Directory (Azure AD) para la autenticación. Para la autorización, el plano de administración usa el control de acceso basado en rol de Azure (Azure RBAC) y el plano de datos utiliza una directiva de acceso de Key Vault.

El modelo de un único mecanismo de autenticación para ambos planos tiene varias ventajas:

- Las organizaciones pueden controlar el acceso de forma centralizada a todos los almacenes de claves de su organización.
- Si un usuario abandona la organización, al instante pierde el acceso a todos los almacenes de claves de la organización.
- Las organizaciones pueden personalizar la autenticación mediante las opciones de Azure AD, como la habilitación de Multi-Factor Authentication para aumentar la seguridad.

### <a name="managing-administrative-access-to-key-vault"></a>Administración del acceso administrativo a Key Vault

Cuando crea un almacén de claves en un grupo de recursos, administra el acceso mediante Azure AD. Puede conceder a usuarios o grupos la capacidad de administrar los almacenes de claves en un grupo de recursos. Puede conceder acceso a un nivel de ámbito específico si asigna los roles de Azure apropiados. Para conceder acceso a un usuario para administrar almacén de claves, debe asignar un rol `key vault Contributor` predefinido al usuario en un ámbito específico. Los siguientes niveles de ámbitos se pueden asignar a un rol de Azure:

- **Suscripción**: Un rol de Azure asignado al nivel de suscripción se aplica a todos los recursos y grupos de recursos de esa suscripción.
- **Grupo de recursos**: Un rol de Azure asignado al nivel de grupo de recursos se aplica a todos los recursos de dicho grupo de recursos.
- **Recurso específico**: un rol de Azure asignado a un recurso concreto se aplica a dicho recurso. En este caso, el recurso es un almacén de claves específico.

Existen varios roles predefinidos. Si un rol predefinido no se ajusta a sus necesidades, puede definir uno propio. Para más información, consulte [Azure RBAC: roles integrados](../../role-based-access-control/built-in-roles.md).

> [!IMPORTANT]
> Si un usuario tiene permisos `Contributor` en un plano de administración de Key Vault, se puede conceder a sí mismo acceso al plano de datos estableciendo una directiva de acceso de Key Vault. Debe controlar estrechamente quién tiene el rol de acceso `Contributor` a los almacenes de claves. Asegúrese de que solo las personas autorizadas pueden acceder a los almacenes de claves, las claves, los secretos y los certificados y administrarlos.

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>Control del acceso a datos de Key Vault

Las directivas de acceso de Key Vault conceden permisos independientes a las claves, los secretos o los certificados. Puede conceder acceso a un usuario solo a las claves y no a los secretos. Los permisos para acceder a las claves, los secretos y los certificados se administran en el nivel del almacén.

> [!IMPORTANT]
> Las directivas de acceso de Key Vault no admiten permisos de nivel de objeto pormenorizados, como una clave, un secreto o un certificado específicos. Cuando se concede a un usuario permiso para crear y eliminar claves, este puede realizar dichas operaciones en todas las claves de dicho almacén de claves.

Puede establecer directivas de acceso de un almacén de claves mediante [Azure Portal](assign-access-policy-portal.md), la [CLI de Azure](assign-access-policy-cli.md), [Azure PowerShell](assign-access-policy-powershell.md) o las [API de REST de administración de Key Vault](/rest/api/keyvault/).

Puede restringir el acceso al plano de datos mediante el uso de [puntos de conexión de servicio de red virtual para Azure Key Vault](overview-vnet-service-endpoints.md). Puede configurar [firewall y reglas de red virtual](network-security.md) para una capa adicional de seguridad.

## <a name="network-access"></a>Acceso de red

Puede reducir la exposición de los almacenes si especifica las direcciones IP que tienen acceso a ellos. Los puntos de conexión de servicio de red virtual para Azure Key Vault permiten restringir el acceso a una red virtual especificada. También permiten restringir el acceso a una lista de intervalos de direcciones IPv4 (protocolo de Internet, versión 4). A todos los usuarios que se conecten a su almacén de claves desde fuera de esos orígenes se les negará el acceso.

Una vez que las reglas del firewall están en vigor, los usuarios solo pueden leer datos de Key Vault cuando las solicitudes se originan desde redes virtuales o rangos de direcciones IPv4 permitidos. Esto también se aplica al acceso de Key Vault desde Azure Portal. Aunque los usuarios pueden ir a un almacén de claves desde Azure Portal, es posible que no pueda enumerar las claves, los secretos o los certificados si su equipo cliente no está en la lista de dispositivos permitidos. Esto también afecta al selector de Key Vault de otros servicios de Azure. Los usuarios podrían ver la lista de almacenes de claves, pero no enumerar las claves si las reglas del firewall limitan su equipo cliente.

Para obtener más información sobre las direcciones de red de Azure Key Vault, revise [Puntos de conexión de servicio de red virtual para Azure Key Vault](overview-vnet-service-endpoints.md))

## <a name="tls-and-https"></a>TLS y HTTPS

*   El front-end de Key Vault (plano de datos) es un servidor de varios inquilinos. Esto significa que los almacenes de claves de distintos clientes pueden compartir la misma dirección IP pública. Con el fin de lograr el aislamiento, cada solicitud HTTP se autentica y autoriza con independencia de otras solicitudes.
*   Puede identificar versiones anteriores de TLS para notificar las vulnerabilidades, pero debido a que la dirección IP pública está compartida, no es posible que el equipo de servicio de Key Vault deshabilite las versiones anteriores de TLS para almacenes de claves individuales en el nivel de transporte.
*   El protocolo HTTPS permite al cliente participar en la negociación de TLS. Los **clientes pueden aplicar la última versión de TLS** y, cada vez que un cliente lo hace, toda la conexión usará la protección de nivel correspondiente. El hecho de que Key Vault siga siendo compatible con versiones anteriores de TLS no afectará a la seguridad de las conexiones con las versiones más recientes de TLS.
*   A pesar de las vulnerabilidades conocidas del protocolo TLS, no hay ningún ataque conocido que permita a un agente malintencionado extraer cualquier información de su almacén de claves cuando el atacante inicie una conexión con una versión de TLS que tenga vulnerabilidades. El atacante todavía tendría que autenticarse y autorizarse a sí mismo, y siempre y cuando los clientes legítimos se conecten siempre con versiones de TLS recientes, no existe la posibilidad de que las credenciales se hayan filtrado de las vulnerabilidades de versiones de TLS anteriores.

## <a name="logging-and-monitoring"></a>Registro y supervisión

Los registros de Key Vault guardan información acerca de las actividades realizadas en el almacén. Para obtener información completa, vea [Registro de Key Vault](logging.md).

Para obtener recomendaciones sobre la administración segura de las cuentas de almacenamiento, revise la [guía de seguridad de Azure Storage](../../storage/blobs/security-recommendations.md).

## <a name="next-steps"></a>Pasos siguientes

- [Puntos de conexión de servicio de red virtual para Azure Key Vault](overview-vnet-service-endpoints.md)
- [RBAC de Azure: roles integrados](../../role-based-access-control/built-in-roles.md)