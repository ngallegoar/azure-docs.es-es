---
title: 'Inicio rápido: Introducción a la creación de un área de trabajo de Synapse'
description: En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 10/07/2020
ms.openlocfilehash: 862d2a93058c63dbfad1db49346edcbfe3c02ad1
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94592454"
---
# <a name="creating-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor. 

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este tutorial, es preciso tener acceso a un grupo de recursos para el que tenga asignado el rol de **propietario**. Cree el área de trabajo de Synapse en este grupo de recursos.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Creación de un área de trabajo de Synapse en Azure Portal

1. Abra [Azure Portal](https://portal.azure.com) y, en la parte superior, busque **Synapse**.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics (versión preliminar de las áreas de trabajo)** .
1. Seleccione **Agregar** para crear un área de trabajo.
1. En **Aspectos básicos**, escriba los valores de **Suscripción**, **Grupo de recursos** y **Región** que prefiera y elija un nombre para el área de trabajo. Para este tutorial usaremos **myworkspace**.
1. Vaya a **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2). 
1. Haga clic en **Create New** (Crear nuevo) y asígnele el nombre **contosolake**.
1. Haga clic en **Sistema de archivos** y asigne el nombre **users**. Se creará un contenedor denominado **users**
1. El área de trabajo usará esta cuenta de almacenamiento como cuenta de almacenamiento "principal" para las tablas de Spark y los registros de aplicaciones de Spark.
1. Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.

## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abrir el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com). En la parte superior de la sección **Información general**, seleccione **Iniciar Synapse Studio**.
* Vaya a `https://web.azuresynapse.net` e inicie sesión en el área de trabajo.

## <a name="create-a-dedicated-sql-pool"></a>Creación de un grupo de SQL dedicado

1. En Synapse Studio, en el panel izquierdo, seleccione **Administrar** > **Grupos de SQL**.
1. Seleccione **Nuevo paso**.
1. En **Nombre del grupo de SQL** seleccione **SQLPOOL1**
1. En **Nivel de rendimiento** seleccione **DW100C**
1. Seleccione **Revisar y crear** > **Crear**. El grupo de SQL dedicado estará listo en unos minutos. El grupo de SQL dedicado se asocia con una base de datos del grupo de SQL dedicado, que también se denomina **SQLPOOL1**.

Los grupos de SQL dedicados consumen recursos facturables mientras están activos. Puede pausar los grupos más adelante para reducir los costos.

## <a name="create-a-serverless-apache-spark-pool"></a>Crear un grupo de Apache Spark sin servidor

1. En Synapse Studio, en el lado izquierdo, seleccione **Administrar** > **Grupos de Apache Spark**.
1. Seleccione **Nuevo paso**. 
1. En **Nombre del grupo de Apache Spark** escriba **Spark1**.
1. En **Tamaño del nodo** escriba **Pequeño**.
1. En **Número de nodos**, establezca el valor mínimo en 3 y el máximo en 3.
1. Seleccione **Revisar y crear** > **Crear**. El grupo de Apache Spark estará listo en unos segundos.

El grupo de Spark indica a Azure Synapse el número de recursos de Spark que se van a usar. Solo paga por los recursos que utiliza. Al dejar de usar el grupo de forma activa, los recursos agotarán el tiempo de espera y se reciclarán automáticamente.

## <a name="the-serverless-sql-pool"></a>El grupo de SQL sin servidor

Todas las áreas de trabajo incluyen un grupo precompilado llamado **Built-in**. Este grupo no se puede eliminar. El grupo de SQL sin servidor permite trabajar con SQL sin tener que crear ni pensar en la administración de un grupo de SQL sin servidor en Azure Synapse. A diferencia de los grupos de SQL dedicados, la facturación de los grupos de SQL sin servidor se basa en la cantidad de datos que se examinan para ejecutar la consulta (no en el número de recursos usados para ello).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis mediante un grupo de SQL dedicado](get-started-analyze-sql-pool.md)
