---
title: Copia de aplicaciones y datos en nodos de grupo
description: Obtenga información sobre cómo copiar aplicaciones y datos en nodos de grupo.
ms.topic: article
ms.date: 02/17/2020
ms.openlocfilehash: 700e9b80f8420266c0300b47bdd30bc271f8421c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115591"
---
# <a name="copying-applications-and-data-to-pool-nodes"></a>Copia de aplicaciones y datos en nodos de grupo

Azure Batch admite varias maneras de copiar datos y aplicaciones en nodos de proceso para que estén disponibles para su uso mediante tareas. Es posible que los datos y las aplicaciones sean necesarios para ejecutar todo el trabajo y, por tanto, deben instalarse en cada nodo. Algunos pueden ser necesarios solo para una tarea específica, o deben instalarse para el trabajo, pero no es necesario que estén en cada uno de los nodos. Batch dispone de herramientas para cada uno de estos escenarios.

- **Archivos de recursos de tareas de inicio de grupo**: para las aplicaciones o los datos que se deben instalar en cada nodo del grupo. Use este método junto con un paquete de aplicación o la recopilación de archivos de recursos de tareas de inicio para ejecutar un comando de instalación.  

Ejemplos: 
- Use la línea de comandos de la tarea de inicio para mover o instalar aplicaciones.

- Especifique una lista de archivos o contenedores específicos en una cuenta de almacenamiento de Azure. Para obtener más información, vea cómo [agregar el elemento resourcefile en la documentación de REST](https://docs.microsoft.com/rest/api/batchservice/pool/add#resourcefile).

- Todos los trabajos que se ejecutan en el grupo ejecutan el archivo MyApplication.exe, el cual se debe instalar primero con el archivo MyApplication.msi. Si usa este método, debe establecer la propiedad **Esperar operación correcta** de la tarea de inicio en **true**. Para obtener más información, vea cómo [agregar el elemento starttask en la documentación de REST](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask).

- **Referencias de paquetes de aplicación** en el grupo: para las aplicaciones o los datos que se deben instalar en cada nodo del grupo. No hay ningún comando de instalación asociado a un paquete de aplicación, pero puede usar una tarea de inicio para ejecutar cualquier comando de instalación. Si la aplicación no requiere instalación o está formada por un gran número de archivos, puede usar este método. Los paquetes de aplicación son idóneos para un gran número de archivos porque combinan muchas referencias de archivo en una carga pequeña. Si intenta incluir más de 100 archivos de recursos independientes en una tarea, el servicio de Batch puede encontrar limitaciones internas del sistema para una sola tarea. Aparte de esto, use paquetes de aplicación si tiene requisitos de versiones estrictos; por ejemplo, puede tener muchas versiones diferentes de la misma aplicación entre las que necesita elegir. Para obtener más información, consulte el artículo [Implementación de aplicaciones en nodos de proceso con paquetes de aplicaciones de Batch](https://docs.microsoft.com/azure/batch/batch-application-packages).

- **Archivos de recursos de tareas de preparación del trabajo**: para las aplicaciones o los datos que se deben instalar para que el trabajo se ejecute, pero que no es necesario instalar en todo el grupo. Por ejemplo, si su grupo tiene muchos tipos diferentes de trabajos y solo uno de ellos necesita MyApplication.msi para ejecutarse, sería conveniente colocar el paso de instalación en una tarea de preparación del trabajo. Para obtener más información sobre las tareas de preparación del trabajo, consulte [Ejecución de tareas de preparación y liberación de trabajos en nodos de proceso de Batch](https://azure.microsoft.com/documentation/articles/batch-job-prep-release/).

- **Archivos de recursos de tareas**: para cuando una aplicación o ciertos datos solo son relevantes para una tarea individual. Por ejemplo: Tiene cinco tareas, cada una procesa un archivo diferente y, a continuación, escribe la salida en el almacenamiento de blobs.  En este caso, se debe especificar el archivo de entrada en la recopilación de **archivos de recursos de tareas** porque cada tarea tiene su propio archivo de entrada.

## <a name="determine-the-scope-required-of-a-file"></a>Determinación del ámbito para el que se necesita un archivo

Debe determinar el ámbito de un archivo, es decir, si el archivo es necesario para un grupo, un trabajo o una tarea. Los archivos cuyo ámbito es el grupo deben usar paquetes de aplicación de grupo o una tarea de inicio. Los archivos cuyo ámbito es el trabajo deben usar una tarea de preparación del trabajo. Un buen ejemplo de archivos cuyo ámbito es el grupo o el trabajo son las aplicaciones. Los archivos cuyo ámbito es la tarea deben usar archivos de recursos de tareas.

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>Otras formas de copiar datos en nodos de proceso de Batch

Existen otros métodos para copiar datos en nodos de proceso de Batch que no están integrados oficialmente en la API REST de Batch. Al tener control sobre los nodos de Azure Batch y poder usar ejecutables personalizados, puede extraer datos de cualquier número de orígenes personalizados siempre que el nodo de Batch tenga conectividad al destino y siempre que usted tenga las credenciales para ese origen en el nodo de Azure Batch. Algunos ejemplos comunes son:

- Descargar datos de SQL.
- Descargar datos de otros servicios web o de ubicaciones personalizadas.
- Asignar un recurso compartido de red.

### <a name="azure-storage"></a>Almacenamiento de Azure

El almacenamiento de blobs tiene objetivos de escalabilidad de descarga. Los objetivos de escalabilidad de recursos compartidos de archivos de almacenamiento de Azure son los mismos que para un solo blob. El tamaño afectará al número de nodos y grupos que necesitará.

