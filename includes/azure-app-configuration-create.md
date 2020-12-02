---
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: include
ms.date: 1/31/2020
ms.openlocfilehash: 48bf5d5df3c54e8b2894ad28fa08553d5add0437
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96182709"
---
1. Para crear un almacén de App Configuration, inicie sesión en [Azure Portal](https://portal.azure.com). En la esquina superior izquierda de la página principal, seleccione **Crear un recurso**. En el cuadro **Buscar en Marketplace**, escriba *App Configuration* y presione <kbd>Entrar</kbd>.

    ![Buscar App Configuration](media/azure-app-configuration-create/azure-portal-search.png)

1. Seleccione **Configuración de la aplicación** en los resultados de la búsqueda y, después, **Crear**.

    ![Seleccionar Crear](media/azure-app-configuration-create/azure-portal-app-configuration-create.png)

1. En el panel **Crear de App Configuration**, introduzca la siguiente configuración:

    | Configuración | Valor sugerido | Descripción |
    |---|---|---|
    | **Suscripción** | Su suscripción | Seleccione la suscripción de Azure que desee usar para probar App Configuration. Si su cuenta solo tiene una suscripción, se selecciona automáticamente y la lista **Suscripción** no aparece. |
    | **Grupos de recursos** | *AppConfigTestResources* | Seleccione o cree un grupo de recursos para el recurso del almacén de App Configuration. Este grupo es útil para organizar los distintos recursos que quiera eliminar al mismo tiempo mediante la eliminación del grupo de recursos. Para más información, consulte [Administración de los recursos de Azure a través del Portal](../articles/azure-resource-manager/management/overview.md). |
    | **Nombre del recurso** | Nombre único globalmente | Escriba un nombre de recurso único para usarlo como recurso de almacén de App Configuration. El nombre debe ser una cadena de entre 5 y 50 caracteres y solo puede contener números, letras y el carácter `-`. El nombre no puede comenzar ni terminar con el carácter `-`. |
    | **Ubicación** | *Centro de EE. UU.* | Use **Ubicación** para especificar la ubicación geográfica en la que se hospeda el almacén de App Configuration. Para optimizar el rendimiento, cree el recurso en la misma región que los demás componentes de la aplicación. |
    | **Plan de tarifa** | *Gratis* | Seleccione el plan de tarifa deseado. Para más información, consulte la [página de precios de App Configuration](https://azure.microsoft.com/pricing/details/app-configuration). |

1. Seleccione **Revisar y crear** para validar la configuración.

1. Seleccione **Crear**. La implementación puede tardar unos minutos.

1. Una vez finalizada la implementación, vaya al recurso de App Configuration. Seleccione **Configuración** > **Claves de acceso**. Tome nota de la cadena de conexión de la clave de solo lectura principal. Usará esta cadena de conexión más adelante para configurar la aplicación y que se comunique con el almacén de Azure App Configuration que ha creado.