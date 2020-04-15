---
title: 'Tutorial: Creación de un clúster de Kubernetes con Azure Kubernetes Service (AKS) mediante Terraform'
description: En este tutorial se crea un clúster de Kubernetes con Azure Kubernetes Service y Terraform
keywords: azure devops terraform aks kubernetes
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: b7a84d7562e99e53ff7be75b7d40795cd3f9e203
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618938"
---
# <a name="tutorial-create-a-kubernetes-cluster-with-azure-kubernetes-service-using-terraform"></a>Tutorial: Creación de un clúster de Kubernetes con Azure Kubernetes Service mediante Terraform

[Azure Kubernetes Service (AKS)](/azure/aks/) administra el entorno hospedado de Kubernetes. Azure Kubernetes Service permite implementar y administrar aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. También permite realizar muchas operaciones de mantenimiento comunes sin desconectar la aplicación. Entre estas operaciones se incluyen el aprovisionamiento, la actualización y el escalado de recursos a petición.

En este tutorial, aprenderá las siguientes tareas:

> [!div class="checklist"]
> * Uso de HCL (HashiCorp Language) para definir un clúster de Kubernetes
> * Uso de Terraform y AKS para crear un clúster de Kubernetes
> * Utilice la herramienta kubectl para probar la disponibilidad de un clúster de Kubernetes

## <a name="prerequisites"></a>Prerrequisitos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Configuración de Terraform**: siga las instrucciones del artículo [Instalación y configuración de Terraform para aprovisionar máquinas virtuales y otras infraestructuras en Azure](terraform-install-configure.md).

- **Entidad de servicio de Azure**: siga las instrucciones de **Creación de la entidad de servicio**, en el artículo [Creación de una entidad de servicio de Azure con la CLI de Azure](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Tome nota de los valores de appId, displayName, contraseña e inquilino.

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios

El primer paso es crear el directorio que contenga los archivos de configuración de Terraform para el ejercicio.

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>Declaración del proveedor de Azure

Cree el archivo de configuración de Terraform que declara el proveedor de Azure.

1. En Cloud Shell, cree un archivo denominado `main.tf`.

    ```bash
    code main.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    provider "azurerm" {
        # The "feature" block is required for AzureRM provider 2.x. 
        # If you are using version 1.x, the "features" block is not allowed.
        version = "~>2.0"
        features {}
    }

    terraform {
        backend "azurerm" {}
    }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

## <a name="define-a-kubernetes-cluster"></a>Definición de un clúster de Kubernetes

Cree el archivo de configuración de Terraform que declara los recursos para el clúster de Kubernetes.

1. En Cloud Shell, cree un archivo denominado `k8s.tf`.

    ```bash
    code k8s.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    resource "azurerm_resource_group" "k8s" {
        name     = var.resource_group_name
        location = var.location
    }
    
    resource "random_id" "log_analytics_workspace_name_suffix" {
        byte_length = 8
    }

    resource "azurerm_log_analytics_workspace" "test" {
        # The WorkSpace name has to be unique across the whole of azure, not just the current subscription/tenant.
        name                = "${var.log_analytics_workspace_name}-${random_id.log_analytics_workspace_name_suffix.dec}"
        location            = var.log_analytics_workspace_location
        resource_group_name = azurerm_resource_group.k8s.name
        sku                 = var.log_analytics_workspace_sku
    }

    resource "azurerm_log_analytics_solution" "test" {
        solution_name         = "ContainerInsights"
        location              = azurerm_log_analytics_workspace.test.location
        resource_group_name   = azurerm_resource_group.k8s.name
        workspace_resource_id = azurerm_log_analytics_workspace.test.id
        workspace_name        = azurerm_log_analytics_workspace.test.name

        plan {
            publisher = "Microsoft"
            product   = "OMSGallery/ContainerInsights"
        }
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = var.cluster_name
        location            = azurerm_resource_group.k8s.location
        resource_group_name = azurerm_resource_group.k8s.name
        dns_prefix          = var.dns_prefix

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
                key_data = file(var.ssh_public_key)
            }
        }

        default_node_pool {
            name            = "agentpool"
            node_count      = var.agent_count
            vm_size         = "Standard_DS1_v2"
        }

        service_principal {
            client_id     = var.client_id
            client_secret = var.client_secret
        }

        addon_profile {
            oms_agent {
            enabled                    = true
            log_analytics_workspace_id = azurerm_log_analytics_workspace.test.id
            }
        }

        tags = {
            Environment = "Development"
        }
    }
    ```

    El código anterior establece el nombre del clúster, la ubicación y el nombre del grupo de recursos. También se establece el prefijo del nombre de dominio completo (FQDN). El nombre de dominio completo se usa para acceder al clúster.

    El registro `linux_profile` permite configurar las opciones que permiten iniciar sesión en los nodos de trabajo mediante SSH.

    Con AKS, solo se paga por los nodos de trabajo. El registro `default_node_pool` configura los detalles de estos nodos de trabajo. El registro `default_node_pool record` incluye el número de nodos de trabajo que se van a crear y el tipo de estos. Si necesita escalar o reducir verticalmente el clúster en el futuro, modifique el valor de `count` en este registro.

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

## <a name="declare-the-variables"></a>Declaración de las variables

1. En Cloud Shell, cree un archivo denominado `variables.tf`.

    ```bash
    code variables.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }

    variable log_analytics_workspace_name {
        default = "testLogAnalyticsWorkspaceName"
    }

    # refer https://azure.microsoft.com/global-infrastructure/services/?products=monitor for log analytics available regions
    variable log_analytics_workspace_location {
        default = "eastus"
    }

   # refer https://azure.microsoft.com/pricing/details/monitor/ for log analytics pricing 
   variable log_analytics_workspace_sku {
        default = "PerGB2018"
   }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

## <a name="create-a-terraform-output-file"></a>Creación de un archivo de salida de Terraform

[Las salidas de Terraform](https://www.terraform.io/docs/configuration/outputs.html) permiten definir valores que se resaltarán al usuario cuando Terraform aplique un plan, y pueden consultarse con el comando `terraform output`. En esta sección, va a crear un archivo de salida que permita el acceso al clúster con [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

1. En Cloud Shell, cree un archivo denominado `output.tf`.

    ```bash
    code output.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    output "client_key" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_key
    }

    output "client_certificate" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate
    }

    output "cluster_ca_certificate" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate
    }

    output "cluster_username" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.username
    }

    output "cluster_password" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.password
    }

    output "kube_config" {
        value = azurerm_kubernetes_cluster.k8s.kube_config_raw
    }

    output "host" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.host
    }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Configuración del almacenamiento de Azure para almacenar el estado de Terraform

Terraform realiza un seguimiento del estado de manera local a través del archivo `terraform.tfstate`. Este patrón funciona bien en un entorno de persona único. En un entorno de varias personas, [Azure Storage](/azure/storage/) se usa para realizar un seguimiento del estado.

En esta sección, verá cómo se realizan las siguientes tareas:
- Recuperar la información de la cuenta de almacenamiento (nombre y clave de la cuenta).
- Cree un contenedor de almacenamiento en el que se almacenará la información de estado de Terraform.

1. En Azure Portal, seleccione **Todos los servicios** en el menú de la izquierda.

1. Seleccione **Cuentas de almacenamiento**.

1. En la pestaña **Cuentas de almacenamiento**, seleccione el nombre de la cuenta de almacenamiento en la que Terraform va a almacenar el estado. Por ejemplo, puede utilizar la cuenta de almacenamiento creada cuando abrió Cloud Shell por primera vez.  El nombre de cuenta de almacenamiento creada por Cloud Shell normalmente comienza por `cs` seguido de una cadena aleatoria de números y letras. Anote la cuenta de almacenamiento que seleccione. Este valor lo necesitará más adelante.

1. En la pestaña Cuenta de almacenamiento, seleccione **Claves de acceso**.

    ![Menú de la cuenta de almacenamiento](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. Anote el valor de la **clave1** **clave**. (Al seleccionar el icono a la derecha de la clave, se copia el valor al portapapeles).

    ![Claves de acceso de cuenta de almacenamiento](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. En Cloud Shell, cree un contenedor en su cuenta de Azure Storage. Reemplace los marcadores de posición por valores adecuados para su entorno.

    ```azurecli
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Crear el clúster de Kubernetes

En esta sección, puede ver cómo usar el comando `terraform init` para crear los recursos definidos en los archivos de configuración que creó en las secciones anteriores.

1. En Cloud Shell, inicialice Terraform. Reemplace los marcadores de posición por valores adecuados para su entorno.

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    El comando `terraform init` muestra el éxito de inicializar el back-end y el complemento del proveedor:

    ![Ejemplo de resultados de "terraform init"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. Exporte las credenciales de la entidad de servicio. Reemplace los marcadores de posición por valores adecuados de su entidad de servicio.

    ```bash
    export TF_VAR_client_id=<service-principal-appid>
    export TF_VAR_client_secret=<service-principal-password>
    ```

1. Ejecute el comando `terraform plan` para crear el plan de Terraform que define los elementos de infraestructura. 

    ```bash
    terraform plan -out out.plan
    ```

    El comando `terraform plan` muestra los recursos que se crearán cuando ejecute el comando `terraform apply`:

    ![Ejemplo de resultados de "terraform plan"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Ejecute el comando `terraform apply` para aplicar el plan para crear el clúster de Kubernetes. El proceso para crear un clúster de Kubernetes puede tardar varios minutos, dado como resultado el agotamiento del tiempo de espera de la sesión de Cloud Shell. Si la sesión de Cloud Shell agota su tiempo de espera, puede seguir los pasos de la sección "Recuperación de un tiempo de espera de Cloud Shell" para poder completar el tutorial.

    ```bash
    terraform apply out.plan
    ```

    El comando `terraform apply` muestra los resultados de la creación de los recursos definidos en sus archivos de configuración:

    ![Ejemplo de resultados de "terraform apply"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. En Azure Portal, seleccione **Todos los recursos** en el menú de la izquierda para ver los recursos que se han creado para su nuevo clúster de Kubernetes.

    ![Todos los recursos en Azure Portal](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Recuperación de un tiempo de espera de Cloud Shell

Si se agota el tiempo de espera de Cloud Shell, puede seguir estos pasos para recuperarla:

1. Inicie una sesión de Cloud Shell.

1. Cambie al directorio que contiene los archivos de configuración de Terraform.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Ejecute el siguiente comando:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Prueba del clúster de Kubernetes

Las herramientas de Kubernetes se pueden usar para comprobar el clúster recién creado.

1. Obtenga la configuración de Kubernetes desde el estado de Terraform y almacénela en un archivo que kubectl puede leer.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. Establezca una variable de entorno para que kubectl seleccione la configuración correcta.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Compruebe el mantenimiento del clúster.

    ```bash
    kubectl get nodes
    ```

    Debería ver los detalles de sus nodos de trabajo, y todos deberían tener el estado **Listo**, como se muestra en la siguiente imagen:

    ![La herramienta kubectl le permite comprobar el mantenimiento de su clúster de Kubernetes](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="monitor-health-and-logs"></a>Supervisión de estado y registros

Cuando se creó el clúster de AKS, se habilitó la supervisión para capturar métricas de mantenimiento para los nodos de clúster y los pods. Estas métricas de mantenimiento están disponibles en Azure Portal. Para obtener más información sobre la supervisión del estado de los contenedores, consulte [Monitor Azure Kubernetes Service health](/azure/azure-monitor/insights/container-insights-overview) (Supervisión del estado de Azure Kubernetes Service).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Más información sobre el uso de Terraform en Azure](/azure/terraform)