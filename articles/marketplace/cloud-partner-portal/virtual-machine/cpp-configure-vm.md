---
title: Configurar la VM hospedada en Microsoft Azure para Azure Marketplace
description: Se explica cómo cambiar el tamaño, actualizar y generalizar una máquina virtual hospedada en Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: f20beced7d977668d12c06375ceb8a2554c6d335
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273433"
---
# <a name="configure-the-azure-hosted-vm"></a>Configurar la máquina virtual hospedada en Azure

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://aka.ms/AzureVMTechAsset) para administrar las ofertas migradas.

En este artículo se explica cómo cambiar el tamaño, actualizar y generalizar una máquina virtual (VM) hospedada en Azure.  Estos pasos son necesarios para preparar la máquina virtual para su implementación desde Azure Marketplace.

## <a name="sizing-the-vhds"></a>Cambio de tamaño de los VHD

<!--TD: Check if the following assertion is true. I didn't understand the original content. -->
Si ha seleccionado una de las VM preconfiguradas con un sistema operativo (y, opcionalmente, otros servicios), ya ha seleccionado un tamaño de VM de Azure estándar, como se explica en [Pestaña SKU de máquina virtual](./cpp-skus-tab.md).  Iniciar la solución con un sistema operativo configurado previamente es el enfoque recomendado.  Pero si va a instalar un sistema operativo manualmente, debe ajustar el tamaño del disco duro virtual principal en la imagen de máquina virtual:

- En Windows, el VHD del sistema operativo debe crearse como VHD de 127-128 GB con formato fijo. 
- En Linux, este VHD debe crearse como VHD de 30-50 GB con formato fijo.

Si el tamaño físico es inferior a 127-128 GB, el VHD debe ser disperso. Las imágenes base de Windows y SQL Server proporcionadas cumplen ya estos requisitos, por lo que no es necesario que cambie el formato ni el tamaño del VHD obtenido. 

Los discos de datos pueden tener un tamaño de hasta 1 TB. A la hora de decidir el tamaño, tenga en cuenta que los clientes no pueden cambiar el tamaño de los VHD de una imagen en el momento de la implementación. Los VHD de discos de datos deben crearse como VHD con formato fijo. También deben ser dispersos. Los discos de datos inicialmente pueden estar vacíos o contener datos.


## <a name="install-the-most-current-updates"></a>Instalar las actualizaciones más recientes

Las imágenes base de máquinas virtuales de sistema operativo contienen las actualizaciones más recientes hasta su fecha de publicación. Antes de publicar el VHD de sistema operativo que ha creado, asegúrese de actualizar el sistema operativo y todos los servicios instalados con todas las revisiones de seguridad y mantenimiento más recientes.

En Windows Server 2016, ejecute el comando **Buscar actualizaciones**.  En versiones anteriores de Windows, vea [Obtener una actualización a través de Windows Update](https://support.microsoft.com/help/3067639/how-to-get-an-update-through-windows-update).  Windows Update instala automáticamente las actualizaciones de seguridad críticas e importantes más recientes.

En distribuciones Linux, las actualizaciones se suelen descargar e instalar mediante una herramienta de línea de comandos o una utilidad gráfica.  Por ejemplo, Ubuntu Linux proporciona el comando [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) y la herramienta [Update Manager](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) para actualizar el sistema operativo.


## <a name="perform-additional-security-checks"></a>Realizar comprobaciones de seguridad adicionales

Debe mantener un alto nivel de seguridad para las imágenes de la solución en Azure Marketplace.  En el siguiente artículo se proporciona una lista de comprobación de las configuraciones y los procedimientos de seguridad para ayudarle a lograrlo: [Recomendaciones de seguridad para imágenes de Azure Marketplace](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images).  Algunas de estas recomendaciones son específicas de imágenes basadas en Linux, pero la mayoría se aplica a cualquier imagen de máquina virtual. 


## <a name="perform-custom-configuration-and-scheduled-tasks"></a>Realizar una configuración personalizada y tareas programadas

Si se necesita alguna configuración adicional, el método recomendado es usar una tarea programada que se ejecute al inicio para realizar posibles cambios finales en la máquina virtual una vez implementada.  Tenga en cuenta además las siguientes recomendaciones:
- Si es una tarea de una ejecución, se recomienda que se elimine a sí misma una vez que se realice correctamente.
- Las configuraciones no deben depender de unidades que no sean C o D, ya que son las dos únicas unidades cuya existencia siempre está garantizada. La unidad C es el disco del sistema operativo y unidad D es el disco local temporal.

Para obtener más información sobre las personalizaciones de Linux, vea [Características y extensiones de las máquinas virtuales para Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux).


## <a name="generalize-the-image"></a>Generalizar la imagen

Todas las imágenes de Azure Marketplace deben ser reutilizables de forma genérica. Para lograrlo, el VHD de sistema operativo debe estar *generalizado*, una operación que quita todos los controladores de software y los identificadores específicos de la instancia de una máquina virtual.

### <a name="windows"></a>Windows

Los discos de sistema operativo Windows se generalizan con la [herramienta sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview). Si posteriormente actualiza o vuelve a configurar el sistema operativo, debe volver a ejecutar sysprep. 

> [!WARNING]
>  Dado que las actualizaciones se pueden ejecutar automáticamente, una vez que ejecute sysprep, debe desactivar la máquina virtual hasta que se implemente.  Este apagado evita que las actualizaciones posteriores realicen cambios específicos de la instancia en el sistema operativo del disco duro virtual o los servicios instalados.

Para obtener más información sobre la ejecución de sysprep, consulte [Pasos para generalizar un disco duro virtual](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep).

### <a name="linux"></a>Linux

El siguiente proceso de dos estapas generaliza una máquina virtual Linux y la vuelve a implementar como una máquina virtual independiente. Estas dos estapas son solo los aspectos básicos del proceso. Para más información sobre estas dos estapas y por qué se deben llevar a cabo, consulte [Creación de una imagen de una máquina virtual o un disco duro virtual](../../../virtual-machines/linux/capture-image.md). Con el fin de crear el disco duro virtual para la oferta de Azure Marketplace, puede detenerse cuando llegue a la sección "Creación de una máquina virtual desde la imagen capturada".

#### <a name="remove-the-azure-linux-agent"></a>Quitar el agente de Linux de Azure
1.  Conexión a una máquina virtual Linux de Azure mediante un cliente SSH.
2.  En la ventana de SSH, escriba el siguiente comando: <br/>
    `sudo waagent -deprovision+user`
3.  Escriba `y` para continuar. (Puede agregar el parámetro `-force` al comando anterior para evitar este paso de confirmación).
4.  Una vez completado el comando, escriba `exit` para cerrar el cliente de SSH.

<!-- TD: I need to add meat and/or references to the following steps -->
#### <a name="capture-the-image"></a>Capturar la imagen
1.  Vaya a Azure Portal, seleccione el grupo de recursos (RG) y anule la asignación de la máquina virtual.
2.  El disco duro virtual se ha generalizado y puede crear una nueva máquina virtual mediante este disco duro virtual.


## <a name="create-one-or-more-copies"></a>Crear una o más copias

Crear copias de máquina virtual suele resultar útil para las copias de seguridad, las pruebas, la conmutación por error personalizada o el equilibrio de carga, para ofrecer distintas configuraciones de una solución, etc. Para obtener información sobre cómo duplicar y descargar un VHD principal, para crear un clon no administrado, vea:

- VM Linux: [Descarga de un VHD de Linux desde Azure](../../../virtual-machines/linux/download-vhd.md)
- VM Windows: [Descargar un VHD de Windows desde Azure](../../../virtual-machines/windows/download-vhd.md)


## <a name="next-steps"></a>Pasos siguientes

Una vez que la máquina virtual está generalizada, se ha desasignado y ha creado una imagen de ella, está preparado para [implementar una máquina virtual desde un disco duro virtual](./cpp-deploy-vm-vhd.md).
