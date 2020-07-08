---
title: Creación de un laboratorio en Azure DevTest Labs | Microsoft Docs
description: Este artículo le guiará por el proceso de creación de un laboratorio mediante Azure Portal y Azure DevTest Labs.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 09999c5b0187f924f9cfbbc2afad8954adee0fd7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85481262"
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Creación de un laboratorio con Laboratorios de desarrollo y pruebas de Azure
En Azure Dev/Test Labs, un laboratorio es la infraestructura que abarca un grupo de recursos, como máquinas virtuales (VM), que permite una mejor administración de dichos recursos mediante la especificación de límites y cuotas. Este artículo le guiará a través del proceso de creación de un laboratorio mediante Azure Portal.

## <a name="prerequisites"></a>Prerequisites
Para crear un laboratorio necesitará:

* Suscripción a Azure. Para información sobre las opciones de compra de Azure, consulte [Instrucciones para contratar Azure](https://azure.microsoft.com/pricing/purchase-options/) o [Evaluación gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/). Debe ser el propietario de la suscripción para crear el laboratorio.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Pasos para crear un laboratorio con Azure DevTest Labs
Los pasos siguientes muestran cómo usar Azure Portal para crear un laboratorio en Azure DevTest Labs. 

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. En el menú principal de la izquierda, seleccione **Todos los servicios** (en la parte superior de la lista). Seleccione * (asterisco) junto a **DevTest Labs** en la sección **DevOps**. Esta acción agrega **DevTest Labs** al menú de navegación izquierdo para facilitarle el acceso la próxima vez. 

    ![Todos los servicios. Selección de DevTest Labs](./media/devtest-lab-create-lab/all-services-select.png)
2. A continuación, seleccione **DevTest Labs** en el menú de navegación izquierdo. En la barra de herramientas, seleccione **Agregar**. 
   
    ![Incorporación de un laboratorio](./media/devtest-lab-create-lab/add-lab-button.png)
1. En la página **Crear un laboratorio de DevTest Labs**, lleve a cabo las siguientes acciones: 
    1. Escriba un **nombre** para el laboratorio.
    2. Seleccione una **suscripción** para asociar al laboratorio.
    3. Escriba un **nombre para el grupo de recursos** para el laboratorio. 
    4. Seleccione una **ubicación** en la que se vaya a almacenar el laboratorio.
    4. Seleccione **Apagado automático** para especificar si desea habilitar y definir los parámetros para el cierre automático de todas las máquinas virtuales del laboratorio. La característica de apagado automático es principalmente una característica de ahorro de costos por la que puede especificar cuándo desea que la máquina virtual se apague automáticamente. Para cambiar la configuración del apagado automático después de crear el laboratorio, siga los pasos que se describen en el artículo [Administración de todas las directivas para un laboratorio de Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).
    1. Escriba la información de **NOMBRE** y **VALOR** en las **Etiquetas**, si desea crear un etiquetado personalizado que se agregue a cada recurso que vaya a crear en el laboratorio. Las etiquetas son útiles para ayudarle a administrar y organizar los recursos del laboratorio por categoría. Para más información acerca de las etiquetas, incluido cómo agregar etiquetas después de crear el laboratorio, consulte [Incorporación de etiquetas a un laboratorio](devtest-lab-add-tag.md).
    6. Seleccione **Opciones de automatización** para obtener plantillas de Azure Resource Manager para la automatización de la configuración. 
    7. Seleccione **Crear**. Para supervisar el estado del proceso de creación de un laboratorio, inspeccione el área **Notificaciones**. 
    
        ![Creación de una sección de laboratorio de DevTest Labs](./media/devtest-lab-create-lab/create-devtestlab-blade.png)
    8. Una vez completado, seleccione **Go to resource** (Ir al recurso) en la notificación. Como alternativa puede actualizar la página **DevTest Labs** para ver el laboratorio recién creado en la lista de laboratorios.  En la lista, seleccione el laboratorio. Verá la página principal del laboratorio. 

        ![Página principal del laboratorio](./media/devtest-lab-create-lab/lab-home-page.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Pasos siguientes
Una vez creado el laboratorio, le presentamos algunos pasos que se deben tener en cuenta:

* [Acceso seguro a un laboratorio](devtest-lab-add-devtest-user.md)
* [Definición de directivas de laboratorio](devtest-lab-set-lab-policy.md)
* [Creación de una plantilla de laboratorio](devtest-lab-create-template.md)
* [Creación de artefactos personalizados para máquinas virtuales](devtest-lab-artifact-author.md)
* [Adición de una máquina virtual a un laboratorio](devtest-lab-add-vm.md)

