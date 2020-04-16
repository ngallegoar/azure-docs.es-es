---
title: Clasificación y detección de datos
description: Clasificación y detección de datos para Azure SQL Database y Azure Synapse Analytics
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
titleSuffix: Azure SQL Database and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: b3a08eb351d29fd71889807c9a21d03b564117a7
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673745"
---
# <a name="data-discovery--classification-for-azure-sql-database-and-azure-synapse-analytics"></a>Clasificación y detección de datos para Azure SQL Database y Azure Synapse Analytics

La clasificación y detección de datos proporciona funcionalidades avanzadas integradas en Azure SQL Database para **detectar**, **clasificar**, **etiquetar** y **notificar** la información confidencial de las bases de datos.

Las funciones de detección y clasificación de la información confidencial más importante (empresarial, financiera, médica, información personal identificable, etc.) desempeñan un rol fundamental en el modo en que se protege la información de su organización. Puede servir como infraestructura para lo siguiente:

- Ayudar a cumplir los requisitos de cumplimiento de normas y los estándares relacionados con la privacidad de datos.
- Varios escenarios de seguridad, como la supervisión (auditoría) y las alertas relacionadas con accesos anómalos a información confidencial.
- Controlar el acceso y mejorar la seguridad de las bases de datos que contienen información altamente confidencial.

La clasificación y detección de datos forma parte de la oferta de [Advanced Data Security (ADS)](sql-database-advanced-data-security.md). Dicha oferta es un paquete unificado para funcionalidades avanzadas de seguridad de SQL. Se puede acceder y administrar la clasificación y detección de datos desde el portal de ADS de SQL.

> [!NOTE]
> Este documento se relaciona con Azure SQL Database y Azure Synapse. Para simplificar, se usa SQL Database cuando se hace referencia tanto a SQL Database como a Azure Synapse. Para SQL Server (local), consulte [Clasificación y detección de datos de SQL](https://go.microsoft.com/fwlink/?linkid=866999).

## <a name="what-is-data-discovery--classification"></a><a id="what-is-dc"></a>¿Qué es la clasificación y detección de datos?

La función de clasificación y detección de datos incluye un conjunto de servicios avanzados y nuevas funcionalidades de SQL y forma un nuevo paradigma de Information Protection de SQL destinado a proteger los datos, no solo la base de datos:

- **Detección y recomendaciones**

  El motor de clasificación examina la base de datos e identifica las columnas que contienen datos potencialmente confidenciales. A continuación, le proporciona una manera sencilla de revisar y aplicar las recomendaciones de clasificación adecuadas a través de Azure Portal.

- **Etiquetado**

  Las etiquetas de clasificación de la confidencialidad se pueden colocar de forma persistente en columnas mediante el uso de nuevos atributos de metadatos de clasificación introducidos en el motor de SQL. A continuación, estos metadatos se pueden utilizar para escenarios avanzados de auditoría y protección basados en la confidencialidad.

- **Confidencialidad del conjunto de resultados de consulta**

  La confidencialidad del conjunto de resultados de consulta se calcula en tiempo real con fines de auditoría.

- **Visibilidad**

  El estado de clasificación de la base de datos puede verse en un panel detallado en el portal. Además, puede descargar un informe (en formato de Excel) para usarlo con fines de auditoría y cumplimiento de normas, así como para otras necesidades.

## <a name="discover-classify--label-sensitive-columns"></a><a id="discover-classify-columns"></a>Descubrir, clasificar y etiquetar columnas confidenciales

En la sección siguiente se describen los pasos necesarios para detectar, clasificar y etiquetar las columnas que contienen datos confidenciales en la base de datos, así como para ver el estado de clasificación actual de la base de datos y exportar informes.

La clasificación incluye dos atributos de metadatos:

- **Etiquetas**: atributos de clasificación principales, que se utilizan para definir el nivel de confidencialidad de los datos almacenados en la columna.  
- **Tipos de información**: proporcionan una granularidad adicional para el tipo de datos almacenados en la columna.

## <a name="define-and-customize-your-classification-taxonomy"></a>Definir y personalizar la taxonomía de clasificación

La función de clasificación y detección de datos incluye un conjunto integrado de etiquetas de confidencialidad y un conjunto integrado de tipos de información y lógica de detección. Ahora puede personalizar esta taxonomía y definir un conjunto y la categoría de construcciones de clasificación específicamente para su entorno.

La definición y la personalización de la taxonomía de clasificación se realizan en una ubicación central para todo el inquilino de Azure. Esa ubicación se encuentra en [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), como parte de la directiva de seguridad. Solo un usuario con derechos administrativos en el grupo de administración raíz del inquilino puede realizar esta tarea.

Como parte de la administración de directivas de SQL Information Protection, puede definir etiquetas personalizadas, clasificarlas y asociarlas con un conjunto de tipos de información seleccionado. También puede agregar sus propios tipos de información personalizados y configurarlos con patrones de cadena, que se agregan a la lógica de detección para identificar este tipo de datos en las bases de datos.
Obtenga más información sobre cómo personalizar y administrar la directiva en la [guía de procedimientos de directivas de SQL Information Protection](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409).

Una vez definida la directiva de todos los inquilinos, puede continuar con la clasificación de bases de datos individuales mediante la directiva personalizada.

## <a name="classify-your-sql-database"></a>Clasifique su instancia de SQL Database

1. Vaya a [Azure Portal](https://portal.azure.com).

2. Navegue a **Advanced Data Security** en el encabezado de Seguridad en el panel de su base de datos de Azure SQL. Haga clic para habilitar Advanced Data Security y haga clic en la tarjeta **Clasificación y detección de datos**.

   ![Examen de una base de datos](./media/sql-data-discovery-and-classification/data_classification.png)

3. La pestaña **Introducción** incluye un resumen del estado actual de clasificación de la base de datos, incluida una lista detallada de todas las columnas clasificadas, que también puede filtrar para ver solo tipos de información, etiquetas y partes del esquema específicos. Si aún no ha clasificado ninguna columna, [vaya al paso 5](#step-5).

   ![Resumen del estado de clasificación actual](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Para descargar un informe en formato de Excel, haga clic en la opción **Exportar** del menú superior de la ventana.

5. <a id="step-5"></a>Para empezar a clasificar los datos, haga clic en la **pestaña Clasificación** en la parte superior de la ventana.

6. El motor de clasificación examina las columnas de su base de datos que contienen datos potencialmente confidenciales y proporciona una lista de **clasificaciones de columna recomendadas**. Para ver y aplicar las recomendaciones de clasificación:

   - Para ver la lista de clasificaciones de columnas recomendadas, haga clic en el panel de recomendaciones en la parte inferior de la ventana.

   - Revise la lista de recomendaciones: para aceptar una recomendación para una columna específica, active la casilla de la columna izquierda de la fila correspondiente. También puede marcar *todas las recomendaciones* como aceptadas. Para ello, active la casilla del encabezado de la tabla de recomendaciones.

       ![Revise la lista de recomendaciones](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Para aplicar las recomendaciones seleccionadas, haga clic en el botón azul **Accept selected recommendations** (Aceptar recomendaciones seleccionadas).

7. También puede **clasificar manualmente** las columnas como alternativa a la clasificación basada en recomendaciones, o además de ella:

   - Haga clic en **Agregar clasificación** en el menú superior de la ventana.

   - En la ventana contextual que se abre, seleccione el esquema > tabla > columna que quiera clasificar, así como el tipo de información y la etiqueta de confidencialidad. Después, haga clic en el botón azul **Agregar clasificación** situado en la parte inferior de la ventana contextual.

      ![Seleccione la columna que quiera clasificar](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Para completar la clasificación y etiquetar de forma persistente las columnas de la base de datos con los nuevos metadatos de clasificación, haga clic en **Guardar** en el menú superior de la ventana.

## <a name="auditing-access-to-sensitive-data"></a><a id="audit-sensitive-data"></a>Auditoría del acceso a datos confidenciales

Un aspecto importante del paradigma de protección de la información es la capacidad de supervisar el acceso a información confidencial. La [auditoría de Azure SQL Database](sql-database-auditing.md) se ha mejorado para incluir un nuevo campo en el registro de auditoría denominado *data_sensitivity_information*, que registra la clasificación de confidencialidad (etiquetas) de los datos reales que devuelve la consulta.

![Registro de auditoría](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a name="permissions"></a><a id="permissions"></a>Permisos

Los siguientes roles integrados pueden leer la clasificación de datos de la base de datos de Azure SQL: `Owner`, `Reader`, `Contributor`, `SQL Security Manager` y `User Access Administrator`.

Los siguientes roles integrados pueden modificar la clasificación de datos de la base de datos de Azure SQL: `Owner`, `Contributor`, `SQL Security Manager`.

Obtenga más información acerca de los [recursos de Azure para RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview).

## <a name="manage-classifications"></a><a id="manage-classification"></a>Administración de clasificaciones

### <a name="using-t-sql"></a>Usar T-SQL
Puede utilizar T-SQL para agregar o quitar las clasificaciones de columna, así como recuperar todas las clasificaciones para toda la base de datos.

> [!NOTE]
> Al usar T-SQL para administrar las etiquetas, no se valida que las etiquetas que se agregan a una columna existan en la directiva de protección de información de la organización (el conjunto de etiquetas que aparecen en las recomendaciones del portal). Por lo tanto, validarlas es su responsabilidad.

- Agregue o actualice la clasificación de una o varias columnas: [ADD SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql) (Agregar clasificación de confidencialidad)
- Quite la clasificación de una o varias columnas: [DROP SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql) (Eliminar clasificación de la confidencialidad)
- Vea todas las clasificaciones de la base de datos: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

### <a name="using-rest-api"></a>Uso de API REST
Puede usar las API REST para administrar las clasificaciones y recomendaciones mediante programación. Las API REST publicadas admiten las siguientes operaciones:

- [Crear o actualizar](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate): crea o actualiza la etiqueta de confidencialidad de una columna determinada
- [Eliminar](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete): elimina la etiqueta de confidencialidad de una columna determinada
- [Deshabilitar recomendación](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation): deshabilita las recomendaciones de confidencialidad de una columna determinada
- [Habilitar recomendación](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation): habilita recomendaciones de confidencialidad en una columna determinada (las recomendaciones están habilitadas de forma predeterminada en todas las columnas)
- [Obtener](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get): obtiene la etiqueta de confidencialidad de una columna determinada
- [List Current By Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase) (Enumerar actuales por base de datos): enumera las etiquetas de confidencialidad actuales de una base de datos determinada
- [List Recommended By Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) (Enumerar recomendadas por base de datos): enumera las etiquetas de confidencialidad recomendadas de una base de datos determinada

### <a name="using-powershell-cmdlet"></a>Uso de cmdlet de PowerShell
Puede usar PowerShell para administrar las clasificaciones y las recomendaciones de Azure SQL Database e Instancia administrada.

#### <a name="powershell-cmdlet-for-azure-sql-database"></a>Cmdlet de PowerShell para la base de datos de Azure SQL
- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-AzSqlDatabaSesensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Disable-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

#### <a name="powershell-cmdlets-for-managed-instance"></a>Cmdlets de PowerShell para la Instancia administrada
- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Disable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)


## <a name="next-steps"></a><a id="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [Advanced Data Security](sql-database-advanced-data-security.md).
- Considere la posibilidad de configurar la [auditoría de Azure SQL Database](sql-database-auditing.md) para supervisar y auditar el acceso a los datos confidenciales clasificados.
- Para ver una presentación que incluye la detección y clasificación de datos, consulte [Detección, clasificación, etiquetado y protección de datos de SQL | Datos expuestos](https://www.youtube.com/watch?v=itVi9bkJUNc).
