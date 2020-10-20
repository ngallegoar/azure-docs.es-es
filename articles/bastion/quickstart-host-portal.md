---
title: 'Inicio rápido: Creación de un host bastión a partir de una máquina virtual y conexión a través de una dirección IP privada'
titleSuffix: Azure Bastion
description: En este artículo de inicio rápido, aprenderá a crear un host de Azure Bastion desde una máquina virtual y a conectarse de forma segura mediante una dirección IP privada.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 6f451e7b115c00bc7b2cf350e00b9f704ab1d29f
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019059"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>Inicio rápido: Conectarse a una máquina virtual mediante una dirección IP privada y Azure Bastion

En este artículo de inicio rápido se muestra cómo conectarse a una máquina virtual mediante el explorador con Azure Bastion y Azure Portal. En Azure Portal, desde la máquina virtual de Azure, puede implementar Bastion en la red virtual. Una vez implementado, puede conectarse a la máquina virtual a través de su dirección IP privada con Azure Portal. La máquina virtual no necesita una dirección IP pública ni un software especial. Una de las ventajas de crear un host bastión para la red virtual directamente desde la máquina virtual es que muchos de los valores se rellenan automáticamente.

Cuando se aprovisiona el servicio, la experiencia de RDP/SSH está disponible para todas las máquinas virtuales de la misma red virtual. Para más información sobre Azure Bastion, consulte [¿Qué es Azure Bastion?](bastion-overview.md)

## <a name="prerequisites"></a><a name="prereq"></a>Requisitos previos

* Una red virtual.
* Una máquina virtual Windows en la red virtual.
* Los siguientes roles necesarios:
  * Rol de lector en la máquina virtual.
  * Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual.

* Puertos: para conectarse a la máquina virtual, debe tener abiertos los siguientes puertos en dicha máquina:
  * Puertos de entrada: RDP (3389)

### <a name="example-values"></a>Valores de ejemplo

|**Nombre** | **Valor** |
| --- | --- |
| Nombre |  TestVNet1-bastion |
| Virtual network |  TestVNet1 (basado en la máquina virtual) |
| + Nombre de subred | AzureBastionSubnet |
| Direcciones de AzureBastionSubnet |  10.1.254.0/27 |
| Dirección IP pública |  Crear nuevo |
| Nombre de la dirección IP pública | VNet1BastionPIP  |
| SKU de la dirección IP pública |  Estándar  |
| Asignación  | estática |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Creación de un host de Bastion

Si crea un host bastión en Azure Portal mediante una máquina virtual existente, varios valores de configuración se establecerán automáticamente y de forma predeterminada de acuerdo con la máquina virtual o la red virtual.

1. Abra [Azure Portal](https://portal.azure.com). Vaya a la máquina virtual y, luego, seleccione **Conectar**.

   :::image type="content" source="./media/quickstart-host-portal/vm-settings.png" alt-text="Configuración de máquina virtual" lightbox="./media/quickstart-host-portal/vm-settings.png":::
1. En el menú desplegable, seleccione **Bastion**.
1. En la **página TestVM | Conectar**, seleccione **Usar Bastion**.

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="Configuración de máquina virtual" border="false":::

1. En la página de **Bastion**, rellene los campos de configuración siguientes:

   * **Nombre**: nombre del host bastión.
   * **Subred**: subred de la red virtual en la que se implementará el recurso de Bastion. La subred debe crearse con el nombre **AzureBastionSubnet**. El nombre permite a Azure saber en qué subred se debe implementar el recurso de Bastion. Esta es diferente de una subred de puerta de enlace. Use una subred de al menos /27 o mayor (/27, /26, /25, etc.).
   
      * Seleccione **Administrar configuración de subred**.
      * Seleccione **AzureBastionSubnet**.
      * Si es necesario, ajuste el intervalo de direcciones en la notación CIDR. Por ejemplo, 10.1.254.0/27:
      * No ajuste ninguna otra configuración. Seleccione **Aceptar** para aceptar y guardar los cambios de la subred, o seleccione **x** en la parte superior de la página si no quiere realizar ningún cambio.
1. Haga clic en el botón Atrás del explorador para volver a la página **Bastion** y seguir especificando valores.
   * **Nombre de dirección IP pública**: nombre del recurso de la dirección IP pública.
   * **Dirección IP pública**: dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). Cree una dirección IP pública.
1. Seleccione **Crear** para crear el host bastión. Azure valida la configuración y luego crea el host. El host y sus recursos tardan unos cinco minutos en crearse e implementarse.

   :::image type="content" source="./media/quickstart-host-portal/validate.png" alt-text="Configuración de máquina virtual":::

## <a name="connect"></a><a name="connect"></a>Conexión

Una vez implementado Bastion en la red virtual, la pantalla cambia a la página Conectar.

1. Escriba el nombre de usuario y la contraseña de la máquina virtual. A continuación, seleccione **Conectar**.

   :::image type="content" source="./media/quickstart-host-portal/connect-vm.png" alt-text="Configuración de máquina virtual":::
1. La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="Configuración de máquina virtual":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar la red virtual y las máquinas virtuales, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba el nombre del grupo de recursos en el cuadro **Buscar** de la parte superior del portal y selecciónelo en los resultados de la búsqueda.

1. Seleccione **Eliminar grupo de recursos**.

1. En **TYPE THE RESOURCE GROUP NAME** (ESCRIBIR EL NOMBRE DEL GRUPO DE RECURSOS), escriba el grupo de recursos y seleccione **Delete** (Eliminar).

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un host bastión para la red virtual y, a continuación, se ha conectado a una máquina virtual de forma segura a través del host bastión. Después, puede continuar con el paso siguiente si quiere conectarse a un conjunto de escalado de máquinas virtuales.

> [!div class="nextstepaction"]
> [Conexión a un conjunto de escalado de máquinas virtuales mediante Azure Bastion](bastion-connect-vm-scale-set.md)
