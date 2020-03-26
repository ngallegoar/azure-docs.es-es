---
title: 'Inicio rápido: Instalación y configuración de Terraform para aprovisionar recursos de Azure'
description: En este inicio rápido, se instala y configura Terraform para crear recursos de Azure
keywords: azure devops terraform install configure
ms.topic: quickstart
ms.date: 03/09/2020
ms.openlocfilehash: 82635f59ec8165add2046a230a040b06f89d9898
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78943499"
---
# <a name="quickstart-install-and-configure-terraform-to-provision-azure-resources"></a>Inicio rápido: Instalación y configuración de Terraform para aprovisionar recursos de Azure
 
Terraform proporciona una manera fácil de definir, previsualizar e implementar infraestructura en la nube con un [lenguaje de plantillas simple](https://www.terraform.io/docs/configuration/syntax.html). En este artículo se describen los pasos necesarios para usar Terraform para aprovisionar recursos en Azure.

Para obtener más información sobre cómo usar Terraform con Azure, visite el [hub de Terraform](/azure/terraform).
> [!NOTE]
> Para obtener soporte técnico específico de Terraform, póngase en contacto directamente con Terraform mediante uno de sus canales de la comunidad:
>
>    * La [sección de Terraform](https://discuss.hashicorp.com/c/terraform-core) del portal de la comunidad contiene preguntas, casos de uso y patrones útiles.
>
>    * Para ver preguntas relacionadas con el proveedor, visite la sección de [proveedores de Terraform](https://discuss.hashicorp.com/c/terraform-providers) del portal de la comunidad.



[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Terraform se instala de forma predeterminada en [Cloud Shell](/azure/terraform/terraform-cloud-shell). Si decide instalar Terraform localmente, complete el paso siguiente; en caso contrario, continúe con la [configuración del acceso de Terraform a Azure](#set-up-terraform-access-to-azure).

## <a name="install-terraform"></a>Instalar Terraform

Para instalar Terraform, [descargue](https://www.terraform.io/downloads.html) el paquete apropiado para su sistema operativo en un directorio de instalación independiente. La descarga contiene un único archivo ejecutable, para el que también debe definir una ruta de acceso global. Para obtener instrucciones sobre cómo establecer la ruta de acceso en Linux y Mac, vaya a [esta página web](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Para obtener instrucciones sobre cómo establecer la ruta de acceso en Windows, vaya a [esta página web](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).

Compruebe la configuración de ruta de acceso con el comando `terraform`. Se muestra una lista de las opciones disponibles de Terraform, como en la siguiente salida de ejemplo:

```console
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Configurar el acceso de Terraform a Azure

Para habilitar Terraform para aprovisionar recursos en Azure, cree una [entidad de servicio de Azure AD](/cli/azure/create-an-azure-service-principal-azure-cli). La entidad de servicio concede sus scripts de Terraform para aprovisionar recursos en su suscripción de Azure.

Si tiene varias suscripciones a Azure, primero consulte su cuenta con [az account list](/cli/azure/account#az-account-list) para obtener una lista de valores de identificadores de suscripción e identificadores de inquilino:

```azurecli-interactive
az account list --query "[].{name:name, subscriptionId:id, tenantId:tenantId}"
```

Para utilizar una suscripción seleccionada, defina la suscripción para esta sesión con [az account set](/cli/azure/account#az-account-set). Establezca la variable de entorno `SUBSCRIPTION_ID` para almacenar el valor devuelto del campo `id` de la suscripción que quiere usar:

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Ahora puede crear una entidad de servicio para su uso con Terraform. Utilice[az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) y defina el *ámbito* de la suscripción como se indica a continuación:

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Se devuelven su `appId`, `password`, `sp_name` y `tenant`. Anote los valores de `appId` y de `password`.

## <a name="configure-terraform-environment-variables"></a>Configuración de las variables de entorno de Terraform

Para configurar Terraform para que use la entidad de servicio de Azure AD, establezca las siguientes variables de entorno, que se usan por los [módulos de Terraform de Azure](https://registry.terraform.io/modules/Azure). También puede establecer el entorno si trabaja con una nube de Azure distinta de la pública.

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

Puede usar el siguiente script de shell de ejemplo para establecer esas variables:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>Ejecución de un script de ejemplo

Cree un archivo `test.tf` en un directorio vacío y péguelo en el siguiente script.

```hcl
provider "azurerm" {
  # The "feature" block is required for AzureRM provider 2.x. 
  # If you are using version 1.x, the "features" block is not allowed.
  version = "~>2.0"
  features {}
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Guarde el archivo y, después, inicialice la implementación de Terraform. Este paso descarga los módulos de Azure necesarios para crear un grupo de recursos de Azure.

```bash
terraform init
```

La salida es similar a la del ejemplo siguiente:

```console
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Puede obtener una vista previa de las acciones que van a completar el script de Terraform con `terraform plan`. Cuando esté listo para crear el grupo de recursos, aplique el plan de Terraform de la forma siguiente:

```bash
terraform apply
```

La salida es similar a la del ejemplo siguiente:

```console
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha instalado Terraform o ha utilizado Cloud Shell para configurar las credenciales de Azure y comenzar a crear recursos en su suscripción a Azure. Para crear una implementación más completa de Terraform en Azure, consulte el artículo siguiente:

> [!div class="nextstepaction"]
> [Crear una máquina virtual de Azure con Terraform](terraform-create-complete-vm.md)