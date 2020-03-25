---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76545229"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Configuración de la sesión de anclaje espacial en la nube

A continuación, vamos a configurar la sesión de anclaje espacial en la nube. En la primera línea, se establece el proveedor del sensor en la sesión. A partir de ahora, todos los delimitadores que se creen en la sesión se asociarán a un conjunto de lecturas del sensor. A continuación, se instancian los criterios de búsqueda de dispositivos cercanos y se inicializan para que coincidan con los requisitos de la aplicación. Por último, se indica a la sesión que use los datos del sensor al localizar los delimitadores mediante la creación de un monitor a partir de nuestros criterios de dispositivos cercanos.