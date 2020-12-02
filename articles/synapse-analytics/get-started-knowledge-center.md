---
title: 'Tutorial: Introducción a la exploración del Centro de conocimientos de Synapse'
description: En este tutorial, aprenderá a usar el Centro de conocimientos de Synapse.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 11/16/2020
ms.openlocfilehash: 0314f5e55a476cd1cd1f8a31bd918e0ebb64c75f
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95254245"
---
# <a name="explore-the-synapse-knowledge-center"></a>Exploración del Centro de conocimientos de Synapse

En este tutorial, aprenderá a usar el Centro de conocimientos de Synapse Studio.

## <a name="getting-to-the-knowledge-center"></a>Búsqueda del Centro de conocimientos

Hay dos formas de buscar el Centro de conocimientos en Synapse Studio:

  1. En el centro de conectividad de inicio, cerca de la parte superior derecha de la página, haga clic en **Obtener información**.
  2. En la barra de menú de la parte superior, haga clic en **?** y, a continuación, en **Centro de conocimientos**.

Elija cualquier método y abra el **Centro de conocimientos**.

## <a name="overview"></a>Información general

El **Centro de conocimientos** le permite hacer tres cosas:
* **Usar ejemplos inmediatamente**. Si quiere un ejemplo rápido de cómo funciona Synapse, elija esta opción.
* **Examinar la galería**. Esta opción le permite vincular conjuntos de datos de ejemplo y agregar código de ejemplo en forma de scripts de SQL, cuadernos y canalizaciones.
* **Visita por Synapse Studio**. Esta opción le permite realizar una breve visita por las partes básicas de Synapse Studio. Resulta útil si nunca ha usado Synapse Studio.

## <a name="exploring-blob-storage-with-serverless-sql-pool"></a>Exploración del almacenamiento de blobs con un grupo de SQL sin servidor

1. Vaya al **Centro de conocimientos**, haga clic en **Usar ejemplos inmediatamente**.
1. Seleccione **Consultar datos con SQL**. 
1. Haga clic en **Usar ejemplos inmediatamente**.
1. Se creará un nuevo script de SQL.
1. Desplácese hasta la primera consulta (líneas 28 a 32) y seleccione el texto de la consulta.
1. Haga clic en Ejecutar. Se ejecutará el texto seleccionado.

## <a name="loading-more-nyc-taxi-data"></a>Carga de más datos de taxis de Nueva York
1. Vaya al **Centro de conocimiento** y haga clic en **Examinar la galería**. 
1. Seleccione la pestaña **Scripts de SQL** en la parte superior.
1. Seleccione **Carga de conjunto de datos de taxis de Nueva York**
1. En **Entradas**, elija **Seleccione un grupo existente** y seleccione **SQLDB1**.
1. Haga clic en **Abrir script**.
1. Aparecerá un nuevo script de SQL.
1. Haga clic en **Ejecutar**
1. Se crearán varias tablas para todos los datos de taxi de Nueva York y se cargarán con el comando T-SQL COPY.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Synapse Analytics](get-started.md)
* [Creación de un área de trabajo](quickstart-create-workspace.md)
* [Uso de grupos de SQL sin servidor](quickstart-sql-on-demand.md)
