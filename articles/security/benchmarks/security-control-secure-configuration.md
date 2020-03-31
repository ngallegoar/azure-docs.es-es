---
title: 'Control de seguridad de Azure: configuración segura'
description: Configuración segura del control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 03564effeee36ddb3316d48329ccab8ccfce75b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934294"
---
# <a name="security-control-secure-configuration"></a>Control de seguridad: Configuración segura

Establezca, implemente y administre activamente (realice un seguimiento, informe y corrija) la configuración de seguridad de los recursos de Azure con el fin de evitar que los atacantes aprovechen los servicios y las configuraciones vulnerables.

## <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.1 | 5,1 | Customer |

Use Azure Policy o Azure Security Center para mantener las configuraciones de seguridad para todos los recursos de Azure.

Configuración y administración de Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.2 | 5,1 | Customer |

Use la recomendación de Azure Security Center &quot;Remediate Vulnerabilities in Security Configurations on your Virtual Machines (Corrija las vulnerabilidades en las configuraciones de seguridad de sus máquinas virtuales)&quot; para mantener las configuraciones de seguridad en todos los recursos de proceso.

Cómo supervisar las recomendaciones de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-recommendations

Cómo corregir las recomendaciones de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.3 | 5.2 | Customer |

Use la directiva de Azure [denegar] e [implementar si no existe] para aplicar la configuración segura en los recursos de Azure.

Configuración y administración de Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Descripción de los efectos de Azure Policy:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.4 | 5.2 | Compartido |

Microsoft administra y mantiene las imágenes del sistema operativo base.

Sin embargo, puede aplicar la configuración de seguridad que requiere la organización con plantillas de Azure Resource Manager o Desired State Configuration.

Cómo crear una máquina virtual de Azure desde una plantilla de Azure Resource Manager:

https://docs.microsoft.com/azure/virtual-machines/windows/ps-template

Descripción de Desired State Configuration para máquinas virtuales de Azure:

https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-overview

## <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.5 | 5.3 | Customer |

Si usa definiciones de directivas personalizadas de Azure, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

Cómo almacenar código en Azure DevOps:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Documentación de Azure Repos:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

## <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.6 | 5.3 | Customer |

Si usa imágenes personalizadas, use RBAC para asegurarse de que solo los usuarios autorizados puedan tener acceso a las imágenes. En el caso de las imágenes de contenedor, almacénelas en Azure Container Registry y aproveche RBAC para asegurarse de que solo los usuarios autorizados puedan acceder a las imágenes.

Descripción de RBAC en Azure:

https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles

Descripción de RBAC para Container Registry:

https://docs.microsoft.com/azure/container-registry/container-registry-roles

Cómo configurar RBAC en Azure:

https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

## <a name="77-deploy-system-configuration-management-tools"></a>7.7: Implemente herramientas de administración de configuración del sistema

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7,7 | 5.4 | Customer |

Use Azure Policy para alertar, auditar y aplicar las configuraciones del sistema. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

Cómo configurar y administrar Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: Implemente herramientas de administración de configuración del sistema para sistemas operativos

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.8 | 5.4 | Customer |

Use extensiones de proceso de Azure, como Desired State Configuration de PowerShell para procesos de Windows o la extensión de Chef de Linux para Linux.

Cómo instalar extensiones de máquina virtual en Azure:

https://docs.microsoft.com/azure/virtual-machines/extensions/overview#how-can-i-install-an-extension

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Implemente la supervisión de configuración automatizada para servicios de Azure

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.9 | 5.5 | Customer |

Use Azure Security Center para realizar análisis de base de referencia para los recursos de Azure

Cómo corregir las recomendaciones en Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.1 | 5.5 | Customer |

Use Azure Security Center para realizar exámenes de base de referencia para la configuración del sistema operativo y de Docker para los contenedores.

Descripción de las recomendaciones sobre contenedores de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-container-recommendations

## <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura 

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.11 | 13.1 | Customer |

Use Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de secretos para las aplicaciones en la nube.

Cómo integrar las identidades administradas de Azure:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Cómo crear un almacén de claves:

https://docs.microsoft.com/azure/key-vault/quick-create-portal

Cómo proporcionar la autenticación de Key Vault con una identidad administrada:

https://docs.microsoft.com/azure/key-vault/managed-identity

## <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.12 | 4,1 | Customer |

Use las identidades administradas para proporcionar a los servicios de Azure una identidad administrada automáticamente en Azure AD. Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluyendo Key Vault, sin necesidad de credenciales en el código.

Cómo configurar las identidades administradas:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

## <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 7.13 | 13.3 | Customer |

Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

Cómo configurar el escáner de credenciales:

https://secdevtools.azurewebsites.net/helpcredscan.html

## <a name="next-steps"></a>Pasos siguientes

Vea el siguiente control de seguridad: [Defensa contra malware](security-control-malware-defense.md)
