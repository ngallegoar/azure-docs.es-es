---
title: API y herramientas para desarrolladores
description: Obtenga información acerca de las API y herramientas disponibles para desarrollar soluciones con el servicio Azure Batch.
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: e345e91b2f7d66f014427770614efe42b5fb7a44
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232690"
---
# <a name="overview-of-batch-apis-and-tools"></a>Información general sobre las API y herramientas de Batch

El procesamiento de cargas de trabajo paralelas con Azure Batch se suele realizar mediante programación con una de las API de Batch. Un servicio o una aplicación cliente pueden usar las API de Batch para comunicarse con el servicio Batch. Con las API de Batch se pueden crear y administrar grupos de nodos de proceso, tanto máquinas virtuales como servicios en la nube. A continuación, se puede programar que se ejecuten trabajos y tareas en dichos nodos. 

Puede procesar de forma eficiente cargas de trabajo a gran escala para su organización, o bien proporcionar un front-end del servicio a los clientes para que puedan ejecutar trabajos y tareas (a petición o de forma programada) en uno, cientos, o incluso, miles de nodos. Azure Batch también se puede usar como parte de un flujo de trabajo mayor, administrado mediante herramientas como [Azure Data Factory](../data-factory/transform-data-using-dotnet-custom-activity.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Cuando esté listo para adentrarse en la API de Batch para una mayor comprensión de las características que proporciona, consulte [Información general de las características de Azure Batch](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Cuentas de Azure para desarrollo con Batch
Cuando se desarrollan soluciones de Batch, se usan las siguientes cuentas de la suscripción de Azure:

* **Cuenta de Batch**: los recursos de Azure Batch, entre los que se incluyen grupos, nodos de proceso, trabajos y tareas, están asociados a una cuenta de [Azure Batch](batch-api-basics.md#account). Cuando la aplicación realiza una solicitud en el servicio Batch, autentica la solicitud mediante el nombre de la cuenta de Azure Batch, la dirección URL de la cuenta y una clave de acceso<o un token de Azure Active Directory. Puede [crear una cuenta de Batch](batch-account-create-portal.md) en Azure Portal o mediante programación.
* **Cuenta de Storage**: Batch incluye compatibilidad integrada para trabajar con archivos en [Azure Storage][azure_storage]. Casi todos los escenarios de Batch usan Azure Blob Storage tanto para el almacenamiento provisional de los programas que ejecutan las tareas como de los datos que procesan, así como para el almacenamiento de los datos de salida que generan. Para conocer las opciones de cuenta de almacenamiento de Batch, consulte la [introducción a la característica de Batch](batch-api-basics.md#azure-storage-account).

## <a name="batch-service-apis"></a>API del servicio Batch

Las aplicaciones y servicios pueden emitir llamadas directas de la API de REST o usar una o varias de las siguientes bibliotecas de cliente para ejecutar y administrar las cargas de trabajo de Azure Batch.

| API | Referencia de API | Descargar | Tutorial | Ejemplos de código | Más información |
| --- | --- | --- | --- | --- | --- |
| **REST de Batch** |[docs.microsoft.com][batch_rest] |N/D |- |- | [Versiones compatibles](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **.NET de Batch** |[docs.microsoft.com][api_net] |[NuGet][api_net_nuget] |[Tutorial](tutorial-parallel-dotnet.md) |[GitHub][api_sample_net] | [Notas de la versión](https://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[docs.microsoft.com][api_python] |[PyPI][api_python_pypi] |[Tutorial](tutorial-parallel-python.md)|[GitHub][api_sample_python] | [Léame](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/batch/azure-batch/README.md) |
| **Batch Node.js** |[docs.microsoft.com][api_nodejs] |[npm][api_nodejs_npm] |[Tutorial](batch-nodejs-get-started.md) |- | [Léame](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[docs.microsoft.com][api_java] |[Maven][api_java_jar] |- |[Léame][api_sample_java] | [Léame](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>API de administración de Batch

Las API de Azure Resource Manager para Batch proporcionan acceso mediante programación a las cuentas de Batch. Gracias a estas API, puede administrar mediante programación las cuentas de Batch, las cuotas, los paquetes de aplicaciones y otros recursos mediante el proveedor Microsoft.Batch.  

| API | Referencia de API | Descargar | Tutorial | Ejemplos de código |
| --- | --- | --- | --- | --- |
| **Batch Management REST** |[docs.microsoft.com][api_rest_mgmt] |N/D |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Management .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet][api_net_mgmt_nuget] | [Tutorial](batch-management-dotnet.md) |[GitHub][api_sample_net] |
| **Batch Management Python** |[docs.microsoft.com][api_python_mgmt] |[PyPI][api_python_mgmt_pypi] |- |- |
| **Batch Management Node.js** |[docs.microsoft.com][api_nodejs_mgmt] |[npm][api_nodejs_mgmt_npm] |- |- | 
| **Batch Management Java** |- |[Maven][api_java_mgmt_jar] |- |- |
## <a name="batch-command-line-tools"></a>Herramientas de línea de comandos de Batch

Estas herramientas de línea de comandos proporcionan la misma funcionalidad que el servicio Batch y las API de administración de Batch: 

* [Cmdlets de PowerShell para Batch][batch_ps]: los cmdlets del módulo [Azure PowerShell](/powershell/azure/overview) para Azure Batch permiten administrar los recursos de Batch con PowerShell.
* [CLI de Azure](/cli/azure): la CLI de Azure es un conjunto de herramientas multiplataforma que proporciona comandos de shell para interactuar con muchos de los servicios de Azure, incluidos el servicio Batch y el servicio de administración de Batch. Consulte [Administración de recursos de Batch con la CLI de Azure](batch-cli-get-started.md) para más información sobre el uso de la CLI de Azure con Batch.

## <a name="other-tools-for-application-development"></a>Otras herramientas de desarrollo de aplicaciones

Estas son algunas herramientas adicionales que pueden ser útiles para crear y depurar los servicios y aplicaciones de Batch:

* [Portal de Azure][portal]: puede crear, supervisar y eliminar grupos, trabajos y tareas de Batch en Azure Portal. La información relativa al estado de estos y otros recursos se puede ver mientras se ejecutan trabajos, e incluso se pueden descargar archivos desde los nodos de proceso de los grupos. Por ejemplo, puede descargar el archivo `stderr.txt` de una tarea con errores mientras soluciona problemas. También puede descargar los archivos de Escritorio remoto (RDP) que puede usar para iniciar sesión en los nodos de proceso.
* [Azure Batch Explorer][batch_labs]: Batch Explorer (anteriormente BatchLabs) es una herramienta de cliente independiente, completa y gratuita que puede ayudarle a crear, depurar y supervisar las aplicaciones de Azure Batch. Descargue un [paquete de instalación](https://azure.github.io/BatchExplorer/) para Mac, Linux o Windows.
* [Azure Batch Shipyard](https://github.com/Azure/batch-shipyard): Batch Shipyard es una herramienta para ayudar a aprovisionar, ejecutar y supervisar el procesamiento por lotes basado en contenedores y las cargas de trabajo HPC en Azure Batch.
* [Explorador de Azure Storage][storage_explorer] aunque no es estrictamente una herramienta de Azure Batch, el Explorador de Storage es otra herramienta muy útil para desarrollar y depurar soluciones de Batch.

## <a name="additional-resources"></a>Recursos adicionales

- Para más información acerca del registro de eventos de la aplicación de Batch, consulte [Registro de eventos para evaluación de diagnóstico y supervisión de soluciones de Batch](batch-diagnostics.md). Para ver una referencia de eventos generados por el servicio Batch, consulte [Análisis de Batch](batch-analytics.md).
- Para más información acerca de las variables de entorno para nodos de proceso, consulte [Variables de entorno de nodos de proceso de Azure Batch](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Información general de las características de Batch para desarrolladores](batch-api-basics.md), donde encontrará información esencial para cualquier persona que vaya a utilizar Batch. El artículo contiene información más detallada acerca de los recursos del servicio Batch, como grupos, nodos, trabajos y tareas, así como las numerosas características de API que se pueden usar al compilar cualquier aplicación de Batch.
* [Introducción a la biblioteca de Azure Batch para .NET](tutorial-parallel-dotnet.md) . También hay disponible una versión del tutorial para [Python](tutorial-parallel-python.md) y [Node.js](batch-nodejs-get-started.md).
* Descargue los [ejemplos de código en GitHub][github_samples] para ver la forma en que C# y Python pueden comunicarse con Batch para programar y procesar las cargas de trabajo de ejemplo.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: /java/api/overview/azure/batch
[api_java_mgmt]: /java/api/overview/azure/batch/managementapi
[api_java_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_java_mgmt_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/
[api_net_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /javascript/api/overview/azure/batch/client
[api_nodejs_mgmt]: /javascript/api/overview/azure/batch/management
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_nodejs_mgmt_npm]: https://www.npmjs.com/package/azure-arm-batch
[api_python]: /python/api/overview/azure/batch/client
[api_python_mgmt]: /python/api/overview/azure/batch/management
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_python_mgmt_pypi]: https://pypi.python.org/pypi/azure-mgmt-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/az.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchExplorer/
[storage_explorer]: https://storageexplorer.com/
[portal]: https://portal.azure.com
