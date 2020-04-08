---
title: 'Introducción a Azure Storage: almacenamiento en la nube en Azure | Microsoft Docs'
description: Azure Storage es una solución de almacenamiento en la nube de Microsoft. Azure Storage proporciona almacenamiento para objetos de datos, que presenta una alta disponibilidad, es seguro, durable y redundante y se puede escalar de forma masiva.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5bab70b6b023a4e6510e32368d407a38388cde2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228576"
---
# <a name="introduction-to-azure-storage"></a>Introducción a Azure Storage

Azure Storage es la solución de almacenamiento de Microsoft para los escenarios modernos de almacenamiento de datos. Azure Storage ofrece un almacén de objetos que se puede escalar de forma masiva destinado a objetos de datos, un servicio de sistema de archivos para la nube, un almacén de mensajería para mensajería confiable y un almacén NoSQL. Azure Storage se caracteriza por ofrecer lo siguiente:

- **Durabilidad y elevada disponibilidad.** La redundancia garantiza que los datos están seguros en caso de errores de hardware transitorios. También puede optar por replicar datos entre centros de datos o regiones geográficas para obtener protección adicional frente a catástrofes locales o desastres naturales. Los datos replicados de esta manera permanecen con una alta disponibilidad en caso de una interrupción inesperada.
- **Seguridad.** Todos los datos escritos en Azure Storage se cifran mediante el servicio. Azure Storage proporciona un control pormenorizado sobre quién tiene acceso a los datos.
- **Escalabilidad.** Azure Storage está diseñado para poderse escalar de forma masiva para satisfacer las necesidades de rendimiento y almacenamiento de datos de las aplicaciones de hoy en día.
- **Administración.** Microsoft Azure controla automáticamente el mantenimiento, las actualizaciones y los problemas críticos del hardware.
- **Accesibilidad.** Es posible acceder a los datos de Azure Storage desde cualquier parte del mundo a través de HTTP o HTTPS. Microsoft proporciona bibliotecas cliente para Azure Storage en diversos lenguajes, incluidos .NET, Java, Node.js, Python, PHP, Ruby, Go y otros, así como una API REST consolidada. Azure Storage admite la escritura en Azure PowerShell o la CLI de Azure. Y Azure Portal y el Explorador de Azure Storage ofrecen soluciones visuales sencillas para trabajar con los datos.  

## <a name="azure-storage-services"></a>Servicios de Azure Storage

Azure Storage incluye estos servicios de datos:

- [Blobs de Azure](../blobs/storage-blobs-introduction.md): un almacén de objetos que se puede escalar de forma masiva para datos de texto y binarios.
- [Azure Files](../files/storage-files-introduction.md): recursos compartidos de archivos administrados para implementaciones locales y en la nube.
- [Colas de Azure](../queues/storage-queues-introduction.md): un almacén de mensajería para mensajería confiable entre componentes de aplicación.
- [Tablas de Azure](../tables/table-storage-overview.md): un almacén NoSQL para el almacenamiento sin esquema de datos estructurados.

Para acceder a cada servicio se usa una cuenta de almacenamiento. Para comenzar, consulte [Crear una cuenta de almacenamiento](storage-account-create.md).

## <a name="blob-storage"></a>Blob Storage

Azure Blob Storage es la solución de almacenamiento de objetos de Microsoft para la nube. Blob Storage está optimizado para el almacenamiento de cantidades masivas de datos no estructurados, como texto o datos binarios.

Blob Storage resulta muy conveniente para lo siguiente:

- Visualización de imágenes o documentos directamente en un explorador.
- Almacenamiento de archivos para acceso distribuido.
- Streaming de audio y vídeo.
- Almacenamiento de datos para copia de seguridad y restauración, recuperación ante desastres y archivado.
- Almacenamiento de datos para el análisis en local o en un servicio hospedado de Azure.

Se puede acceder a los objetos de Blob Storage desde cualquier lugar del mundo a través de HTTP o HTTPS. Los usuarios o las aplicaciones cliente pueden acceder a los blobs mediante direcciones URL, la [API REST de Azure Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), la [CLI de Azure](https://docs.microsoft.com/cli/azure/storage) o una biblioteca cliente de Azure Storage. Las bibliotecas de cliente de almacenamiento están disponibles para varios lenguajes, como [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), [Java](https://docs.microsoft.com/java/api/overview/azure/storage), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](https://azure.github.io/azure-storage-php/) y [Ruby](https://azure.github.io/azure-storage-ruby).

Para más información acerca de Azure Blob Storage, consulte [Introducción al almacenamiento de objetos en Azure](../blobs/storage-blobs-introduction.md).

## <a name="azure-files"></a>Azure Files

[Azure Files](../files/storage-files-introduction.md) permite configurar recursos compartidos de archivos de red de alta disponibilidad a los que se puede acceder mediante el protocolo Bloque de mensajes del servidor (SMB) estándar. Esto significa que varias máquinas virtuales pueden compartir los mismos archivos con acceso de lectura y escritura. También puede leer los archivos mediante la interfaz REST o las bibliotecas de cliente de Storage.

Lo que distingue Azure Files de los archivos en un recurso compartido de archivos corporativo es que puede acceder a ellos desde cualquier lugar del mundo mediante una dirección URL que apunte al archivo e incluya un token de firma de acceso compartido (SAS). Puede generar tokens SAS; permiten el acceso específico a un recurso privado durante un período de tiempo determinado.

Los recursos compartidos de archivos se pueden utilizar para muchos escenarios comunes:

- Muchas aplicaciones locales usan recursos compartidos de archivos. Esta característica facilita la migración de las aplicaciones que comparten datos en Azure. Si monta el recurso compartido de archivos en la misma letra de unidad que usa la aplicación local, la parte de la aplicación que tiene acceso al recurso compartido de archivos debería funcionar con cambios mínimos, si es que hay alguno.

- Los archivos de configuración se pueden almacenar en un recurso compartido de archivos y se puede acceder a ellos desde varias máquinas virtuales. Las herramientas y utilidades que usen varios desarrolladores de un grupo pueden almacenarse en un recurso compartido de archivos, lo que garantiza que todos los usuarios puedan encontrarlas y que utilizan la misma versión.

- Los registros de diagnóstico, las métricas y los volcados de memoria son solo tres ejemplos de datos que se pueden escribir en un recurso compartido de archivos y procesarse o analizarse posteriormente.

En este momento, las listas de control de acceso (ACL) y la autenticación basada en Active Directory no se admiten, pero se admitirán en algún momento. Las credenciales de las cuentas de almacenamiento se usan para permitir la autenticación al recurso compartido de archivos. Esto significa que cualquier usuario con el recurso compartido montado tendrá acceso completo de lectura/escritura al recurso compartido.

Para más información sobre Azure Files, consulte [Introducción a Azure Files](../files/storage-files-introduction.md).

## <a name="queue-storage"></a>Queue Storage

El servicio Azure Queue se utiliza para almacenar y recuperar mensajes. Los mensajes de la cola pueden tener un tamaño de hasta 64 KB y una cola contener millones de mensajes. Las colas se utilizan generalmente para almacenar listas de mensajes y procesarlas de forma asincrónica.

Por ejemplo, supongamos que quiere que los clientes puedan cargar imágenes y desea crear vistas en miniatura de cada una. El cliente puede esperar a que cree las vistas en miniatura mientras se cargan las imágenes. Una alternativa sería utilizar una cola. Cuando el cliente finalice la carga, escriba un mensaje en la cola. A continuación, haga que una función de Azure recupera el mensaje de la cola y cree las vistas en miniatura. Las partes de este procesamiento se pueden escalar por separado, por lo que controlará mejor el ajuste para usarlas.

Para más información sobre las colas de Azure, consulte [Introducción a las colas](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Almacenamiento de tablas

Azure Table Storage ahora forma parte de Azure Cosmos DB. Para ver la documentación de Azure Table Storage, consulte [Introducción a Azure Table Storage](../tables/table-storage-overview.md). Además del servicio Azure Table Storage existente, hay una nueva Table API de Azure Cosmos DB que ofrece tablas con rendimiento optimizado, distribución global e índices secundarios automáticos. Para más información sobre la nueva experiencia prémium y poder probarla, consulte [Introducción a Table API de Azure Cosmos DB.](https://aka.ms/premiumtables)

Para más información sobre Table Storage, consulte [Introducción a Azure Table Storage](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Disk Storage

Un disco administrado de Azure es un disco duro virtual (VHD). Se puede considerar como un disco físico en un servidor en el entorno local, pero virtualizado. Los discos administrados de Azure se almacenan como blobs en páginas, que son un objeto de almacenamiento de E/S aleatorio en Azure. A estos discos los denominamos "administrados", ya que son una abstracción sobre los blobs en páginas, los contenedores de blobs y las cuentas de almacenamiento de Azure. Con los discos administrados, lo único que debe hacer es aprovisionar el disco y Azure se encarga del resto.

Para más información sobre discos administrados, consulte [Introducción a los discos administrados de Azure](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="types-of-storage-accounts"></a>Tipos de cuentas de almacenamiento

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

Para más información acerca de los tipos de cuentas de almacenamiento, consulte la [Introducción a la cuenta de Azure Storage](storage-account-overview.md).

## <a name="securing-access-to-storage-accounts"></a>Protección del acceso a las cuentas de almacenamiento

Cada solicitud para Azure Storage se debe autorizar. Azure Storage admite los siguientes métodos de autorización:

- **Integración de Azure Active Directory (Azure AD) para datos de blob y cola.** Azure Storage admite la autenticación y autorización con Azure AD para los servicios Blob y Queue a través del control de acceso basado en rol (RBAC). Se recomiendan la autorización de solicitudes con Azure AD para mayor seguridad y facilidad de uso. Para más información, consulte [Autenticación del acceso a blobs y colas de Azure con Azure Active Directory](storage-auth-aad.md).
- **Autorización de Azure AD a través de SMB para Azure Files (versión preliminar).** Azure Files admite la autorización basada en identidades a través de SMB (Bloque de mensajes del servidor) mediante Azure Active Directory Domain Services. Las máquinas virtuales (VM) Windows unidas al dominio pueden acceder a los recursos compartidos de archivos de Azure con las credenciales de Azure AD. Para más información, consulte [Introducción a la autenticación de Azure Active Directory sobre SMB para Azure Files (versión preliminar)](../files/storage-files-active-directory-overview.md).
- **Autorización con clave compartida.** Los servicios de Azure Storage Blob, Queue y Table y Azure Files admiten la autorización con clave compartida. Un cliente que usa la autorización de clave compartida pasa un encabezado con cada solicitud que se firma con la clave de acceso de la cuenta de almacenamiento. Para más información, consulte el artículo sobre la [Autorización con clave compartida](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key).
- **Autorización mediante las firmas de acceso compartido (SAS).** Una firma de acceso compartido (SAS) es una cadena que contiene un token de seguridad que se puede asociar a un URI para un recurso de almacenamiento. El token de seguridad encapsula las restricciones, como pueden ser los permisos y el intervalo de acceso. Para más información, consulte [Uso de firmas de acceso compartido (SAS)](storage-sas-overview.md).
- **Acceso anónimo a contenedores y blobs.** Un contenedor y sus blobs pueden estar disponibles públicamente. Cuando se especifica que un contenedor o blob es público, todos los usuarios pueden leerlo de forma anónima: no se requiere autenticación. Para más información, consulte [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md) (Administración del acceso de lectura anónimo a contenedores y blobs).

## <a name="encryption"></a>Cifrado

Hay dos tipos básicos de cifrado disponibles para los servicios de almacenamiento. Para más información sobre la seguridad y el cifrado, consulte [Guía de seguridad de Azure Storage](../blobs/security-recommendations.md).

### <a name="encryption-at-rest"></a>Cifrado en reposo

El cifrado de Azure Storage protege y salvaguarda los datos para satisfacer los compromisos de cumplimiento y seguridad de su organización. Azure Storage cifra automáticamente todos los datos antes de permanecer en la cuenta de almacenamiento y los descifra después de la recuperación. Los procesos de cifrado, descifrado y administración de claves son totalmente transparentes para los usuarios. Los clientes también pueden optar por administrar sus propias claves con Azure Key Vault. Para más información, consulte [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md).

### <a name="client-side-encryption"></a>Cifrado de cliente

Las bibliotecas cliente de Azure Storage proporcionan métodos para cifrar los datos de la biblioteca cliente antes de transmitirlos y descifrar la respuesta. Azure Storage también cifra en reposo los datos cifrados mediante el cifrado del lado cliente. Para más información sobre el cifrado en el lado cliente, consulte [Cifrado del lado de cliente y Azure Key Vault para Microsoft Azure Storage](storage-client-side-encryption.md).

## <a name="redundancy"></a>Redundancia

Para asegurarse de que los datos son duraderos, Azure Storage almacena varias copias de los mismos. Al configurar la cuenta de almacenamiento, selecciona una opción de redundancia.

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

## <a name="transferring-data-to-and-from-azure-storage"></a>Transferencia de datos hacia y desde Azure Storage

Tiene varias opciones para introducir o sacar datos de Azure Storage. La opción que elija depende del tamaño del conjunto de datos y del ancho de banda de red. Para más información, consulte [Elección de la solución de Azure para la transferencia de datos](storage-choose-data-transfer-solution.md).

## <a name="pricing"></a>Precios

Para información detallada sobre los precios de Azure Storage, consulte la [página de precios](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>API, bibliotecas y herramientas de Storage

Es posible acceder a los recursos de Azure Storage por medio de cualquier lenguaje que pueda realizar solicitudes HTTP/HTTPS. Además, Azure Storage ofrece bibliotecas de programación para varios lenguajes de amplio uso. Estas bibliotecas simplifican muchos aspectos relacionados con el uso de Azure Storage, ya que abordan detalles como la invocación sincrónica y asincrónica, el procesamiento por lotes de las operaciones, la administración de excepciones, los reintentos automáticos y el comportamiento operativo, entre otros. Actualmente hay bibliotecas disponibles para los siguientes lenguajes y plataformas, además de otros previstos:

### <a name="azure-storage-data-api-and-library-references"></a>Referencias de biblioteca y de API de datos de Azure Storage

- [API de REST de Azure Storage](https://docs.microsoft.com/rest/api/storageservices/)
- [Biblioteca cliente de Azure Storage para .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
- [Biblioteca cliente de Azure Storage para Java/Android](https://docs.microsoft.com/java/api/overview/azure/storage)
- [Biblioteca cliente de Azure Storage para Node.js](https://docs.microsoft.com/javascript/api/overview/azure/storage-overview)
- [Biblioteca cliente de Azure Storage para Python](https://github.com/Azure/azure-storage-python)
- [Biblioteca cliente de Azure Storage para PHP](https://github.com/Azure/azure-storage-php)
- [Biblioteca cliente de Azure Storage para Ruby](https://github.com/Azure/azure-storage-ruby)
- [Biblioteca cliente de Azure Storage para C++](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>Referencias de biblioteca y de API de administración de Azure Storage

- [API REST del proveedor de recursos de almacenamiento](https://docs.microsoft.com/rest/api/storagerp/)
- [Biblioteca de cliente de proveedor de recursos de almacenamiento para .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
- [API de REST de administración de servicios de almacenamiento (clásico)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>Referencias de biblioteca y de API de movimiento de datos de Azure Storage

- [API de REST del servicio Import/Export de Storage](https://docs.microsoft.com/rest/api/storageimportexport/)
- [Biblioteca de cliente de movimiento de datos de almacenamiento para .NET](/dotnet/api/microsoft.azure.storage.datamovement)

### <a name="tools-and-utilities"></a>Herramientas y utilidades

- [Cmdlets de Azure PowerShell para Storage](https://docs.microsoft.com/powershell/module/az.storage)
- [Cmdlets de la CLI de Azure para Storage](https://docs.microsoft.com/cli/azure/storage)
- [Utilidad de línea de comandos AzCopy](https://aka.ms/downloadazcopy)
- El [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) es una aplicación independiente y gratuita de Microsoft que permite trabajar visualmente con los datos de Azure Storage en Windows, macOS y Linux.
- [Herramientas de cliente de Azure Storage](../storage-explorers.md)
- [Herramientas de desarrollo de Azure](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Pasos siguientes

Para iniciar y ejecutar con Azure Storage, consulte [Crear una cuenta de almacenamiento](storage-account-create.md).
