---
author: IEvangelist
ms.author: dapine
ms.date: 08/22/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 081155c8984f39cc9cc8e905eb108c07dee98d2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "70034423"
---
## <a name="validate-that-a-container-is-running"></a>Comprobación de que un contenedor está en ejecución 

Hay varias maneras de comprobar que el contenedor está en ejecución. Busque la dirección *IP externa* y el puerto expuesto del contenedor en cuestión y abra el explorador web que prefiera. Use las distintas direcciones URL de solicitud para validar que el contenedor se está ejecutando. Las direcciones URL de solicitud de ejemplo que se enumeran a continuación son `http://localhost:5000`, pero el contenedor específico puede variar. Tenga en cuenta que va a confiar en la *dirección IP externa* y el puerto expuesto del contenedor.

| URL de la solicitud | Propósito |
|--|--|
| `http://localhost:5000/` | El contenedor ofrece una página principal. |
| `http://localhost:5000/status` | Se solicitó con HTTP GET, para comprobar que el contenedor está en ejecución sin causar una consulta al punto de conexión. Esta solicitud se puede usar con los [sondeos de ejecución y preparación](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) de Kubernetes. |
| `http://localhost:5000/swagger` | El contenedor cuenta con un completo conjunto de documentación sobre los puntos de conexión y una característica de **prueba**. Esta característica le permite especificar la configuración en un formulario HTML basado en web y realizar la consulta sin necesidad de escribir código. Una vez que la consulta devuelve resultados, se proporciona un ejemplo del comando CURL para mostrar los encabezados HTTP y el formato de cuerpo requeridos. |

![Página principal del contenedor](./media/cognitive-services-containers-api-documentation/container-webpage.png)
