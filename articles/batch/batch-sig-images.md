---
title: 'Uso de Shared Image Gallery para crear un grupo personalizado: Azure Batch | Microsoft Docs'
description: Cree un grupo de Batch con Shared Image Gallery para aprovisionar imágenes personalizadas para los nodos de ejecución que contienen el software y los datos que se necesitan para la aplicación. Las imágenes personalizadas son una manera eficaz de configurar los nodos de proceso para ejecutar las cargas de Batch.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 08/28/2019
ms.author: labrenne
ms.openlocfilehash: 95f27d913cd288c186bae1a6375212b072f50bb4
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422431"
---
# <a name="use-the-shared-image-gallery-to-create-a-custom-pool"></a>Uso de Shared Image Gallery para crear un grupo personalizado

Al crear un grupo en Azure Batch con Configuración de máquina virtual, se especifica una imagen de máquina virtual (VM) que proporciona el sistema operativo para cada nodo de proceso en el grupo. Puede crear un grupo de máquinas virtuales con una imagen de Azure Marketplace compatible o crear una imagen personalizada con [Shared Image Gallery](../virtual-machines/windows/shared-image-galleries.md).

## <a name="benefits-of-the-shared-image-gallery"></a>Ventajas de Shared Image Gallery

Al usar Shared Image Gallery para la imagen personalizada, tiene el control sobre la configuración y el tipo del sistema operativo, así como el tipo de discos de datos. La imagen de Shared Image puede incluir los datos de referencia y las aplicaciones que están disponibles en los nodo de grupo de Batch en cuanto se aprovisionan.

También puede tener varias versiones de una imagen según sea necesario para su entorno. Cuando se usa una versión de la imagen para crear una VM, la versión de la imagen se usa para crear nuevos discos para la VM.

El uso de una imagen de Shared Image le ahorra tiempo a la hora de preparar los nodos de ejecución del grupo para ejecutar la carga de trabajo de Batch. Se puede usar una imagen de Azure Marketplace e instalar el software en cada nodo de ejecución una vez que se haya aprovisionado, pero el uso de una imagen de Shared Image podría ser más eficiente. Además, puede especificar varias réplicas para la imagen de Shared Image, por lo que al crear grupos con muchas VM (más de 600), ahorrará tiempo en la creación del grupo.

El uso de una imagen de Shared Image configurada para su escenario puede proporcionar varias ventajas:

* **Uso de las mismas imágenes en todas las regiones.** Puede crear réplicas de imágenes de Shared Image en distintas regiones para que todos los grupos usen la misma imagen.
* **Configuración del sistema operativo (SO).** La configuración del disco de sistema operativo de la imagen se puede personalizar.
* **Preinstalar las aplicaciones.** Preinstalar las aplicaciones en el disco del sistema operativo es más eficaz y menos propenso a errores que instalar las aplicaciones después de aprovisionar los nodos de ejecución con una tarea de inicio.
* **Copia de grandes cantidades de datos una vez.** Convierta en estáticos parte de los datos de la imagen de Shared Image administrada copiándolos en los discos de datos de una imagen administrada. Solo debe hacerse una vez y los datos estarán disponibles para cada nodo del grupo.
* **Aumento de los grupos a tamaños más grandes.** Con Shared Image Gallery puede crear grupos más grandes con sus imágenes personalizadas junto con más réplicas de Shared Image.
* **Mejor rendimiento que la imagen personalizada.** Mediante el uso de imágenes de Shared Image, el tiempo que tarda el grupo en alcanzar el estado estable es hasta un 25 % más rápido y la latencia de inactividad de la VM es hasta un 30 % más breve.
* **Control de versiones y agrupación de imágenes para facilitar su administración.** La definición de la agrupación de imágenes contiene información acerca de por qué se creó la imagen, para qué sistema operativo sirve e información acerca del uso de la imagen. La agrupación de imágenes permite una administración más sencilla de las imágenes. Para más información, consulte [Definiciones de imagen](../virtual-machines/windows/shared-image-galleries.md#image-definitions).

## <a name="prerequisites"></a>Prerrequisitos

> [!NOTE]
> Debe autenticarse mediante Azure AD. Si usa shared-key-auth, obtendrá un error de autenticación.  

* **Una cuenta de Azure Batch** Para crear una cuenta de Batch, consulte las guías de inicio rápido de Batch con [Azure Portal](quick-create-portal.md) o la [CLI de Azure](quick-create-cli.md).

* **Una imagen de Shared Image Gallery**. Para crear una imagen de Shared Image Gallery, debe tener o crear un recurso de imagen administrada. La imagen debe crearse desde instantáneas del disco del sistema operativo de la máquina virtual y, opcionalmente, de sus discos de datos conectados. Para más información, consulte [Preparación de una imagen administrada](#prepare-a-managed-image).

> [!NOTE]
> La imagen de Shared Image debe estar en la misma suscripción que la cuenta de Batch. La imagen de Shared Image puede estar en distintas regiones, siempre que tenga réplicas en la misma región que la cuenta de Batch.

## <a name="prepare-a-managed-image"></a>Preparación de una imagen administrada

En Azure, puede preparar una imagen administrada a partir de:

* Instantáneas del sistema operativo y los discos de datos de una máquina virtual de Azure
* Una máquina virtual de Azure generalizada con discos administrados
* Un disco duro virtual local generalizado cargado en la nube

Para escalar grupos de Batch de forma confiable con una imagen personalizada se recomienda crear una imagen administrada *solo* con el primer método: instantáneas de los discos de la máquina virtual. Consulte los siguientes pasos para preparar la máquina virtual, tomar una instantánea y crear una imagen a partir de ella.

### <a name="prepare-a-vm"></a>Preparación de la máquina virtual

Si va a crear una máquina virtual para la imagen, use una imagen propia de Azure Marketplace admitida por Batch como imagen base para la imagen administrada. Solo pueden usarse imágenes propias como imagen base. Para obtener una lista completa de las referencias de imagen de Azure Marketplace compatibles con Azure Batch, consulte la operación [List node agent SKUs](/java/api/com.microsoft.azure.batch.protocol.accounts.listnodeagentskus).

> [!NOTE]
> No se puede usar una imagen de terceros que tenga licencias adicionales y términos de compra como imagen base. Para información sobre estas imágenes de Marketplace, consulte las instrucciones para las máquinas virtuales [Linux](../virtual-machines/linux/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) o [Windows](../virtual-machines/windows/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
).

* Asegúrese de que la máquina virtual se crea con un disco administrado. Se trata de la configuración de almacenamiento predeterminada cuando se crea una máquina virtual.
* No instale extensiones de Azure, como la extensión de script personalizado, en la máquina virtual. Si la imagen contiene una extensión preinstalada, Azure podría experimentar problemas al implementar el grupo de Batch.
* Cuando usa los discos de datos incluidos, debe montar y dar formato a los discos desde una máquina virtual para usarlos.
* Asegúrese de que la imagen del sistema operativo base que proporcione usa la unidad temporal predeterminada. El agente de nodo de Batch actualmente espera la unidad temporal predeterminada.
* Una vez que la máquina virtual está en ejecución, conéctese a ella a través de RDP (para Windows) o SSH (para Linux). Instale el software necesario o copie los datos deseados.  

### <a name="create-a-vm-snapshot"></a>Creación de una instantánea de máquina virtual

Una instantánea es una copia completa de solo lectura de un disco duro virtual. Para crear una instantánea del sistema operativo la máquina virtual o discos de datos, puede usar Azure Portal o herramientas de la línea de comandos. Para pasos y opciones de creación de una instantánea, consulte la guía para máquinas virtuales [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) o [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md).

### <a name="create-an-image-from-one-or-more-snapshots"></a>Creación de una imagen a partir de una o varias instantáneas

Para crear una imagen administrada a partir de una instantánea, use las herramientas de la línea de comandos de Azure, como el comando [az image create](/cli/azure/image). Cree una imagen mediante la especificación de una instantánea del disco del sistema operativo y, opcionalmente, una o varias instantáneas de disco de datos.

### <a name="create-a-shared-image-gallery"></a>Creación de una instancia de Shared Image Gallery

Una vez que haya creado correctamente la imagen administrada, debe crear una instancia de Shared Image Gallery para que la imagen personalizada esté disponible. Para más información acerca de cómo crear una instancia de Shared Image Gallery para sus imágenes, consulte [Creación de una imagen de Shared Image Gallery con la CLI de Azure](../virtual-machines/linux/shared-images.md) o [Creación de una imagen de Shared Image Gallery mediante Azure Portal](../virtual-machines/linux/shared-images-portal.md).

## <a name="create-a-pool-from-a-shared-image-using-the-azure-cli"></a>Creación de un grupo a partir de una imagen de Shared Image con la CLI de Azure

Para crear un grupo a partir de la imagen de Shared Image mediante la CLI de Azure, use el comando `az batch pool create`. Especifique el identificador de la imagen de Shared Image en el campo `--image`. Asegúrese de que el tipo de sistema operativo y la SKU coinciden con las versiones especificadas por `--node-agent-sku-id`.

> [!NOTE]
> Debe autenticarse mediante Azure AD. Si usa shared-key-auth, obtendrá un error de autenticación.  

```azurecli
az batch pool create \
    --id mypool --vm-size Standard_A1_v2 \
    --target-dedicated-nodes 2 \
    --image "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}" \
    --node-agent-sku-id "batch.node.ubuntu 16.04"
```

## <a name="create-a-pool-from-a-shared-image-using-c"></a>Creación de un grupo a partir de una imagen de Shared Image mediante C#

Como alternativa, puede crear un grupo a partir de una imagen de Shared Image mediante el SDK de C#.

```csharp
private static VirtualMachineConfiguration CreateVirtualMachineConfiguration(ImageReference imageReference)
{
    return new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: "batch.node.windows amd64");
}

private static ImageReference CreateImageReference()
{
    return new ImageReference(
        virtualMachineImageId: "/subscriptions/{sub id}/resourceGroups/{resource group name}/providers/Microsoft.Compute/galleries/{gallery name}/images/{image definition name}/versions/{version id}");
}

private static void CreateBatchPool(BatchClient batchClient, VirtualMachineConfiguration vmConfiguration)
{
    try
    {
        CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: PoolId,
            targetDedicatedComputeNodes: PoolNodeCount,
            virtualMachineSize: PoolVMSize,
            virtualMachineConfiguration: vmConfiguration);

        pool.Commit();
    }
    ...
}
```

## <a name="create-a-pool-from-a-shared-image-using-the-azure-portal"></a>Creación de una imagen compartida mediante Azure Portal

Use los pasos siguientes para crear un grupo a partir de una imagen compartida en Azure Portal.

1. Abra [Azure Portal](https://portal.azure.com).
1. Vaya a **Cuentas de Batch** y seleccione su cuenta.
1. Seleccione **Grupos** y, luego, **Agregar** para crear un grupo.
1. En la sección **Tipo de imagen**, seleccione **Shared Image Gallery**.
1. Complete el resto de secciones con información sobre la imagen administrada.
1. Seleccione **Aceptar**.

![Cree un grupo a partir de una imagen compartida con el portal.](media/batch-sig-images/create-custom-pool.png)

## <a name="considerations-for-large-pools"></a>Consideraciones para los grupos grandes

Si tiene previsto crear un grupo con cientos o miles de VM o más mediante una imagen de Shared Image, use la siguiente guía.

* **Números de réplicas de Shared Image Gallery.**  Para cada grupo con hasta 600 instancias, se recomienda conservar al menos una réplica. Por ejemplo, si va a crear un grupo con 3000 VM, debe mantener al menos cinco réplicas de la imagen. Siempre se recomienda mantener más réplicas que las indicadas por los requisitos mínimos para un mejor rendimiento.

* **Tiempo de espera del cambio de tamaño.** Si el grupo contiene un número de nodos fijo (no se escala automáticamente), aumente la propiedad `resizeTimeout` del grupo en función de su tamaño. Por cada 1000 VM, el tiempo de espera del cambio de tamaño recomendado es de al menos 15 minutos. Por ejemplo, el tiempo de espera del cambio de tamaño recomendado para un grupo con 2000 VM es de al menos 30 minutos.

## <a name="next-steps"></a>Pasos siguientes

* Para información general más detallada acerca de Batch, consulte [Desarrollo de soluciones de procesos paralelos a gran escala con Batch](batch-api-basics.md).
