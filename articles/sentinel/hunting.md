---
title: Capacidades de búsqueda en Azure Sentinel| Microsoft Docs
description: Use las consultas de búsqueda integradas de Azure Sentinel a modo de guía para formular las preguntas adecuadas que lleven a detectar problemas en los datos.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/10/2019
ms.author: yelevin
ms.openlocfilehash: cceeb1fc9715dfd7b1ac714c424421c51d292757
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85552281"
---
# <a name="hunt-for-threats-with-azure-sentinel"></a>Búsqueda de amenazas con Azure Sentinel

Si es un investigador que quiere ser proactivo a la hora de buscar amenazas de seguridad, use las eficaces herramientas de búsqueda y consulta de Azure Sentinel para encontrar amenazas de seguridad en cualquier origen de datos de la organización. Pero los sistemas y dispositivos de seguridad generan infinidad de datos que pueden ser difíciles de analizar y filtrar para hallar eventos significativos. Para ayudar a los analistas de seguridad a buscar proactivamente nuevas anomalías que las aplicaciones de seguridad no han sido capaces de identificar, las consultas de búsqueda integradas de Azure Sentinel servirán de guía para formular las preguntas adecuadas para detectar problemas en los datos que ya hay en la red. 

Por ejemplo, una consulta integrada proporciona datos sobre los procesos menos habituales que se ejecutan en la infraestructura; no tiene sentido recibir una alerta cada vez que estos procesos se ejecuten (pueden ser totalmente inofensivos), pero sí conviene echar un vistazo a la consulta de vez en cuando para ver si hay algo inusual. 



Con la función de búsqueda de Azure Sentinel, puede aprovechar las siguientes capacidades:

- Consultas integradas: para ayudarle a comenzar, una página de inicio proporciona ejemplos precargados de consultas diseñadas para que pueda empezar a trabajar y a familiarizarse con las tablas y el lenguaje de consulta. Estas consultas de búsqueda integradas están desarrolladas por investigadores de seguridad de Microsoft, y lo hacen de forma continua, agregando nuevas consultas y ajustando las consultas existentes para que sean un punto de entrada para buscar nuevas detecciones y averiguar dónde empezar a buscar comienzos de nuevos ataques. 

- Lenguaje de consulta eficaz con IntelliSense: se basa en un lenguaje de consulta que le reporta la flexibilidad que necesita para llevar la búsqueda al siguiente nivel.

- Creación de marcadores propios: durante el proceso de búsqueda, puede encontrar coincidencias, hallazgos, paneles o actividades que tienen un aspecto sospechoso o poco usual. A fin de marcar esos elementos de forma que se pueda volver a ellos en el futuro, utilice la funcionalidad de marcador. Los marcadores permiten guardar los elementos para usarlos más adelante para crear un incidente de investigación. Para obtener más información sobre los marcadores, vea el tema sobre el [uso de marcadores de búsqueda](hunting.md).
- Uso de cuadernos para automatizar la investigación: los cuadernos son cuadernos de estrategias pormenorizados que se pueden crear para recorrer paso a paso un proceso investigación y de búsqueda.  Los cuadernos aglutinan todos los pasos de búsqueda en un cuaderno de estrategias reutilizable que se puede compartir con otras personas de la organización. 
- Consulta de los datos almacenados: los datos están accesibles en las tablas para que se puedan consultar. Así, por ejemplo, se puede consultar la creación de procesos, los eventos DNS y otros muchos tipos de eventos.

- Vínculos a la comunidad: saque partido de la tremenda eficacia de la comunidad más amplia para acceder a más consultas y orígenes de datos.
 
## <a name="get-started-hunting"></a>Empezar a realizar búsquedas

1. En el portal de Azure Sentinel, haga clic en **Hunting** (Búsqueda).
  ![Azure Sentinel empieza a buscar](media/tutorial-hunting/hunting-start.png)

2. Al abrir la página **Hunting** (Búsqueda), todas las consultas de búsqueda se muestran en una misma tabla. En esa tabla se enumeran todas las consultas escritas por el equipo de analistas de seguridad de Microsoft, así como otras consultas adicionales que haya creado o modificado. En cada consulta se proporciona una descripción de lo que se busca y en qué tipo de datos se ejecuta. Estas plantillas se agrupan según diversas tácticas; así, los iconos de la derecha clasifican el tipo de amenaza, como acceso inicial, persistencia y filtración. Estas plantillas de consulta de búsqueda se pueden filtrar mediante cualquiera de los campos. Puede guardar cualquier consulta en sus favoritos. Al guardar una consulta en favoritos, esta se ejecuta automáticamente cada vez que se acceda a la página **Hunting** (Búsqueda). Puede crear sus propias consultas de búsqueda o clonar y personalizar una plantilla de consulta de búsqueda ya existente. 
 
2. Haga clic en **Run query** (Ejecutar consulta) en la página de detalles de la consulta de búsqueda para ejecutar la consulta que quiera sin salir de la página de búsquedas.  El número de coincidencias encontradas se muestra en la tabla. Revise la lista de consultas de búsqueda y las coincidencias encontradas correspondientes. Compruebe con qué fase del ciclo de ciberataque está asociada una coincidencia.

3. Revise rápidamente la consulta subyacente en el panel de detalles de la consulta o haga clic en **View query result** (Ver resultado de la consulta) para abrir la consulta en Log Analytics. Al final, revise a las coincidencias encontradas relativas a la consulta.

4.    Haga clic en la fila y seleccione **Add bookmark** (Agregar marcador) para agregar las filas que se van a investigar. Esto se puede realizar con cualquier elemento que considere sospechoso. 

5. Luego, vuelva a la página principal **Hunting** (Búsqueda) y haga clic en la pestaña **Bookmarks** (Marcadores) para ver todas las actividades sospechosas. 

6. Seleccione un marcador y haga clic en **Investigate** (Investigar) para abrir la experiencia de investigación. Los marcadores se pueden filtrar. Por ejemplo, si está investigando una campaña, puede crear una etiqueta de la campaña y, después, filtrar todos los marcadores por esa campaña.

1. Una vez que ha descubierto qué consulta de búsqueda proporciona las conclusiones más valiosas sobre posibles ataques, también puede crear reglas de detección personalizadas basadas en la consulta y exponer esas conclusiones como alertas para los respondedores a los incidentes de seguridad.

 

## <a name="query-language"></a>Lenguaje de consulta 

Las búsquedas de Azure Sentinel se basan en el lenguaje de consultas de Kusto. Para obtener más información sobre el lenguaje de consulta y los operadores admitidos, consulte la [referencia de idioma de consulta](/azure/azure-monitor/log-query/get-started-queries).

## <a name="public-hunting-query-github-repository"></a>Repositorio de GitHub de consultas de búsquedas públicas

Eche un vistazo al [repositorio de consultas de búsqueda](https://github.com/Azure/Orion). Use las consultas de ejemplo compartidas por nuestros clientes y aporte las suyas propias.

 

## <a name="sample-query"></a>Consulta de ejemplo

Una consulta típica empieza por un nombre de tabla, seguido de una serie de operadores separados por \|.

En el ejemplo anterior, empiece con la tabla SecurityEvent y agregue elementos canalizados según sea necesario.

1. Defina un filtro de tiempo para revisar solo los registros de los últimos siete días.

2. Agregue un filtro a la consulta para mostrar únicamente el evento con el identificador 4688.

3. Agregue un filtro a la consulta en CommandLine para que contenga solo instancias de cscript.exe.

4. Proyecte solo las columnas que esté interesado en explorar, limite los resultados a 1000 y haga clic en **Run query** (Ejecutar consulta).
5. Haga clic en el triángulo verde y ejecute la consulta. Puede comprobar la consulta y ejecutarla para hallar comportamientos anómalos.

## <a name="useful-operators"></a>Operadores útiles

El lenguaje de consulta es muy eficaz y cuenta con numerosos operadores disponibles. Estos son algunos de operadores útiles:

**where**: filtra una tabla por el subconjunto de filas que cumplen un predicado.

**summarize**: crea una tabla que agrega el contenido de la tabla de entrada.

**join**: combina las filas de dos tablas para formar una nueva tabla, haciendo coincidir los valores de las columnas especificadas de cada tabla.

**count**: devuelve el número de registros en el conjunto de registros de entrada.

**top**: devuelve los primeros N registros ordenados por las columnas especificadas.

**limit**: devuelve hasta el número de filas especificado.

**project**: selecciona las columnas que se incluirán, las cambia de nombre o las quita e inserta nuevas columnas calculadas.

**extend**: crea columnas calculadas y las anexa al conjunto de resultados.

**makeset**: devuelve una matriz dinámica (JSON) del conjunto de valores distintos que una expresión toma en el grupo.

**find**: busca filas que coinciden con un predicado a través de un conjunto de tablas.

## <a name="save-a-query"></a>Almacenamiento de una consulta

Puede crear o modificar una consulta y guardarla como consulta propia o compartirla con los usuarios que están en el mismo inquilino.

   ![Guardar consulta](./media/tutorial-hunting/save-query.png)

Cree una consulta de búsqueda:

1. Haga clic en **New query** (Nueva consulta) y seleccione **Save** (Guardar).
2. Rellene todos los campos en blanco y seleccione **Save** (Guardar).

   ![Nueva consulta](./media/tutorial-hunting/new-query.png)

Clone y modifique una consulta de búsqueda existente:

1. Seleccione la consulta de búsqueda en la tabla que quiere modificar.
2. Seleccione el botón de puntos suspensivos (...) en la línea de la consulta que quiere modificar y seleccione **Clone query** (Clonar consulta).

   ![Clonar una consulta](./media/tutorial-hunting/clone-query.png)
 

3. Modifique la consulta y seleccione **Create** (Crear).

   ![Consulta personalizada](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a realizar una investigación de búsqueda en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:


- [Uso de cuadernos de Jupyter Notebook para buscar amenazas de seguridad](notebooks.md)
- [Realizar un seguimiento de los datos durante una búsqueda](bookmarks.md)
