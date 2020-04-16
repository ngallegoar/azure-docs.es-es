---
title: Copia o clonación de una factoría de datos en Azure Data Factory
description: Obtenga información sobre cómo copiar o clonar una factoría de datos en Azure Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 01/09/2019
ms.openlocfilehash: 5e44bda8648fbf26487b04cf36a8fd0ec085c411
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414105"
---
# <a name="copy-or-clone-a-data-factory-in-azure-data-factory"></a>Copia o clonación de una factoría de datos en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se describe cómo copiar o clonar una factoría de datos en Azure Data Factory.

## <a name="use-cases-for-cloning-a-data-factory"></a>Casos de uso para la clonación de una factoría de datos

Estas son algunas de las circunstancias en las que podría resultar útil copiar o clonar una factoría de datos:

-   **Cambiar el nombre de los recursos**. Azure no permite cambiar el nombre de los recursos. Si quiere cambiar el nombre de una factoría de datos, puede clonar la factoría de datos con otro nombre y, luego, eliminar la existente.

-   **Depurar los cambios** cuando las características de depuración no sean suficientes. En algunas ocasiones, para probar los cambios, es posible que quiera probarlos en otra factoría antes de aplicarlos en la principal. En la mayoría de los escenarios, puede usar Depurar. Sin embargo, probablemente los cambios en los desencadenadores no se puedan probar fácilmente sin insertarlos en el repositorio, por ejemplo, como el comportamiento que tienen los cambios cuando un desencadenador se invoca automáticamente o durante una ventana de tiempo. En estos casos, clonar la factoría y aplicar ahí los cambios tiene mucho sentido. Como Azure Data Factory cobra principalmente por la cantidad de ejecuciones, la segunda factoría no genera ningún cobro adicional.

## <a name="how-to-clone-a-data-factory"></a>Clonación de una factoría de datos

1. La interfaz de usuario de Data Factory en Azure Portal permite exportar toda la carga de la factoría de datos a una plantilla de Resource Manager, junto con un archivo de parámetro que permite cambiar cualquier valor que quiera cuando se clone la factoría.

1. Como requisito previo, debe crear la factoría de datos de destino desde Azure Portal.

1. Si tiene una instancia de Integration Runtime autohospedada en la factoría de origen, deberá crearla previamente con el mismo nombre en el generador de destino. Si desea compartir las instancias de Integration Runtime autohospedada entre diferentes factorías, puede usar el modelo publicado [aquí](source-control.md#best-practices-for-git-integration).

1. Si está en modo GIT, cada vez que publique desde el portal, la plantilla de Resource Manager de la factoría se guarda en GIY en la rama adf_publish del repositorio.

1. Para otros escenarios, la plantilla de Resource Manager se puede descargar con un clic en el botón **Exportar plantilla de Resource Manager** en el portal.

1. Después de descargar la plantilla de Resource Manager, puede implementarla a través de métodos de implementación de plantilla de Resource Manager estándar.

1. Por motivos de seguridad, la plantilla de Resource Manager generada no contiene información secreta, como contraseñas para los servicios vinculados. Como resultado, debe proporcionar estas contraseñas como parámetros de implementación. Si no es recomendable proporcionar parámetros, debe obtener las cadenas de conexión y las contraseñas de los servicios vinculados desde Azure Key Vault.

## <a name="next-steps"></a>Pasos siguientes

Revise la guía para crear una factoría de datos en Azure Portal en [Creación de una factoría de datos con la interfaz de usuario de Azure Data Factory](quickstart-create-data-factory-portal.md).
