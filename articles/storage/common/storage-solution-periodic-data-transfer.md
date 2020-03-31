---
title: Elección de la solución de Azure para la transferencia de datos periódica | Microsoft Docs
description: Aprenda a elegir una solución de Azure para cuando desee una transferencia de datos periódica.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: fb49802adf6242f445b700d06622d7e6aa336b4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67357036"
---
# <a name="solutions-for-periodic-data-transfer"></a>Soluciones para la transferencia de datos periódica
 
En este artículo se proporciona información general sobre las soluciones para las transferencias de datos periódicas. La transferencia de datos periódica en la red puede categorizarse como periódica en intervalos regulares o como movimiento de datos continuo. En el artículo también se describen las opciones recomendadas de transferencia de datos y la matriz de funcionalidades clave para este escenario.

Para una visión general de todas las opciones de transferencia de datos disponibles, vaya a [Choose an Azure data transfer solution](storage-choose-data-transfer-solution.md) (Elección de una solución de transferencia de datos de Azure).

## <a name="recommended-options"></a>Opciones recomendadas

Las opciones recomendadas para la transferencia de datos periódica se dividen en dos categorías en función de si la transferencia es continua o periódica.

- **Herramientas de scripts o de programación**: para las transferencias de datos que se producen a intervalos regulares, use las herramientas de scripts o de programación (AzCopy y las API REST de Azure Storage). Estas herramientas están destinadas a los desarrolladores y profesionales de TI.

    - **AzCopy**: use esta herramienta de la línea de comandos para copiar fácilmente datos desde y hacia Azure Blobs, Files y Table Storage con un rendimiento óptimo. AzCopy admite la simultaneidad y el paralelismo, y permite reanudar operaciones de copia cuando si se interrumpen.
    - **API REST/SDK de Azure Storage**: al compilar una aplicación, puede desarrollar las API REST de Azure Storage y usar los SDK de Azure que se ofrecen en varios lenguajes. Las API REST también pueden aprovechar Data Movement Library de Azure Storage, diseñada para la copia de datos de alto rendimiento en Azure y desde Azure.

- **Herramientas de ingesta continua de datos**: para la ingesta de datos continua en intervalos regulares puede seleccionar un dispositivo de transferencia en línea de Data Box o Azure Data Factory. Estas herramientas las configuran los profesionales de TI y automatizan de manera transparente la transferencia de datos.

    - **Azure Data Factory**: se debe usar Data Factory para escalar horizontalmente una operación de transferencia y saber si existe la necesidad de tener funcionalidades de orquestación y supervisión a nivel empresarial. Use Azure Data Factory para configurar una canalización en la nube para transferir archivos periódicamente entre distintos servicios de Azure, locales o de ambos tipos. Azure Data Factory permite orquestar flujos de trabajo basados en datos que ingieren datos de distintos almacenes de datos y automatizar el movimiento y la transformación de datos.
    - **Familia de Azure Data Box para transferencias en línea**: Data Box Edge y Data Box Gateway son dispositivos de red en línea que pueden mover datos dentro y fuera de Azure. Data Box Edge usa procesos perimetrales compatibles con la inteligencia artificial para el procesamiento previo de los datos antes de la carga. Data Box Gateway es una versión virtual del dispositivo con las mismas funcionalidades de transferencia de datos.


## <a name="comparison-of-key-capabilities"></a>Comparación de funcionalidades clave

En la siguiente tabla se resumen las diferencias de las funcionalidades clave.

### <a name="scriptedprogrammatic-network-data-transfer"></a>Transferencia de datos a través de la red mediante scripts o programación

| Capacidad                  | AzCopy                                 | API REST de Azure Storage       |
|-----------------------------|----------------------------------------|-------------------------------|
| Factor de forma                 | Herramienta de línea de comandos de Microsoft       | Los clientes desarrollan en Storage <br> API REST que usan las bibliotecas de cliente de Azure |
| Instalación única inicial     | mínimo                                | Esfuerzo de desarrollo moderado, variable    |
| Formato de datos                 | Azure Blobs, Azure Files, Azure Tables | Azure Blobs, Azure Files, Azure Tables   |
| Rendimiento                 | Ya se ha optimizado                      | Se optimiza durante el desarrollo                  |
| Precios                     | Gratis, se aplican los cargos de salida      | Gratis, se aplican los cargos de salida        |

### <a name="continuous-data-ingestion-over-network"></a>Ingesta de datos continua a través de la red

| Característica                                       | Data Box Gateway | Data Box Edge   | Azure Data Factory        |
|----------------------------------|-----------------------------------------|--------------------------|---------------------------|
| Factor de forma                                   | Dispositivo virtual             | Dispositivo físico          | Servicio en Azure Portal, agente local                                                            |
| Hardware                                      | Hipervisor del usuario            | Suministrado por Microsoft    | N/D                                                            |
| Esfuerzo de instalación inicial                          | Bajo (<30 minutos)            | Moderado (un par de horas) | Grande (unos días)                                                 |
| Formato de datos                                   | Azure Blobs, Azure Files   | Azure Blobs, Azure Files | [Admite más de 70 conectores de datos para almacenes y formatos de datos](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| Procesamiento previo de los datos                           | No                         | Sí, mediante un proceso perimetral    | Sí                                                           |
| Caché local<br>(para almacenar datos locales)    | Sí                        | Sí                      | No                                                            |
| Transferencia desde otras nubes                    | No                         | No                       | Sí                                                           |
| Precios                                       | [Precios](https://azure.microsoft.com/pricing/details/storage/databox/gateway/)                    | [Precios](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                  | [Precios](https://azure.microsoft.com/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>Pasos siguientes

- [Transferencia de datos con AzCopy](/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json).
- [Más información sobre la transferencia de datos con las API REST de Storage](https://docs.microsoft.com/dotnet/api/overview/azure/storage?view=azure-dotnet).
- Más información:
    - [Transferencia de datos con Data Box Gateway](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Transformación de datos con Data Box Edge antes del envío a Azure](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Transferencia de datos con Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-bulk-copy-portal).
