---
title: 'Tutorial: Creación de una topología de red híbrida en estrella tipo hub-and-spoke en Azure con Terraform'
description: Tutorial que ilustra cómo crear una arquitectura de referencia de red híbrida completa en Azure mediante Terraform
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 6f156dd90b83ceaf5749c8c2acebae35bcb54a92
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77472186"
---
# <a name="tutorial-create-a-hub-and-spoke-hybrid-network-topology-in-azure-using-terraform"></a>Tutorial: Creación de una topología de red híbrida en estrella tipo hub-and-spoke en Azure con Terraform

Esta serie de tutoriales muestra cómo usar Terraform para implementar en Azure una [topología de red en estrella tipo hub-and-spoke](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). 

Una topología en estrella tipo hub-and-spoke es una forma de aislar las cargas de trabajo mientras se comparten servicios comunes. Estos servicios incluyen la identidad y la seguridad. El centro es una red virtual que actúa como punto central de conexión a una red local. Los radios son redes virtuales que se emparejan con el concentrador. Los servicios compartidos se implementan en el centro, mientras que las cargas de trabajo individuales se implementan allí como redes radiales.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Uso de HCL (lenguaje de HashiCorp) para disponer los recursos de la arquitectura de referencia de la red híbrida en estrella tipo hub-and-spoke
> * Uso de Terraform para crear recursos de dispositivo de red del centro
> * Uso de Terraform para crear una red de centro en Azure que actúe como punto común a todos los recursos
> * Uso de Terraform para crear cargas de trabajo individuales como redes virtuales radiales en Azure
> * Uso de Terraform para establecer puertas de enlace y conexiones entre redes locales y de Azure
> * Uso de Terraform para emparejar redes virtuales a redes radiales

## <a name="prerequisites"></a>Prerrequisitos

- **Suscripción de Azure**: Si no tiene ya una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Instalación y configuración de Terraform**: Para aprovisionar máquinas virtuales y otras infraestructuras en Azure, [instale y configure Terraform](terraform-install-configure.md)

## <a name="hub-and-spoke-topology-architecture"></a>Arquitectura de la topología de red en estrella tipo hub-and-spoke

En la topología en estrella tipo hub-and-spoke, el centro es una red virtual. La red es un punto central de conectividad para la red local. Las redes son redes virtuales del mismo nivel que el centro y se pueden usar para aislar cargas de trabajo. El tráfico fluye entre el centro de datos local y el concentrador a través de una conexión a ExpressRoute o a VPN Gateway. La siguiente imagen muestra los componentes de una topología de red en estrella tipo hub-and-spoke:

![Arquitectura de la topología de red en estrella tipo hub-and-spoke en Azure](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-architecture.png)

## <a name="benefits-of-the-hub-and-spoke-topology"></a>Ventajas de una topología de red en estrella tipo hub-and-spoke

Una topología de red en estrella tipo hub-and-spoke es una forma de aislar las cargas de trabajo mientras se comparten servicios comunes. Estos servicios incluyen la identidad y la seguridad. El centro es una red virtual que actúa como punto central de conexión a una red local. Los radios son redes virtuales que se emparejan con el concentrador. Los servicios compartidos se implementan en el centro, mientras que las cargas de trabajo individuales se implementan allí como redes radiales. Estas son algunas de las ventajas de la topología de red en estrella tipo hub-and-spoke:

- **Ahorro de costos** mediante la centralización de los servicios en una sola ubicación en la que se puedan compartir entre varias cargas de trabajo. Estas cargas de trabajo incluyen dispositivos virtuales de red y servidores DNS.
- **Superar los límites de las suscripciones** gracias al emparejamiento de las redes virtuales de diferentes suscripciones con el concentrador central.
- **Separación de intereses** entre la TI central (SecOps e InfraOps) y las cargas de trabajo (DevOps).

## <a name="typical-uses-for-the-hub-and-spoke-architecture"></a>Usos típicos de la arquitectura en estrella tipo hub-and-spoke

Algunos de los usos típicos de una arquitectura en estrella tipo hub-and-spoke incluyen:

- Muchos clientes tienen cargas de trabajo que se implementan en entornos diferentes. Estos entornos incluyen desarrollo, pruebas y producción. Muchas veces, estas cargas de trabajo necesitan compartir servicios como DNS, IDS, NTP o AD DS. Estos servicios compartidos se pueden colocar en la red virtual del centro. De este modo, cada entorno se implementa en un radio para mantener el aislamiento.
- Cargas de trabajo que no requieren conectividad entre sí, pero requieren acceso a los servicios compartidos.
- Empresas que requieren el control centralizado sobre aspectos de seguridad.
- Empresas que requieren administración segregada de las cargas de trabajo en cada radio.

## <a name="preview-the-demo-components"></a>Vista previa de los componentes de demostración

Al trabajar a través de cada tutorial de esta serie se definen varios componentes en distintos scripts de Terraform. La arquitectura de demostración creada e implementada está formada por los siguientes componentes:

- **Red local**. Una red de área local privada que se ejecuta dentro de una organización. Para la arquitectura de referencia de la red en estrella tipo hub-and-spoke, se usa una red virtual de Azure para simular una red local.

- **Dispositivo VPN**. Un dispositivo o servicio VPN proporciona conectividad externa a la red local. El dispositivo VPN puede ser un dispositivo de hardware o una solución de software. 

- **Red virtual del concentrador**. El centro es un punto central de conectividad para la red local y un logar donde hospedar servicios. Estos servicios pueden consumirlos las diferentes cargas de trabajo que se hospedan en las redes virtuales radiales.

- **Subred de puerta de enlace**. Las puertas de enlace de red virtual se conservan en la misma subred.

- **Redes virtuales de radios**. Los radios pueden utilizarse para aislar las cargas de trabajo en sus propias redes virtuales, administradas por separado desde otros radios. Cada carga de trabajo puede incluir varios niveles, con varias subredes que se conectan a través de equilibradores de carga de Azure. 

- **Emparejamiento de VNET**. Se pueden conectar dos redes virtuales mediante una conexión de emparejamiento. Las conexiones de emparejamiento son conexiones no transitivas de baja latencia entre las redes virtuales. Una vez establecido el emparejamiento, las redes virtuales intercambian el tráfico mediante la red troncal de Azure, sin necesidad de un enrutador. En una topología de red en estrella tipo hub-and-spoke, el emparejamiento de redes virtuales se usa para conectar el centro a cada radio. Puede emparejar redes virtuales de la misma región o de regiones diferentes.

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios

Cree el directorio que contenga los archivos de configuración de Terraform para la demostración.

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `hub-spoke`.

    ```bash
    mkdir hub-spoke
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-azure-provider"></a>Declaración del proveedor de Azure

Cree el archivo de configuración de Terraform que declara el proveedor de Azure.

1. En Cloud Shell, abra un archivo nuevo llamado `main.tf`.

    ```bash
    code main.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    provider "azurerm" {
        version = "~>1.22"
    }
    ```

1. Guarde el archivo y salga del editor.

## <a name="create-the-variables-file"></a>Creación del archivo de variables

Cree el archivo de configuración de Terraform para las variables comunes que se usan en los distintos scripts.

1. En Cloud Shell, abra un archivo nuevo llamado `variables.tf`.

    ```bash
    code variables.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    variable "location" {
      description = "Location of the network"
      default     = "centralus"
    }
    
    variable "username" {
      description = "Username for Virtual Machines"
      default     = "testadmin"
    }
    
    variable "password" {
      description = "Password for Virtual Machines"
      default     = "Password1234!"
    }
    
    variable "vmsize" {
      description = "Size of the VMs"
      default     = "Standard_DS1_v2"
    }
    ```

1. Guarde el archivo y salga del editor.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Creación de una red virtual local con Terraform en Azure](./terraform-hub-spoke-on-prem.md)