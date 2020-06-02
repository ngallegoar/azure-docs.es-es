---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2020
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: c53486bf3368039f172c7a13420e2291dd9c9892
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122733"
---
1. En el menú de Azure Portal o en la página **Principal**, seleccione **Crear un recurso**.

1. En la página **Nuevo**, seleccione **Compute** > **Function App**.

1. En la página **Básico**, utilice la configuración de la aplicación de funciones como se especifica en la tabla siguiente:

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. |
    | **[Grupo de recursos](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. |
    | **Nombre de la aplicación de función** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z` (no distingue mayúsculas de minúsculas), `0-9` y `-`.  |
    |**Publicar**| Código | Opción para publicar archivos de código o un contenedor de Docker. |
    | **Pila en tiempo de ejecución** | Lenguaje preferido | Elija un tiempo de ejecución que admita su lenguaje de programación de funciones preferido. Elija **.NET** para funciones de C# y F#. |
    |**Región**| Región preferida | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |

    ![Página Datos básicos](./media/functions-premium-create/function-app-create-basics.png)

1. Seleccione **Siguiente: Hospedaje**. En la página **Hospedaje**, escriba la siguiente configuración:

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **[Cuenta de almacenamiento](../articles/storage/common/storage-account-create.md)** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    |**Sistema operativo**| Sistema operativo preferido | Se preselecciona un sistema operativo en función de la selección de pila en tiempo de ejecución, pero puede cambiar esta configuración si es necesario. |
    | **[Plan](../articles/azure-functions/functions-scale.md)** | Premium | Plan de hospedaje que define cómo se asignan los recursos a la Function App. Seleccione **Premium** y, a continuación, seleccione los valores predeterminados de **Plan de Windows** y **SKU y tamaño**. |

    ![Página Hospedaje](./media/functions-premium-create/function-app-premium-create-hosting.png)

1. Seleccione **Siguiente: Supervisión**. En la página **Supervisión**, escriba la siguiente configuración:

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Valor predeterminado | Crea un recurso de Application Insights con el mismo *nombre de aplicación* en la región más cercana que lo admita. Si expande esta configuración, puede cambiar el valor de **Nuevo nombre de recurso**  o elegir otro valor en **Ubicación** en la [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) para almacenar los datos. |

    ![Página Supervisión](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

1. Seleccione **Revisar y crear** para revisar las selecciones de configuración de la aplicación.

1. En la página **Revisar y crear**, revise la configuración y, a continuación, seleccione **Crear** para aprovisionar e implementar la aplicación de función.

1. Seleccione el icono **Notificaciones** de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**.

1. Seleccione **Ir al recurso** para ver la nueva aplicación de función. También puede seleccionar **Anclar al panel**. Dicho anclaje facilita la vuelta a este recurso de aplicación de función desde el panel.

    ![Notificación de implementación](./media/functions-premium-create/function-app-create-notification2.png)