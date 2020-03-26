---
title: 'Tutorial: Aprovisionamiento de una infraestructura con espacios de implementación de Azure mediante Terraform'
description: En este tutorial, se usa Terraform con ranuras de implementación del proveedor de Azure
keywords: azure devops terraform deployment slots
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: ddd4d84ee8bf4ab1e90dd68da185cdd9075fe1e0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78943484"
---
# <a name="tutorial-provision-infrastructure-with-azure-deployment-slots-using-terraform"></a>Tutorial: Aprovisionamiento de infraestructuras con espacios de implementación de Azure mediante Terraform

Puede usar [espacios de implementación de Azure](/azure/app-service/deploy-staging-slots) para cambiar de una versión de la aplicación a otra. Esa capacidad ayuda a minimizar el impacto de las implementaciones rotas. 

Este artículo muestra un ejemplo del uso de las ranuras de implementación, en el que se le guía a través de la implementación de dos aplicaciones mediante GitHub y Azure. Una aplicación se hospeda en un espacio de producción. La segunda aplicación se hospeda en un espacio de ensayo. Los nombres "producción" y "ensayo" son arbitrarios; pueden adaptarse a lo que resulte adecuado para su escenario. Una vez configurados los espacios de implementación, usará Terraform para cambiar de uno a otro según sea necesario.

## <a name="prerequisites"></a>Prerrequisitos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Cuenta de GitHub**: se necesita una cuenta de [GitHub](https://www.github.com) para bifurcar y usar el repositorio de GitHub de prueba.

## <a name="create-and-apply-the-terraform-plan"></a>Creación y aplicar el plan de Terraform

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `deploy`.

    ```bash
    mkdir deploy
    ```

1. Cree un directorio llamado `swap`.

    ```bash
    mkdir swap
    ```

1. Use el comando de bash `ls` para verificar que ha creado ambos directorios correctamente.

    ![Cloud Shell después de crear directorios](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Cambie al directorio `deploy`.

    ```bash
    cd deploy
    ```

1. En Cloud Shell, cree un archivo denominado `deploy.tf`.

    ```bash
    code deploy.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    # Configure the Azure provider
    provider "azurerm" { 
        # The "feature" block is required for AzureRM provider 2.x. 
        # If you are using version 1.x, the "features" block is not allowed.
        version = "~>2.0"
        features {}
    }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = azurerm_resource_group.slotDemo.location
        resource_group_name = azurerm_resource_group.slotDemo.name
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = azurerm_resource_group.slotDemo.location
        resource_group_name = azurerm_resource_group.slotDemo.name
        app_service_plan_id = azurerm_app_service_plan.slotDemo.id
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = azurerm_resource_group.slotDemo.location
        resource_group_name = azurerm_resource_group.slotDemo.name
        app_service_plan_id = azurerm_app_service_plan.slotDemo.id
        app_service_name    = azurerm_app_service.slotDemo.name
    }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

1. Ahora que ha creado el archivo, verifique el contenido.

    ```bash
    cat deploy.tf
    ```

1. Inicialice Terraform.

    ```bash
    terraform init
    ```

1. Cree el plan de Terraform.

    ```bash
    terraform plan
    ```

1. Aprovisione los recursos que se definen en el archivo de configuración `deploy.tf`. (para confirmar la acción escriba `yes` en el aviso).

    ```bash
    terraform apply
    ```

1. Cierre la ventana de Cloud Shell.

1. En el menú principal de Azure Portal, seleccione **Grupos de recursos**.

    ![Selección de "Grupos de recursos" en el portal](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. En la pestaña **Grupos de recursos**, seleccione **slotDemoResourceGroup**.

    ![Grupo de recursos creado por Terraform](./media/terraform-slot-walkthru/resource-group.png)

Ahora verá todos los recursos que ha creado Terraform.

![Recursos creados por Terraform](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Bifurcación del proyecto de prueba

Para poder probar la creación y cambiar de un espacio de implementación a otro, es preciso necesita bifurcar el proyecto de prueba de GitHub.

1. Vaya al [repositorio awesome-terraform on GitHub](https://github.com/Azure/awesome-terraform).

1. Bifurque el repositorio **awesome-terraform**.

    ![Bifurcar el repositorio awesome-terraform de GitHub](./media/terraform-slot-walkthru/fork-repo.png)

1. Siga las indicaciones para bifurcar a su entorno.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Implementación desde GitHub a sus ranuras de implementación

Una vez bifurcado el repositorio del proyecto de prueba, siga estos pasos para configurar los espacios de implementación:

1. En el menú principal de Azure Portal, seleccione **Grupos de recursos**.

1. Seleccione **slotDemoResourceGroup**.

1. Seleccione **slotAppService**.

1. Seleccione **Opciones de implementación**.

    ![Opciones de implementación de un recurso de App Service](./media/terraform-slot-walkthru/deployment-options.png)

1. En la pestaña **Opción de implementación**, seleccione **Elegir origen**y **GitHub**.

    ![Seleccionar origen de la implementación](./media/terraform-slot-walkthru/select-source.png)

1. Una vez que Azure crea la conexión y muestra todas las opciones, seleccione **Autorización**.

1. En la pestaña **Autorización**, seleccione **Autorizar** y especifique las credenciales que Azure necesita para acceder a la cuenta de GitHub. 

1. Una vez que Azure valida las credenciales de GitHub, se muestra un mensaje que indica que se ha completado el proceso de autorización. Seleccione **Aceptar** para cerrar la pestaña **Autorización**.

1. Seleccione **Choose your organization** (Elegir su organización) y seleccione su organización.

1. Seleccione **Elegir proyecto**.

1. En la pestaña **Elegir proyecto**, seleccione el proyecto **awesome-terraform**.

    ![Elegir el proyecto awesome-terraform](./media/terraform-slot-walkthru/choose-project.png)

1. Seleccione **Elegir rama**.

1. En la pestaña **Elegir rama**, seleccione **maestra**.

    ![Elegir la rama maestra](./media/terraform-slot-walkthru/choose-branch-master.png)

1. En la pestaña **Opción de implementación**, haga clic en **Aceptar**.

En este punto, ya ha implementado el espacio de producción. Para implementar el espacio de ensayo, lleve a cabo los pasos anteriores con las siguientes modificaciones:

- En el paso 3, seleccione el recurso **slotAppServiceSlotOne**.

- En el paso 13, seleccione la rama de trabajo en lugar de la maestra.

    ![Elección de rama de trabajo](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Prueba de las implementaciones de aplicaciones

En las secciones anteriores, se configuraron dos espacios (**slotAppService** y **slotAppServiceSlotOne**), con el fin de poder realizar implementaciones desde distintas bifurcaciones de GitHub. Vamos a realizar una versión preliminar de las aplicaciones web para validar que se han implementado correctamente.

1. En el menú principal de Azure Portal, seleccione **Grupos de recursos**.

1. Seleccione **slotDemoResourceGroup**.

1. Seleccione **slotAppService** o **slotAppServiceSlotOne**.

1. En la página de información general, seleccione **URL**.

    ![Seleccione la dirección URL en la pestaña de información general para representar la aplicación](./media/terraform-slot-walkthru/resource-url.png)

1. En función de la aplicación seleccionada, verá los siguientes resultados:
    - Aplicación web **slotAppService**: página azul con el título **Slot Demo App 1**. 
    - Aplicación web **slotAppServiceSlotOne**: página verde con el título **Slot Demo App 2**.

    ![Obtener una versión preliminar de las aplicaciones para probar si se han implementado correctamente](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>Intercambio de las dos ranuras de implementación

Para probar el cambio entre los dos espacios de implementación, siga estos pasos:
 
1. Cambie a la pestaña del explorador en la que se ejecuta **slotAppService** (la aplicación con la página azul). 

1. Vuelva a Azure Portal en otra pestaña.

1. Abra Cloud Shell.

1. Cambie al directorio **clouddrive/swap**.

    ```bash
    cd clouddrive/swap
    ```

1. En Cloud Shell, cree un archivo denominado `swap.tf`.

    ```bash
    code swap.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    # Configure the Azure provider
    provider "azurerm" { 
        # The "feature" block is required for AzureRM provider 2.x. 
        # If you are using version 1.x, the "features" block is not allowed.
        version = "~>2.0"
        features {}
    }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

1. Inicialice Terraform.

    ```bash
    terraform init
    ```

1. Cree el plan de Terraform.

    ```bash
    terraform plan
    ```

1. Aprovisione los recursos que se definen en el archivo de configuración `swap.tf`. (para confirmar la acción escriba `yes` en el aviso).

    ```bash
    terraform apply
    ```

1. Después de que Terraform haya cambiado los espacios, vuelva al explorador. Actualice la página. 

La aplicación web del espacio de ensayo **slotAppServiceSlotOne** se ha intercambiado por el espacio de producción y ahora se representa en verde. 

![Las ranuras de implementación se han intercambiado](./media/terraform-slot-walkthru/slots-swapped.png)

Para volver a la versión de producción original de la aplicación, vuelva a aplicar el plan de Terraform que creó a partir del archivo de configuración `swap.tf`.

```bash
terraform apply
```

Tras el intercambio de la aplicación, se ve la configuración original.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Más información sobre el uso de Terraform en Azure](/azure/terraform)