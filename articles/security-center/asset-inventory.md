---
title: Inventario de activos de Azure Security Center
description: Aprenda sobre la experiencia de administración de recursos de Azure Security Center que proporciona visibilidad completa sobre todos los recursos supervisados de Security Center.
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 09/22/2020
ms.service: security-center
ms.topic: how-to
ms.openlocfilehash: d15d73b0f2b87b8e6f66c7bd4e7fb34f6b06e1a0
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92341930"
---
# <a name="explore-and-manage-your-resources-with-asset-inventory-and-management-tools"></a>Explore y administre los recursos con las herramientas de administración e inventario de recursos.

La página de inventario de recursos de Azure Security Center proporciona una sola página para ver la posición de seguridad de los recursos que se han conectado a Security Center. 

Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles puntos vulnerables de la seguridad. A continuación, se proporcionan recomendaciones sobre cómo corregir dichos puntos vulnerables.

Cuando algún recurso tenga recomendaciones pendientes, aparecerán en el inventario.

Use esta vista y sus filtros para abordar preguntas como:

- ¿Cuál de mis suscripciones con Azure Defender habilitado tiene recomendaciones pendientes?
- ¿A cuál de mis máquinas con la etiqueta "producción" le falta el agente de Log Analytics?
- ¿Cuántas máquinas, etiquetadas con una etiqueta específica, tienen recomendaciones pendientes?
- ¿Cuántos recursos de un grupo de recursos específico tienen resultados de seguridad de un servicio de evaluación de vulnerabilidades?

Las posibilidades de administración de recursos de esta herramienta son sustanciales y continúan creciendo. 

> [!TIP]
> Las recomendaciones de seguridad de la página del inventario de recursos son las mismas que las de la página **Recomendaciones**, pero aquí se muestran según el recurso afectado. Para más información sobre cómo resolver las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).


## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponible con carácter general|
|Precios:|Gratuito|
|Roles y permisos necesarios:|Todos los usuarios|
|Nubes:|![Sí](./media/icons/yes-icon.png) Nubes comerciales<br>![No](./media/icons/no-icon.png) Nacionales o soberanas (US Gov, China Gov, otros gobiernos)|
|||


## <a name="what-are-the-key-features-of-asset-inventory"></a>¿Cuáles son las características clave del inventario de recursos?

La página de inventario proporciona las siguientes herramientas:

- **Resúmenes**: antes de definir algún filtro, una franja destacada de valores en la parte superior de la vista de inventario muestra:

    - **Recursos totales**: número total de recursos conectados a Security Center.
    - **Recursos con estado incorrecto** : recursos con recomendaciones de seguridad activas. [Más información sobre las recomendaciones de seguridad](security-center-recommendations.md).
    - **Unmonitored resources** (Recursos no supervisados): recursos con problemas de supervisión del agente; tienen implementado el agente de Log Analytics, pero no envía datos o tiene otros problemas de estado.

- **Filtros**: los distintos filtros de la parte superior de la página proporcionan una manera de refinar rápidamente la lista de recursos según la pregunta que intenta responder. Por ejemplo, si quisiera responder a la pregunta *¿A cuáles de mis máquinas con la etiqueta "producción" les falta el agente de Log Analytics?* , podría combinar el filtro **Agent monitoring** (Supervisión del agente) con el filtro **Etiquetas**, como se muestra en el siguiente clip:

    :::image type="content" source="./media/asset-inventory/filtering-to-prod-unmonitored.gif" alt-text="Filtrado de los recursos de producción que no están supervisados":::

    En cuanto haya aplicado filtros, los valores de resumen se actualizarán para relacionarlos con los resultados de la consulta. 

- **Opciones de exportación**: el inventario ofrece la opción de exportar los resultados de las opciones de filtro seleccionadas a un archivo CSV. Además, puede exportar la consulta propiamente dicha a Resource Graph Explorer para refinar aún más, guardar o modificar la consulta de KQL.

    ![Opciones de exportación del inventario](./media/asset-inventory/inventory-export-options.png)

    > [!TIP]
    > La documentación de KQL proporciona una base de datos con algunos datos de ejemplo junto con algunas consultas sencillas para saber cómo funciona el lenguaje. [Más información en este tutorial de KQL](/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer).

- **Opciones de administración de recursos**: el inventario le permite realizar consultas de detección complejas. Cuando haya encontrado los recursos que coinciden con las consultas, el inventario proporciona accesos directos a operaciones como:

    - Asignación de etiquetas a los recursos filtrados: active las casillas junto a los recursos que quiere etiquetar.
    - Incorporación de nuevos servidores a Security Center: use el botón de la barra de herramientas **Agregar servidores que no son de Azure**.
    - Automatización de cargas de trabajo con Azure Logic Apps: use el botón **Desencadenar aplicación lógica** para ejecutar una aplicación lógica en uno o varios recursos. Las aplicaciones lógicas deben prepararse de antemano y aceptar el tipo de desencadenador correspondiente (solicitud HTTP). [Más información sobre las aplicaciones lógicas](../logic-apps/logic-apps-overview.md)


## <a name="how-does-asset-inventory-work"></a>¿Cómo funciona el inventario de recursos?

El inventario de recursos emplea [Azure Resource Graph (ARG)](../governance/resource-graph/index.yml), un servicio de Azure que proporciona la capacidad de consultar los datos de la posición de seguridad de Security Center en varias suscripciones.

ARG está diseñado para proporcionar una exploración de recursos eficaz con la posibilidad de realizar consultas a escala.

Con el [lenguaje de consulta de Kusto (KQL)](/azure/data-explorer/kusto/query/), el inventario de recursos puede generar rápidamente información detallada gracias a la referencia cruzada de los datos de ASC con otras propiedades de recursos.


## <a name="how-to-use-asset-inventory"></a>Uso del inventario de recursos

1. En la barra lateral de Security Center, seleccione **Inventario**.

1. Use el cuadro **Filtrar por nombre** para mostrar un recurso específico o use los filtros como se describe a continuación.

1. Seleccione las opciones correspondientes de los filtros para crear la consulta específica que quiere realizar.

    :::image type="content" source="./media/asset-inventory/inventory-filters.png" alt-text="Filtrado de los recursos de producción que no están supervisados" lightbox="./media/asset-inventory/inventory-filters.png":::

    De forma predeterminada, los recursos se ordenan por el número de recomendaciones de seguridad activas.

    > [!IMPORTANT]
    > Las opciones de cada filtro son específicas de los recursos de las suscripciones seleccionadas actualmente **y** de las selecciones de los otros filtros.
    >
    > Por ejemplo, si ha seleccionado solo una suscripción y esta no tiene ningún recurso con recomendaciones de seguridad pendientes de corregir (0 recursos incorrectos), el filtro **Recomendaciones** no tendrán ninguna opción. 

1. Para usar el filtro **Security findings contain** (Las conclusiones de seguridad contienen), escriba texto libre para el identificador, la comprobación de seguridad o el nombre de CVE de una conclusión de vulnerabilidad para filtrar los recursos afectados:

    ![Filtro "Security findings contain" (Las conclusiones de seguridad contienen)](./media/asset-inventory/security-findings-contain-elements.png)

    > [!TIP]
    > Los filtros **Security findings contain** (Las conclusiones de seguridad contienen) y **Etiquetas** solo aceptan un único valor. Para filtrar por más de uno, use **Agregar filtros**.

1. Para usar el filtro **Azure Defender**, seleccione una o varias opciones (Desactivado, Activado o Parcial):

    - **Desactivado**: recursos que no están protegidos por un plan de Azure Defender. Puede hacer clic con el botón derecho en cualquiera de ellos y actualizarlos:

        :::image type="content" source="./media/asset-inventory/upgrade-resource-inventory.png" alt-text="Filtrado de los recursos de producción que no están supervisados" lightbox="./media/asset-inventory/upgrade-resource-inventory.png":::

    - **Activado**: recursos que están protegidos por un plan de Azure Defender.
    - **Parcial**: esto se aplica a **suscripciones** que tienen algunos planes de Azure Defender deshabilitados, pero no todos. Por ejemplo, la siguiente suscripción tiene cinco planes de Azure Defender deshabilitados. 

        :::image type="content" source="./media/asset-inventory/pricing-tier-partial.png" alt-text="Filtrado de los recursos de producción que no están supervisados":::

1. Para examinar más detalladamente los resultados de la consulta, seleccione los recursos que le interesen.

1. Para ver las opciones de filtro seleccionadas actualmente como una consulta en Resource Graph Explorer, seleccione **Ver en Resource Graph Explorer**.

    ![Consulta de inventario en ARG](./media/asset-inventory/inventory-query-in-resource-graph-explorer.png)

1. Para ejecutar una aplicación lógica definida previamente con 

1. Si ha definido algunos filtros y ha dejado la página abierta, Security Center no actualizará los resultados de forma automática. Cualquier cambio en los recursos no afectará a los resultados mostrados a menos que se vuelva a cargar manualmente la página o se seleccione **Actualizar**.


## <a name="faq---inventory"></a>Preguntas frecuentes sobre el inventario

### <a name="why-arent-all-of-my-subscriptions-machines-storage-accounts-etc-shown"></a>¿Por qué no se muestran todas las suscripciones, máquinas, cuentas de almacenamiento, etc.?

En la vista de inventario se muestran los recursos conectados de Security Center desde la perspectiva de la administración de la posición de seguridad en la nube (CSPM). Los filtros no devuelven todos los recursos del entorno; solo los que tengan recomendaciones pendientes (o "activas"). 

Por ejemplo, en la captura de pantalla siguiente se muestra un usuario con acceso a 38 suscripciones, pero solo 10 tienen recomendaciones actualmente. Por tanto, cuando se filtran por **Tipo de recurso = Suscripciones**, solo las 10 suscripciones con recomendaciones activas aparecen en el inventario:

:::image type="content" source="./media/asset-inventory/filtered-subscriptions-some.png" alt-text="Filtrado de los recursos de producción que no están supervisados":::

### <a name="why-do-some-of-my-resources-show-blank-values-in-the-azure-defender-or-agent-monitoring-columns"></a>¿Por qué algunos de mis recursos muestran valores en blanco en las columnas de supervisión del agente o de Azure Defender?

No todos los recursos supervisados de Security Center tienen agentes. Por ejemplo, no los tienen las cuentas de Azure Storage ni los recursos PaaS, como discos, Logic Apps, Data Lake Analytics y Event Hubs.

Cuando el precio o la supervisión del agente no son pertinentes para un recurso, no se mostrará nada en esas columnas de inventario.

:::image type="content" source="./media/asset-inventory/agent-pricing-blanks.png" alt-text="Filtrado de los recursos de producción que no están supervisados":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo se describe la página del inventario de recursos de Azure Security Center.

Para más información sobre las herramientas relacionadas, consulte las siguientes páginas:

- [Azure Resource Graph (AGR)](../governance/resource-graph/index.yml)
- [Lenguaje de consulta de Kusto (KQL)](/azure/data-explorer/kusto/query/)