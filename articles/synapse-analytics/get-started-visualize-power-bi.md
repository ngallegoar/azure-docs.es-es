---
title: 'Tutorial: Introducción a Azure Synapse Analytics: visualización de datos de un área de trabajo con Power BI'
description: En este tutorial, aprenderá a crear un área de trabajo de Power BI, vinculará el área de trabajo de Azure Synapse y creará un conjunto de datos de Power BI que use los datos en el área de trabajo de Azure Synapse.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: business-intelligence
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: b72d083216b7cd0ae7a588bfd721d8981829bfc7
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94844376"
---
# <a name="visualize-data-with-power-bi"></a>Visualización de datos con Power BI

En este tutorial, aprenderá a crear un área de trabajo de Power BI, vinculará el área de trabajo de Azure Synapse y creará un conjunto de datos de Power BI que use los datos en el área de trabajo de Azure Synapse. 

## <a name="overview"></a>Información general

A partir de los datos de taxis de Nueva York creamos conjuntos de datos agregados en dos tablas:
- **nyctaxi.passengercountstats**
- **SQLDB1.dbo.PassengerCountStats**

Puede vincular un área de trabajo de Power BI a su área de trabajo de Azure Synapse. Esta funcionalidad le permite obtener fácilmente datos en el área de trabajo de Power BI. Puede editar los informes de Power BI directamente en el área de trabajo de Azure Synapse.

### <a name="create-a-power-bi-workspace"></a>Creación de un área de trabajo de Power BI

1. Inicie sesión en [powerbi.microsoft.com](https://powerbi.microsoft.com/).
1. Cree una nueva área de trabajo de Power BI denominada **NYCTaxiWorkspace1**.

### <a name="link-your-azure-synapse-workspace-to-your-new-power-bi-workspace"></a>Vinculación de un área de trabajo de Azure Synapse a un área de trabajo de Power BI

1. En Synapse Studio, vaya a **Administrar** > **Servicios vinculados**.
1. Seleccione **Nuevo** > **Conectar con Power BI**
1. Establezca el **nombre** en **NYCTaxiWorkspace1**
1. Establezca el **nombre del área de trabajo** en **NYCTaxiWorkspace1**
1. Seleccione **Crear**.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-azure-synapse-workspace"></a>Creación de un conjunto de datos de Power BI que use los datos del área de trabajo de Azure Synapse

1. En Synapse Studio, vaya al centro **Develop**(Desarrollo) > **Power BI**.
1. Vaya a **NYCTaxiWorkspace1** > **Conjuntos de datos de Power BI** y seleccione **New Power BI dataset** (Nuevo conjunto de datos de Power BI).
1. Mantenga el puntero sobre la base de datos **SQLDB1** y seleccione **Download .pbids file** (Descargar archivo .pbids).
1. Abra el archivo **.pbids** que ha descargado. Power BI Desktop se abre y se conecta automáticamente a **SQLDB1** en el área de trabajo de Azure Synapse.
1. Si aparece un cuadro de diálogo denominado **Base de datos SQL Server**:
    1. Seleccione **Microsoft account** (Cuenta de Microsoft).
    1. Seleccione **Iniciar sesión** e inicie sesión en su cuenta.
    1. Seleccione **Conectar**.
1. Después de abrir el cuadro de diálogo **Navegador**, compruebe la tabla **PassengerCountStats** y seleccione **Cargar**.
1. Después de que aparezca el cuadro de diálogo **Configuración de la conexión**, seleccione **DirectQuery** > **Aceptar**.
1. Seleccione el botón **Informar** de la izquierda.
1. Agregue **Line Chart** (Gráfico de líneas) al informe.
    1. Arrastre la columna **PassengerCount** a **Visualizaciones** > **Eje**.
    1. Arrastre las columnas **SumTripDistance** y **AvgTripDistance** a **Visualizaciones** > **Valores**.
1. En la pestaña **Inicio**, seleccione **Publicar**.
1. Haga clic en **Guardar** para guardar los cambios.
1. Elija el nombre de archivo **PassengerAnalysis.pbix** y, después, seleccione **Guardar**.
1. En **Seleccionar un destino**, elija **NYCTaxiWorkspace1** y, a continuación, haga clic en **Seleccionar**.
1. Espere a que finalice la publicación.

### <a name="configure-authentication-for-your-dataset"></a>Configuración de la autenticación del conjunto de datos

1. Abra [powerbi.microsoft.com](https://powerbi.microsoft.com/) y seleccione **Iniciar sesión**.
1. En el lado izquierdo, en **Áreas de trabajo**, seleccione el área de trabajo **NYCTaxiWorkspace1**.
1. Dentro de esa área de trabajo, busque un conjunto de datos llamado **Passenger Analysis** (Análisis de pasajeros) y un informe con el mismo nombre.
1. Mantenga el puntero sobre el conjunto de datos **PassengerAnalysis**, seleccione el botón de puntos suspensivos (...) y, a continuación, seleccione **Configuración**.
1. En **Credenciales de origen de datos**, en **Método de autenticación**, seleccione **OAuth2** y, después, seleccione **Iniciar sesión**.

### <a name="edit-a-report-in-synapse-studio"></a>Edición de informes en Synapse Studio

1. Vuelva a Synapse Studio y seleccione **Close and refresh** (Cerrar y actualizar).
1. Vaya al centro **Develop** (Desarrollo).
1. Mantenga el puntero sobre **Power BI** y seleccione el nodo **Informes de Power BI**.
1. En **Power BI** debería ver:
    * En **NYCTaxiWorkspace1Power** > **Conjuntos de datos de Power BI**, un nuevo conjunto de datos denominado **PassengerAnalysis**.
    * En **NYCTaxiWorkspace1** > **Informes de Power BI**, un nuevo informe denominado **PassengerAnalysis**.
1. Seleccione el informe **PassengerAnalysis**. El informe se abre y puede editarlo directamente en Synapse Studio.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Supervisión](get-started-monitor.md)
                                 

