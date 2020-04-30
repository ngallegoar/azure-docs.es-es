---
title: Traslado de un espacio de nombres de Azure Event Hubs a otra región | Microsoft Docs
description: En este artículo se muestra cómo trasladar un espacio de nombres de Azure Event Hubs desde la región actual a otra región.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 04/14/2020
ms.author: spelluru
ms.reviewer: shvija
ms.openlocfilehash: 2dfc9c517605bbb48bee0b306fb275464cfebe39
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606707"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Traslado de un espacio de nombres de Azure Event Hubs a otra región
Existen varios escenarios en los que puede que deba mover su espacio de nombres de Event Hubs existente de una región a otra. Por ejemplo, si desea crear un espacio de nombres con la misma configuración para realizar pruebas. También es posible que desee mover un espacio de nombres secundario a otra región como parte del [planeamiento de recuperación ante desastres](event-hubs-geo-dr.md#setup-and-failover-flow).

> [!NOTE]
> En este artículo se muestra cómo exportar una plantilla de Azure Resource Manager para un espacio de nombres de Event Hubs existente y, a continuación, usar la plantilla para crear un espacio de nombres con la misma configuración en otra región. Sin embargo, este proceso no traslada los eventos que todavía no se han procesado. Debe procesar los eventos del espacio de nombres original antes de eliminarlo.

## <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que los servicios y las características que usa su cuenta se admitan en la región de destino.
- En el caso de las características en vista previa (GB), asegúrese de que la suscripción está en la lista de permitidos para la región de destino.
- Si tiene **Característica de captura** habilitada para Event Hubs en el espacio de nombres, mueva las cuentas de [Azure Storage o Azure Data Lake Store Gen 2](../storage/common/storage-account-move.md) o [Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-migration-cross-region.md) antes de mover el espacio de nombres Event Hubs. También puede trasladar el grupo de recursos que contiene los espacios de nombres de Storage y de Event Hubs a la otra región siguiendo unos pasos similares a los que se describen en este artículo. 
- Si el espacio de nombres de Event Hubs está en un **clúster de Event Hubs**, [cree un clúster dedicado](event-hubs-dedicated-cluster-create-portal.md) en la **región de destino** antes de seguir los pasos de este artículo. 

## <a name="prepare"></a>Preparación
Para empezar, exporte una plantilla de Resource Manager. Esta plantilla contiene la configuración que describe el espacio de nombres de Event Hubs.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Seleccione **Todos los recursos** y, después, seleccione su espacio de nombres de Event Hubs.

3. Seleccione > **Configuración** > **Exportar plantilla**.

4. Elija **Descargar** en la página **Exportar plantilla**.

    ![Descarga de la plantilla de Resource Manager](./media/move-across-regions/download-template.png)

5. Busque el archivo ZIP que descargó desde el portal y descomprímalo en la carpeta que prefiera.

   Este archivo ZIP contiene los archivos .json que incluyen la plantilla y los scripts para implementar la plantilla.


## <a name="move"></a>Move

Implemente la plantilla para crear un espacio de nombres de Event Hubs en la región de destino. 


1. En Azure Portal, haga clic en **Crear un recurso**.

2. En **Buscar en Marketplace**, escriba **implementación de plantillas** y, después, presione **ENTRAR**.

3. Seleccione **Implementación de plantillas**.

4. Seleccione **Crear**.

5. Seleccione **Cree su propia plantilla en el editor**.

6. Seleccione **Cargar archivo** y, después, siga las instrucciones para cargar el archivo **template.json** que descargó en la última sección.

7. Seleccione **Guardar** para guardar la plantilla. 

8. En la página **Implementación personalizada**, siga estos pasos: 

    1. Seleccione una **suscripción** de Azure. 

    2. Seleccione un **grupo de recursos** existente o cree uno. Si el espacio de nombres de origen se encontraba en un clúster de Event Hubs, seleccione el grupo de recursos que contiene el clúster en la región de destino. 

    3. Seleccione la **ubicación** o región de destino. Si ha seleccionado un grupo de recursos existente, esta configuración es de solo lectura. 

    4. En la sección **CONFIGURACIÓN**, lleve a cabo estos pasos:
    
        1. Escriba el **nombre del nuevo espacio de nombres**. 

            ![Implementación de la plantilla de Resource Manager](./media/move-across-regions/deploy-template.png)

        2. Si el espacio de nombres de origen se encontraba en un **clúster de Event Hubs**, escriba los nombres del **grupo de recursos** y del **clúster de Event Hubs** como parte del **identificador externo**. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Si el centro de eventos del espacio de nombres usa una cuenta de Storage para capturar eventos, especifique el nombre del grupo de recursos y la cuenta de almacenamiento para el campo `StorageAccounts_<original storage account name>_external`. 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. Seleccione la casilla **Acepto los términos y condiciones indicados anteriormente**. 
    
    6. A continuación, seleccione **Select Purchase** (Seleccionar compra) para iniciar el proceso de implementación. 

## <a name="discard-or-clean-up"></a>Descarte o limpieza
Después de la implementación, si quiere empezar de nuevo, puede eliminar el **espacio de nombres de Event Hubs de destino** y repetir los pasos descritos en las secciones [Preparación](#prepare) y [Traslado](#move) de este artículo.

Para confirmar los cambios y completar el traslado de un espacio de nombres de Event Hubs, elimine el **espacio de nombres de Event Hubs de origen**. Asegúrese de que ha procesado todos los eventos en el espacio de nombres antes de eliminarlo. 

Para eliminar un espacio de nombres de Event Hubs (de origen o destino) mediante Azure Portal:

1. En la ventana de búsqueda de la parte superior de Azure Portal, escriba **Event Hubs** y seleccione **Event Hubs** en los resultados de la búsqueda. Verá los espacios de nombres de Event Hubs en una lista.

2. Seleccione el espacio de nombres de destino que desea eliminar y, a continuación, seleccione **Eliminar** en la barra de herramientas. 

    ![Eliminar espacio de nombres - botón](./media/move-across-regions/delete-namespace-button.png)

3. En la página **Eliminar recursos***, compruebe los recursos seleccionados y confirme la eliminación escribiendo **yes** y, a continuación, seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha trasladado un espacio de nombres de Azure Event Hubs de una región a otra y ha limpiado los recursos de origen.  Para obtener más información sobre cómo trasladar recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
