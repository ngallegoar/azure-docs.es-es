---
title: Introducción a la autenticación en Azure Automation
description: Este artículo proporciona información general sobre la seguridad de Automation y los diferentes métodos de autenticación disponibles para las cuentas de automatización de Azure Automation.
keywords: seguridad de automatización, automatización segura; autenticación de automatización
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 3d3dbaad18f6acbe1ddf17d81f54e4232c838dd7
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82787420"
---
# <a name="introduction-to-authentication-in-azure-automation"></a>Introducción a la autenticación en Azure Automation

Azure Automation le permite automatizar tareas en recursos de Azure, locales y de otros proveedores de servicios en la nube como Amazon Web Services (AWS). Para que un runbook realice las acciones necesarias, debe tener permiso de acceso seguro a los recursos con los derechos mínimos necesarios en la suscripción.

En este artículo se tratarán los distintos escenarios de autenticación admitidos por Azure Automation y se le mostrará cómo empezar a trabajar según los entornos que necesite administrar.  

## <a name="automation-account-overview"></a>Información general sobre las cuentas de Automation

Cuando inicia Azure Automation por primera vez, debe crear al menos una cuenta de Automation. Las cuentas de Automation le permiten aislar los recursos de Automation (Runbooks, recursos, configuraciones) desde los recursos contenidos en otras cuentas de Automation. Puede usar cuentas de Automation para separar los recursos en entornos lógicos independientes. Por ejemplo, puede usar una cuenta para desarrollo, otra para producción y otra para su entorno local. Una cuenta de Azure Automation es diferente de su cuenta Microsoft o de las cuentas creadas en su suscripción de Azure.

Los recursos de Automation de cada cuenta de Automation están asociados con una sola región de Azure, pero las cuentas de Automation pueden administrar recursos en su suscripción. El principal motivo para crear cuentas de Automation en distintas regiones sería si tiene directivas que requieren que los datos y recursos se aíslen en una región específica.

Todas las tareas que realice con recursos mediante Azure Resource Manager y los cmdlets de Azure en Azure Automation deben autenticarse en Azure con una autenticación basada en credenciales de identidad organizativa de Azure Active Directory. Las cuentas de ejecución de Azure Automation proporcionan autenticación para administrar los recursos de Azure con los cmdlets de Azure. Cuando crea una cuenta de ejecución, se crea un nuevo usuario de la entidad de servicio en Azure Active Directory (AD) y se asigna el rol Colaborador a este usuario en el nivel de suscripción. Para los runbooks que utilizan instancias de Hybrid Runbook Worker en máquinas virtuales de Azure, puede utilizar [autenticación de runbook con identidades administradas](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) en lugar de cuentas de ejecución para autenticarse en sus recursos de Azure.

La entidad de servicio para una cuenta de ejecución no tiene los permisos para leer Azure Active Directory de manera predeterminada. Si quiere agregar permisos para leer o administrar Azure AD, deberá conceder los permisos a la entidad de servicio en **Permisos de API**. Para más información, consulte [Adición de permisos para acceder a las API web](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

El control de acceso basado en rol está disponible en Azure Resource Manager para conceder las acciones permitidas a una cuenta de usuario de Azure AD y a una cuenta de ejecución, y para autenticar dicha entidad de servicio. Para más información que le ayude a desarrollar su modelo de administración de permisos de Automation, consulte el [artículo Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md).  

Los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker en su centro de datos o en los servicios de computación de otros entornos de nube como AWS no pueden usar el mismo método que se usa normalmente para los runbooks que se autentican en recursos de Azure. Esto se debe a que esos recursos se ejecutan fuera de Azure y, por lo tanto, requieren sus propias credenciales de seguridad definidas en Automatización para la autenticación en los recursos a los que tienen acceso localmente. Para más información sobre la autenticación de runbooks con trabajos de runbook, consulte [Autenticación de runbooks para Hybrid Runbook Worker](automation-hrw-run-runbooks.md). 

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una cuenta de Automation desde Azure Portal](automation-create-standalone-account.md)

* [Creación de una cuenta de Automation con una plantilla de Azure Resource Manager](automation-create-account-template.md)

* [Autenticación con Amazon Web Services (AWS)](automation-config-aws-account.md)
