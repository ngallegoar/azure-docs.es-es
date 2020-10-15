---
title: Copia de archivos de varios contenedores
description: Aprenda a usar una plantilla de solución para copiar archivos de varios contenedores con Azure Data Factory.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/1/2018
ms.openlocfilehash: 73560c49e10ab96c934d4dd3cea9395093a26420
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82629055"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Copia de archivos de varios contenedores con Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se describe una plantilla de solución que puede usar para copiar archivos de varios contenedores entre almacenes de archivos. Por ejemplo, puede usarla para migrar su instancia de Data Lake de AWS S3 a Azure Data Lake Store. O podría usarla para replicar todo el contenido de una cuenta de Azure Blob Storage a otra.

> [!NOTE]
> Si desea copiar archivos desde un único contenedor, es más eficaz utilizar la [herramienta Copiar datos](copy-data-tool.md) para crear una canalización con una sola actividad de copia. La plantilla de este artículo es más de lo que necesita para esa situación.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla enumera los contenedores del almacén del almacenamiento de origen. A continuación, copia esos contenedores en el almacén de destino.

La plantilla contiene tres actividades:
- **GetMetadata** examina el almacenamiento de origen y obtiene la lista de contenedores.
- **ForEach** obtiene la lista de contenedores de la actividad **GetMetadata** y, a continuación, recorre en iteración la lista y pasa cada contenedor a la actividad Copy.
- **Copy** copia cada contenedor desde el almacén de almacenamiento de origen al almacén de destino.

La plantilla define los parámetros siguientes:
- *SourceFileFolder* es la ruta de acceso de la carpeta del almacén de origen de datos, donde puede obtener una lista de los contenedores. La ruta de acceso es el directorio raíz, que contiene varias carpetas de contenedor. El valor predeterminado para este parámetro es `sourcefolder`.
- *SourceFileDirectory* es la ruta de acceso de la subcarpeta en el directorio raíz del almacén de origen de datos. El valor predeterminado para este parámetro es `subfolder`.
- *DestinationFileFolder* es la ruta de acceso de la carpeta donde se copiarán los archivos en el almacén de destino. El valor predeterminado para este parámetro es `destinationfolder`.
- *DestinationFileDirectory* es la ruta de acceso de la subcarpeta donde se copiarán los archivos en el almacén de destino. El valor predeterminado para este parámetro es `subfolder`.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **Copy multiple files containers between File Stores**(Copiar varios contenedores de archivos entre almacenes de archivos). Cree una **nueva** conexión con el almacén de almacenamiento de origen. El almacén de almacenamiento de origen es desde donde se van a copiar los archivos de varios contenedores.

    ![Creación de una nueva conexión con el origen](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Cree una **nueva** conexión con el almacén de almacenamiento de destino.

    ![Creación de una nueva conexión con el destino](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Seleccione **Usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Verá la canalización, como en el ejemplo siguiente:

    ![Visualización de la canalización](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Seleccione **Depurar**, escriba los **parámetros** y, a continuación, seleccione **Finalizar**.

    ![Ejecución de la canalización](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Revise el resultado.

    ![Revisión del resultado](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Pasos siguientes

- [Copia masiva de una base de datos con una tabla de control](solution-template-bulk-copy-with-control-table.md)

- [Copia de archivos de varios contenedores con Azure Data Factory](solution-template-copy-files-multiple-containers.md)
