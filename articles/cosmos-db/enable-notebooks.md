---
title: Habilitación de cuadernos en la cuenta de Azure Cosmos DB (versión preliminar)
description: La integración de cuadernos en Azure Cosmos DB permite analizar y visualizar los datos desde el portal. En este artículo se describe cómo habilitar esta característica para las cuentas de Cosmos.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/22/2019
ms.author: dech
ms.openlocfilehash: 4c485bf6b9eb34e68e399c24e51286428f47586f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85261909"
---
# <a name="enable-notebooks-for-azure-cosmos-db-accounts-preview"></a>Habilitación de cuadernos para las cuentas de Azure Cosmos DB (versión preliminar)

> [!IMPORTANT]
> Los cuadernos integrados para Azure Cosmos DB están disponibles actualmente en las siguientes regiones de Azure: Este de Australia, Este de EE. UU., Este de EE. UU. 2, Norte de Europa, Centro-sur de EE. UU.., Sudeste de Asia, Sur de Reino Unido, Oeste de Europa y Oeste de EE. UU. 2. Para usar cuadernos, [cree una cuenta nueva con Notebooks](#enable-notebooks-in-a-new-cosmos-account) o [habilite los Notebooks en una cuenta existente](#enable-notebooks-in-an-existing-cosmos-account) en una de estas regiones.

Los cuadernos de Jupyter integrados en Azure Cosmos DB permiten analizar y visualizar los datos desde Azure Portal. En este artículo se describe cómo habilitar esta característica para la cuenta de Azure Cosmos DB.

## <a name="enable-notebooks-in-a-new-cosmos-account"></a>Habilitación de cuadernos en una cuenta nueva de Cosmos

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
1. Seleccione **Crear un recurso** > **Bases de datos** > **Azure Cosmos DB**.
1. En la página **Creación de una cuenta de Azure Cosmos DB**, seleccione **Notebooks**. 
 
    :::image type="content" source="media/enable-notebooks/create-new-account-with-notebooks.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

1. Seleccione **Revisar + crear**. Puede omitir las opciones **Red** y **Etiquetas**. 
1. Revise la configuración de la cuenta y seleccione **Crear**. La operación de creación de la cuenta tarda unos minutos. Espere hasta que la página del portal muestre **Se completó la implementación** . 

   :::image type="content" source="media/enable-notebooks/create-new-account-with-notebooks-complete.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

1. Seleccione **Ir al recurso** para ir a la página de la cuenta de Azure Cosmos DB.

   :::image type="content" source="../../includes/media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-3.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

1. Vaya al panel **Explorador de datos**. Ahora debería ver el área de trabajo de Notebooks.

    :::image type="content" source="media/enable-notebooks/new-notebooks-workspace.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

## <a name="enable-notebooks-in-an-existing-cosmos-account"></a>Habilitación de cuadernos en una cuenta de Cosmos existente

También puede habilitar cuadernos en las cuentas existentes. Este paso debe realizarse una sola vez por cuenta.

1. Vaya al panel **Explorador de datos** en su cuenta de Cosmos.
1. Seleccione **Habilitar Notebooks**.

    :::image type="content" source="media/enable-notebooks/enable-notebooks-workspace.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

1. Se le pedirá que cree un área de trabajo de Notebooks. Seleccione **Complete setup** (Completar configuración).
1. Ahora su cuenta está habilitada para usar Notebooks.

## <a name="create-and-run-your-first-notebook"></a>Creación y ejecución del primer cuaderno

Para comprobar que puede usar Notebooks, seleccione uno de los cuadernos de Sample Notebooks (Cuadernos de ejemplo). Se guardará una copia del cuaderno en el área de trabajo y se abrirá.

En este ejemplo, usaremos **GettingStarted.ipynb**. 

:::image type="content" source="media/enable-notebooks/select-getting-started-notebook.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

Para ejecutar el cuaderno:
1. Seleccione la primera celda de código que contiene el código de Python. 
1. Seleccione **Ejecutar** para ejecutar la celda. También puede usar **Mayús+Entrar** para ejecutar la celda.
1. Actualice el panel de recursos para ver la base de datos y el contenedor que se han creado.

    :::image type="content" source="media/enable-notebooks/run-first-notebook-cell.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

También puede seleccionar **Nuevo Notebook** para crear un notebook o cargar un archivo de Notebook (.ipynb) existente al seleccionar **Cargar archivo** en el menú **Mis Notebooks**. 

:::image type="content" source="media/enable-notebooks/create-or-upload-new-notebook.png" alt-text="Selección de la opción Cuadernos en la hoja Creación de una cuenta de Azure Cosmos DB":::

## <a name="next-steps"></a>Pasos siguientes

- Conozca las ventajas de los [cuadernos de Jupyter para Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)
