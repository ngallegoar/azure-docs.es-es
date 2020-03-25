---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: 32f4545a45eda8acddd7c93cc4917dbadca9ad4d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67135029"
---
## <a name="create-a-spatial-anchors-resource"></a>Creación de un recurso de Spatial Anchors

Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

En el panel de navegación izquierdo de Azure Portal, seleccione **Crear un recurso**.

Use el cuadro de búsqueda para buscar **Spatial Anchors**.

   ![Búsqueda de Spatial Anchors](./media/spatial-anchors-get-started-create-resource/portal-search.png)

Seleccione **Spatial Anchors**. En el cuadro de diálogo, seleccione **Crear**.

En el cuadro de diálogo **Cuenta de Spatial Anchors**:

- Escriba un nombre de recurso único con caracteres alfanuméricos normales.
- Seleccione la suscripción a la que desea asociar el recurso.
- Cree un grupo de recursos, para lo que debe seleccionar **Crear nuevo**. Asígnele el nombre **myResourceGroup** y seleccione **Aceptar**.
      [!INCLUDE [resource group intro text](resource-group.md)]
- Seleccione la ubicación (región) en la que desea colocar el recurso.
- Seleccione **New** (Nuevo) para empezar a crear el recurso.

   ![Creación de un recurso](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Una vez creado el recurso, Azure Portal mostrará que se completó la implementación. Haga clic en **Ir al recurso**.

![Implementación completada](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

A continuación, puede ver las propiedades del recurso. Copie el valor de **Id. de cuenta** en un editor de texto, ya que lo necesitará más adelante.

   ![Propiedades del recurso](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

En **Configuración**, seleccione **Clave**. Copie el valor de **Clave principal** en un editor de texto. Este valor es `Account Key`. Lo necesitará más adelante.

   ![Clave de cuenta](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
