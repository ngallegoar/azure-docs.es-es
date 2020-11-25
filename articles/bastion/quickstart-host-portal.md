---
title: 'Inicio rápido: Configuración de Azure Bastion y conexión a una máquina virtual mediante una dirección IP privada y un explorador'
titleSuffix: Azure Bastion
description: En este artículo de inicio rápido aprenderá a crear un host de Azure Bastion desde una máquina virtual y a conectarse a ella de forma segura mediante el explorador y una dirección IP privada.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 10/15/2020
ms.author: cherylmc
ms.openlocfilehash: 325f39b695d80c14ed7097d071380b937458546c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96021493"
---
# <a name="quickstart-connect-to-a-vm-securely-through-a-browser-via-private-ip-address"></a>Inicio rápido: conexión a una máquina virtual de forma segura mediante un explorador y una dirección IP privada

Puede conectarse a una máquina virtual mediante el explorador con Azure Bastion y Azure Portal. En este artículo de inicio rápido se muestra cómo configurar Azure Bastion en función de la configuración de la máquina virtual y conectarse a la máquina virtual mediante el portal. La máquina virtual no necesita una dirección IP pública, software cliente, agente ni una configuración especial. Cuando se aprovisiona el servicio, la experiencia de RDP/SSH está disponible para todas las máquinas virtuales de la misma red virtual. Para más información sobre Azure Bastion, consulte [¿Qué es Azure Bastion?](bastion-overview.md)

## <a name="prerequisites"></a><a name="prereq"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. Si no tiene ninguna, [cree una gratis](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Para poder conectarse a una máquina virtual mediante el explorador con Bastion, debe poder iniciar sesión en Azure Portal.

* Una máquina virtual Windows en una red virtual. Si no tiene una máquina virtual, cree una mediante [Inicio rápido: creación de una máquina virtual](../virtual-machines/windows/quick-create-portal.md).

  * Si necesita valores de ejemplo, consulte los [Valores de ejemplo](#values) proporcionados.
  * Si ya tiene una red virtual, asegúrese de seleccionarla en la pestaña Redes al crear la máquina virtual.
  * Si aún no tiene una, puede crearla al mismo tiempo que la máquina virtual.
  * No es necesario tener una dirección IP pública para esta máquina virtual para poder conectarse mediante Azure Bastion.

* Roles de máquina virtual obligatorios:
  * Rol de lector en la máquina virtual.
  * Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual.
  
* Puertos de máquina virtual obligatorios:
  * Puertos de entrada: RDP (3389)

### <a name="example-values"></a><a name="values"></a>Valores del ejemplo

Puede usar los siguientes valores de ejemplo al crear esta configuración, o puede sustituirlos por los propios.

**Valores básicos de la red virtual y la máquina virtual:**

|**Nombre** | **Valor** |
| --- | --- |
| Máquina virtual| TestVM |
| Resource group | TestRG |
| Region | Este de EE. UU. |
| Virtual network | TestVNet1 |
| Espacio de direcciones | 10.0.0.0/16 |
| Subredes | front-end: 10.0.0.0/24 |

**Valores de Azure Bastion:**

|**Nombre** | **Valor** |
| --- | --- |
| Nombre | TestVNet1-bastion |
| + Nombre de subred | AzureBastionSubnet |
| Direcciones de AzureBastionSubnet | Una subred dentro del espacio de direcciones de la red virtual con una máscara de subred/27. Por ejemplo, 10.0.1.0/27.  |
| Dirección IP pública |  Crear nuevo |
| Nombre de la dirección IP pública | VNet1BastionPIP  |
| SKU de la dirección IP pública |  Estándar  |
| Asignación  | estática |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Creación de un host de Bastion

Hay varias maneras de configurar un host bastión. En los pasos siguientes creará un host bastión en Azure Portal directamente desde la máquina virtual. A crear un host desde una máquina virtual, varias opciones de configuración se rellenarán automáticamente con los valores de la máquina virtual o la red virtual.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a la máquina virtual a la que quiere conectarse y seleccione **Conectar**.

   :::image type="content" source="./media/quickstart-host-portal/vm-settings.png" alt-text="Configuración de máquina virtual" lightbox="./media/quickstart-host-portal/vm-settings.png":::
1. En el menú desplegable, seleccione **Bastion**.
1. En la **página TestVM | Conectar**, seleccione **Usar Bastion**.

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="Selección de Bastion" border="false":::

1. En la página de **Bastion**, rellene los campos de configuración siguientes:

   * **Nombre**: nombre del host bastión.
   * **Subred**: Este es el espacio de direcciones de red virtual en el que se implementará el recurso de Bastion. La subred debe crearse con el nombre **AzureBastionSubnet**. Use una subred de al menos /27 o mayor (/27, /26, /25, etc.).
   * Seleccione **Administrar configuración de subred**.
1. En la página **Subredes**, seleccione **+Subred**.

   :::image type="content" source="./media/quickstart-host-portal/subnet.png" alt-text="+ Subred":::
    
1. En la página **Agregar subred**, en **Nombre** escriba **AzureBastionSubnet**.
   * En intervalo de direcciones de subred, elija una dirección de subred que esté dentro del espacio de direcciones de la red virtual.
   * No ajuste ninguna otra configuración. Seleccione **Aceptar** para aceptar y guardar los cambios en la subred.

   :::image type="content" source="./media/quickstart-host-portal/add-subnet.png" alt-text="Agregar subred":::
1. Haga clic en el botón Atrás del explorador para volver a la página **Bastion** y seguir especificando valores.
   * **Dirección IP pública**: deje el campo como **Crear nuevo**.
   * **Nombre de dirección IP pública**: nombre del recurso de la dirección IP pública.
   * **Asignación**: el valor predeterminado es Estático. No se puede usar una asignación dinámica para Azure Bastion.
   * **Grupo de recursos**: el mismo que la máquina virtual.

   :::image type="content" source="./media/quickstart-host-portal/validate.png" alt-text="Creación del host bastión":::
1. Seleccione **Crear** para crear el host bastión. Azure valida la configuración y luego crea el host. El host y sus recursos tardan unos cinco minutos en crearse e implementarse.

## <a name="connect"></a><a name="connect"></a>Conexión

Una vez implementado Bastion en la red virtual, la pantalla cambia a la página Conectar.

1. Escriba el nombre de usuario y la contraseña de la máquina virtual. A continuación, seleccione **Conectar**.

   :::image type="content" source="./media/quickstart-host-portal/connect-vm.png" alt-text="Captura de pantalla que muestra el cuadro de diálogo Conectar usando Azure Bastion.":::
1. La conexión RDP con esta máquina virtual a se abrirá directamente en Azure Portal (mediante HTML5) con el puerto 443 y el servicio Bastion.

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="Conexión RDP":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar la red virtual y las máquinas virtuales, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba el nombre del grupo de recursos en el cuadro **Buscar** de la parte superior del portal y selecciónelo en los resultados de la búsqueda.

1. Seleccione **Eliminar grupo de recursos**.

1. En **TYPE THE RESOURCE GROUP NAME** (ESCRIBIR EL NOMBRE DEL GRUPO DE RECURSOS), escriba el grupo de recursos y seleccione **Delete** (Eliminar).

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha creado un host bastión para la red virtual y, a continuación, se ha conectado a una máquina virtual de forma segura mediante Bastion. Después, puede continuar con el paso siguiente si quiere conectarse a un conjunto de escalado de máquinas virtuales.

> [!div class="nextstepaction"]
> [Conexión a un conjunto de escalado de máquinas virtuales mediante Azure Bastion](bastion-connect-vm-scale-set.md)
