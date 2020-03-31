---
title: Creación de reglas de análisis personalizadas para detectar amenazas sospechosas con Azure Sentinel | Microsoft Docs
description: Use este tutorial para aprender a crear reglas de análisis personalizadas para detectar amenazas sospechosas con Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: cea7429ecea105355b0afe306bfa334e55d5d9c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585114"
---
# <a name="tutorial-create-custom-analytic-rules-to-detect-suspicious-threats"></a>Tutorial: Creación de reglas de análisis personalizadas para detectar amenazas sospechosas

Después de [conectar los orígenes de datos](quickstart-onboard.md) con Azure Sentinel, puede crear reglas personalizadas que busquen criterios específicos en el entorno y generen incidentes si se detectan estos criterios para que pueda investigarlos. Este tutorial le ayuda a crear reglas personalizadas para detectar amenazas con Azure Sentinel.

Este tutorial ayuda a detectar amenazas con Azure Sentinel.
> [!div class="checklist"]
> * Creación de reglas de análisis
> * Automatizar las respuestas frente a amenazas

## <a name="create-custom-analytic-rules"></a>Creación de reglas de análisis personalizadas

Puede crear reglas de análisis personalizadas que le ayuden a buscar los tipos de amenazas y anomalías que sean sospechosas en el entorno. La regla garantiza que se le notificará de inmediato, de modo que pueda evaluar, investigar y corregir las amenazas.

1. En Azure Portal en Azure Sentinel, seleccione **Analytics**.

1. En la barra de menús superior, seleccione **+Crear** y seleccione**Regla de consulta programada**. Así se abre el **Asistente para reglas de Analytics**.

    ![Crear una consulta programada](media/tutorial-detect-threats-custom/create-scheduled-query.png)

1. En la pestaña **General**, especifique unos valores únicos en los campos **Name** (Nombre) y **Description** (Descripción). En el campo **Tactics** (Táctica), puede elegir cualquiera de las categorías de ataques por las que se clasifica la regla. Establezca la **gravedad** según sea necesario. Cuando cree la regla, el valor predeterminado del campo **Status** (Estado) es **Enabled** (Habilitado), lo que significa que se ejecutará inmediatamente después de que termine de crearla. Si no desea ejecutarla de inmediato, seleccione **Disabled** (Deshabilitado) para agregar la regla a la pestaña **Active rules** (Reglas activas), desde donde podrá habilitarla cuando sea necesario.

    ![Empezar a crear una regla de análisis personalizada](media/tutorial-detect-threats-custom/general-tab.png)

1. En la pestaña **Set rule logic** (Establecer lógica de regla), puede escribir una consulta directamente en el campo **Rule query** (Consulta de búsqueda), o bien crearla en Log Analytics y, después, copiarla y pegarla ahí.
 
   ![Crear una consulta en Azure Sentinel](media/tutorial-detect-threats-custom/settings-tab.png)

   - Vea el área **Results preview** (Vista previa de resultados) de la derecha, donde Azure Sentinel muestra el número de resultados (eventos de registro) que va a generar la consulta, lo que cambia sobre la marcha mientras se escribe y configura la consulta. El gráfico muestra el número de resultados en un periodo definido, lo que determina la configuración de la sección **Query scheduling** (Programación de consultas).
    - Si ve que su consulta desencadena demasiadas alertas o alertas demasiado frecuentes, puede establecer una base de referencia en la sección **Alert threshold** (Umbral de alerta).

      Aquí tiene una consulta de ejemplo que le alertará cuando se cree una cantidad anómala de recursos en Azure Activity.

      `AzureActivity
     \| where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
     \| where ActivityStatus == "Succeeded"
     \| make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller`

      > [!NOTE]
      > La longitud de la consulta debe estar entre 1 y 10 000 caracteres y no puede contener las palabras "buscar \*" ni "unión \*".

    1. Use la sección **Map entities** (Asignar entidades) para vincular parámetros de los resultados de las consultas a entidades reconocidas de Azure Sentinel. Estas entidades forman la base del posterior análisis, lo que incluye la agrupación de alertas en incidentes en la pestaña **Incident settings** (Configuración de incidentes).
    1. En la sección **Query scheduling** (Programación de consultas), establezca los siguientes parámetros:

       1. Establezca la opción **Run query every** (Ejecutar consulta cada) para controlar la frecuencia de ejecución de la consulta (puede establecer frecuencias de 5 minutos o de una vez al día).

       1. Set **Lookup data from the last** (Datos de la búsqueda a partir del último) para determinar el periodo de los datos que cubre la consulta (por ejemplo, puede consultar los 10 últimos minutos de datos o las 6 últimas horas de datos).

       > [!NOTE]
       > Estos dos valores son independientes entre sí, hasta cierto punto. Puede ejecutar una consulta a un intervalo corto que abarque un periodo mayor que el intervalo (teniendo en efecto consultas que se solapan), pero no puede ejecutar una consulta a un intervalo que supere el periodo de cobertura, ya que, de lo contrario tendrá lagunas en la cobertura general de la consulta.

    1. Use la sección **Alert threshold** (Umbral de alerta) para definir una base de referencia. Por ejemplo, en **Generate alert when number of query results** (Generar una alerta cuando el número de resultados de la consulta), seleccione **Is greater than** (Es mayor que) y escriba el número 1000 si desea que la regla genere una alerta solo si la consulta genera más de 1000 resultados cada vez que se ejecuta. Al ser este un campo obligatorio, si no desea establecer una base de referencia (es decir, si desea que su alerta registre todos los eventos) escriba 0 en el campo numérico.

    1. In the **Suppression** (Supresión), en el valor **Stop running query after alert is generated** (Detener la ejecución de la consulta después de que se genere una alerta), seleccione **On** (Activar) si, una vez que recibe la alerta, desea suspender la operación de esta reglar durante un periodo que supere el intervalo de consulta. Si lo activa, en **Stop running query for** (Detener la ejecución de la consulta durante), seleccione el periodo durante el que debe detenerse la consulta, con un máximo de 24 horas.

1. En la pestaña **Incident Settings** (Configuración de incidentes), puede elegir si Azure Sentinel convierte las alertas en incidentes sobre los que se pueden realizar acciones y cómo lo hace. Si esta pestaña no se modifica, Azure Sentinel creará un solo incidente independiente a partir de todas y cada una de las alertas. Puede elegir que no se creen incidentes o agrupar varias alertas en un solo incidente. Para ello, solo debe cambiar el valor de esta pestaña.

    1. En la sección **Incident Settings** (Configuración de incidentes), el valor predeterminado de **Create incidents from alerts triggered by this analytics rule** (Crear incidentes a partir de alertas desencadenadas por esta regla de análisis) es **Enabled** (Habilitado), lo que significa que Azure Sentinel creará un solo incidente independiente por cada una de las alertas que desencadene la regla.<br></br>Si no desea que esta regla provoque la aparición de incidentes (por ejemplo, si esta regla es solo para recopilar información para su posterior análisis), seleccione **Disabled** (Deshabilitado).

    1. En la sección **Alert grouping** (Agrupación de alertas), si desea que se genere un solo incidente a partir de un grupo de alertas similares o recurrentes, en **Group related alerts, triggered by this analytics rule, into incidents** (Agrupar en incidentes alertas relacionadas desencadenadas por esta regla de análisis) seleccione **Enabled** (Habilitado) y establezca los siguientes parámetros.

    1. **Limite el grupo a las alertas creadas en el período de tiempo seleccionado**:<br></br> Determine el período de tiempo en el que se agruparán las alertas similares o periódicas. Todas las alertas correspondientes que se encuentren dentro de este periodo de tiempo generarán colectivamente un incidente o un conjunto de incidentes (en función de la configuración de agrupación que encontrará a continuación). Las alertas que aparezcan fuera de este periodo de tiempo generarán un incidente, o conjunto de incidentes, independientes.

    2. **Agrupe las alertas desencadenadas por esta regla de análisis en un único incidente por**: elija el motivo por el que se agruparán las alertas:

        - **Agrupar las alertas en un solo incidente si todas las entidades coinciden**: <br></br>Las alertas se agrupan si comparten los mismos valores entre todas las entidades asignadas (definidas en la pestaña Set rule logic [Establecer lógica de regla] anterior). Éste es el valor recomendado.

        - **Agrupar todas las alertas desencadenadas por esta regla en un único incidente**: <br></br>Todas las alertas que genera esta regla se agrupan aunque no compartan valores idénticos.

        - **Agrupar las alertas en un solo incidente si las entidades seleccionadas coinciden**: <br></br>Las alertas se agrupan si comparten valores idénticos en algunas de las entidades asignadas (que puede seleccionar en la lista desplegable). Es posible que desee usar esta opción si, por ejemplo, desea crear incidentes independientes basados en las direcciones IP de origen o de destino.

    3. **Vuelva a abrir los incidentes coincidentes cerrados**: si se ha cerrado un incidente (lo que significa que se ha resuelto el problema subyacente) y, posteriormente, se genera otra alerta que se habría agrupado en ese incidente, establezca esta opción en **Enabled** (Habilitado) si desea que el incidente cerrado se vuelva a abrir, o bien déjela como **Disabled** (Deshabilitado) si desea que la alerta cree un incidente nuevo.

1. En la pestaña **Automated responses** (Respuestas automatizadas), seleccione todos los cuadernos de estrategias que desee ejecutar automáticamente cuando la regla personalizada genere una alerta. Para más información sobre la creación y automatización de los cuadernos de estrategias, consulte [Respuesta a amenazas](tutorial-respond-threats-playbook.md).

1. Seleccione **Review and create** (Revisar y crear) para revisar todos los valores de la nueva regla de alertas y, después, seleccione **Create to initialize your alert rule** (Crear para inicializar la regla de alertas).
  
1. Una vez creada la alerta, se agrega una regla personalizada a la tabla en **Active rules** (Reglas activas). Desde esta lista puede habilitar, deshabilitar o eliminar cada regla.

1. Para ver los resultados de las reglas de alertas, vaya a la página de **incidentes**, donde puede evaluar las prioridades,  [investigar los incidentes](tutorial-investigate-cases.md) y solucionar las amenazas.


> [!NOTE]
> Las alertas generadas en Azure Sentinel están disponibles mediante  [Microsoft Graph Security](https://aka.ms/securitygraphdocs). Para más información, consulte la [documentación sobre alertas de Microsoft Graph Security](https://aka.ms/graphsecurityreferencebetadocs).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a detectar amenazas casos mediante Azure Sentinel.

Para aprender a automatizar las respuestas a las amenazas, consulte  [Configuración de respuestas automatizadas frente a amenazas en Azure Sentinel](tutorial-respond-threats-playbook.md).

