---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: craigshoemaker
ms.service: azure-policy
ms.topic: include
ms.date: 05/18/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: e6a0ded137162328fd446b65ddb4a15fa6f1db88
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186528"
---
## <a name="deleting-personal-information"></a>Eliminación de información personal

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

La información personal es importante para el servicio Import/Export (a través del portal y de la API) durante las operaciones de importación y exportación. Entre los datos que se usan durante estos procesos se incluyen:

- Nombre de contacto
- Número de teléfono
- Email
- Dirección
- City
- Código postal
- State
- País/región/provincia
- Id. de unidad
- Número de cuenta del transportista
- Número de seguimiento del envío

Cuando se crea un trabajo de importación y exportación, los usuarios proporcionan información de contacto y una dirección de envío. La información personal se almacena hasta en dos ubicaciones diferentes: en el trabajo y, opcionalmente, en la configuración del portal. La información personal solo se almacena en la configuración del portal, si activa la casilla llamada **Save carrier and return address as default** (Guardar transportista y dirección de devolución como opción predeterminada) en la sección *Información de envío de devolución* del proceso de exportación.

La información de contacto personal se puede eliminar de las maneras siguientes:

- Los datos guardados con el trabajo se eliminan junto con el trabajo. Los usuarios pueden eliminar manualmente los trabajos y los trabajos completados se eliminan automáticamente después de 90 días. Puede eliminar manualmente los trabajos a través de la API REST o de Azure Portal. Para eliminar el trabajo en Azure Portal, vaya a su trabajo de importación y exportación y haga clic en *Eliminar* en la barra de comandos. Para más información sobre cómo eliminar un trabajo de importación y exportación mediante la API REST, consulte [Cancelación y eliminación de trabajos de Azure Import/Export](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md).

- La información de contacto que se guarda en la configuración del portal se puede quitar eliminando esta configuración. Puede eliminar la configuración del portal si realiza estos pasos:
  - Inicie sesión en [Azure Portal](https://portal.azure.com).
  - Haga clic en el icono *Configuración*![icono de configuración de Azure](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - Haga clic en *Exportar todas las opciones de configuración* (para guardar la configuración actual en un archivo `.json`).
  - Haga clic en *Eliminar todas las opciones de configuración y los paneles privados* para eliminar todas las opciones de configuración, incluida la información de contacto de guardada.

Para más información, revise la directiva de privacidad de Microsoft en el [Centro de confianza](https://www.microsoft.com/trustcenter).