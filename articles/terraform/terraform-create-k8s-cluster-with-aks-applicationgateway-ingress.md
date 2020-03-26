---
title: 'Tutorial: Creación de un controlador de entrada de Application Gateway en Azure Kubernetes Service'
description: En este tutorial se crea un clúster de Kubernetes con Azure Kubernetes Service con Application Gateway como controlador de entrada
keywords: azure devops terraform application gateway ingress aks kubernetes
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 6b48d0acb654f0b0643c0754e53f6bc6ea76bb45
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945316"
---
# <a name="tutorial-create-an-application-gateway-ingress-controller-in-azure-kubernetes-service"></a>Tutorial: Creación de un controlador de entrada de Application Gateway en Azure Kubernetes Service

[Azure Kubernetes Service (AKS)](/azure/aks/) administra el entorno hospedado de Kubernetes. AKS facilita y acelera la implementación y administración de aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. AKS también elimina la carga de dejar las aplicaciones sin conexión para las tareas funcionales y de mantenimiento. Con AKS, estas tareas, incluidos el aprovisionamiento, la actualización y el escalado de recursos, se pueden realizar a petición.

Un controlador de entrada proporciona varias características para los servicios de Kubernetes. Estas características incluyen el proxy inverso, el enrutamiento de tráfico configurable y la terminación de TLS. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, una sola dirección IP puede enrutar el tráfico a varios servicios en un clúster de Kubernetes. Azure [Application Gateway](/azure/Application-Gateway/) proporciona todas las funcionalidades anteriores, lo que convierte este servicio en un controlador de entrada perfecto para Kubernetes en Azure. 

En este tutorial, aprenderá las siguientes tareas:

> [!div class="checklist"]
> * Cree un clúster de [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) con AKS y Application Gateway como controlador de entrada.
> * Use HCL (lenguaje de HashiCorp) para definir un clúster de Kubernetes.
> * Use Terraform para crear un recurso de Application Gateway.
> * Use Terraform y AKS para crear un clúster de Kubernetes.
> * Utilice la herramienta kubectl para probar la disponibilidad de un clúster de Kubernetes.

## <a name="prerequisites"></a>Prerrequisitos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Configuración de Terraform**: siga las instrucciones del artículo [Instalación y configuración de Terraform para aprovisionar máquinas virtuales y otras infraestructuras en Azure](terraform-install-configure.md).

- **Grupo de recursos de Azure**: Si no tiene ningún grupo de recursos de Azure que pueda usar para la demostración, [créelo](/azure/azure-resource-manager/manage-resource-groups-portal#create-resource-groups). Tome nota del nombre y la ubicación del grupo de recursos, ya que estos valores se usan en la demostración.

- **Entidad de servicio de Azure**: siga las instrucciones de la sección **Creación de la entidad de servicio** del artículo [Creación de una entidad de servicio de Azure con la CLI de Azure](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Anote los valores de appId, displayName y contraseña.

- **Obtenga el identificador del objeto de la entidad de servicio**: En Cloud Shell, ejecute el comando siguiente: `az ad sp list --display-name <displayName>`

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios

El primer paso es crear el directorio que contenga los archivos de configuración de Terraform para el ejercicio.

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview).

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `terraform-aks-appgw-ingress`.

    ```bash
    mkdir terraform-aks-appgw-ingress
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd terraform-aks-appgw-ingress
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

## <a name="define-input-variables"></a>Definición de las variables de entrada

Cree el archivo de configuración de Terraform que muestre todas las variables necesarias para esta implementación.

1. En Cloud Shell, cree un archivo denominado `variables.tf`.

    ```bash
    code variables.tf
    ```

1. Pegue el siguiente código en el editor:
    
    ```hcl
    variable "resource_group_name" {
      description = "Name of the resource group."
    }

    variable "location" {
      description = "Location of the cluster."
    }

    variable "aks_service_principal_app_id" {
      description = "Application ID/Client ID  of the service principal. Used by AKS to manage AKS related resources on Azure like vms, subnets."
    }

    variable "aks_service_principal_client_secret" {
      description = "Secret of the service principal. Used by AKS to manage Azure."
    }

    variable "aks_service_principal_object_id" {
      description = "Object ID of the service principal."
    }

    variable "virtual_network_name" {
      description = "Virtual network name"
      default     = "aksVirtualNetwork"
    }

    variable "virtual_network_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/8"
    }

    variable "aks_subnet_name" {
      description = "AKS Subnet Name."
      default     = "kubesubnet"
    }

    variable "aks_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/16"
    }

    variable "app_gateway_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.1.0.0/16"
    }

    variable "app_gateway_name" {
      description = "Name of the Application Gateway."
      default = "ApplicationGateway1"
    }

    variable "app_gateway_sku" {
      description = "Name of the Application Gateway SKU."
      default = "Standard_v2"
    }

    variable "app_gateway_tier" {
      description = "Tier of the Application Gateway SKU."
      default = "Standard_v2"
    }

    variable "aks_name" {
      description = "Name of the AKS cluster."
      default     = "aks-cluster1"
    }
    variable "aks_dns_prefix" {
      description = "Optional DNS prefix to use with hosted Kubernetes API server FQDN."
      default     = "aks"
    }

    variable "aks_agent_os_disk_size" {
      description = "Disk size (in GB) to provision for each of the agent pool nodes. This value ranges from 0 to 1023. Specifying 0 applies the default disk size for that agentVMSize."
      default     = 40
    }

    variable "aks_agent_count" {
      description = "The number of agent nodes for the cluster."
      default     = 3
    }

    variable "aks_agent_vm_size" {
      description = "The size of the Virtual Machine."
      default     = "Standard_D3_v2"
    }

    variable "kubernetes_version" {
      description = "The version of Kubernetes."
      default     = "1.11.5"
    }

    variable "aks_service_cidr" {
      description = "A CIDR notation IP range from which to assign service cluster IPs."
      default     = "10.0.0.0/16"
    }

    variable "aks_dns_service_ip" {
      description = "Containers DNS server IP address."
      default     = "10.0.0.10"
    }

    variable "aks_docker_bridge_cidr" {
      description = "A CIDR notation IP for Docker bridge."
      default     = "172.17.0.1/16"
    }

    variable "aks_enable_rbac" {
      description = "Enable RBAC on the AKS cluster. Defaults to false."
      default     = "false"
    }

    variable "vm_user_name" {
      description = "User name for the VM"
      default     = "vmuser1"
    }

    variable "public_ssh_key_path" {
      description = "Public key path for SSH."
      default     = "~/.ssh/id_rsa.pub"
    }

    variable "tags" {
      type = "map"

      default = {
        source = "terraform"
      }
    }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

## <a name="define-the-resources"></a>Definición de los recursos 
Cree el archivo de configuración de Terraform que crea todos los recursos. 

1. En Cloud Shell, cree un archivo denominado `resources.tf`.

    ```bash
    code resources.tf
    ```

1. Pegue el bloque de código siguiente para crear un bloque de variables locales para que lo reutilicen las variables calculadas:

    ```hcl
    # # Locals block for hardcoded names. 
    locals {
        backend_address_pool_name      = "${azurerm_virtual_network.test.name}-beap"
        frontend_port_name             = "${azurerm_virtual_network.test.name}-feport"
        frontend_ip_configuration_name = "${azurerm_virtual_network.test.name}-feip"
        http_setting_name              = "${azurerm_virtual_network.test.name}-be-htst"
        listener_name                  = "${azurerm_virtual_network.test.name}-httplstn"
        request_routing_rule_name      = "${azurerm_virtual_network.test.name}-rqrt"
        app_gateway_subnet_name = "appgwsubnet"
    }
    ```

1. Pegue el bloque de código siguiente para crear un origen de datos para el grupo de recursos, la nueva identidad de usuario:

    ```hcl
    data "azurerm_resource_group" "rg" {
      name = var.resource_group_name
    }

    # User Assigned Identities 
    resource "azurerm_user_assigned_identity" "testIdentity" {
      resource_group_name = data.azurerm_resource_group.rg.name
      location            = data.azurerm_resource_group.rg.location

      name = "identity1"

      tags = var.tags
    }
    ```

1. Pegue el bloque de código siguiente para crear recursos de red de base:

    ```hcl
    resource "azurerm_virtual_network" "test" {
      name                = var.virtual_network_name
      location            = data.azurerm_resource_group.rg.location
      resource_group_name = data.azurerm_resource_group.rg.name
      address_space       = [var.virtual_network_address_prefix]

      subnet {
        name           = var.aks_subnet_name
        address_prefix = var.aks_subnet_address_prefix
      }

      subnet {
        name           = "appgwsubnet"
        address_prefix = var.app_gateway_subnet_address_prefix
      }

      tags = var.tags
    }

    data "azurerm_subnet" "kubesubnet" {
      name                 = var.aks_subnet_name
      virtual_network_name = azurerm_virtual_network.test.name
      resource_group_name  = data.azurerm_resource_group.rg.name
    }

    data "azurerm_subnet" "appgwsubnet" {
      name                 = "appgwsubnet"
      virtual_network_name = azurerm_virtual_network.test.name
      resource_group_name  = data.azurerm_resource_group.rg.name
    }

    # Public Ip 
    resource "azurerm_public_ip" "test" {
      name                         = "publicIp1"
      location                     = data.azurerm_resource_group.rg.location
      resource_group_name          = data.azurerm_resource_group.rg.name
      allocation_method            = "Static"
      sku                          = "Standard"

      tags = var.tags
    }
    ```

1. Pegue el bloque de código siguiente para crear el recurso de Application Gateway:

    ```hcl
    resource "azurerm_application_gateway" "network" {
      name                = var.app_gateway_name
      resource_group_name = data.azurerm_resource_group.rg.name
      location            = data.azurerm_resource_group.rg.location

      sku {
        name     = var.app_gateway_sku
        tier     = "Standard_v2"
        capacity = 2
      }

      gateway_ip_configuration {
        name      = "appGatewayIpConfig"
        subnet_id = data.azurerm_subnet.appgwsubnet.id
      }

      frontend_port {
        name = local.frontend_port_name
        port = 80
      }

      frontend_port {
        name = "httpsPort"
        port = 443
      }

      frontend_ip_configuration {
        name                 = local.frontend_ip_configuration_name
        public_ip_address_id = azurerm_public_ip.test.id
      }

      backend_address_pool {
        name = local.backend_address_pool_name
      }

      backend_http_settings {
        name                  = local.http_setting_name
        cookie_based_affinity = "Disabled"
        port                  = 80
        protocol              = "Http"
        request_timeout       = 1
      }

      http_listener {
        name                           = local.listener_name
        frontend_ip_configuration_name = local.frontend_ip_configuration_name
        frontend_port_name             = local.frontend_port_name
        protocol                       = "Http"
      }

      request_routing_rule {
        name                       = local.request_routing_rule_name
        rule_type                  = "Basic"
        http_listener_name         = local.listener_name
        backend_address_pool_name  = local.backend_address_pool_name
        backend_http_settings_name = local.http_setting_name
      }

      tags = var.tags

      depends_on = ["azurerm_virtual_network.test", "azurerm_public_ip.test"]
    }
    ```

1. Pegue el bloque de código siguiente para crear asignaciones de roles:

    ```hcl
    resource "azurerm_role_assignment" "ra1" {
      scope                = data.azurerm_subnet.kubesubnet.id
      role_definition_name = "Network Contributor"
      principal_id         = var.aks_service_principal_object_id 

      depends_on = ["azurerm_virtual_network.test"]
    }

    resource "azurerm_role_assignment" "ra2" {
      scope                = azurerm_user_assigned_identity.testIdentity.id
      role_definition_name = "Managed Identity Operator"
      principal_id         = var.aks_service_principal_object_id
      depends_on           = ["azurerm_user_assigned_identity.testIdentity"]
    }

    resource "azurerm_role_assignment" "ra3" {
      scope                = azurerm_application_gateway.network.id
      role_definition_name = "Contributor"
      principal_id         = azurerm_user_assigned_identity.testIdentity.principal_id
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }

    resource "azurerm_role_assignment" "ra4" {
      scope                = data.azurerm_resource_group.rg.id
      role_definition_name = "Reader"
      principal_id         = azurerm_user_assigned_identity.testIdentity.principal_id
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    ```

1. Pegue el bloque de código siguiente para crear el clúster de Kubernetes:

    ```hcl
    resource "azurerm_kubernetes_cluster" "k8s" {
      name       = var.aks_name
      location   = data.azurerm_resource_group.rg.location
      dns_prefix = var.aks_dns_prefix

      resource_group_name = data.azurerm_resource_group.rg.name

      linux_profile {
        admin_username = var.vm_user_name

        ssh_key {
          key_data = file(var.public_ssh_key_path)
        }
      }

      addon_profile {
        http_application_routing {
          enabled = false
        }
      }

      default_node_pool {
        name            = "agentpool"
        node_count      = var.aks_agent_count
        vm_size         = var.aks_agent_vm_size
        os_disk_size_gb = var.aks_agent_os_disk_size
        vnet_subnet_id  = data.azurerm_subnet.kubesubnet.id
      }

      service_principal {
        client_id     = var.aks_service_principal_app_id
        client_secret = var.aks_service_principal_client_secret
      }

      network_profile {
        network_plugin     = "azure"
        dns_service_ip     = var.aks_dns_service_ip
        docker_bridge_cidr = var.aks_docker_bridge_cidr
        service_cidr       = var.aks_service_cidr
      }

      depends_on = ["azurerm_virtual_network.test", "azurerm_application_gateway.network"]
      tags       = var.tags
    }

    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

El código que se presenta en esta sección establece el nombre del clúster, la ubicación y el nombre del grupo de recursos. Se establece el valor `dns_prefix`, que forma parte del nombre de dominio completo (FQDN) utilizado para acceder al clúster.

El registro `linux_profile` permite configurar las opciones que permiten iniciar sesión en los nodos de trabajo mediante SSH.

Con AKS, solo se paga por los nodos de trabajo. El registro `agent_pool_profile` configura los detalles de estos nodos de trabajo. El registro `agent_pool_profile record` incluye el número de nodos de trabajo que se van a crear y el tipo de estos. Si necesita escalar o reducir verticalmente el clúster en el futuro, modifique el valor de `count` en este registro.

## <a name="create-a-terraform-output-file"></a>Creación de un archivo de salida de Terraform

Las [salidas de Terraform](https://www.terraform.io/docs/configuration/outputs.html) permiten definir valores que se resaltan al usuario cuando Terraform aplica un plan y pueden consultarse con el comando `terraform output`. En esta sección, va a crear un archivo de salida que permita el acceso al clúster con [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

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

    output "identity_resource_id" {
        value = azurerm_user_assigned_identity.testIdentity.id
    }

    output "identity_client_id" {
        value = azurerm_user_assigned_identity.testIdentity.client_id
    }
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

## <a name="configure-azure-storage-to-store-terraform-state"></a>Configuración de Azure Storage para almacenar el estado de Terraform

Terraform realiza un seguimiento del estado de manera local a través del archivo `terraform.tfstate`. Este patrón funciona bien en un entorno de persona único. Sin embargo, en un entorno más práctico de varias personas, debe realizar el seguimiento del estado en el servidor mediante [Azure Storage](/azure/storage/). En esta sección aprenderá a recuperar la información de la cuenta de almacenamiento necesaria y a crear un contenedor de almacenamiento. La información de estado Terraform se almacena en ese contenedor.

1. En Azure Portal, en **Servicios de Azure**, seleccione **Cuentas de almacenamiento**. (si la opción **Cuentas de almacenamiento** no está visible en la página principal, seleccione **Más servicios** y, después, búsquelo y selecciónelo).

1. En la página **Cuentas de almacenamiento**, seleccione el nombre de la cuenta de almacenamiento en la que Terraform va a almacenar el estado. Por ejemplo, puede utilizar la cuenta de almacenamiento creada cuando abrió Cloud Shell por primera vez.  El nombre de cuenta de almacenamiento creada por Cloud Shell normalmente comienza por `cs` seguido de una cadena aleatoria de números y letras. 

    Anote la cuenta de almacenamiento que seleccione; lo necesitará más tarde.

1. En la página Cuenta de almacenamiento, seleccione **Claves de acceso**.

    ![Menú de la cuenta de almacenamiento](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account.png)

1. Anote el valor de la **clave1** **clave**. (Al seleccionar el icono a la derecha de la clave, se copia el valor al portapapeles).

    ![Claves de acceso de cuenta de almacenamiento](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account-access-key.png)

1. En Cloud Shell, cree un contenedor en su cuenta de Azure Storage. Reemplace los marcadores de posición por los valores adecuados para su cuenta de Azure Storage.

    ```azurecli
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Crear el clúster de Kubernetes
En esta sección, puede ver cómo usar el comando `terraform init` para crear los recursos definidos en los archivos de configuración que se crearon en las secciones anteriores.

1. En Cloud Shell, inicialice Terraform. Reemplace los marcadores de posición por los valores adecuados para su cuenta de Azure Storage.

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
  
    El comando `terraform init` muestra el éxito de inicializar el back-end y el complemento del proveedor:

    ![Ejemplo de resultados de "terraform init"](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-init-complete.png)

1. En Cloud Shell, cree un archivo denominado `terraform.tfvars`:

    ```bash
    code terraform.tfvars
    ```

1. Pegue las siguientes variables, que creó anteriormente en el editor. Para obtener el valor de ubicación del entorno, use `az account list-locations`.

    ```hcl
    resource_group_name = "<Name of the Resource Group already created>"

    location = "<Location of the Resource Group>"
      
    aks_service_principal_app_id = "<Service Principal AppId>"
      
    aks_service_principal_client_secret = "<Service Principal Client Secret>"
      
    aks_service_principal_object_id = "<Service Principal Object Id>"
        
    ```

1. Guarde el archivo ( **&lt;Ctrl > S**) y salga del editor ( **&lt;Ctrl > Q**).

1. Ejecute el comando `terraform plan` para crear el plan de Terraform que define los elementos de infraestructura. 

    ```bash
    terraform plan -out out.plan
    ```

    El comando `terraform plan` muestra los recursos que se crearán al ejecutar el comando `terraform apply`:

    ![Ejemplo de resultados de "terraform plan"](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-plan-complete.png)

1. Ejecute el comando `terraform apply` para aplicar el plan para crear el clúster de Kubernetes. El proceso para crear un clúster de Kubernetes puede tardar varios minutos, dado como resultado el agotamiento del tiempo de espera de la sesión de Cloud Shell. Si la sesión de Cloud Shell agota su tiempo de espera, puede seguir los pasos de la sección "Recuperación de un tiempo de espera de Cloud Shell" para poder completar el tutorial.

    ```bash
    terraform apply out.plan
    ```

    El comando `terraform apply` muestra los resultados de la creación de los recursos definidos en sus archivos de configuración:

    ![Ejemplo de resultados de "terraform apply"](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-apply-complete.png)

1. En Azure Portal, seleccione **Grupos de recursos** en el menú izquierdo para ver los recursos creados para el nuevo clúster de Kubernetes en el grupo de recursos seleccionado.

    ![Aviso de Cloud Shell](./media/terraform-k8s-cluster-appgw-with-tf-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Recuperación de un tiempo de espera de Cloud Shell

Si se agota el tiempo de espera de la sesión de Cloud Shell, puede realizar los siguientes pasos para recuperarla:

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

    ![La herramienta kubectl le permite comprobar el mantenimiento de su clúster de Kubernetes](./media/terraform-k8s-cluster-appgw-with-tf-aks/kubectl-get-nodes.png)

## <a name="install-azure-ad-pod-identity"></a>Instalación de Azure AD Pod Identity

Azure Active Directory Pod Identity proporciona acceso basado en token a [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview).

[Azure AD Pod Identity](https://github.com/Azure/aad-pod-identity) agrega los siguientes componentes al clúster de Kubernetes:

  - [CRD](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/) de Kubernetes: `AzureIdentity`, `AzureAssignedIdentity`, `AzureIdentityBinding`
  - Componente [Controlador de identidades administradas (MIC)](https://github.com/Azure/aad-pod-identity#managed-identity-controllermic)
  - Componente [Identidad administrada del nodo (NMI)](https://github.com/Azure/aad-pod-identity#node-managed-identitynmi)

Si RBAC está **Habilitado**, ejecute el siguiente comando para instalar Azure AD Pod Identity en el clúster:

```bash
kubectl create -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
```

Si RBAC está **Deshabilitado**, ejecute el siguiente comando para instalar Azure AD Pod Identity en el clúster:

```bash
kubectl create -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment.yaml
```

## <a name="install-helm"></a>Instalación de Helm

En el código de esta sección se usa [Helm](/azure/aks/kubernetes-helm), el administrador de paquetes de Kubernetes, para instalar el paquete `application-gateway-kubernetes-ingress`:

1. Si RBAC está **Habilitado**, ejecute el siguiente conjunto de comandos para instalar y configurar Helm:

    ```bash
    kubectl create serviceaccount --namespace kube-system tiller-sa
    kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller-sa
    helm init --tiller-namespace kube-system --service-account tiller-sa
    ```

1. Si RBAC está **Deshabilitado**, ejecute el siguiente comando para instalar y configurar Helm:

    ```bash
    helm init
    ```

1. Agregue el repositorio de Helm de AGIC:

    ```bash
    helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    helm repo update
    ```

## <a name="install-ingress-controller-helm-chart"></a>Instalación del gráfico de Helm del controlador de entrada

1. Descargue `helm-config.yaml` para configurar AGIC:

    ```bash
    wget https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/sample-helm-config.yaml -O helm-config.yaml
    ```

1. Edite `helm-config.yaml` y escriba los valores adecuados para las secciones `appgw` y `armAuth`.

    ```bash
    code helm-config.yaml
    ```

    Los valores se describen de la siguiente manera:

    - `verbosityLevel`: establece el nivel de detalle de la infraestructura de registro de AGIC. Consulte [Niveles de registro](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/463a87213bbc3106af6fce0f4023477216d2ad78/docs/troubleshooting.md#logging-levels) para los valores posibles.
    - `appgw.subscriptionId`: identificador de suscripción de Azure para la instancia de Application Gateway. Ejemplo: `a123b234-a3b4-557d-b2df-a0bc12de1234`
    - `appgw.resourceGroup`: nombre del grupo de recursos de Azure donde se creó Application Gateway. 
    - `appgw.name`: nombre de la instancia de Application Gateway. Ejemplo: `applicationgateway1`.
    - `appgw.shared`: se debe establecer el valor predeterminado de esta marca booleana en `false`. Establézcalo en `true` si necesita una [instancia de Application Gateway compartida](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/072626cb4e37f7b7a1b0c4578c38d1eadc3e8701/docs/setup/install-existing.md#multi-cluster--shared-app-gateway).
    - `kubernetes.watchNamespace`: especifica el espacio de nombres que debe ver AGIC. El espacio de nombres puede ser un valor de cadena único o una lista separada por comas de espacios de nombres. Si marca esta variable como comentario o la establece como una cadena en blanco o vacía, el controlador de entrada que observa todos los espacios de nombres accesibles.
    - `armAuth.type`: valor de `aadPodIdentity` o `servicePrincipal`.
    - `armAuth.identityResourceID`: identificador de recurso de la identidad administrada.
    - `armAuth.identityClientId`: identificador de cliente de la identidad.
    - `armAuth.secretJSON`: solo es necesario cuando se elige el tipo de secreto de entidad de servicio (cuando se establece `armAuth.type` en `servicePrincipal`).

    Notas clave:
    - El valor `identityResourceID` se crea en el script de Terraform y se encuentra al ejecutar: `echo "$(terraform output identity_resource_id)"`.
    - El valor `identityClientID` se crea en el script de Terraform y se encuentra al ejecutar: `echo "$(terraform output identity_client_id)"`.
    - El valor `<resource-group>` es el grupo de recursos de la instancia de Application Gateway.
    - El valor `<identity-name>` es el nombre de la identidad creada.
    - Todas las identidades de una suscripción determinada se pueden enumerar mediante: `az identity list`.

1. Instale el paquete del controlador de entrada de Application Gateway:

    ```bash
    helm install -f helm-config.yaml application-gateway-kubernetes-ingress/ingress-azure
    ```

### <a name="install-a-sample-app"></a>Instalación de una aplicación de ejemplo

Una vez que haya instalado Application Gateway, AKS y AGIC, puede instalar una aplicación de ejemplo mediante [Azure Cloud Shell](https://shell.azure.com/):

1. Use el comando curl para descargar el archivo YAML:

    ```bash
    curl https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml -o aspnetapp.yaml
    ```

2. Aplique el archivo YAML:

    ```bash
    kubectl apply -f aspnetapp.yaml
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine los recursos creados en este artículo.  

Reemplace el marcador de posición por el valor adecuado. Se eliminarán todos los recursos del grupo de recursos especificado.

```azurecli
az group delete -n <resource-group>
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Controlador de entrada de Application Gateway](https://azure.github.io/application-gateway-kubernetes-ingress/)