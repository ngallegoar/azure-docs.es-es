---
title: 'Tutorial: Configuración de Azure Cloud Shell para Terraform'
description: En este tutorial, se usa Terraform con Azure Cloud Shell para simplificar la autenticación y configuración de plantillas.
keywords: azure devops terraform cloud shell
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 3a9db1143ba07b549a271d53d610e0a4853467c6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945332"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>Tutorial: Configuración de Azure Cloud Shell para Terraform

Terraform funciona bien desde una línea de comandos de Bash en macOS, Windows o Linux. La ejecución de las configuraciones de Terraform en la experiencia Bash de [Azure Cloud Shell](/azure/cloud-shell/overview) tiene algunas ventajas exclusivas. En este tutorial se muestra cómo escribir scripts de Terraform que se implementan en Azure mediante Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuración automática de credenciales

Terraform se instala y está disponible inmediatamente en Cloud Shell. Los scripts de Terraform se autentican en Azure cuando se ha iniciado sesión en Cloud Shell para administrar la infraestructura sin configuración adicional. La autenticación automática omite dos procesos manuales:
- Creación de una entidad de servicio de Azure Active Directory
- Configuración de las variables de proveedor de Azure Terraform


## <a name="use-modules-and-providers"></a>Uso de módulos y proveedores

Los módulos de Azure Terraform requieren credenciales para acceder a los recursos de Azure y modificarlos. Para usar los módulos de Terraform en Cloud Shell, agregue el código siguiente:


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you are using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
}
```

Cloud Shell pasa los valores necesarios al proveedor de `azurerm` a través de las variables de entorno cuando se usa cualquiera de los comandos de la CLI de `terraform`.

## <a name="other-cloud-shell-developer-tools"></a>Otras herramientas de desarrollo de Cloud Shell

Los archivos y los estados de shell se conservan en Azure Storage entre sesiones de Cloud Shell. Use el [Explorador de Azure Storage](/azure/vs-azure-tools-storage-manage-with-storage-explorer) para copiar y cargar archivos a Cloud Shell desde un equipo local.

La CLI de Azure está disponible en Cloud Shell y es una fantástica herramienta para probar las configuraciones y comprobar el trabajo después de que se completan `terraform apply` o `terraform destroy`.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de un pequeño clúster de VM mediante el registro de módulos](terraform-create-vm-cluster-module.md)