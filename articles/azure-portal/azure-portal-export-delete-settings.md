---
title: Exportación o eliminación de la configuración de Azure portal
description: Obtenga información sobre cómo exportar o eliminar la configuración de usuario, los paneles privados y la configuración personalizada en Azure Portal.
services: azure-portal
keywords: ''
author: santhoshsomayajula
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: mblythe
ms.openlocfilehash: accdfbd939fad73ca7d008450c358d366e7f8b70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76900754"
---
# <a name="export-or-delete-user-settings"></a>Exportación o eliminación de la configuración de usuario

Puede usar la configuración y las características de Azure Portal para crear una experiencia personalizada. La información sobre la configuración personalizada se almacena en Azure. Puede exportar o eliminar los siguientes datos de usuario:

* Paneles privados de Azure Portal
* Configuración del usuario, como suscripciones o directorios favoritos y último directorio de inicio de sesión
* Temas y otra configuración del portal personalizada

Es una buena idea exportar y revisar la configuración antes de eliminarla. Volver a generar paneles o rehacer la configuración personalizada puede llevar mucho tiempo.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Exportación o eliminación de la configuración del portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el encabezado del portal, seleccione ![icono de configuración](media/azure-portal-export-delete-settings/settings-icon.png) **Configuración**.

1. Seleccione **Exportar todas las opciones de configuración** o **Eliminar todas las opciones de configuración y los paneles privados**.

    ![Opciones de configuración y configuración de Azure Portal](media/azure-portal-export-delete-settings/azure-portal-settings-with-export-delete.png)

      En la siguiente tabla se describen estas acciones.

      | Acción | Descripción |
      | --- | --- |
      | **Exportar todas las opciones de configuración** | Crea un archivo *.json* que contiene la configuración de usuario, como el tema de color, los favoritos y los paneles privados.|
      | **Eliminar todas las opciones de configuración y los paneles privados** | Elimina todos los vínculos a paneles privados y otras configuraciones personalizadas que haya realizado en el portal. |

> [!NOTE]
> Debido a la naturaleza dinámica de la configuración de usuario y el riesgo de daños en los datos, no se puede importar la configuración desde el archivo *.json*.
>
>

## <a name="next-steps"></a>Pasos siguientes

* [Uso compartido de paneles de Azure mediante el control de acceso basado en rol](azure-portal-dashboard-share-access.md)
* [Agregar, quitar y reorganizar favoritos](azure-portal-add-remove-sort-favorites.md)
