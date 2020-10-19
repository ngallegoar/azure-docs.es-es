---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: b83951cd743afc7e249e3ee37182007bbe40cb5c
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971394"
---
## <a name="create-a-spatial-anchors-resource"></a>Creación de un recurso de Spatial Anchors

Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

En el menú izquierdo, seleccione **Crear un recurso**.

Use el cuadro de búsqueda para buscar **Spatial Anchors**.

![Captura de pantalla que muestra los resultados de una búsqueda de Spatial Anchors.](./media/spatial-anchors-get-started-create-resource/portal-search.png)

Seleccione **Spatial Anchors** y, después, seleccione **Crear**.

En el panel **Cuenta de Spatial Anchors**, haga lo siguiente:

* Escriba un nombre de recurso único con caracteres alfanuméricos normales.  
* Seleccione la suscripción a la que desea asociar el recurso.  
* Cree un grupo de recursos, para lo que debe seleccionar **Crear nuevo**. Asígnele el nombre **myResourceGroup** y luego seleccione **Aceptar**.  

  [!INCLUDE [resource group intro text](resource-group.md)]
  
* Seleccione la ubicación (región) en la que desea colocar el recurso.  
* Seleccione **New** (Nuevo) para empezar a crear el recurso.

![Captura de pantalla del panel Spatial Anchors para crear un recurso.](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Una vez creado el recurso, Azure Portal muestra que se ha completado la implementación. 
   
![Captura de pantalla que muestra que la implementación del recurso se ha completado.](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

Haga clic en **Go to resource** (Ir al recurso). Ahora puede ver las propiedades del recurso. 
   
Copie el valor de **Id. de cuenta** del recurso en un editor de texto para usarlo más adelante.

![Captura de pantalla del panel de propiedades de recursos.](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Copie también el valor de **Account Domain** (Dominio de cuenta) del recurso en un editor de texto para usarlo más adelante.

![Captura de pantalla que muestra el valor del dominio de la cuenta del recurso.](./media/spatial-anchors-get-started-create-resource/view-resource-domain.png)

En **Configuración**, seleccione **Clave**. Copie el valor de **Clave principal**, **Clave de cuenta**, en un editor de texto para su uso posterior.

![Captura de pantalla del panel de claves de la cuenta.](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
