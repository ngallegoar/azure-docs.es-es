---
title: Creación de un disco duro virtual compatible con Azure para Azure Marketplace
description: Se explica cómo crear un disco duro virtual para una oferta de máquina virtual en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: dsindona
ms.openlocfilehash: 99d2bc95c1dd837bfc3bcabcead28777b7e6f746
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273943"
---
# <a name="create-an-azure-compatible-vhd"></a>Creación de un disco duro virtual compatible con Azure

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://aka.ms/AzureVMTechAsset) para administrar las ofertas migradas.

En este artículo se detallan los pasos necesarios para crear un disco duro virtual (VHD) para una oferta de máquina virtual (VM) en Azure Marketplace.  También se incluyen procedimientos recomendados para diversos aspectos, por ejemplo, el uso del Protocolo de escritorio remoto (RDP), la selección de un tamaño para la máquina virtual, la instalación de las actualizaciones de Windows más recientes y cómo generalizar la imagen de disco duro virtual.  Las secciones siguientes se centran principalmente en discos duros virtuales basados en Windows; para obtener más información sobre cómo crear discos duros virtuales basados en Linux, vea [Linux en distribuciones aprobadas por Azure](../../../virtual-machines/linux/endorsed-distros.md). 

> [!WARNING]
> Se recomienda encarecidamente seguir las instrucciones de este tema para usar Azure para crear una máquina virtual que contenga un sistema operativo preconfigurado y aprobado.  Si esto no es compatible con la solución, es posible crear y configurar una máquina virtual de manera local mediante un sistema operativo aprobado.  Después, puede configurarlo y prepararlo para la carga, como se describe en [Preparación de un VHD o un VHDX de Windows antes de cargarlo en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).


## <a name="select-an-approved-base"></a>Selección de una base aprobada
Los VHD del sistema operativo para su imagen de máquina virtual deben basarse en una imagen aprobada para Azure que contenga Windows Server o SQL Server.
Para comenzar, cree una máquina virtual a partir de alguna de las imágenes siguientes, que se encuentran en Microsoft Azure Portal:

-    Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
-    [SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)
-    [SQL Server 2012 SP2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)

> [!TIP]
> Si usa el Portal de Azure actual o PowerShell, las imágenes de Windows Server publicadas a partir del 8 de septiembre de 2014 están aprobadas.

Como alternativa, Azure ofrece una gama de distribuciones de Linux aprobadas.  Para obtener una lista actual, vea [Linux en distribuciones aprobadas por Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).


## <a name="create-vm-in-the-azure-portal"></a>Creación de la máquina virtual en Azure Portal 

En [Microsoft Azure Portal](https://ms.portal.azure.com/), cree la imagen base mediante los pasos siguientes.

1. Inicie sesión en el portal con la cuenta Microsoft de la suscripción de Azure que quiera usar para publicar la oferta de máquina virtual.
2. Cree un grupo de recursos y proporcione el **Nombre del grupo de recursos**, la **Suscripción** y la **Ubicación del grupo de recursos**.  Para obtener instrucciones, vea [Administración de grupos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
3. Haga clic en **Máquinas virtuales** en la barra de menús de la izquierda para mostrar la página de detalles Máquinas virtuales. 
4. En esta página nueva, haga clic en **+Agregar** para mostrar la hoja **Proceso**.  Si no ve el tipo de máquina virtual en la pantalla inicial, puede buscar el nombre de la máquina virtual base, por ejemplo:

    ![Hoja Proceso de la nueva máquina virtual](./media/publishvm_014.png)

5. Después de seleccionar la imagen virtual adecuada, proporcione los valores siguientes:
   * En la hoja **Básico**, escriba un **Nombre** para la máquina virtual, de entre 1 y 15 caracteres alfanuméricos. (En este ejemplo se usa `DemoVm009`).
   * Escriba un **nombre de usuario** y una **contraseña** segura, que se usarán para crear una cuenta local en la máquina virtual.  (En este caso se usa `adminUser`).  La contraseña debe tener entre 8 y 123 caracteres y reunir, al menos, tres de los cuatro requisitos de complejidad siguientes: contener al menos una minúscula, una mayúscula, un número y un carácter especial. Para más información, vea [Requisitos de nombre de usuario y contraseña](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-faq#what-are-the-username-requirements-when-creating-a-vm).
   * Seleccione el grupo de recursos que ha creado (en este caso, `DemoResourceGroup`).
   * Seleccione una **ubicación** del centro de datos de Azure (en este caso, `West US`).
   * Haga clic en **Aceptar** para guardar estos valores. 

6. Seleccione el tamaño de la máquina virtual que se va a implementar mediante las recomendaciones siguientes:
   * Si planea desarrollar el VHD en modo local, el tamaño no importa. Considere usar una de las máquinas virtuales de menor tamaño.
   * Si planea desarrollar la imagen en Azure, considere usar uno de los tamaños de máquina virtual recomendados para la imagen seleccionada.
   * Para obtener información de precios, consulte el selector de **plan de tarifa recomendada** que se muestra en el portal. Se mostrarán los tres tamaños recomendados proporcionados por el publicador. (En este caso, el publicador es Microsoft).

   ![Hoja Tamaño de la nueva máquina virtual](./media/publishvm_015.png)

7. En la hoja **Configuración**, establezca la opción **Usar discos administrados** en **No**.  Esto permite administrar el nuevo disco duro virtual de forma manual. (La hoja **Configuración** también permite realizar otro cambio en las opciones de red almacenamiento, por ejemplo, seleccionar **Premium (SSD)** en **Tipo de disco**).  Haga clic en **Aceptar** para continuar.

    ![Hoja Configuración de la nueva máquina virtual](./media/publishvm_016.png)

8. Haga clic en **Resumen** para revisar sus opciones. Cuando reciba el mensaje **Validación superada**, haga clic en **Aceptar**.

    ![Hoja Resumen de la nueva máquina virtual](./media/publishvm_017.png)

Azure comienza el aprovisionamiento de la máquina virtual especificada.  Puede seguir su progreso si hace clic en la pestaña **Máquinas virtuales** de la izquierda.  Una vez creada, el estado cambiará a **Running** (En ejecución).  En ese momento, puede [conectarse a la máquina virtual](./cpp-connect-vm.md).


## <a name="next-steps"></a>Pasos siguientes

Si tiene dificultades para crear el disco duro virtual basado en Azure, vea [Common issues during VHD creation](./cpp-common-vhd-creation-issues.md) (Problemas comunes durante la creación de discos duros virtuales).  En caso contrario, a continuación tendrá que [conectarse a las máquinas virtuales](./cpp-connect-vm.md) que ha creado en Azure. 
