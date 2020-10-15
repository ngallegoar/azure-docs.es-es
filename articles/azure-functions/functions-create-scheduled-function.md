---
title: Creación de una función que se ejecuta según una programación en Azure
description: Obtenga información sobre cómo crear una función de Azure que se ejecuta según la programación que usted defina.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 04/16/2020
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 9e542c493c02174364072f91d092f05ad9ec69cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90973093"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Cree una función en Azure que se desencadena mediante un temporizador

Aprenda a usar Azure Functions para crear una función [sin servidor](https://azure.microsoft.com/solutions/serverless/) que se ejecute según la programación que se defina.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-function-app"></a>Creación de una Function App de Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

La nueva aplicación de funciones está lista para usarse. A continuación, creará una función en la nueva aplicación de funciones.

:::image type="content" source="./media/functions-create-scheduled-function/function-app-create-success.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Creación de una función desencadenada por un temporizador

1. En la aplicación de funciones, seleccione **Funciones** y, a continuación, seleccione **+ Agregar**. 

   :::image type="content" source="./media/functions-create-scheduled-function/function-add-function.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::.

1. Seleccione la plantilla **Desencadenador de temporizador**. 

    :::image type="content" source="./media/functions-create-scheduled-function/function-select-timer-trigger.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

1. Configure el nuevo desencadenador según la configuración especificada en la tabla que aparece debajo de la imagen y, a continuación, seleccione **Crear función**.

    :::image type="content" source="./media/functions-create-scheduled-function/function-configure-timer-trigger.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::
    
    | Configuración | Valor sugerido | Descripción |
    |---|---|---|
    | **Nombre** | Valor predeterminado | Define el nombre de la función desencadenada por el temporizador. |
    | **Programación** | 0 \*/1 \* \* \* \* | [Expresión CRON](functions-bindings-timer.md#ncrontab-expressions) de seis campos que programa la función para que se ejecute cada minuto. |

## <a name="test-the-function"></a>Prueba de la función

1. En la función, seleccione **Código y prueba** y expanda los registros.

    :::image type="content" source="./media/functions-create-scheduled-function/function-test-timer-trigger.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

1. Vea la información escrita en los registros para verificar la ejecución.

    :::image type="content" source="./media/functions-create-scheduled-function/function-view-timer-logs.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

Ahora puede cambiar la programación de la función para que se ejecute una vez cada hora, en lugar de cada minuto.

## <a name="update-the-timer-schedule"></a>Actualizar la programación del temporizador

1. En la función, seleccione **Integración**. Aquí es donde puede definir los enlaces de entrada y salida de la función, así como establecer la programación. 

1. Seleccione **Timer (myTimer)** (Temporizador [myTimer]).

    :::image type="content" source="./media/functions-create-scheduled-function/function-update-timer-schedule.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

1. Actualice el valor **Programación**  a `0 0 */1 * * *` y, luego, seleccione **Guardar**.  

    :::image type="content" source="./media/functions-create-scheduled-function/function-edit-timer-schedule.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

Ahora tiene una función que se ejecuta una vez cada hora.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una función que se ejecuta según una programación. Para más información sobre los desencadenadores del temporizador, consulte [Programación de la ejecución de código con Azure Functions](functions-bindings-timer.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
