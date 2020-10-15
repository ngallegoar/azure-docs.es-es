---
title: Cómo solucionar problemas de Azure Data Catalog
description: En este artículo se describen cuestiones comunes de solución de problemas para los recursos de Azure Data Catalog.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: troubleshooting
ms.date: 08/01/2019
ms.openlocfilehash: 84bd14f8ae18527b4f6e9d8509a12555baec8771
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68879552"
---
# <a name="troubleshooting-azure-data-catalog"></a>Solución de problemas de Azure Data Catalog

En este artículo se describen cuestiones comunes de solución de problemas para los recursos de Azure Data Catalog. 

## <a name="functionality-limitations"></a>Limitaciones de funcionalidad

Cuando se usa Azure Data Catalog, la funcionalidad siguiente está limitada:

- Las cuentas de tipo **Función de invitado** no se admiten. No se pueden agregar las cuentas de invitado como usuarios de Azure Data Catalog y los usuarios invitados no pueden usar el portal en [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com).

- No se admite la creación de recursos de Azure Data Catalog mediante plantillas de Azure Resource Manager ni comandos de Azure PowerShell.

- El recurso de Azure Data Catalog no se puede mover entre inquilinos de Azure.

## <a name="azure-active-directory-policy-configuration"></a>Configuración de directivas de Azure Active Directory

Se puede dar el caso de que pueda iniciar sesión en el portal de Azure Data Catalog, pero al intentar iniciar sesión en la herramienta de registro de orígenes de datos, se mostrará un mensaje de error que le impedirá hacerlo. Dicho error puede aparecer cuando se utiliza la red de la empresa o cuando la conexión se realiza desde fuera de la red de la empresa.

La herramienta de registro usa la *autenticación de formularios* para validar los inicios de sesión de usuario en Azure Active Directory. Para que el inicio de sesión sea correcto, un administrador de Azure Active Directory tiene que habilitar la autenticación de formularios en la *directiva de autenticación global*.

Con la directiva de autenticación global se puede habilitar la autenticación de forma independiente para las conexiones de extranet y de intranet, como se muestra en la siguiente imagen. Pueden producirse errores de inicio de sesión si no está habilitada la autenticación de formularios en la red desde la que se conecta.

 ![Directiva de autenticación global de Azure Active Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Para más información, consulte [Configuración de directivas de autenticación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una instancia de Azure Data Catalog](data-catalog-get-started.md)
