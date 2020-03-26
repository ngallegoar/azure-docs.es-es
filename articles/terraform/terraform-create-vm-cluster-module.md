---
title: 'Tutorial: Creación de un clúster de VM de Azure con Terraform mediante el registro de módulos'
description: En este tutorial, se usan módulos de Terraform para crear un clúster de máquinas virtuales Windows en Azure
keywords: azure devops terraform vm virtual machine cluster module registry
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 9faeee9bb2f0fb6dc148a3868f6fc0dae3833a2a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945290"
---
# <a name="tutorial-create-an-azure-vm-cluster-with-terraform-using-the-module-registry"></a>Tutorial: Creación de un clúster de VM de Azure con Terraform mediante el registro de módulos

En este artículo se describe el proceso para crear un clúster de VM con el [módulo de proceso de Azure](https://registry.terraform.io/modules/Azure/compute/azurerm/1.0.2) de Terraform. En este tutorial, aprenderá a: 

> [!div class="checklist"]
> * Configurar la autenticación con Azure
> * Crear la plantilla de Terraform
> * Visualizar los cambios con un plan
> * Aplicar la configuración para crear el clúster de VM

Para obtener más información sobre Terraform, consulte la [documentación de Terraform](https://www.terraform.io/docs/index.html).

## <a name="set-up-authentication-with-azure"></a>Configurar la autenticación con Azure

> [!TIP]
> Si [usa variables de entorno de Terraform](terraform-install-configure.md) o ejecuta este tutorial en [Azure Cloud Shell](/azure/cloud-shell/overview), omita este paso.

 Revise el artículo sobre cómo [Instalar Terraform y configurar el acceso a Azure](terraform-install-configure.md) para crear una entidad de servicio de Azure. Use esta entidad de servicio para completar un nuevo archivo `azureProviderAndCreds.tf` en un directorio vacío con el código siguiente:

```hcl
variable subscription_id {}
variable tenant_id {}
variable client_id {}
variable client_secret {}

provider "azurerm" {
    version = "~>1.40"

    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## <a name="create-the-template"></a>Creación de la plantilla

Cree una plantilla de Terraform nueva denominada `main.tf` con el código siguiente:

```hcl
module mycompute {
    source = "Azure/compute/azurerm"
    resource_group_name = "myResourceGroup"
    location = "East US 2"
    admin_password = "ComplxP@assw0rd!"
    vm_os_simple = "WindowsServer"
    is_windows_image = "true"
    remote_port = "3389"
    nb_instances = 2
    public_ip_dns = ["unique_dns_name"]
    vnet_subnet_id = module.network.vnet_subnets[0]
}

module "network" {
    source = "Azure/network/azurerm"
    location = "East US 2"
    resource_group_name = "myResourceGroup"
}

output "vm_public_name" {
    value = module.mycompute.public_ip_dns_name
}

output "vm_public_ip" {
    value = module.mycompute.public_ip_address
}

output "vm_private_ips" {
    value = module.mycompute.network_interface_private_ip
}
```

Ejecute `terraform init` en el directorio de configuración. Al usar la versión de Terraform 0.10.6 o posterior, se muestra el siguiente resultado:

![Terraform Init](media/terraformInitWithModules.png)

## <a name="visualize-the-changes-with-plan"></a>Visualizar los cambios con un plan

Ejecute `terraform plan` para obtener una vista previa de la infraestructura de máquinas virtuales que creó la plantilla.

![Plan de Terraform](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)


## <a name="create-the-virtual-machines-with-apply"></a>Crear las máquinas virtuales con apply

Ejecute `terraform apply` para aprovisionar las VM en Azure.

![Terraform Apply](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Examinar la lista de módulos Terraform de Azure](https://registry.terraform.io/modules/Azure)