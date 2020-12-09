---
title: Identidades administradas para Azure
description: Obtenga información sobre el uso de identidades administradas para Azure con Service Fabric.
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: 28c992792d4572a43e12f5d32855f8411b0f4c6f
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96574758"
---
# <a name="using-managed-identities-for-azure-with-service-fabric"></a>Uso de identidades administradas para Azure con Service Fabric

Un desafío común al compilar aplicaciones en la nube es cómo administrar de forma segura las credenciales en el código para autenticarse en varios servicios sin guardarlos localmente en una estación de trabajo de desarrollador ni en el control de código fuente. Las *identidades administradas para Azure* resuelven este problema para todos los recursos de Azure Active Directory (Azure AD) proporcionándoles identidades administradas automáticamente dentro de Azure AD. Puede usar la identidad de un servicio para autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de almacenar ninguna credencial en el código.

Las *identidades administradas para recursos de Azure* se incluyen gratuitamente en Azure AD con las suscripciones de Azure. No hay ningún costo adicional.

> [!NOTE]
> Las *identidades administradas para recursos de Azure* es el nombre con el que ahora se conoce al servicio Managed Service Identity (MSI).

## <a name="concepts"></a>Conceptos

Las identidades administradas para Azure se basan en varios conceptos clave:

- **Id. de cliente**: identificador único que genera Azure AD y que está asociado a una aplicación y entidad de servicio durante su aprovisionamiento inicial (consulte también [Identificador de aplicación](../active-directory/develop/developer-glossary.md#application-id-client-id)).

- **Id. de entidad de seguridad**: identificador de objeto del objeto de entidad de servicio de la identidad administrada que se usa para conceder acceso basado en roles a los recursos de Azure.

- **Entidad de servicio**: objeto de Azure Active Directory que representa la proyección de una aplicación de AAD en un inquilino determinado (consulte también [Entidad de servicio](../active-directory/develop/developer-glossary.md#service-principal-object)).

Hay dos tipos de identidades administradas:

- Las **identidades administradas asignadas por el sistema** se habilitan directamente en las instancias de servicio de Azure.  El ciclo de vida de una identidad administrada asignada por el sistema es único para la instancia de servicio de Azure en que está habilitada.
- Las **identidades administrada asignadas por el usuario** se crean como recursos de Azure independientes. La identidad se puede asignar a una o varias instancias de servicio de Azure y se administra de forma independiente de los ciclos de vida de esas instancias.

Para comprender mejor la diferencia entre los tipos de identidades administradas, consulte [¿Qué son las identidades administradas de recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types)

## <a name="supported-scenarios-for-service-fabric-applications"></a>Escenarios admitidos para las aplicaciones de Service Fabric

Las identidades administradas para Service Fabric solo se admiten en los clústeres de Service Fabric implementados de Azure y únicamente para las aplicaciones implementadas como recursos de Azure; no se puede asignar una identidad a una aplicación que no se implemente como recurso de Azure. En términos conceptuales, la compatibilidad con identidades administradas en el clúster de Azure Service Fabric consta de dos fases:

1. Asigne una o más identidades administradas al recurso de la aplicación; una aplicación puede tener asignada una única identidad asignada por el sistema y hasta 32 identidades asignadas por el usuario, respectivamente.

2. Dentro de la definición de la aplicación, asigne una de las identidades asignadas a la aplicación a cualquier servicio individual que contenga la aplicación.

La identidad asignada por el sistema de una aplicación es única de esa aplicación, mientras que una identidad asignada por el usuario es un recurso independiente, que se puede asignar a varias aplicaciones. Dentro de una aplicación, se puede asignar una identidad (ya sea asignada por el sistema o asignada por el usuario) a varios servicios de la aplicación, pero cada servicio individual solo puede tener asignada una identidad. Por último, se debe asignar una identidad explícitamente a un servicio para que tenga acceso a esta característica. De hecho, la asignación de las identidades de una aplicación a los servicios que la componen permite el aislamiento en la aplicación. Un servicio solo puede usar la identidad que se le ha asignado.  

Actualmente, se admiten los escenarios siguientes para esta característica:

- Implementación de una nueva aplicación con uno o más servicios, y con una o varias identidades asignadas.

- Asignación de una o varias identidades administradas a una aplicación existente (implementada en Azure) para acceder a los recursos de Azure.

Los escenarios siguientes no se admiten o no se recomiendan. Tenga en cuenta que es posible que estas acciones no se bloqueen, pero podrían provocar interrupciones en las aplicaciones:

- Eliminación o cambio de las identidades asignadas a una aplicación. Si debe realizar cambios, envíe implementaciones independientes para agregar primero una nueva asignación de identidad y, después, quitar una ya asignada. La eliminación de una identidad de una aplicación existente puede tener efectos no deseados, como dejar la aplicación en un estado que no se puede actualizar. Es seguro eliminar la aplicación por completo si es necesario quitar una identidad. Tenga en cuenta que de este modo se eliminará la identidad asignada por el sistema (si se ha definido) asociada a la aplicación y se quitarán todas las asociaciones con las identidades asignadas por el usuario que se han asignado a la aplicación.

- En este momento, la compatibilidad de Service Fabric con identidades administradas no está integrada en [AzureServiceTokenProvider](../key-vault/general/service-to-service-authentication.md).

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de un nuevo clúster de Azure Service Fabric con compatibilidad con la identidad administrada](./configure-new-azure-service-fabric-enable-managed-identity.md)
- [Habilitación de la compatibilidad con la identidad administrada en un clúster de Azure Service Fabric existente](./configure-existing-cluster-enable-managed-identity-token-service.md)
- [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el sistema](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
- [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el usuario](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
- [Aprovechamiento de la identidad administrada de una aplicación de Service Fabric desde el código de servicio](./how-to-managed-identity-service-fabric-app-code.md)
- [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](./how-to-grant-access-other-resources.md)
- [Declaración y uso de secretos de aplicación como KeyVaultReferences](./service-fabric-keyvault-references.md)
