---
title: 'Terminología: Azure Synapse Analytics (versión preliminar de las áreas de trabajo)'
description: Guía de referencia para dirigir a los usuarios por Azure Synapse Analytics
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 11/18/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c58ee46a608ccdcbb01a082ee278d9e0f8a07f6e
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030686"
---
# <a name="azure-synapse-analytics-terminology"></a>Terminología de Azure Synapse Analytics

[!INCLUDE [preview](includes/note-preview.md)]

Este documento es una guía de los conceptos básicos de Azure Synapse Analytics.

## <a name="basics"></a>Aspectos básicos

Un **área de trabajo de Synapse** es un límite de colaboración protegible para realizar análisis empresariales basados en la nube en Azure. El área de trabajo se implementa en una región específica y tiene una cuenta de Azure Data Lake Storage Gen2 y un sistema de archivos asociados (para almacenar los datos temporales). Un área de trabajo está en un grupo de recursos.

Un área de trabajo le permite realizar análisis con SQL y Apache Spark. Los recursos disponibles para los análisis de SQL y Spark se organizan en **grupos** de SQL y Spark. 

## <a name="linked-services"></a>Servicios vinculados

Las áreas de trabajo pueden contener cualquier número de cadenas de conexión esenciales de un **servicio vinculado** que definen la información de conexión necesaria para que el área de trabajo se conecte a recursos externos.

## <a name="synapse-sql"></a>SQL de Synapse

**Synapse SQL** es la capacidad de realizar análisis basados en T-SQL en el área de trabajo de Synapse. Synapse SQL tiene dos modelos de consumo: dedicado y sin servidor.  Para el modelo dedicado, use **grupos de SQL dedicados**. Un área de trabajo puede tener un número ilimitado de estos grupos. Para usar el modelo sin servidor, use los **grupos de SQL sin servidor**. Cada área de trabajo tiene uno de estos grupos.

* **Solicitud de SQL**: algunas operaciones, como las consultas, se ejecutan mediante un grupo de SQL dedicado o un grupo de SQL sin servidor.
* **Script de SQL**: conjunto de comandos SQL guardados en un archivo. Un script SQL puede contener una o varias instrucciones SQL. Se puede usar para ejecutar solicitudes SQL a través del grupo de SQL dedicado o el grupo de SQL sin servidor.

## <a name="apache-spark-for-synapse"></a>Apache Spark para Synapse

Para usar el análisis de Spark, cree y use **grupos de Apache Spark sin servidor** en el área de trabajo de Synapse. Al empezar a usar un grupo de Spark, las áreas de trabajo crean una **sesión de Spark** para administrar los recursos asociados con esa sesión. 

En Synapse hay dos maneras de usar Spark:
* **Cuadernos de Spark** para realizar ciencia e ingeniería de datos con Scala, PySpark, C# y SparkSQL.
* **Definiciones de trabajo de Spark** para ejecutar trabajos de Spark por lotes con archivos jar.

Compatibilidad de versiones:
* Spark 2.4
* Python 3.6.1.
* Scala 2.11.12
* .NET para Apache Spark 1.0
* Delta Lake 0.3.  

## <a name="pipelines"></a>Procesos

* **Integración de datos**: proporciona la funcionalidad de ingerir datos entre varios orígenes y coordinar las actividades que se ejecutan en un área de trabajo o fuera de ella.
* **Flujo de datos**: proporciona una experiencia completamente visual que no requiere codificación para realizar la transformación de macrodatos. Toda la optimización y ejecución se administra sin servidor.
* **Canalización**: agrupación lógica de actividades que realizan una tarea de manera conjunta.
* **Actividad**: define las acciones que se deben realizar en los datos, como la copia de datos, la ejecución de un cuaderno o un script de SQL.
* **Desencadenador**: ejecuta una canalización. Se puede ejecutar de forma manual o automática (programación, ventana de saltos de tamaño constante o basada en eventos).
* **Conjunto de datos de integración**: vista con nombre de los datos que simplemente apunta o hace referencia a los datos que se van a usar en una actividad como entrada y salida. Pertenece a un servicio vinculado.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Synapse Analytics](get-started.md)
* [Creación de un área de trabajo](quickstart-create-workspace.md)
* [Uso de grupos de SQL sin servidor](quickstart-sql-on-demand.md)

