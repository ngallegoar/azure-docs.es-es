---
author: cynthn
ms.author: cynthn
ms.date: 01/23/2020
ms.topic: include
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: 658910dc4291375c7b2ab22e88c599b970b885af
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80419093"
---
Las imágenes estandarizadas de máquinas virtuales permiten a las organizaciones migrar a la nube y garantizar la coherencia de las implementaciones. Normalmente, las imágenes incluyen opciones de seguridad y de configuración predefinidas y el software necesario. La configuración de su propia canalización de creación de imágenes requiere tiempo, una infraestructura y el programa de instalación, pero con Image Builder de máquina virtual de Azure, basta con que proporcione una configuración sencilla que describa la imagen y la envíe al servicio para que se cree y se distribuya.
 
Image Builder de máquina virtual de Azure (Azure Image Builder) le permite empezar con una imagen de Azure Marketplace basada en Windows o Linux, imágenes personalizadas existentes o ISO de Red Hat Enterprise Linux (RHEL) y empezar a agregar sus propias personalizaciones. Image Builder se compila en [HashiCorp Packer](https://packer.io/), por lo que puede importar incluso los scripts del aprovisionador de shell de Packer existentes. También puede especificar dónde quiere que se hospeden sus imágenes, en la [Azure Shared Image Gallery](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries), como una imagen administrada o un VHD.

> [!IMPORTANT]
> Actualmente, el generador de imágenes de Azure se encuentra en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="preview-features"></a>Características en vista previa

En la versión preliminar se admiten estas características:

- Creación de imágenes de línea de base para comercializar, que incluyen las configuraciones corporativas y de seguridad mínimas y permiten que los departamentos puedan personalizarlas aún más para sus necesidades.
- Aplicación de revisiones de imágenes existentes. Image Builder permitirá revisar continuamente las imágenes personalizadas existentes.
- Conecte el generador de imágenes a las redes virtuales existentes para que pueda conectarse a los servidores de configuración existentes (DSC, Chef, Puppet, etc.), los recursos compartidos de archivos o cualquier otro servidor o servicio enrutable.
- Integración con Azure Shared Image Gallery, que permite distribuir, crear versiones y escalar imágenes globalmente y ofrece un sistema de administración de imágenes.
- Integración con las canalizaciones de compilación de imágenes existentes. Simplemente llame a Image Builder desde la canalización o use la tarea sencilla de Azure DevOps de versión preliminar de Image Builder.
- Migración de una canalización de personalización de imagen existente a Azure. Use los scripts, comandos y procesos existentes para personalizar las imágenes.
- Creación de imágenes en formato VHD.
 

## <a name="regions"></a>Regions
El servicio Azure Image Builder estará disponible en versión preliminar en estas regiones. Las imágenes se pueden distribuir fuera de estas regiones.
- Este de EE. UU.
- Este de EE. UU. 2
- Centro-Oeste de EE. UU.
- Oeste de EE. UU.
- Oeste de EE. UU. 2
- Norte de Europa
- Oeste de Europa

## <a name="os-support"></a>SO compatible
AIB será compatible con imágenes del sistema operativo base de Azure Marketplace:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7.6, 7.7
- CentOS 7.6, 7.7
- SLES 12 SP4
- SLES 15, SLES 15 SP1
- Windows 10 RS5 Enterprise/Enterprise multisesión/Professional
- Windows 2016
- Windows 2019

La compatibilidad con archivos ISO de RHEL está en desuso. Consulte la documentación de la plantilla para obtener más detalles.

## <a name="how-it-works"></a>Funcionamiento


![Dibujo conceptual de Azure Image Builder](./media/virtual-machines-image-builder-overview/image-builder.png)

Azure Image Builder es un servicio de Azure totalmente administrado al que se accede a través de un proveedor de recursos de Azure. El proceso Azure Image Builder tiene tres partes principales: origen, personalización y distribución, representadas en una plantilla. El diagrama siguiente muestra los componentes, con algunas de sus propiedades. 
 


**Proceso de Image Builder** 

![Dibujo conceptual del proceso de Azure Image Builder](./media/virtual-machines-image-builder-overview/image-builder-process.png)

1. Cree la plantilla de imagen como un archivo .json. Este archivo .json contiene información sobre el origen, las personalizaciones y la distribución de la imagen. Hay varios ejemplos en el [repositorio de GitHub de Azure Image Builder](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).
1. Al enviarla al servicio, se creará un artefacto de plantilla de imagen en el grupo de recursos que especifique. En segundo plano, Image Builder descargará la imagen de origen o ISO y los scripts, según sea necesario. Estos se almacenan en un grupo de recursos independiente que se crea automáticamente en la suscripción, en el formato siguiente: IT_\<DestinationResourceGroup>_\<TemplateName>. 
1. Una vez creada la plantilla de imagen, podrá compilar la imagen. En segundo plano, Image Builder usa la plantilla y los archivos de código fuente para crear una máquina virtual (tamaño predeterminado: Standard_D1_v2), red, dirección IP pública, NSG y almacenamiento en el grupo de recursos IT_\<DestinationResourceGroup>_\<NombreDePlantilla>.
1. Como parte de la creación de la imagen, Image Builder distribuye la imagen según la plantilla y luego elimina los recursos adicionales en el grupo de recursos IT_\<DestinationResourceGroup > _\<TemplateName > que se ha creado para el proceso.


## <a name="permissions"></a>Permisos

Para permitir que Azure VM Image Builder distribuya las imágenes a las imágenes administradas o a Shared Image Gallery, deberá proporcionar permisos de "colaborador" para el servicio "Azure Virtual Machine Image Builder" (Id. de aplicación: cf32a0cc-373c-47c9-9156-0db11f6a6dfc ) en los grupos de recursos. 

Si usa una imagen administrada personalizada existente o una versión de la imagen, Azure Image Builder necesitará como mínimo un acceso de "lector" a esos grupos de recursos.

Puede asignar accesos con la CLI de Azure:

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName>
```

Puede asignar el acceso con PowerShell:

```azurePowerShell-interactive
New-AzRoleAssignment -ObjectId ef511139-6170-438e-a6e1-763dc31bdf74 -Scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName> -RoleDefinitionName Contributor
```


Si no se encuentra la cuenta de servicio, puede que la suscripción en la que va a agregar la asignación de roles aún no se haya registrado para el proveedor de recursos.


## <a name="costs"></a>Costos
Se incurrirá en algunos costos de procesos, redes y almacenamiento al crear, compilar y almacenar las imágenes con Azure Image Builder. Estos costos son similares a los que conlleva la creación manual de imágenes personalizadas. En el caso de los recursos, se le cargarán las tarifas que tenga en Azure. 

Durante el proceso de creación de imagen, los archivos se descargan y se almacenan en el grupo de recursos de `IT_<DestinationResourceGroup>_<TemplateName>`, que incurrirá en costos menores de almacenamiento. Si no quiere conservarlos, elimine la **plantilla de imagen** después de la compilación de la imagen.
 
Image Builder crea una máquina virtual con un tamaño D1v2 y el almacenamiento y redes que necesita. Estos recursos estarán en vigor durante el proceso de compilación y se eliminarán una vez que Image Builder haya terminado de crear la imagen. 
 
Azure Image Builder distribuirá la imagen a las regiones elegidas, lo que podría suponer cargos de salida de red.
 
## <a name="next-steps"></a>Pasos siguientes 
 
Para probar Azure Image Builder, consulte los artículos para la compilación de imágenes de [Linux](../articles/virtual-machines/linux/image-builder.md) o [Windows](../articles/virtual-machines/windows/image-builder.md).
 
 
