---
title: Preguntas más frecuentes sobre Peering Service
titleSuffix: Azure
description: Preguntas más frecuentes sobre Peering Service
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: reference
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 507e503b881df123ffc6694c53b0e9cc9b6a8872
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75774027"
---
# <a name="peering-service---faqs"></a>Preguntas más frecuentes sobre Peering Service

Puede revisar la información que se ofrece más abajo para ver las preguntas generales.

**¿Un operador puede usar el emparejamiento directo existente con Microsoft para admitir Peering Service?**

Sí, un operador puede aprovechar su PNI para admitir Peering Service. Una interconexión de redes privadas (PNI) de Peering Service necesita diversidad para admitir la alta disponibilidad. Si la PNI existente ya tiene diversidad, no se necesita ninguna infraestructura nueva. Si la PNI existente necesita diversidad, se puede aumentar.

**¿Los operadores pueden usar un nuevo emparejamiento directo con Microsoft para admitir Peering Service?**

Sí, eso también es posible. Microsoft trabajará con el operador para crear un nuevo emparejamiento directo para admitir Peering Service.  

**¿Por qué se necesita el emparejamiento directo para admitir Peering Service?**

Uno de los objetivos principales de Peering Service es proporcionar conectividad a los servicios en línea de Microsoft a través de un SP bien conectado. Las PNI siempre están en el intervalo de Gbps, por lo tanto, se trata de un bloque de creación fundamental para la conectividad de alto rendimiento entre el operador y Microsoft.

**¿Cuáles son los requisitos de diversidad en el emparejamiento directo para admitir Peering Service?**

La PNI debe admitir la redundancia local y la redundancia geográfica. La redundancia local se define como dos conjuntos distintos de rutas de acceso en un sitio de emparejamiento determinado. La redundancia geográfica necesita que el operador tenga conectividad adicional en un sitio de Microsoft Edge diferente en caso de que se produzca un error en el sitio principal. En el caso un error de corta duración, el operador puede redirigir el tráfico a través del sitio de copia de seguridad.

**El operador ya ofrece un Acuerdo de Nivel de Servicio e Internet de categoría empresarial, ¿en qué se diferencia esta oferta?**

Algunos operadores ofrecen un Acuerdo de Nivel de Servicio e Internet de categoría empresarial en su parte de la red. En Peering Service, Microsoft proporcionará un Acuerdo de Nivel de Servicio en cuanto al tráfico en la parte de Microsoft de la red. Al seleccionar Peering Service, el cliente obtendrá un Acuerdo de Nivel de Servicio completo. El ISP puede cubrir el Acuerdo de Nivel de Servicio de su sitio en Microsoft Edge, en la red del ISP. Ahora, Microsoft se encarga de la aplicación del Acuerdo de Nivel de Servicio de su red global desde Microsoft Edge a los usuarios finales.

**Si un proveedor de servicios ya está en el mismo nivel que Microsoft con una PNI, ¿qué tipo de cambios son necesarios para admitir Peering Service?**

* Cambios de software para identificar a un usuario de Peering Service y su tráfico. Puede ser necesario realizar cambios en la directiva de enrutamiento para intercambiar el tráfico de un usuario en la instancia de Microsoft Edge más próxima a través de la conexión de Peering Service.
* Es necesario asegurarse de que la conectividad tiene redundancia local y redundancia geográfica.
