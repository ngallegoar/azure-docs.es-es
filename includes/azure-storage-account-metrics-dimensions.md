---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 6bda702a90d1204de6f2b80ced9a4704f1e8426d
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2020
ms.locfileid: "91711084"
---
| Nombre de dimensión | Descripción |
| ------------------- | ----------------- |
| **GeoType** | Transacción de clúster principal o secundario. Los valores disponibles incluyen **Principal** y **Secundario**. Se aplica al Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) al leer objetos de un inquilino secundario. |
| **ResponseType** | Tipo de respuesta de la transacción. Los valores disponibles son: <br/><br/> <li>**ServerOtherError**: todos los errores del servidor, excepto los descritos. </li> <li>**ServerBusyError**: solicitud autenticada que devolvió un código de estado HTTP 503. </li> <li>**ServerTimeoutError**: solicitud autenticada que ha superado el tiempo de espera y que devolvió un código de estado HTTP 500. El tiempo de espera se superó debido a un error del servidor. </li> <li>**AuthorizationError**: solicitud autenticada errónea debido a un acceso no autorizado de los datos o a un error de autorización. </li> <li>**NetworkError**: solicitud autenticada errónea debido a errores de red. Normalmente se produce cuando un cliente cierra prematuramente una conexión antes de que se haya superado el tiempo de expiración. </li><li>**ClientAccountBandwidthThrottlingError**: la solicitud está limitada por el ancho de banda para superar los [límites de escalabilidad de la cuenta de almacenamiento ](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account?toc=/azure/storage/blobs/toc.json).</li><li>**ClientAccountRequestThrottlingError**: la solicitud está limitada por la tasa de solicitud para superar los [límites de escalabilidad de la cuenta de almacenamiento ](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account?toc=/azure/storage/blobs/toc.json).<li>**ClientThrottlingError**: Otro error de limitación del lado del cliente. ClientAccountBandwidthThrottlingError y ClientAccountRequestThrottlingError se excluyen.</li> <li>**ClientTimeoutError**: solicitud autenticada que ha superado el tiempo de espera y que devolvió un código de estado HTTP 500. Si el tiempo de expiración de la red del cliente o el tiempo de expiración de la solicitud se establece en un valor menor de lo que espera el servicio de almacenamiento, es un tiempo de expiración esperado. De lo contrario, se notifica como un error ServerTimeoutError. </li> <li>**ClientOtherError**: todos los errores del lado cliente, excepto los descritos. </li> <li>**Correcto**: solicitud correcta.</li> <li> **SuccessWithThrottling**: solicitud correcta cuando un cliente SMB obtiene una limitación en el primer intento, pero se ejecuta correctamente después de varios reintentos.</li> |
| **ApiName** | El nombre de la operación. 
| **Autenticación** | Tipos de autenticación que se usan en las transacciones. Los valores disponibles son: <br/> <li>**AccountKey**: la transacción se autentica con la clave de la cuenta de almacenamiento.</li> <li>**SAS**: la transacción se autentica con firmas de acceso compartido.</li> <li>**OAuth**: la transacción se autentica con tokens de acceso de OAuth.</li> <li>**Anonymous**: la transacción se solicita de forma anónima. No incluye las solicitudes preparatorias.</li> <li>**AnonymousPreflight**: la transacción es una solicitud preparatoria.</li> |