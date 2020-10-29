---
title: Protección de datos en Azure Stream Analytics
description: En este artículo se explica cómo cifrar los datos privados que usa un trabajo de Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/23/2020
ms.openlocfilehash: b54076413d3a6cabf2e3ef0b06e8e17875efbf97
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92746409"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Protección de datos en Azure Stream Analytics 

Azure Stream Analytics es una plataforma como servicio completamente administrada que permite crear canalizaciones de análisis en tiempo real. Todo el trabajo pesado, como el aprovisionamiento de clústeres, el escalado de nodos para ajustarse a su uso y la administración de los puntos de control internos, se administra en un segundo plano.

## <a name="private-data-assets-that-are-stored"></a>Recursos de datos privados que se almacenan

Azure Stream Analytics conserva los siguientes metadatos y datos para ejecutar los siguiente: 

* La definición de la consulta y su configuración relacionada.  

* Las funciones o elementos agregados que haya definido el usuario.  

* Los puntos de control que necesita el runtime de Stream Analytics.

* Las instantáneas de los datos de referencia. 

* Los detalles de conexión de los recursos que use el trabajo de Stream Analytics.

Para ayudarle a cumplir las obligaciones de cumplimiento normativo de cualquier entorno o sector regulado, puede leer más sobre las [ofertas de cumplimiento de Microsoft](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 

## <a name="in-region-data-residency"></a>Residencia de datos en la región
Azure Stream Analytics almacena los datos del cliente y otros metadatos descritos anteriormente. Los datos del cliente se almacenan Azure Stream Analytics en una sola región de forma predeterminada, por lo que este servicio satisface automáticamente los requisitos de residencia de datos de la región, incluidos los especificados en el [centro de confianza](https://azuredatacentermap.azurewebsites.net/).
Asimismo, puede almacenar todos los recursos de datos (por ejemplo, datos de clientes y otros metadatos) relacionados con el trabajo de Stream Analytics en una sola región mediante su cifrado en una cuenta de almacenamiento de su elección.

## <a name="encrypt-your-data"></a>Cifrado de los datos

Stream Analytics emplea automáticamente los mejores estándares de cifrado de su infraestructura para cifrar y proteger sus datos. Puede confiar a ciegas en Stream Analytics para almacenar de forma segura todos sus datos, ya que no tiene que preocuparse de administrar la infraestructura.

Si desea usar claves administradas por el cliente (CMK) para cifrar sus datos, puede usar su propia cuenta de almacenamiento (de uso general V1 o V2) para almacenar todos los recursos de datos privados que necesita el runtime de Stream Analytics. La cuenta de almacenamiento se puede cifrar tanto como sea necesario. La infraestructura de Stream Analytics no almacena ninguno de los recursos de datos privados. 

Este valor se debe configurar en el momento en que se crea el trabajo de Stream Analytics y no se puede modificar a lo largo del ciclo de vida de este. No se recomienda modificar o eliminar el almacenamiento que usa Stream Analytics. Si elimina su cuenta de almacenamiento, eliminará todos los recursos de datos privados de forma permanentemente, lo que provocará un error en el trabajo. 

Ni la actualización ni la rotación de claves de su cuenta de almacenamiento se pueden realizar mediante el portal de Stream Analytics. Las claves se pueden actualizar mediante las API REST.


### <a name="configure-storage-account-for-private-data"></a>Configuración de una cuenta de almacenamiento para los datos privados 


Cifre su cuenta de almacenamiento para proteger todos los datos y elija explícitamente la ubicación de los datos privados. 

Para ayudarle a cumplir las obligaciones de cumplimiento normativo de cualquier entorno o sector regulado, puede leer más sobre las [ofertas de cumplimiento de Microsoft](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 



Siga estos pasos para configurar su cuenta de almacenamiento para recursos de datos privados. Esta configuración se realiza desde el trabajo de Stream Analytics, no desde su cuenta de almacenamiento.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal. 

1. Seleccione  **Analytics** > **Stream Analytics job**  (Análisis > Trabajo de Stream Analytics) en la lista de resultados. 

1. Rellene la página del trabajo de Stream Analytics con los datos necesarios, como el nombre, la región y la escala. 

1. Active la casilla *Secure all private data assets needed by this job in my Storage account* (Proteger los recursos de datos privados que necesita este trabajo en mi cuenta de Storage).

1. Seleccione una cuenta de almacenamiento en su suscripción. Tenga en cuenta que este valor no se puede modificar a lo largo del ciclo de vida del trabajo. 

   ![Configuración de una cuenta de almacenamiento de datos privados](./media/data-protection/storage-account-create.png)


## <a name="next-steps"></a>Pasos siguientes

* [Creación de una cuenta de Azure Storage](../storage/common/storage-account-create.md)
* [Información sobre las entradas de Azure Stream Analytics](stream-analytics-add-inputs.md)
* [Conceptos de punto de control y reproducción en trabajos de Azure Stream Analytics](stream-analytics-concepts-checkpoint-replay.md)
* [Uso de datos de referencia para las búsquedas en Stream Analytics](stream-analytics-use-reference-data.md)
