---
title: 'Preguntas frecuentes: Azure Synapse Analytics'
description: Preguntas frecuentes sobre Azure Synapse Analytics
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 10/25/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 838276c47085a3c7ad0f7c0a35a2578b13eb5026
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96511274"
---
# <a name="azure-synapse-analytics-frequently-asked-questions"></a>Preguntas frecuentes sobre Azure Synapse Analytics

En esta guía, encontrará las preguntas más frecuentes sobre Azure Synapse Analytics.

## <a name="general"></a>General

### <a name="q-what-rbac-roles-exist-and-how-do-i-use-them-to-secure-a-synapse-workspace"></a>P: ¿Qué roles de RBAC existen y cómo se usan para proteger áreas de trabajo de Synapse?

A. Azure Synapse introduce varios roles y ámbitos para asignarlos, lo que simplificará la protección del área de trabajo.

Roles de Synapse RBAC:
* Administrador de Synapse
* Administrador de Synapse SQL
* Administrador de Synapse Spark
* Colaborador de Synapse (versión preliminar)
* Editor de artefactos de Synapse (versión preliminar)
* Usuario de artefactos de Synapse (versión preliminar)
* Operador de proceso de Synapse (versión preliminar)
* Usuario de credenciales Synapse (versión preliminar)

Ámbitos de Synapse RBAC:
* Área de trabajo 
* Grupo de Spark
* Tiempo de ejecución de integración
* Servicio vinculado
* Credential:

Además, con los grupos de SQL dedicados tiene las mismas características de seguridad que conoce y tanto le gusta.

### <a name="q-how-can-i-cost-control-for-the-capabilities-inside-a-synapse-workspace-such-as-dedicated-sql-pools-serverless-spark-pools-and-serverless-sql-pools"></a>P: ¿Cómo puedo controlar el costo de las funcionalidades de las áreas de trabajo de Synapse, como los grupos de SQL dedicados, los grupos de Spark sin servidor y los grupos de SQL sin servidor?

R: Como punto de partida, Azure Synapse funciona con las alertas de costos y el análisis de costos integrados disponibles en el nivel de suscripción de Azure.

- Grupos de SQL dedicados: tiene visibilidad directa del costo y control sobre el costo, ya que crea y especifica los tamaños de los grupos de SQL dedicados. Puede controlar aún más qué usuarios que pueden crear o escalar grupos de SQL dedicados con roles de Azure RBAC.

- Grupos de SQL sin servidor: tiene controles de supervisión y administración de costos que le permiten poner un tope de gasto a nivel diario, semanal y mensual. Para más información, [consulte Administración de costos para un grupo de SQL sin servidor](./sql/data-processed.md). 

- Grupos de Spark sin servidor: puede restringir quién puede crear grupos de Spark con roles de Synapse RBAC.  

### <a name="q-will-synapse-workspace-support-folder-organization-of-objects-and-granularity-at-ga"></a>P: ¿Admitirá el área de trabajo de Synapse la organización en carpeta de objetos y la granularidad en GA?

R: Las áreas de trabajo de Synapse admiten carpetas definidas por el usuario.

### <a name="q-can-i-link-more-than-one-power-bi-workspaces-to-a-single-azure-synapse-workspace"></a>P: ¿Puedo vincular más de un área de trabajo de Power BI a una sola área de trabajo de Azure Synapse?
    
R: Actualmente, solo se puede vincular un área de trabajo de Power BI a un área de trabajo de Azure Synapse. 

### <a name="q-is-synapse-link-to-cosmos-db-ga"></a>P: ¿Es Synapse Link a Cosmos DB GA?

R: Synapse Link para Apache Spark es GA. Synapse Link para un grupo de SQL sin servidor está en versión preliminar pública.

### <a name="q-does-azure-synapse-workspace-support-cicd"></a>P: ¿Admite el área de trabajo de Azure Synapse CI/CD? 

R: Sí. Todos los artefactos de canalización, cuadernos, scripts SQL y definiciones de trabajos de Spark residirán en GIT. Todas las definiciones del grupo se almacenarán en GIT como plantillas de Azure Resource Manager. Los objetos del grupo de SQL dedicado (esquemas, tablas, vistas, etc.) se administrarán con proyectos de base de datos con compatibilidad con CI/CD.

## <a name="pipelines"></a>Procesos

### <a name="q-how-do-i-ensure-i-know-what-credential-is-being-used-to-run-a-pipeline"></a>P: ¿Cómo me aseguro de que conozco qué credencial se usa para ejecutar una canalización? 

R: Cada una de las actividades de una canalización de Synapse se ejecuta mediante la credencial especificada en el servicio vinculado.

### <a name="q-are-ssis-irs-supported-in-synapse-integrate"></a>P: ¿Se admiten las instancias de IR de SSIS en Synapse Integrate?

R: No en este momento. 

### <a name="q-how-do-i-migrate-existing-pipelines-from-azure-data-factory-to-an-azure-synapse-workspace"></a>P: ¿Cómo se migran las canalizaciones existentes de Azure Data Factory a un área de trabajo de Azure Synapse?

R: Actualmente, tanto las canalizaciones de Azure Data Factory como los artefactos relacionados se deben volver a crear manualmente. 

## <a name="apache-spark"></a>Spark de Apache

### <a name="q-what-is-the-difference-between-apache-spark-for-synapse-and-apache-spark"></a>P: ¿En qué se diferencian Apache Spark para Synapse y Apache Spark?

R: Apache Spark para Synapse ES Apache Spark con compatibilidad agregada para las integraciones con otros servicios (AAD, AzureML, etc.) y bibliotecas adicionales (mssparktuils, Hummingbird) y configuraciones del rendimiento preajustadas.

Todas las cargas de trabajo que se ejecuten actualmente en Apache Spark se ejecutarán en MSFT Spark sin cambio alguno. 

### <a name="q-what-versions-of-spark-are-available"></a>P: ¿Qué versiones de Spark están disponibles?

R: Azure Synapse Apache Spark es totalmente compatible con Spark 2.4. Para ver una lista completa de los componentes principales y la versión compatible actualmente, consulte [Compatibilidad de las versiones de Apache Spark ](./spark/apache-spark-version-support.md).

### <a name="q-is-there-an-equivalent-of-dbutils-in-azure-synapse-spark"></a>P: ¿Hay un equivalente de DButils en Azure Synapse Spark?

R: Sí, Azure Synapse Apache Spark proporciona la biblioteca **mssparkutils**. Para obtener la documentación completa de la utilidad, consulte [Introducción a las utilidades Spark para Microsoft](./spark/microsoft-spark-utilities.md).

### <a name="q-how-do-i-set-session-parameters-in-apache-spark"></a>P: ¿Cómo establecer los parámetros de sesión en Apache Spark?

R: Para establecer los parámetros de sesión, use %%configure magic available. Para que los parámetros surtan efecto, es preciso reiniciar la sesión. 

### <a name="q-how-do-i-set-cluster-level-parameters-in-a-serverless-spark-pool"></a>P: ¿Cómo se establecen parámetros de nivel de clúster en un grupo de Spark sin servidor?

R: Para establecer parámetros de nivel de clúster, puede proporcionar un archivo spark.conf para el grupo de Spark. A continuación, este grupo respetará los parámetros que se han pasado en el archivo de configuración. 

### <a name="q-can-i-run-a-multi-user-spark-cluster-in-azure-synapse-analytics"></a>P: ¿Se puede ejecutar un clúster de Spark multiusuario en Azure Synapse Analytics?
 
R: Azure Synapse proporciona motores creados específicamente para casos de uso concretos. Apache Spark para Synapse está diseñado como un servicio de trabajo, no como un modelo de clúster. Hay dos escenarios en los que las personas solicitan un modelo de clúster multiusuario.

**Escenario 1 : muchos usuarios acceden a un clúster para servir datos para la inteligencia empresarial.**

La forma más fácil de realizar esta tarea es preparar los datos con Spark y, luego, aprovechar las funcionalidades de servicio de Synapse SQL para que puedan conectarse Power BI a esos conjuntos de datos.

**Escenario 2: tener varios desarrolladores en un solo clúster para ahorrar dinero.**
 
Para satisfacer este escenario, debe proporcionar a cada desarrollador un grupo de Spark sin servidor que esté configurado para usar un reducido número de recursos de Spark. Dado que los grupos de Spark sin servidor no cuestan nada hasta que se usan activamente se minimiza el costo cuando hay varios desarrolladores. Los grupos comparten metadatos (tablas de Spark) para que puedan funcionar con facilidad entre sí.

### <a name="q-how-do-i-include-manage-and-install-libraries"></a>P: Cómo incluir, administrar e instalar bibliotecas 

R:  Puede instalar paquetes externos a través del archivo requirements.txt al crear el grupo de Spark tanto desde el área de trabajo de Synapse como desde Azure Portal. Consulte [Administración de bibliotecas para Apache Spark en Azure Synapse Analytics](./spark/apache-spark-azure-portal-add-libraries.md).

## <a name="dedicated-sql-pools"></a>Grupos de SQL dedicados

### <a name="q-what-are-the-functional-differences-between-dedicated-sql-pools-and-serverless-pools"></a>P: ¿Cuáles son las diferencias funcionales entre los grupos de SQL dedicados y los grupos sin servidor?

R: Puede encontrar una lista completa de las diferencias en [Diferencias entre las características de T-SQL en Synapse SQL](./sql/overview-features.md).

### <a name="q-now-that-azure-synapse-is-ga-how-do-i-move-my-dedicated-sql-pools-that-were-previously-standalone-into-azure-synapse"></a>P: Ahora que Azure Synapse está disponible de forma general, ¿cómo puedo trasladar grupos de SQL dedicados que anteriormente eran independientes en Azure Synapse? 

R: No hay "movimiento" o "migración". Puede elegir habilitar nuevas características de área de trabajo en los grupos existentes. Si lo hace, no se producen cambios importantes, sino que podrá usar nuevas características como Synapse Studio, Spark y grupos de SQL sin servidor.

### <a name="q-what-is-the-default-deployment-of-dedicated-sql-pools-now"></a>P: ¿Cuál es la implementación predeterminada de los grupos de SQL dedicados ahora? 

R: De forma predeterminada, todos los nuevos grupos de SQL dedicados se implementarán en un área de trabajo; sin embargo, si lo necesita, puede crear un grupo de SQL dedicado (anteriormente SQL DW) en un factor de forma independiente. 


### <a name="q-what-are-the-functional-differences-between-dedicated-sql-pools-and-serverless-sql-pool"></a>P: ¿Cuáles son las diferencias funcionales entre los grupos de SQL dedicados y los grupos de SQL sin servidor? 

R: Puede encontrar una lista completa de las diferencias en [Diferencias entre las características de T-SQL en Synapse SQL](./sql/overview-features.md).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Synapse Analytics](get-started.md)
* [Creación de un área de trabajo](quickstart-create-workspace.md)
* [Uso de grupos de SQL sin servidor](quickstart-sql-on-demand.md)
