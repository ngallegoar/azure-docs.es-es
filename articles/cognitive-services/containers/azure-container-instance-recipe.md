---
title: Fórmula de instancia de Azure Container
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo implementar contenedores de Cognitive Services en una instancia de Azure Container
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: f247465c7e2c0a212df2821ebc7165d3ee5b15f3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80876664"
---
# <a name="deploy-and-run-container-on-azure-container-instance"></a>Implementación y ejecución de un contenedor en Azure Container Instance

Con los pasos siguientes, escale aplicaciones de Azure Cognitive Services en la nube fácilmente con [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). La creación de contenedores permite centrarse en la compilación de aplicaciones en lugar de en la administración de la infraestructura. Para más información sobre el uso de contenedores, consulte [Características y ventajas](../cognitive-services-container-support.md#features-and-benefits).

## <a name="prerequisites"></a>Requisitos previos

La fórmula funciona con cualquier contenedor de Cognitive Services. El recurso de Cognitive Services debe crearse en Azure Portal antes de usar la fórmula. Todos los servicios de Cognitive Services que admiten contenedores tienen un documento del "Procedimiento de instalación" específico para instalar y configurar el servicio de un contenedor. Algunos servicios requieren un archivo o un conjunto de archivos como entrada para el contenedor, así que es importante que entienda y haya usado el contenedor correctamente antes de usar esta solución.

* Un recurso de Cognitive Services, creado en Azure Portal.
* **Dirección URL del punto de conexión** de Cognitive Services: revise el "Procedimiento de instalación" de su servicio específico para el contenedor. Así, encontrará el origen de la dirección URL del punto de conexión dentro de Azure Portal y el aspecto de un ejemplo correcto de la dirección URL. El formato exacto puede cambiar para cada servicio.
* **Clave** de Cognitive Services: las claves se encuentran en la página **Claves** del recurso de Azure. Solo necesita una de las dos claves. La clave es una cadena de 32 caracteres alfanuméricos.
* Un solo contenedor de Cognitive Services en su host local (su equipo). Asegúrese de que puede hacer lo siguiente:
  * Descargar la imagen con un comando `docker pull`.
  * Ejecutar el contenedor local correctamente con todos los valores de configuración necesarios mediante un comando `docker run`.
  * Llamar al punto de conexión del contenedor para obtener una respuesta de HTTP 2xx y una respuesta JSON.

Todas las variables en corchetes angulares, `<>`, deben reemplazarse por sus propios valores. Este reemplazo incluye los corchetes angulares.

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](includes/create-container-instances-resource.md)]

## <a name="use-the-container-instance"></a>Uso de la instancia de contenedor

1. Seleccione la **Información general** y copie la dirección IP. Será una dirección IP numérica como `55.55.55.55`.
1. Abra una nueva pestaña del explorador y use la dirección IP; por ejemplo, `http://<IP-address>:5000 (http://55.55.55.55:5000`). Verá la página de inicio del contenedor, que le permite saber que el contenedor se está ejecutando.

1. Seleccione **Service API Description** (descripción de la API de servicio) para ver la página Swagger del contenedor.

1. Seleccione cualquiera de las API **POST** y seleccione **Probarlo**.  Se muestran los parámetros junto con la entrada. Rellene los parámetros.

1. Seleccione **Ejecutar** para enviar la solicitud a la instancia de contenedor.

    Ha creado y usado correctamente contenedores de Cognitive Services en Azure Container Instance.
