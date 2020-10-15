---
title: 'Azure Service Bus: actualización automática de las unidades de mensajería'
description: En este artículo se muestra cómo puede actualizar automáticamente las unidades de mensajería de un espacio de nombres de Service Bus.
ms.topic: how-to
ms.date: 09/15/2020
ms.openlocfilehash: 0a72cc991e768a7bed01762d984cc56238ae0ad0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90984764"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Actualización automática de las unidades de mensajería de un espacio de nombres de Azure Service Bus 
Gracias al escalado automático, puede ejecutar la cantidad correcta de recursos para administrar la carga de la aplicación. Permite agregar recursos para controlar el aumento de la carga y ahorrar dinero mediante la eliminación de recursos inactivos. Consulte [Información general sobre el escalado automático en Microsoft Azure](../azure-monitor/platform/autoscale-overview.md) para más información sobre la característica de escalado automático de Azure Monitor. 

La mensajería Premium de Service Bus proporciona aislamiento de recursos en el nivel de CPU y memoria para que cada carga de trabajo de cliente se ejecute de forma aislada. Este contenedor de recursos se llama **unidad de mensajería**. Para más información sobre las unidades de mensajería, consulte la [Mensajería Premium de Service Bus](service-bus-premium-messaging.md). 

Mediante el uso de la característica de escalado automático para los espacios de nombres de Service Bus Premium, puede especificar un número mínimo y máximo de [unidades de mensajería](service-bus-premium-messaging.md) y agregar o quitar unidades de mensajería automáticamente en función de un conjunto de reglas. 

Por ejemplo, puede implementar los siguientes escenarios de escalado para los espacios de nombres de Service Bus mediante la característica de escalado automático. 

- Aumento de las unidades de mensajería para un espacio de nombres de Service Bus cuando el uso de CPU del espacio de nombres supera el 75 %. 
- Disminución de las unidades de mensajería para un espacio de nombres de Service Bus cuando el uso de CPU del espacio de nombres cae por debajo del 25 %. 
- Uso de más unidades de mensajería durante el horario comercial y menos durante las horas de inactividad. 

En este artículo se muestra cómo puede escalar automáticamente un espacio de nombres de Service Bus (actualizar las [unidades de mensajería](service-bus-premium-messaging.md)) en Azure Portal. 

> [!IMPORTANT]
> Este artículo se aplica solo al nivel **Premium** de Azure Service Bus. 

## <a name="autoscale-setting-page"></a>Página de configuración del escalado automático
En primer lugar, siga estos pasos para navegar a la página **Configuración de la escalabilidad automática** de su espacio de nombres de Service Bus.

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. En la barra de búsqueda, escriba **Service Bus**, seleccione **Service Bus** en la lista desplegable y presione **Entrar**. 
1. Seleccione el **espacio de nombres premium** en la lista de espacios de nombres. 
1. Cambie a la página **Escala**. 

    :::image type="content" source="./media/automate-update-messaging-units/scale-page.png" alt-text="Espacio de nombres de Service Bus: página Escala":::

## <a name="manual-scale"></a>Escala manual 
Esta configuración permite establecer un número fijo de unidades de mensajería para el espacio de nombres. 

1. En la página **Configuración de la escalabilidad automática**, seleccione **Escala manual** si aún no está seleccionada. 
1. En la configuración **Unidades de mensajería**, seleccione el número de unidades de mensajería en la lista desplegable.
1. Seleccione **Guardar** en la barra de herramientas para guardar la configuración. 

    :::image type="content" source="./media/automate-update-messaging-units/manual-scale.png" alt-text="Espacio de nombres de Service Bus: página Escala":::       


## <a name="custom-autoscale---default-condition"></a>Escalado automático personalizado: condición predeterminada
Puede configurar el escalado automático de unidades de mensajería mediante el uso de condiciones. Esta condición de escalado se ejecuta cuando ninguna de las otras condiciones de escalado coincide. Puede establecer la condición predeterminada de una de las siguientes maneras:

- Escalado basado en una métrica (como el uso de CPU o de memoria)
- Escalado a un número específico de unidades de mensajería

No se puede establecer una programación para el escalado automático en un determinado día o intervalo de fechas para una condición predeterminada. Esta condición de escalado se ejecuta cuando no coincide ninguna de las otras condiciones de escalado con programaciones. 

### <a name="scale-based-on-a-metric"></a>Escalado basado en una métrica
En el procedimiento siguiente se muestra cómo agregar una condición para aumentar automáticamente las unidades de mensajería (escalar horizontalmente) cuando el uso de CPU es superior al 75 % y reducir las unidades de mensajería (reducir horizontalmente) cuando el uso de CPU es inferior al 25 %. Los incrementos se realizan de 1 a 2, de 2 a 4 y de 4 a 8. De forma similar, los decrementos se realizan de 8 a 4, de 4 a 2 y de 2 a 1. 

1. En la página **Configuración de la escalabilidad automática**, seleccione **Escalabilidad automática personalizada** para la opción **Choose how to scale your resource** (Elija cómo escalar el recurso). 
1. En la sección **Valor predeterminado** de la página, especifique un **nombre** para la condición predeterminada. Seleccione el icono de **lápiz** para editar el texto. 
1. Seleccione **Escalado basado en una métrica**, para **Modo de escala**. 
1. Seleccione **+Agregar una regla**. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-add-rule-link.png" alt-text="Espacio de nombres de Service Bus: página Escala":::    
1. En la página **Escalar regla**, siga estos pasos:
    1. Seleccione una métrica de la lista desplegable **Nombre de métrica**. En este ejemplo, es **CPU**. 
    1. Seleccione un operador y los valores de umbral. En este ejemplo, son **Mayor que** y **75** para **Umbral de la métrica para desencadenar la acción de escalado**. 
    1. Seleccione una **operación** en la sección **Acción**. En este ejemplo, se ha establecido en **Aumentar**. 
    1. Luego, seleccione **Agregar**.
    
        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-75.png" alt-text="Espacio de nombres de Service Bus: página Escala":::       

        > [!NOTE]
        > La característica de escalado automático aumenta las unidades de mensajería del espacio de nombres si el uso total de CPU supera el 75 % en este ejemplo. Los incrementos se realizan de 1 a 2, de 2 a 4 y de 4 a 8. 
1. Seleccione **+ Agregar una regla** y siga estos pasos en la página **Escalar regla**:
    1. Seleccione una métrica de la lista desplegable **Nombre de métrica**. En este ejemplo, es **CPU**. 
    1. Seleccione un operador y los valores de umbral. En este ejemplo, son **Menor que** y **25** para **Umbral de la métrica para desencadenar la acción de escalado**. 
    1. Seleccione una **operación** en la sección **Acción**. En este ejemplo, se ha establecido en **Disminuir**. 
    1. Luego, seleccione **Agregar**. 

        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-25.png" alt-text="Espacio de nombres de Service Bus: página Escala":::       

        > [!NOTE]
        > La característica de escalado automático disminuye las unidades de mensajería del espacio de nombres si el uso total de CPU cae por debajo del 25 % en este ejemplo. Los decrementos se realizan de 8 a 4, de 4 a 2 y de 2 a 1. 
1. Establezca el número **mínimo**, **máximo** y **predeterminado** de unidades de mensajería.

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-based.png" alt-text="Espacio de nombres de Service Bus: página Escala":::
1. Seleccione **Guardar** en la barra de herramientas para guardar la configuración de escalado automático. 
        
### <a name="scale-to-specific-number-of-messaging-units"></a>Escalado a un número específico de unidades de mensajería
Siga estos pasos para configurar la regla con el fin de escalar el espacio de nombres para usar un número específico de unidades de mensajería. De nuevo, se aplica la condición predeterminada cuando no coincide ninguna de las otras condiciones de escalado. 

1. En la página **Configuración de la escalabilidad automática**, seleccione **Escalabilidad automática personalizada** para la opción **Choose how to scale your resource** (Elija cómo escalar el recurso). 
1. En la sección **Valor predeterminado** de la página, especifique un **nombre** para la condición predeterminada. 
1. Seleccione **Scale to specific messaging units** (Escalar a un número específico de unidades de mensajería) para **Modo de escala**. 
1. En **Unidades de mensajería**, seleccione el número predeterminado de unidades de mensajería. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-messaging-units.png" alt-text="Espacio de nombres de Service Bus: página Escala":::       

## <a name="custom-autoscale---additional-conditions"></a>Escalado automático personalizado: condiciones adicionales
En la sección anterior se muestra cómo agregar una condición predeterminada a la configuración de escalado automático. En esta sección se muestra cómo agregar más condiciones a la configuración de escalado automático. Para estas condiciones adicionales no predeterminadas, puede establecer una programación basada en días específicos de la semana o en un intervalo de fechas. 

### <a name="scale-based-on-a-metric"></a>Escalado basado en una métrica
1. En la página **Configuración de la escalabilidad automática**, seleccione **Escalabilidad automática personalizada** para la opción **Choose how to scale your resource** (Elija cómo escalar el recurso). 
1. Seleccione **Agregar una condición de escala** en el bloque **Valor predeterminado**. 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Espacio de nombres de Service Bus: página Escala":::    
1. Especifique un **nombre** para la condición. 
1. Confirme que la opción **Escalado basado en una métrica** está seleccionada. 
1. Seleccione **+ Agregar una regla** para agregar una regla a fin de aumentar las unidades de mensajería cuando el uso total de CPU supere el 75 %. Siga los pasos de la sección sobre la [condición predeterminada](#custom-autoscale---default-condition). 
5. Establezca el número **mínimo**, **máximo** y **predeterminado** de unidades de mensajería.
6. También puede establecer una **programación** en una condición personalizada (pero no en la condición predeterminada). Puede especificar las fechas de inicio y finalización de la condición, o seleccionar días específicos (lunes, martes, etc.) de una semana. 
    1. Si selecciona **Especifique las fechas de inicio y finalización**, seleccione la **Zona horaria**, la **Fecha y hora de inicio** y la **Fecha y hora de finalización** (como se muestra en la siguiente imagen) para que la condición esté en vigor. 

       :::image type="content" source="./media/automate-update-messaging-units/custom-min-max-default.png" alt-text="Espacio de nombres de Service Bus: página Escala":::
    1. Si selecciona **Repetir en días específicos**, seleccione los días de la semana, la zona horaria, la hora de inicio y la hora de finalización en que debe aplicarse la condición. 

        :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days.png" alt-text="Espacio de nombres de Service Bus: página Escala":::
  
### <a name="scale-to-specific-number-of-messaging-units"></a>Escalado a un número específico de unidades de mensajería
1. En la página **Configuración de la escalabilidad automática**, seleccione **Escalabilidad automática personalizada** para la opción **Choose how to scale your resource** (Elija cómo escalar el recurso). 
1. Seleccione **Agregar una condición de escala** en el bloque **Valor predeterminado**. 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Espacio de nombres de Service Bus: página Escala":::    
1. Especifique un **nombre** para la condición. 
2. Seleccione la opción **Scale to specific messaging units** (Escalar a un número específico de unidades de mensajería) para **Modo de escala**. 
1. Seleccione el número de **unidades de mensajería** en la lista desplegable. 
6. Para la **programación**, especifique las fechas de inicio y finalización de la condición, o seleccione días específicos (lunes, martes, etc.) de una semana y las horas. 
    1. Si selecciona **Especifique las fechas de inicio y finalización**, seleccione la **Zona horaria**, la **Fecha y hora de inicio** y la **Fecha y hora de finalización** para que la condición esté en vigor. 
    
    :::image type="content" source="./media/automate-update-messaging-units/scale-specific-messaging-units-start-end-dates.png" alt-text="Espacio de nombres de Service Bus: página Escala":::        
    1. Si selecciona **Repetir en días específicos**, seleccione los días de la semana, la zona horaria, la hora de inicio y la hora de finalización en que debe aplicarse la condición.
    
    :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days-2.png" alt-text="Espacio de nombres de Service Bus: página Escala":::

> [!IMPORTANT]
> Para más información sobre cómo funciona la configuración del escalado automático, especialmente cómo se elige un perfil o condición y se evalúan varias reglas, consulte [Información acerca de la configuración de escalado automático](../azure-monitor/platform/autoscale-understanding-settings.md).          

## <a name="next-steps"></a>Pasos siguientes
Para obtener información acerca de las unidades de mensajería, consulte la [mensajería Prémium](service-bus-premium-messaging.md)

