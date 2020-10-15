---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: aac2f3ea2b52ac0319f96279deed13c1145749bd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74451412"
---
## <a name="deploy-template-from-cloud-shell"></a>Implementación de una plantilla desde Cloud Shell

Puede usar [Cloud Shell](../articles/cloud-shell/overview.md) para implementar la plantilla. Para implementar una plantilla externa, proporcione el identificador URI de la plantilla exactamente como lo haría para cualquier implementación externa. Para implementar una plantilla local, primero debe cargar la plantilla en la cuenta de almacenamiento de Cloud Shell. En esta sección se describe cómo cargar la plantilla a su cuenta de Cloud Shell e implementarla como un archivo local. Si no ha usado Cloud Shell, consulte la [Introducción a Azure Cloud Shell](../articles/cloud-shell/overview.md) para más información sobre su configuración.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione el grupo de recursos de Cloud Shell. El patrón de nombre es `cloud-shell-storage-<region>`.

   ![Selección de un grupo de recursos](./media/resource-manager-cloud-shell-deploy/select-cloud-shell-resource-group.png)

1. Seleccione la cuenta de almacenamiento de Cloud Shell.

   ![Selección de la cuenta de almacenamiento](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Seleccione **Blobs**.

   ![Selección de blobs](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. Seleccione **+ Contenedor**.

   ![Agregar contenedor](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Asigne un nombre y un nivel de acceso al contenedor. El la plantilla de ejemplo de este artículo no se incluye información confidencial, por lo que puede permitir el acceso de lectura anónimo. Seleccione **Aceptar**.

   ![Proporcionar valores de contenedor](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Seleccione el contenedor creado.

   ![Selección del nuevo contenedor](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. Seleccione **Cargar**.

   ![Carga de blob](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Busque y cargue la plantilla.

   ![Carga de archivo](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Una vez ha cargado, seleccione la plantilla.

   ![Selección de la nueva plantilla](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. Copie la dirección URL.

   ![Copiar la dirección URL](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. Abra el símbolo del sistema.

   ![Apertura de Cloud Shell](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
