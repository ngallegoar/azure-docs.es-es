---
title: 'Inicio rápido: Conectarse a una máquina virtual mediante una dirección IP privada y Azure Bastion'
description: En este artículo, aprenderá a crear un host de Azure Bastion desde una máquina virtual y a conectarse de forma segura mediante una dirección IP privada.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: charwen
ms.openlocfilehash: a420a3253040fff8b767a81f298ede283c1d214b
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619249"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>Inicio rápido: Conectarse a una máquina virtual mediante una dirección IP privada y Azure Bastion

Este artículo de inicio rápido muestra cómo conectarse a una máquina virtual mediante una dirección IP privada. Cuando se conecta a través de Azure Bastion, las máquinas virtuales no necesitan una dirección IP pública. Los pasos de este artículo le ayudarán a implementar Bastion en la red virtual a través de la máquina virtual en el portal. Cuando se aprovisiona el servicio, la experiencia de RDP/SSH está disponible para todas las máquinas virtuales de la misma red virtual.

## <a name="prerequisites"></a><a name="prereq"></a>Requisitos previos

* Una red virtual de Azure.
* Una máquina virtual de Azure en la red virtual con el puerto 3389 abierto.

### <a name="example-values"></a>Valores de ejemplo

|**Nombre** | **Valor** |
| --- | --- |
| Nombre |  VNet1Bastion |
| Region | estado |
| Virtual network |  VNet1 |
| + Nombre de subred | AzureBastionSubnet |
| Direcciones de AzureBastionSubnet |  10.1.254.0/27 |
| Dirección IP pública |  Crear nuevo |
| Nombre de la dirección IP pública | VNet1BastionPIP  |
| SKU de la dirección IP pública |  Estándar  |
| Asignación  | estática |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Creación de un host de Bastion

Si crea un host bastión en el portal mediante una máquina virtual existente, varios valores de configuración se establecerán automáticamente y de forma predeterminada de acuerdo con la máquina virtual o la red virtual.

1. Abra [Azure Portal](https://portal.azure.com). Vaya a la máquina virtual y, a continuación, haga clic en **Conectar**.

   ![Configuración de máquina virtual](./media/quickstart-host-portal/vm-settings.png)
1. En el menú desplegable, seleccione **Bastion**.
1. En la página Conectar, seleccione **Usar Bastion**.

   ![Seleccionar Bastion](./media/quickstart-host-portal/select-bastion.png)

1. En la página de Bastion, rellene los campos de configuración siguientes:

   * **Name**: nombre del host bastión
   * **Subred**: subred de la red virtual en la que se implementará el recurso de Bastion. La subred debe crearse con el nombre **AzureBastionSubnet**. El nombre permite a Azure saber en qué subred se debe implementar el recurso de Bastion. Esto no es lo mismo que una subred de puerta de enlace. Use una subred de al menos /27 o mayor (/27, /26, /25, etc.).
   
      * Seleccione **Administrar configuración de subred** y, a continuación, **+ Subred**.
      * En la página Agregar subred, escriba **AzureBastionSubnet**.
      * Especifique el intervalo de direcciones en la notación CIDR. Por ejemplo, 10.1.254.0/27:
      * Seleccione **Aceptar** para crear la subred. En la parte superior de la página, vuelva a Bastion para completar el resto de la configuración.

         ![Navegar a la configuración de Bastion](./media/quickstart-host-portal/navigate-bastion.png)
   * **Dirección IP pública**: dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). Cree una dirección IP pública o use una existente. La dirección debe estar en la misma región que el recurso de Bastion que está creando.
   * **Nombre de dirección IP pública**: nombre del recurso de la dirección IP pública.
1. En la pantalla de validación, haga clic en **Crear**. Espere unos cinco minutos a que el recurso de Bastion se cree e implemente.

   ![Crear un host de Bastion](./media/quickstart-host-portal/bastion-settings.png)

## <a name="connect"></a><a name="connect"></a>Conexión

Una vez implementado Bastion en la red virtual, la pantalla cambia a la página Conectar.

1. Escriba el nombre de usuario y la contraseña de la máquina virtual. A continuación, seleccione **Conectar**.

   ![conectar](./media/quickstart-host-portal/connect.png)
1. La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

   ![Conexión RDP](./media/quickstart-host-portal/443-rdp.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar la red virtual y las máquinas virtuales, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *TestRG1* en el cuadro **Buscar** de la parte superior del portal y seleccione **TestRG1** en los resultados de la búsqueda.

2. Seleccione **Eliminar grupo de recursos**.

3. Escriba *TestRG1* en la sección **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un host bastión para la red virtual y, a continuación, se ha conectado a una máquina virtual de forma segura a través del host bastión.

* Para obtener más información acerca de Azure Bastion, consulte [Introducción a Bastion](bastion-overview.md) y [Preguntas más frecuentes sobre Bastion](bastion-faq.md).
* Para usar grupos de seguridad de red con la subred de Azure Bastion, consulte [Trabajo con grupos de seguridad de red](bastion-nsg.md).
* Para obtener instrucciones que contengan explicaciones sobre la configuración del host de Azure Bastion, consulte el [tutorial](bastion-create-host-portal.md).
* Para conectarse a un conjunto de escalado de máquinas virtuales, consulte [Conexión a un conjunto de escalado de máquinas virtuales mediante Azure Bastion](bastion-connect-vm-scale-set.md).