---
title: Consultas guardadas en Log Analytics de Azure Monitor
description: Consultas desde las que puede empezar y modificar según las necesidades
ms.subservice: logs
ms.topic: article
author: rboucher
ms.author: robb
ms.date: 06/16/2020
ms.openlocfilehash: 462358ab967d4ba26a265d7b5f401b995223815e
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959849"
---
# <a name="saved-queries-in-azure-monitor-log-analytics"></a>Consultas guardadas en Log Analytics de Azure Monitor

Log Analytics ofrece conjuntos de consultas de ejemplo que puede ejecutar de forma independiente o usar como punto de partida para consultas propias. En este artículo se describen las consultas de ejemplo y cómo usarlas.

Si no está familiarizado con Log Analytics o con el lenguaje de consulta KQL, las consultas de ejemplo son una excelente manera de empezar. Pueden proporcionar conclusiones instantáneas sobre un recurso y constituyen una manera cómoda de empezar a aprender y usar KQL, lo que reduce el tiempo que se tarda en empezar a usar Log Analytics. Hemos recopilado y seleccionado más de 250 consultas de ejemplo diseñadas para proporcionar un valor instantáneo y el número crece continuamente.

## <a name="in-context-queries"></a>Consultas en contexto

La nueva experiencia filtra y sugiere consultas en contexto. Es decir, el sistema muestra de forma automática solo las consultas pertinentes para el ámbito que haya seleccionado.

- Para un **solo recurso**: las consultas se filtran según el tipo de recurso.
- Para un **grupo de recursos**: las consultas se filtran según los recursos del grupo de recursos específico.
- Para un **área de trabajo**: las consultas se filtran según las soluciones instaladas en el área de trabajo.

Este comportamiento es coherente para todos los ámbitos de Log Analytics. Si no ve una consulta de ejemplo para el tipo de recurso que quiere, es posible que se deba a filtros locales. En una sección posterior se describe cómo quitar el ámbito local para que pueda ver todas las consultas posibles.

> [!TIP]
> Cuantos más recursos tenga en el ámbito, más tiempo tardará el portal en filtrar y mostrar el cuadro de diálogo de la consulta de ejemplo.

## <a name="example-query-user-interface"></a>Interfaz de usuario de la consulta de ejemplo

Puede obtener consultas de ejemplo de dos ubicaciones diferentes.

### <a name="example-query-dialog"></a>Cuadro de diálogo de la consulta de ejemplo

La primera vez que acceda a la experiencia de Log Analytics, se muestra de forma automática el *cuadro de diálogo Consultas de ejemplo*.  También se puede acceder si hace clic en la parte superior derecha de la pantalla en **Consultas de ejemplo**.

![Barra lateral](media/saved-queries/sidebar-2.png)

El cuadro de diálogo de la consulta de ejemplo aparece como se muestra a continuación:  

![Pantalla Consultas de ejemplo](media/saved-queries/example-query-start.png)

En la captura de pantalla anterior se muestra la pantalla de registros de una instancia de Azure Key Vault. Como se ha mencionado antes en este artículo, las consultas se muestran en contexto.  Como resultado, en la captura de pantalla solo se muestran ejemplos relacionados con Key Vault. Si selecciona una suscripción completa, se muestran las consultas de todos los tipos de recursos de esa suscripción.  

Cada consulta de ejemplo se representa mediante una tarjeta. Puede examinar rápidamente las consultas para encontrar lo que necesita. Puede ejecutar la consulta directamente desde el cuadro de diálogo o elegir cargarla en el editor de consultas para ajustarla más.

### <a name="sidebar-query-experience"></a>Experiencia de la barra lateral

Se puede acceder a la misma funcionalidad de la experiencia del cuadro de diálogo desde el panel Consultas de la barra lateral izquierda de Log Analytics. Puede mantener el mouse sobre el nombre de una consulta para obtener su descripción y funciones adicionales.

![Barra lateral](media/saved-queries/sidebar-3.png)

## <a name="finding-and-filtering-queries"></a>Búsqueda y filtrado de consultas

Las opciones de esta sección están disponibles en la experiencia de consulta del cuadro de diálogo y la barra lateral, pero con una interfaz de usuario ligeramente diferente.  

### <a name="use-favorites"></a>Uso de favoritos

Puede agregar como favoritas las consultas que use con frecuencia para disponer de un acceso más rápido.

> [!TIP]
> Recopilar y ver las consultas con posterioridad es una buena manera de empezar. Busque las consultas que necesita y haga clic en la estrella situada junto a la consulta para agregarla a los favoritos. Si más adelante descubre que la consulta no le resulta útil, puede quitarla de los favoritos.  

### <a name="filtering-and-group-by"></a>Filtrar y Agrupar por

Aunque la experiencia del cuadro de diálogo de consulta filtra para mostrar solo las consultas con el contexto correcto, puede optar por quitar el filtro y verlas todas.

### <a name="group-by"></a>Agrupar por

Para cambiar la agrupación de las consultas, haga clic en la lista desplegable *Agrupar por*:

![Agrupar por en la pantalla de consultas de ejemplo](media/saved-queries/example-query-groupby.png)

El cuadro de diálogo admite la agrupación por:

- **Tipo de recurso**: un recurso como se define en Azure, como una máquina virtual. Vea la [Referencia de tabla de Azure Monitor](/azure/azure-monitor/reference/tables/tables-resourcetype) para obtener una asignación completa entra las tablas de registros de Azure Monitor y Log Analytics, y el tipo de recurso.  
- **Categoría**: un tipo de información como *Seguridad* o *Auditoría*. Las categorías son idénticas a las que se definen en el panel lateral Tablas. Para obtener una lista completa de las categorías, vea la [ referencia de tabla de Azure Monitor](/azure/azure-monitor/reference/tables/tables-category).  
- **Solución**: una solución de Azure Monitor asociada a las consultas
- **Tema**: el tema de la consulta de ejemplo, como *Registros de actividad* o *Registros de aplicación*. La propiedad de tema es exclusiva de las consultas de ejemplo y puede diferir según el tipo de recurso específico.

Los valores de agrupación también actúan como una tabla de contenido activa. Al hacer clic en uno de los valores en el lado izquierdo de la pantalla, la vista de consultas se desplaza directamente al elemento en el que haya hecho clic.

### <a name="filter"></a>Filter

También puede filtrar las consultas según los valores de agrupación mencionados antes. En el cuadro de diálogo de la consulta de ejemplo, los filtros se encuentran en la parte superior.

![Filtro en la pantalla de consultas de ejemplo](media/saved-queries/example-query-filter.png)

### <a name="combining-group-by-and-filter"></a>Combinación de Agrupar por y Filtrar

La funcionalidad de filtrar y agrupar por está diseñada para trabajar de manera conjunta. Proporcionan flexibilidad en el modo de organizar las consultas. Por ejemplo, si usa un grupo de recursos con varios recursos, es posible que quiera filtrar por un tipo de recurso específico y organizar las consultas resultantes por tema.

## <a name="sample-query-dialog-appearance-behavior"></a>Comportamiento de la apariencia del cuadro de diálogo de consulta de ejemplo

Si es un profesional de KQL y prefiere ir directamente al editor de consultas, puede "desactivar" el cuadro de diálogo de consulta. Al desactivarlo, el cuadro de diálogo de consulta de ejemplo no se abre al cargar la pantalla de Log Analytics.

![Activación y desactivación de los ejemplos](media/saved-queries/examples-on-off.png)

Siempre puede acceder a la experiencia del cuadro emergente de la consulta de ejemplo desde el botón *Consultas de ejemplo* de la barra superior de Log Analytics.

## <a name="query-explorer"></a>Explorador de consultas

La experiencia del explorador de consultas para guardar y compartir consultas generadas por el usuario se conserva sin cambios por el momento.

## <a name="next-steps"></a>Pasos siguientes

[Introducción a las consultas KQL](get-started-queries.md)

