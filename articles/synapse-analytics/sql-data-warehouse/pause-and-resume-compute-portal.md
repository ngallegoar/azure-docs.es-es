---
title: Pausa y reanudación del proceso en el grupo de SQL de Synapse mediante Azure Portal
description: Use Azure Portal para pausar el proceso en el grupo de SQL con la finalidad de ahorrar costos. Reanude el proceso cuando esté listo para usar el almacenamiento de datos.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: e3ab6e623b79e3ec3a35e055d239f6c1937fa8ed
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79539321"
---
# <a name="quickstart-pause-and-resume-compute-in-synapse-sql-pool-via-the-azure-portal"></a>Inicio rápido: Pausa y reanudación del proceso en el grupo de SQL de Synapse mediante Azure Portal

Puede usar Azure Portal para pausar y reanudar los recursos de proceso del grupo de SQL de Synapse (almacenamiento de datos). Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de empezar

Use [Crear y conectar: Portal](create-data-warehouse-portal.md) para crear un grupo de SQL denominado **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Pausa del proceso

Para reducir costos, puede pausar y reanudar los recursos de proceso a petición. Por ejemplo, si no va a usar la base de datos durante la noche y los fines de semana, puede pausarla durante esas horas y reanudarla durante el día. 
>[!NOTE]
>No se le cobrará por recursos de proceso mientras la base de datos se encuentre en pausa. Sin embargo, se le seguirá cobrando por el almacenamiento. 

Para pausar un grupo de SQL, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Haga clic en **Azure Synapse Analytics (formerly SQL DW)** en la página de navegación izquierda de Azure Portal.
2. Seleccione **mySampleDataWarehouse** en la página de **Azure Synapse Analytics (formerly SQL DW)** para abrir el grupo de SQL. 
3. En la página **mySampleDataWarehouse**, observe que el **Estado** sea **En línea**.

    ![Proceso en línea](././media/pause-and-resume-compute-portal/compute-online.png)

4. Para pausar el grupo de SQL, haga clic en el botón **Pausar**. 
5. Aparece una pregunta de confirmación en la que tiene que indicar si quiera continuar. Haga clic en **Sí**.
6. Espere un momento y, a continuación, observe que el **Estado** sea **Pausando**.

    ![Pausando](./media/pause-and-resume-compute-portal/pausing.png)

7. Una vez completada la operación de pausa, el estado es **En pausa** y el botón de opción es **Reanudar**.
8. Ahora, los recursos de proceso para el grupo de SQL no tienen conexión. No se le cobrará el proceso hasta que reanude el servicio.

    ![Proceso sin conexión](././media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Reanudación del proceso

Para reanudar un grupo de SQL, siga estos pasos.

1. Haga clic en **Azure Synapse Analytics (formerly SQL DW)** en la página izquierda de Azure Portal.
2. Seleccione **mySampleDataWarehouse** en la página de **Azure Synapse Analytics (formerly SQL DW)** para abrir la página del grupo de SQL. 
3. En la página **mySampleDataWarehouse**, observe que el **Estado** sea **En pausa**.

    ![Proceso sin conexión](././media/pause-and-resume-compute-portal/compute-offline.png)

4. Para reanudar el grupo de SQL, haga clic en **Reanudar**. 
5. Aparece una pregunta de confirmación en la que tiene que indicar si lo quiere iniciar. Haga clic en **Sí**.
6. Observe que el **Estado** sea **Reanudando**.

    ![Reanudando](./media/pause-and-resume-compute-portal/resuming.png)

7. Cuando el grupo de SQL vuelva a estar en línea, el estado será **En línea** y el botón de opción será **Pausar**.
8. Los recursos de proceso para el grupo de SQL ahora están en línea y se puede usar el servicio. Los cargos del proceso se han reanudado.

    ![Proceso en línea](././media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Se le está cobrando por unidades de almacenamiento de datos y por los datos almacenados en el grupo de SQL. Estos recursos de proceso y de almacenamiento se facturan por separado. 

- Si quiere conservar los datos de almacenamiento, pause el proceso.
- Si quiere eliminar cobros futuros, puede eliminar el grupo de SQL. 

Siga estos pasos para limpiar los recursos según estime oportuno.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en el grupo de SQL.

    ![Limpieza de recursos](./media/pause-and-resume-compute-portal/clean-up-resources.png)

1. Para pausar el proceso, haga clic en el botón **Pausar**. 

2. Para quitar el grupo de SQL para que no le cobren por proceso o almacenamiento, haga clic en **Eliminar**.

3. Para quitar el servidor SQL que creó, haga clic en **sqlpoolservername.database.windows.net** y, luego, en **Eliminar**.  

   > [!CAUTION]
   > Debe tener cuidado con este procedimiento, ya que la eliminación del servidor elimina también todas las bases de datos asignadas al servidor.

5. Para quitar el grupo de recursos, haga clic en **myResourceGroup** y luego haga clic en **Eliminar grupo de recursos**.


## <a name="next-steps"></a>Pasos siguientes

Ya ha pausado y reanudado el proceso para el grupo de SQL. Continúe con el siguiente artículo para más información sobre cómo [cargar datos en un grupo de SQL](load-data-from-azure-blob-storage-using-polybase.md). Para más información acerca de la administración de funcionalidades de proceso, consulte el artículo sobre [introducción a la administración de proceso](sql-data-warehouse-manage-compute-overview.md). 

