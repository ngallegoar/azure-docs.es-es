---
title: 'Tutorial: Validación de una red en estrella tipo hub-and-spoke en Azure con Terraform'
description: Tutorial para validar la topología de red en estrella tipo hub-and-spoke con redes virtuales conectadas entre sí.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 9ba4780c59e5e9da4999573abbc08ecd2738a2cd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74159193"
---
# <a name="tutorial-validate-a-hub-and-spoke-network-in-azure-using-terraform"></a>Tutorial: Validación de una red en estrella tipo hub-and-spoke en Azure con Terraform

En este artículo se ejecutan los archivos de terraform creados en el artículo anterior de esta serie. El resultado es una validación de la conectividad entre las redes virtuales de demostración.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Uso de HCL (HashiCorp Language) para implementar la red virtual del centro en topología en estrella tipo hub-and-spoke
> * Uso del plan de Terraform para comprobar los recursos que se van a implementar
> * Uso de un dispositivo de Terraform para crear los recursos en Azure
> * Comprobación de la conectividad entre distintas redes
> * Uso de Terraform para destruir todos los recursos

## <a name="prerequisites"></a>Prerrequisitos

1. [Creación de una topología de red híbrida en estrella tipo hub-and-spoke con Terraform en Azure](./terraform-hub-spoke-introduction.md).
1. [Creación de una red virtual local con Terraform en Azure](./terraform-hub-spoke-on-prem.md).
1. [Creación de una red virtual del centro con Terraform en Azure](./terraform-hub-spoke-hub-network.md).
1. [Creación de un dispositivo de red virtual de centro con Terraform en Azure](./terraform-hub-spoke-hub-nva.md).
1. [Creación de redes virtuales en estrella tipo hub-and-spoke con Terraform en Azure](./terraform-hub-spoke-spoke-network.md).

## <a name="verify-your-configuration"></a>Comprobación de la configuración

Después de completar los [requisitos previos](#prerequisites), compruebe que existen los archivos de configuración adecuados.

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd hub-spoke
    ```

1. Ejecute el comando `ls` para comprobar que se enumeran los archivos de configuración `.tf` creados en los tutoriales anteriores:

    ![Archivos de configuración de demostración de Terraform](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-config-files.png)

## <a name="deploy-the-resources"></a>Implementación de los recursos

1. Inicialice el proveedor de Terraform:
    
    ```bash
    terraform init
    ```
    
    ![Resultados de ejemplo del comando "terraform init"](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-init.png)
    
1. Ejecute el comando `terraform plan` para ver el efecto de la implementación antes de la ejecución:

    ```bash
    terraform plan
    ```
    
    ![Resultados de ejemplo del comando "terraform plan"](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-plan.png)

1. Implemente la solución:

    ```bash
    terraform apply
    ```
    
    Escriba `yes` cuando se le pida que confirme la implementación.

    ![Resultados de ejemplo del comando "terraform apply"](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-apply.png)
    
## <a name="test-the-hub-vnet-and-spoke-vnets"></a>Prueba de la red virtual del centro y las radiales

En esta sección se muestra cómo probar la conectividad del entorno local simulado a la red virtual del centro.

1. En Azure Portal, vaya al grupo de recursos **onprem-vnet-rg**.

1. En la pestaña **onprem-vnet-rg**, seleccione la máquina virtual denominada **onprem-vm**.

1. Seleccione **Conectar**.

1. Junto al texto **Iniciar sesión con la cuenta local de VM**, copie el comando **ssh** en el portapapeles.

1. Desde un símbolo del sistema de Linux, ejecute `ssh` para conectar con el entorno local simulado. Use la contraseña que especificó en el archivo de parámetros `on-prem.tf`.

1. Ejecute el comando `ping` para probar la conectividad con la máquina virtual de JumpBox en la red virtual del centro:

   ```bash
   ping 10.0.0.68
   ```

1. Ejecute el comando `ping` para probar la conectividad con las máquinas virtuales de JumpBox en cada radio:

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

1. Para salir de la sesión de ssh de la máquina virtual **onprem-vm**, escriba `exit` y presione &lt;Entrar>.

## <a name="troubleshoot-vpn-issues"></a>Solución de problemas de VPN

Para información sobre cómo resolver errores VPN, consulte el artículo [Solución de problemas de una conexión VPN híbrida](/azure/architecture/reference-architectures/hybrid-networking/troubleshoot-vpn).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine los recursos creados en esta serie de tutoriales.

1. Quite los recursos declarados en el plan:

    ```bash
    terraform destroy
    ```

    Escriba `yes` cuando se le pida que confirme la eliminación de los recursos.

1. Cambie los directorios al principal:

    ```bash
    cd ..
    ```

1. Elimine el directorio `hub-scope` (incluidos todos sus archivos):

    ```bash
    rm -r hub-spoke
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Más información sobre el uso de Terraform en Azure](/azure/terraform)