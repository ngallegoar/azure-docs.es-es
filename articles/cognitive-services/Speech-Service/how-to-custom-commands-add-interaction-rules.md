---
title: 'Procedimientos: Adición de una confirmación a un comando personalizado'
titleSuffix: Azure Cognitive Services
description: En este artículo, obtendrá información sobre cómo implementar confirmaciones para un comando en Comandos personalizados.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 5f66e29e4c1bc85981202251e0de8288f4baee4e
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307340"
---
# <a name="add-interaction-rules"></a>Adición de reglas de interacción

En este artículo, aprenderá acerca de las **reglas de interacción**. Estas son reglas adicionales para controlar situaciones más específicas o complejas. Aunque tiene la libertad de crear sus propias reglas de interacción personalizadas, en este artículo, podrá usarlas para los siguientes escenarios objetivo:

* Confirmación de comandos
* Adición de correcciones de pasos a los comandos

Vaya a la sección de [referencia](./custom-commands-references.md) para obtener más información sobre las reglas de interacción.

## <a name="prerequisites"></a>Requisitos previos

Debe haber completado los pasos descritos en los siguientes artículos:
> [!div class="checklist"]
> * [Cómo: Creación de una aplicación con comandos simples](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Cómo: Adición de parámetros a comandos](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="add-confirmations-to-a-command"></a>Adición de confirmaciones a un comando

Para agregar una confirmación, use el comando **SetTemperature**. Para conseguir la confirmación, use los pasos siguientes para crear reglas de interacción.

1. Seleccione el comando **SetTemperature** en el panel izquierdo.
2. Para agregar las reglas de interacción, seleccione **Agregar** en el panel central y, a continuación, **Interaction rules (reglas de interacción)**  > **Confirm command (confirmar comando)** .

    De este modo, se agregarán tres reglas de interacción; esta regla pedirá al usuario que confirme la fecha y la hora de la alarma y espera una confirmación (sí/no) para el siguiente turno.

    1. Modifique la regla de interacción **Confirm Command** (confirmar comando) según la configuración siguiente.
        1. Cambie el **Nombre** por **`Confirm Temperature`** .
        1. Agregue una nueva condición como **Parámetros obligatorios > Temperature**.
        1. Agregue una nueva acción como **Tipo > Enviar respuesta de voz > `Are you sure you want to set the temperature as {Temperature} degrees?`** .
        1. Deje el valor predeterminado de **Expecting confirmation from user** (a la espera de confirmación del usuario) en la sección Expectativas.
      
         > [!div class="mx-imgBorder"]
         > ![Crear respuesta de parámetro requerida](media/custom-speech-commands/add-validation-set-temperature.png)
    

    1. Modifique la regla de interacción **Confirmation succeeded** (confirmación correcta) para controlar una confirmación correcta (el usuario dijo sí).
      
          1. Modifique el **nombre** por **`Confirmation temperature succeeded`** .
          1. Deje la condición **La confirmación se realizó correctamente** existente.
          1. Agregue una nueva condición como **Tipo > Parámetros obligatorios > Temperature**.
          1. Deje el valor predeterminado del **Post-execution state** (estado posterior a la ejecución) como **Execute completion rules** (ejecutar reglas de finalización).

    1. Modifique la condición **Confirmation denied** (confirmación denegada) (el usuario dijo que no) para controlar los escenarios en los que se denegó la confirmación.

          1. Modifique el **nombre** por **`Confirmation temperature denied`** .
          1. Deje la condición **La confirmación se denegó** existente.
          1. Agregue una nueva condición como **Tipo > Parámetros obligatorios > Temperature**.
          1. Agregue una nueva acción como **Tipo > Enviar respuesta de voz > `No problem. What temperature then?`** .
          1. Deje el valor predeterminado del **Post-execution state** (estado posterior a la ejecución) como **Wait for user's input** (esperar la entrada del usuario).

> [!IMPORTANT]
> En este artículo, se usó la funcionalidad de confirmación integrada. También puede lograr el mismo resultado al agregar las reglas de interacción de una en una, de forma manual.
   

### <a name="try-out-the-changes"></a>Prueba de los cambios

Seleccione **Entrenar**, espere a que se complete el entrenamiento y elija **Probar**.

- Entrada: set temperature to 80 degrees (establece la temperatura en 80 grados)
- Salida: ok 80? (¿80 está bien?)
- Entrada: No
- Salida: no problem. (No hay problema). what temperature then? (Entonces, ¿qué temperatura?)
- Entrada: 83 degrees (83 grados)
- Salida: ok 83? (¿83 está bien?)
- Entrada: Sí
- Salida: Ok, setting temperature to 83 degrees (de acuerdo, definiré la temperatura en 83 grados)


## <a name="implementing-corrections-in-a-command"></a>Implementación de correcciones en un comando

En esta sección, configurará la corrección de un paso, que se usará una vez que ya se ha ejecutado la acción de cumplimiento. También verá un ejemplo de cómo la corrección está habilitada de forma predeterminada, en caso de que el comando no se cumpla aún. Para agregar una corrección cuando no se haya completado el comando, agregue un nuevo parámetro **AlarmTone**.

Seleccione el comando **SetAlarm** en el panel izquierdo y agregue un nuevo parámetro **AlarmTone**.
        
- **Nombre** > `AlarmTone`
- **Tipo** > Cadena
- **Valor predeterminado** > `Chimes`
- **Configuración** > Aceptar valores de entrada predefinidos del catálogo interno
- **Predefined input values (valores de entrada predefinidos)**  > `Chimes`, `Jingle` y `Echo`. Cada uno como un valor de entradas predefinido individual.


A continuación, actualice la respuesta para el parámetro DateTime a `Ready to set alarm with tone as {AlarmTone}. For what time?`. A continuación, modifique la regla de finalización como se indica a continuación.

1. Seleccione la regla de finalización existente **ConfirmationResponse**.
1. En el panel derecho, mantenga el mouse sobre la acción existente y seleccione el botón **Editar**.
1. Actualice la respuesta de voz a `Ok, alarm set for {DateTime}. The alarm tone is {AlarmTone}.`.

### <a name="try-out-the-changes"></a>Prueba de los cambios

Seleccione **Entrenar**, espere hasta que se complete el entrenamiento y elija **Probar**.
Pruebe las siguientes expresiones:

- Entrada: set an alarm (establece una alarma)
- Salida: Ready to set alarm with tone as Chimes. (Listo para establecer una alarma con el tono campanillas). For what time? (¿A qué hora?)
- Entrada: set an alarm with tone as jingle for 9 am tomorrow (establece una alarma con el tono tintineo para mañana a las 9:00)
- Salida: OK, alarm set for 2020-05-30 09:00:00. (De acuerdo, alarma establecida para el 30/05/2020 a las 9:00:00). The alarm tone is jingle. (El tono de alarma es tintineo).

> [!IMPORTANT]
> Observe cómo se puede cambiar el tono de alarma sin ninguna configuración explícita en un comando en curso; es decir, cuando el comando no se ha completado todavía. **La corrección está habilitada de forma predeterminada para todos los parámetros de comando, independientemente del número de turnos si el comando todavía no finaliza.**

### <a name="correction-when-command-is-completed"></a>Corrección cuando el comando se ha completado

La plataforma de Comandos personalizados también proporciona la funcionalidad para corregir un paso, incluso cuando se ha completado el comando. Sin embargo, esta característica no está habilitada de forma predeterminada y debe configurarse de forma explícita. Siga estos pasos para configurar la corrección de un paso.

1. En el comando **SetAlarm**, agregue una regla de interacción de tipo **Update previous command** (actualizar el comando anterior) para actualizar la alarma establecida previamente. Cambie el **nombre** predeterminado de la regla de interacción a **Update previous alarm** (actualizar la alarma anterior).
1. Deje la condición predeterminada **Previous command needs to be updated** (se debe actualizar el comando anterior) tal como está.
1.  Agregue una nueva condición como **Tipo > Parámetro obligatorio > DateTime**.
1. Agregue una nueva acción como **Tipo > Enviar respuesta de voz > Editor sencillo > `Updating previous alarm time to {DateTime}.`** .
1. Deje el valor predeterminado del estado posterior a la ejecución como **Comando finalizado**.

### <a name="try-out-the-changes"></a>Prueba de los cambios

Seleccione **Entrenar**, espere a que se complete el entrenamiento y elija **Probar**.

- Entrada: set an alarm (establece una alarma)
- Salida: Ready to set alarm with tone as Chimes. (Listo para establecer una alarma con el tono campanillas). What time? (¿A qué hora?)
- Entrada: set an alarm with tone as jingle for 9 am tomorrow (establece una alarma con el tono tintineo para mañana a las 9:00)
- Salida: OK, alarm set for 2020-05-21 09:00:00 (de acuerdo, alarma establecida para el 21/05/2020 a las 9:00:00) The alarm tone is jingle. (El tono de alarma es tintineo).
- Entrada: no, 8 am (no, a las 8:00)
- Salida: Updating previous alarm time to 2020-05-29 08:00 (actualizaré la hora de la alarma anterior al 29/05/2020 a las 8:00:00).

> [!NOTE]
> En una aplicación real, en la sección Acciones de esta regla de corrección, también deberá enviar una actividad al cliente o llamar un punto de conexión HTTP para actualizar la hora de la alarma en el sistema. En particular, esta acción solo debe ser responsable de actualizar la hora de la alarma y no de cualquier otro atributo del comando; en este caso, el tono de alarma.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Adición de plantillas de generación de idioma para respuestas de voz](./how-to-custom-commands-add-language-generation-templates.md)