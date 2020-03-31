---
title: Almacenamiento en la nube para crear aplicaciones muy escalables, seguras y duraderas con Azure Storage
description: Conozca los servicios para almacenar en la nube grandes volúmenes de datos (estructurados y no estructurados) de aplicaciones móviles.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 682f06936480d9018d759b4df3bed80be1206faf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240971"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>Almacenamiento en la nube de aplicaciones muy escalables, seguras y duraderas con Azure Storage
[Azure Storage](https://azure.microsoft.com/services/storage/) es la solución de almacenamiento en la nube de Microsoft para las aplicaciones modernas que ofrece un almacén de objetos escalable de forma masiva para objetos de datos, un servicio de sistema de archivos para la nube, un almacén de mensajería confiable y un almacén NoSQL. Azure Storage se caracteriza por:
- **Durabilidad y elevada disponibilidad.** La redundancia garantiza que los datos están seguros en caso de errores de hardware transitorios. También puede optar por replicar datos entre centros de datos o regiones geográficas para obtener protección adicional frente a catástrofes locales o desastres naturales. Los datos replicados de esta manera permanecen con una alta disponibilidad en caso de una interrupción inesperada.
- **Seguridad.** Todos los datos escritos en Azure Storage se cifran mediante el servicio. Azure Storage proporciona un control pormenorizado sobre quién tiene acceso a los datos.
- **Escalable:** los servicios están diseñados para escalarse de forma masiva con el fin de satisfacer las necesidades de rendimiento y almacenamiento de datos de las aplicaciones de hoy en día.
- **Capacidad de administración.** Azure controla automáticamente el mantenimiento, las actualizaciones y los problemas críticos del hardware.
- **Accesibilidad.** Es posible acceder a los datos desde cualquier parte del mundo a través de HTTP o HTTPS. Microsoft proporciona bibliotecas cliente en diversos lenguajes, como .NET, Java, Node.js, Python, PHP, Ruby y Go, así como una API REST consolidada. El scripting se admite en Azure PowerShell o en la CLI de Azure. Azure Portal y el Explorador de Azure Storage ofrecen soluciones visuales sencillas para trabajar con los datos.

Use los servicios siguientes para habilitar el almacenamiento en la nube en sus aplicaciones móviles.

## <a name="azure-blob-storage"></a>Azure Blob Storage
[Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) es la solución de almacenamiento de objetos de Microsoft para la nube. Blob Storage está optimizado para almacenar grandes cantidades de datos no estructurados que no se adhieren a una definición o un modelo de datos determinado, como texto o binario. Admite diversos lenguajes que usan las bibliotecas cliente. Blob Storage está diseñado para:
- Servir imágenes o documentos directamente a un explorador.
- Almacenar archivos para disfrutar de acceso distribuido.
- Streaming de vídeo y audio.
- Escribir en archivos de registro.
- Almacenar datos para realizar operaciones de copia de seguridad y restauración, recuperación ante desastres y archivado.
- Almacenar datos para realizar análisis en local o en un servicio hospedado en Azure.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación de Azure Blob Storage](/azure/storage/blobs/storage-blobs-introduction)
- [Guías de inicio rápido](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [Muestras](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure
[Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) es un servicio que almacena datos NoSQL estructurados en la nube y proporciona un almacén de claves y atributos con un diseño sin esquema. Azure Table Storage permite almacenar una gran cantidad de datos estructurados. El servicio es un almacén de datos NoSQL que acepta llamadas autenticadas desde dentro y fuera de la nube de Azure. Las tablas de Azure son ideales para el almacenamiento de datos estructurados no relacionales. Normalmente, el almacenamiento de tablas se usa para:
- Almacenar terabytes de datos estructurados capaces de ofrecer servicio a aplicaciones de escala web.
- Almacenar conjuntos de datos que no requieren uniones complejas, claves externas o procedimientos almacenados y que pueden desnormalizarse para un acceso rápido.
- Consultar rápida datos mediante un índice agrupado.
- Obtenga acceso a los datos mediante el protocolo OData y las consultas LINQ con las bibliotecas .NET de Windows Communication Foundation (WCF) Data Services.

Puede usar Table Storage para almacenar y consultar conjuntos grandes de datos estructurados y no relacionales. Las tablas se escalan a medida que aumenta la demanda.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación de Azure Table Storage](/azure/storage/tables/table-storage-overview)
- [Muestras](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Guías de inicio rápido](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-files"></a>Archivos de Azure
[Azure Files](https://azure.microsoft.com/services/storage/files/) permite configurar recursos compartidos de archivos de red de alta disponibilidad a los que se puede acceder mediante el protocolo Bloque de mensajes del servidor (SMB) estándar. Varias máquinas virtuales pueden compartir los mismos archivos con acceso de lectura y escritura. También puede leer los archivos mediante la interfaz REST o las bibliotecas de cliente de Storage. Puede acceder a los archivos desde cualquier lugar del mundo mediante una dirección URL que apunte al archivo e incluya un token de firma de acceso compartido (SAS). Puede generar tokens SAS; permiten el acceso específico a un recurso privado durante un período de tiempo determinado.

Los recursos compartidos de archivos de Azure se pueden usar para:
- **Reemplazar o complementar servidores de archivos locales.** Desde sistemas operativos tan extendidos como Windows, macOS y Linux se puede montar directamente un recurso compartido de Azure Files desde cualquier lugar del mundo. Los recursos compartidos de Azure Files se pueden replicar también con Azure File Sync en servidores de Windows Server, locales o en la nube, para obtener un almacenamiento en caché eficiente y distribuido de los datos allí donde se usan.
- **Aplicaciones "Lift-and-shift"** . Migre mediante "lift and shift" aplicaciones a la nube que esperan un recurso compartido de archivos para almacenar datos de la aplicación del usuario.
- **Simplificar el desarrollo en la nube**. Azure Files también se puede utilizar de muchas formas para simplificar los nuevos proyectos de desarrollo en la nube. Por ejemplo:
    - **Configuración de aplicaciones compartidas**. Un patrón habitual entre las aplicaciones distribuidas es contar con archivos de configuración en una ubicación centralizada que permite tener acceso a ellos desde muchas instancias de aplicaciones. Las instancias de la aplicación pueden cargar su configuración a través de la API REST de File. Los usuarios pueden acceder a ellos según sea necesario montando el recurso compartido de SMB localmente.
    - **Recurso compartido de diagnóstico**. Un recurso compartido de Azure Files es un lugar adecuado para que las aplicaciones en la nube escriban sus registros, métricas y volcados de memoria. Las instancias de la aplicación pueden escribir los registros a través de la API REST de File. Los desarrolladores pueden acceder a ellos montando el recurso compartido de archivos en su máquina local. Esta funcionalidad permite una gran flexibilidad. Los desarrolladores pueden adoptar el desarrollo en la nube sin tener que abandonar las herramientas que ya conoce.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación de Azure Files](/azure/storage/files/storage-files-introduction)
- [Guías de inicio rápido](/azure/storage/files/storage-files-quick-create-use-windows)

## <a name="azure-queue-storage"></a>Azure Queue Storage
[Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/) es un servicio para almacenar grandes cantidades de mensajes, a los que se puede acceder desde cualquier lugar del mundo a través de llamadas autenticadas mediante HTTP o HTTPS. Un mensaje de la cola puede llegar a tener hasta 64 KB. Una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento. Las colas se utilizan normalmente para crear un trabajo pendiente del trabajo que se va a procesar de forma asincrónica.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación de Azure Queue Storage](/azure/storage/queues/)
- [Guías de inicio rápido](/azure/storage/queues/storage-quickstart-queues-portal)
- [Muestras](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
