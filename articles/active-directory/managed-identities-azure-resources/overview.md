---
title: Identidades administradas de recursos de Azure
description: Información general sobre Managed Identities for Azure Resources.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 04/18/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2231d70e6c4368a7c896f9063b58cc97ee292f53
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81682586"
---
# <a name="what-are-managed-identities-for-azure-resources"></a>¿Qué son las identidades administradas de recursos de Azure?

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Un desafío común al compilar aplicaciones en la nube consiste en el modo de administrar las credenciales del código para autenticar los servicios en la nube. Proteger las credenciales es una tarea esencial. Lo ideal sería que nunca aparecieran en las estaciones de trabajo de los desarrolladores y que no se controlaran en el código fuente. Azure Key Vault proporciona una manera segura de almacenar credenciales, secretos y otras claves pero el código tiene que autenticarse en Key Vault para recuperarlos.

Las identidades administradas para recursos de Azure con las que cuenta Azure Active Directory (Azure AD) resuelven este problema. Esta característica proporciona a los servicios de Azure una identidad de sistema administrada automáticamente en Azure AD. Puede usar esta identidad para autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de credenciales en el código.

La característica Managed Identities for Azure Resources se incluye gratuitamente en Azure AD con las suscripciones de Azure. No hay ningún costo adicional.

> [!NOTE]
> Identidades administradas para recursos de Azure es el nombre con el que ahora se conoce al servicio Managed Service Identity (MSI).

## <a name="terminology"></a>Terminología

Los siguientes términos se usan en las identidades administradas para el conjunto de documentación de los recursos de Azure:

- **Id. de cliente**: un identificador único que genera Azure AD y que está asociado a una aplicación y entidad de servicio durante su aprovisionamiento inicial.
- **Id. de entidad de seguridad**: identificador de objeto del objeto de entidad de servicio de la identidad administrada que se usa para conceder acceso basado en roles a los recursos de Azure.
- **Azure Instance Metadata Service (IMDS)** : un punto de conexión de REST al que pueden acceder todas las máquinas virtuales IaaS creadas mediante Azure Resource Manager. El punto de conexión está disponible en una dirección IP no enrutable conocida (169.254.169.254) a la que se puede acceder solo desde dentro de la máquina virtual.

## <a name="how-does-the-managed-identities-for-azure-resources-work"></a>¿Cómo funcionan las identidades administradas de los recursos de Azure?

Hay dos tipos de identidades administradas:

- Las **identidades administradas asignadas por el sistema** se habilitan directamente en las instancias de servicio de Azure. Cuando se habilita la identidad, Azure crea una identidad para la instancia del servicio en el inquilino de Azure AD de confianza de la suscripción de la instancia. Una vez creada la identidad, las credenciales se aprovisionan en la instancia. El ciclo de vida de una identidad administrada asignada por el sistema está vinculado directamente a la instancia de servicio de Azure en que está habilitada. Si se elimina la instancia, Azure limpia automáticamente las credenciales y la identidad en Azure AD.
- Las **identidades administrada asignadas por el usuario** se crean como recursos de Azure independientes. Mediante un proceso de creación, Azure crea una identidad en el inquilino de Azure AD de confianza para la suscripción que se utiliza. Una vez creada la identidad, esta puede asignarse a una o varias instancias de servicio de Azure. El ciclo de vida de una identidad asignada por el usuario no se administra junto con el ciclo de vida de las instancias de servicio de Azure a las que se asigna.

Internamente, las identidades administradas son entidades de servicio de un tipo especial, que se bloquean para que solo puedan usarse con recursos de Azure. Cuando se elimina la identidad administrada, se quita automáticamente la entidad de servicio correspondiente.
Además, cuando se crea una identidad asignada por el usuario o asignada por el sistema, el proveedor de recursos de identidades administradas (MSRP) genera un certificado internamente para esa identidad. 

El código puede usar una identidad administrada para solicitar tokens de acceso de los servicios que admiten la autenticación de Azure AD. Azure se encarga de rotar las credenciales que usa la instancia del servicio. 

## <a name="credential-rotation"></a>Rotación de credenciales
La rotación de credenciales la controla el proveedor de recursos que hospeda el recurso de Azure. La rotación predeterminada de la credencial se produce cada 46 días. Es responsabilidad del proveedor de recursos llamar para obtener nuevas credenciales, por lo que el proveedor de recursos puede tardar más de 46 días en hacerlo.

En el diagrama siguiente se muestra cómo funcionan las identidades de servicio administradas con máquinas virtuales (VM) de Azure:

![Identidades de Managed Service Identity y máquinas virtuales de Azure](media/overview/data-flow.png)

|  Propiedad    | Identidad administrada asignada por el sistema | Identidad administrada asignada por el usuario |
|------|----------------------------------|--------------------------------|
| Creación |  Se crea como parte de un recurso de Azure (por ejemplo, una máquina virtual de Azure o Azure App Service) | Se crea como un recurso de Azure independiente |
| Ciclo de vida | Se comparte el ciclo de vida con el recurso de Azure con el que se creó la identidad administrada. <br/> Si se elimina el recurso primario, se elimina también la identidad administrada. | Ciclo de vida independiente. <br/> Se debe eliminar explícitamente. |
| Uso compartido de recursos de Azure | No se puede compartir. <br/> Solo se puede asociar con un único recurso de Azure. | Se puede compartir <br/> La misma identidad administrada asignada por el usuario se puede asociar con más de un recurso de Azure. |
| Casos de uso comunes | Cargas de trabajo contenidas en un único recurso de Azure <br/> Cargas de trabajo para las que necesita identidades independientes. <br/> Por ejemplo, una aplicación que se ejecuta en una sola máquina virtual | Cargas de trabajo que se ejecutan en varios recursos y que pueden compartir una única identidad. <br/> Cargas de trabajo que necesitan autorización previa para un recurso seguro como parte de un flujo de aprovisionamiento. <br/> Cargas de trabajo donde los recursos se reciclan con frecuencia, pero los permisos deben permanecer coherentes. <br/> Por ejemplo, una carga de trabajo en la que varias máquinas virtuales tienen que acceder al mismo recurso |

### <a name="how-a-system-assigned-managed-identity-works-with-an-azure-vm"></a>Funcionamiento de una identidad administrada asignada por el sistema con una máquina virtual de Azure

1. Azure Resource Manager recibe una solicitud para habilitar la identidad administrada asignada por el sistema de una máquina virtual.

2. Entonces crea una entidad de servicio en Azure AD para la identidad de la máquina virtual. La entidad de servicio se crea en el inquilino de Azure AD que sea de confianza para la suscripción.

3. Azure Resource Manager configura la identidad de la máquina virtual mediante la actualización del punto de conexión de identidad de Azure Instance Metadata Service con el identificador de cliente y el certificado de la entidad de servicio.

4. Ahora que la máquina virtual tiene una identidad, se usa la información de la entidad de servicio para conceder a la máquina virtual acceso a los recursos de Azure. Para llamar a Azure Resource Manager, use el control de acceso basado en rol (RBAC) de Azure AD para asignar el rol apropiado a la entidad de servicio de la máquina virtual. Para llamar a Key Vault, conceda a su código acceso al secreto o a la clave específicos en Key Vault.

5. El código que se ejecuta en la máquina virtual puede solicitar un token del punto de conexión de Azure Instance Metadata Service, accesible únicamente desde dentro de la máquina virtual: `http://169.254.169.254/metadata/identity/oauth2/token`
    - El parámetro del recurso especifica el servicio al que se va a enviar el token. Para autenticarse en Azure Resource Manager, use `resource=https://management.azure.com/`.
    - El parámetro de versión de API especifica la versión de IMDS, use api-version=2018-02-01 o posterior.

6. Se realiza una llamada a Azure AD para solicitar un token de acceso, tal y como se especifica en el paso 5, con el identificador de cliente y el certificado configurado en el paso 3. Azure AD devuelve un token de acceso JSON Web Token (JWT).

7. El código envía el token de acceso en una llamada a un servicio que admite la autenticación de Azure AD.

### <a name="how-a-user-assigned-managed-identity-works-with-an-azure-vm"></a>Funcionamiento de una identidad administrada asignada por el usuario con una máquina virtual de Azure

1. Azure Resource Manager recibe una solicitud para crear una identidad administrada asignada por el usuario.

2. Luego crea una entidad de servicio en Azure AD para la identidad asignada por el usuario. La entidad de servicio se crea en el inquilino de Azure AD que sea de confianza para la suscripción.

3. Azure Resource Manager recibe una solicitud para configurar la identidad administrada asignada por el usuario en una máquina virtual y actualiza el punto de conexión de identidad de Azure Instance Metadata Service con el identificador de cliente y el certificado de la entidad de servicio de la identidad administrada asignada por el usuario.

4. Una vez que se ha creado la identidad administrada asignada por el usuario, se usa la información de la entidad de servicio para conceder a la identidad acceso a los recursos de Azure. Para llamar a Azure Resource Manager, use el control de acceso basado en rol (RBAC) de Azure AD para asignar el rol apropiado a la entidad de servicio de la identidad asignada por el usuario. Para llamar a Key Vault, conceda a su código acceso al secreto o a la clave específicos en Key Vault.

   > [!Note]
   > También puede realizar este paso antes del paso 3.

5. El código que se ejecuta en la máquina virtual puede solicitar un token del punto de conexión de identidad de Azure Instance Metadata Service, accesible únicamente desde dentro de la máquina virtual: `http://169.254.169.254/metadata/identity/oauth2/token`.
    - El parámetro del recurso especifica el servicio al que se va a enviar el token. Para autenticarse en Azure Resource Manager, use `resource=https://management.azure.com/`.
    - El parámetro de identificador de cliente especifica la identidad para la que se solicita el token. Este valor es necesario para eliminar la ambigüedad cuando hay varias identidades asignadas por el usuario en la misma máquina virtual.
    - El parámetro de versión de API especifica la versión de Azure Instance Metadata Service. Use `api-version=2018-02-01` o superior.

6. Se realiza una llamada a Azure AD para solicitar un token de acceso, tal y como se especifica en el paso 5, con el identificador de cliente y el certificado configurado en el paso 3. Azure AD devuelve un token de acceso JSON Web Token (JWT).
7. El código envía el token de acceso en una llamada a un servicio que admite la autenticación de Azure AD.

## <a name="how-can-i-use-managed-identities-for-azure-resources"></a>¿Cómo se usan las identidades administradas de Managed Identities for Azure Resources?

Si quiere aprender a usar las identidades administradas para acceder a diferentes recursos de Azure, pruebe estos tutoriales.

> [!NOTE]
> Consulte el curso [Implementing Managed Identities for Microsoft Azure Resources](https://www.pluralsight.com/courses/microsoft-azure-resources-managed-identities-implementing) (Implementación de identidades administradas para los recursos de Microsoft Azure) para más información acerca de las identidades administradas, además de tutoriales en vídeo de varios escenarios admitidos.

Aprenda a usar una identidad administrada con una máquina virtual Windows:

* [Acceso a Azure Data Lake Store](tutorial-windows-vm-access-datalake.md)
* [Acceso a Azure Resource Manager](tutorial-windows-vm-access-arm.md)
* [Acceso a Azure SQL](tutorial-windows-vm-access-sql.md)
* [Acceso a Azure Storage mediante una clave de acceso](tutorial-vm-windows-access-storage.md)
* [Acceso a Azure Storage mediante firmas de acceso compartido](tutorial-windows-vm-access-storage-sas.md)
* [Acceso a un recurso que no es de Azure AD con Azure Key Vault](tutorial-windows-vm-access-nonaad.md)

Aprenda a usar una identidad administrada con una máquina virtual Linux:

* [Acceso a Azure Container Registry](../../container-registry/container-registry-authentication-managed-identity.md)
* [Acceso a Azure Data Lake Store](tutorial-linux-vm-access-datalake.md)
* [Acceso a Azure Resource Manager](tutorial-linux-vm-access-arm.md)
* [Acceso a Azure Storage mediante una clave de acceso](tutorial-linux-vm-access-storage.md)
* [Acceso a Azure Storage mediante firmas de acceso compartido](tutorial-linux-vm-access-storage-sas.md)
* [Acceso a un recurso que no es de Azure AD con Azure Key Vault](tutorial-linux-vm-access-nonaad.md)

Aprenda a usar una identidad administrada con otros servicios de Azure:

* [Azure App Service](/azure/app-service/overview-managed-identity)
* [Azure API Management](../../api-management/api-management-howto-use-managed-service-identity.md)
* [Azure Container Instances](../../container-instances/container-instances-managed-identity.md)
* [Azure Container Registry Tasks](../../container-registry/container-registry-tasks-authentication-managed-identity.md)
* [Azure Event Hubs](../../event-hubs/authenticate-managed-identity.md)
* [Funciones de Azure](/azure/app-service/overview-managed-identity)
* [Azure Kubernetes Service](/azure/aks/use-managed-identity)
* [Azure Logic Apps](/azure/logic-apps/create-managed-service-identity)
* [Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md)
* [Azure Data Factory](../../data-factory/data-factory-service-identity.md)


## <a name="what-azure-services-support-the-feature"></a>¿Qué servicios de Azure admiten la característica?<a name="which-azure-services-support-managed-identity"></a>

Las identidades administradas de Managed Identities for Azure Resources se pueden usar para autenticarse en servicios que admiten la autenticación de Azure AD. Para ver una lista de los servicios de Azure que admiten la característica Managed Identities for Azure Resources, consulte [Services that support managed identities for Azure resources](services-support-msi.md) (Servicios que admiten la característica Managed Identities for Azure Resources).

## <a name="next-steps"></a>Pasos siguientes

Empiece a utilizar la característica Managed Identities for Azure Resources con las guías de inicio rápido siguientes:

* [Uso de las identidades administradas asignadas por el sistema de una máquina virtual Windows para acceder a Resource Manager](tutorial-windows-vm-access-arm.md)
* [Uso de las identidades administradas asignadas por el sistema de una máquina virtual Linux para acceder a Resource Manager](tutorial-linux-vm-access-arm.md)
