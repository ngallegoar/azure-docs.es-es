---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/07/2020
ms.custom: include file
ms.openlocfilehash: 9ccdc7a438d1dade534d39dc97a39a3bdae37dc4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "75895422"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione la barra lateral principal, después **+ Crear un recurso**. 

   [![Seleccione la barra lateral principal, después + Crear un recurso](./media/create-digital-twins-portal/azure-portal-create-a-resource.png)](./media/create-digital-twins-portal/azure-portal-create-a-resource.png#lightbox)

1. Busque **Digital Twins** y seleccione **Digital Twins**. 

   [![Selecciones para crear una nueva instancia de Digital Twins](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png)](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png#lightbox)

   Como alternativa, seleccione **Internet de las cosas**y seleccione **Digital Twins(versión preliminar)** .

1. Haga clic en **Crear** para iniciar el proceso de implementación.

   [![Crear y confirmar la implementación del recurso](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png)](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png#lightbox)

1. En el panel **Digital Twins**, escriba la siguiente información:
   * **Nombre del recurso**: cree un nombre único para la instancia de Digital Twins.
   * **Suscripción**: elija la suscripción que quiere usar para crear esta instancia de Digital Twins. 
   * **Grupo de recursos**: seleccione o cree un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) para la instancia de Digital Twins.
   * **Ubicación**: seleccione la ubicación más cercana a sus dispositivos.

     [![Panel de Digital Twins en el que se ha especificado información](./media/create-digital-twins-portal/create-digital-twins-param.png)](./media/create-digital-twins-portal/create-digital-twins-param.png#lightbox)

1. Revise la información de Digital Twins y, a continuación, seleccione **Crear**. La instancia de Digital Twins podría tardar unos minutos en crearse. Puede ver el progreso en el panel **Notificaciones**.

1. Abra el panel **Información general** de la instancia de Digital Twins. Observe el vínculo situado bajo **Management API**. La dirección URL de **Management API** tiene el formato siguiente: 
   
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger
   ```
   
   Esta dirección URL le lleva a la documentación de la API REST de Azure Digital Twins que se aplica a la instancia. Lea [Uso de Azure Digital Twins Swagger](../articles/digital-twins/how-to-use-swagger.md) para aprender a leer y usar esta documentación de API. Copie y modifique la dirección URL de **API de Administración** para que tenga este formato: 
    
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/
   ```
    
   La aplicación usará la dirección URL modificada como dirección URL base para acceder a la instancia. Copie esta dirección URL modificada en un archivo temporal. La necesitará en la siguiente sección.

   [![Introducción a la API de administración](./media/create-digital-twins-portal/digital-twins-management-api.png)](./media/create-digital-twins-portal/digital-twins-management-api.png#lightbox)