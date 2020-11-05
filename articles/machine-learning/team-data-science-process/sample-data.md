---
title: 'Datos de ejemplo en diferentes ubicaciones de Azure Storage: proceso de ciencia de datos en equipos'
description: Datos de ejemplo en contenedores de blob de Azure, SQL Server y tablas de Hive para reducirlos a un tamaño más pequeño pero representativo y más manejable.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 878a2a47dfaddf1d3399a1dbd98865332838e148
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321741"
---
# <a name="sample-data-in-azure-blob-containers-sql-server-and-hive-tables"></a><a name="heading"></a>Muestra de datos en contenedores de blob de Azure, SQL Server y tablas de Hive

Los artículos siguientes describen cómo realizar un muestreo de los datos que se almacenan en una de las tres diferentes ubicaciones de Azure:

* Para muestrear los [**datos del contenedor de blobs de Azure**](sample-data-blob.md), se descargan mediante programación y, a continuación, se realiza un muestreo de ellos con el código Python de ejemplo.
* Los [**datos de SQL Server**](sample-data-sql-server.md) se muestrean tanto con SQL como con el lenguaje de programación de Python. 
* Los [**datos de las tablas de Hive**](sample-data-hive.md) se muestrean con consultas de Hive.

Esta tarea de muestreo es un paso en el [proceso de ciencia de datos en equipos (TDSP)](./index.yml).

**Razones para el uso de datos de ejemplo**

Si el conjunto de datos que pretende analizar es grande, es recomendable reducirlo a un tamaño más pequeño, pero representativo, que sea más manejable. La reducción del tamaño puede facilitar la comprensión y exploración de los datos, y el diseño de características. Este rol de muestreo en el proceso de análisis de Cortana es permitir la rápida creación de prototipos de las funciones de procesamiento de datos y de los modelos de Machine Learning.