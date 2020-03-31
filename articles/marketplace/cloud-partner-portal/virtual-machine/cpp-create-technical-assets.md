---
title: Creación de recursos técnicos para una oferta de máquina virtual para Azure Marketplace
description: Se explica cómo crear los recursos técnicos para una oferta de máquina virtual en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: dsindona
ms.openlocfilehash: 57f56a341cfc3db6a5f0664503809e6ab6cf3d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278031"
---
# <a name="create-technical-assets-for-a-virtual-machine-offer"></a>Creación de recursos técnicos para una oferta de máquina virtual

Esta sección le guía a través de la creación y configuración de los recursos técnicos para una oferta de máquina virtual (VM) para Azure Marketplace.  Una máquina virtual contiene dos componentes: el disco duro virtual (VHD) de la solución y los discos de datos asociados opcionales.  

- *Discos duros virtuales (VHD)* , que contienen el sistema operativo y la solución, que se van a implementar con la oferta de Azure Marketplace. El proceso de preparación del disco duro virtual es diferente en función de si se basa en Linux, en Windows o una máquina virtual personalizada.
- *Los discos de datos* representan el almacenamiento persistente dedicado para una máquina virtual. *No* use el disco duro virtual de la solución (por ejemplo, la unidad `C:`) para almacenar información persistente.

Una imagen de máquina virtual contiene un disco del sistema operativo y cero o más discos de datos. Es necesario un VHD por disco. Incluso los discos de datos vacíos requieren la creación de un VHD.
Debe configurar el sistema operativo de la máquina virtual, el tamaño, los puertos que se deben abrir y hasta 15 discos de datos conectados.

> [!TIP] 
> Independientemente del sistema operativo que use, agregue solo el número mínimo de discos de datos necesarios para la SKU. Los usuarios no pueden quitar los discos que formen parte de una imagen durante la implementación, pero siempre pueden agregar discos durante o después de la implementación. 

> [!IMPORTANT]
> *No cambie el número de discos en una nueva versión de imagen.* Si necesita volver a configurar los discos de datos de la imagen, defina una nueva SKU. Si publica una versión nueva de la imagen con otro número de discos, las nuevas implementaciones que se realicen en función de la versión nueva pueden registrar problemas con el escalado automático, las implementaciones automáticas de soluciones mediante plantillas de Azure Resource Manager y otros escenarios.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="fundamental-technical-knowledge"></a>Conocimientos técnicos básicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta. Además del dominio de la solución, el equipo de ingeniería debe tener conocimientos sobre las siguientes tecnologías de Microsoft: 
-   Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/). 
-   Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
-   Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking).
-   Conocimientos prácticos de [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
-   Conocimientos prácticos de [JSON](https://www.json.org/).


## <a name="suggested-tools"></a>Herramientas sugeridas 

Elija uno o ambos de los entornos de scripting siguientes para ayudar a administrar los discos duros virtuales y las máquinas virtuales:
-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [CLI de Azure](https://docs.microsoft.com/cli/azure)

Además, se recomienda agregar las herramientas siguientes al entorno de desarrollo: 

-   [Explorador de Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Extensión: [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Extensión: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Extensión: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

También se recomienda revisar las herramientas disponibles en la página [Herramientas de desarrollo de Azure](https://azure.microsoft.com/tools/) y, si usa Visual Studio, en [Visual Studio Marketplace](https://marketplace.visualstudio.com/).


## <a name="next-steps"></a>Pasos siguientes

Los artículos siguientes de esta sección le guían por los pasos necesarios para crear y registrar estos recursos de máquina virtual:

1. En [Create an Azure-compatible virtual hard disk](./cpp-create-vhd.md) (Creación de un disco duro virtual compatible con Azure) se explica cómo crear un disco duro virtual basado en Linux o Windows que es compatible con Azure.  Incluye los procedimientos recomendados, como el cambio de tamaño, la aplicación de revisiones y la preparación de la máquina virtual para la carga.

2. En [Connect to the virtual machine](./cpp-connect-vm.md) (Conexión a la máquina virtual) se explica cómo conectarse de forma remota a la máquina virtual recién creada e iniciar sesión en ella.  En este artículo también se explica cómo detener la máquina virtual para ahorrar en costos de uso.

3. En [Configure the virtual machine](./cpp-configure-vm.md) (Configuración de la máquina virtual) se explica cómo elegir el tamaño de disco duro virtual correcto, generalizar la imagen, aplicar las actualizaciones recientes (revisiones) y programar configuraciones personalizadas.

4. En [Deploy a virtual machine from a virtual hard disk](./cpp-deploy-vm-vhd.md) (Implementación de una máquina virtual a partir de un disco duro virtual) se explica cómo registrar una máquina virtual a partir de un disco duro virtual implementado en Azure.  Se enumeran las herramientas necesarias y cómo usarlas para crear una imagen de máquina virtual de usuario que luego se implementa en Azure mediante [Microsoft Azure Portal](https://ms.portal.azure.com/) o scripts de PowerShell. 

5. En [Certify a virtual machine image](./cpp-certify-vm.md) (Certificación de una imagen de máquina virtual) se explica cómo probar y enviar una imagen de máquina virtual para la certificación de Azure Marketplace. Se explica dónde obtener la herramienta *Certification Test Tool for Azure Certified* (Herramienta de prueba de certificación para Azure Certified) y cómo usarla para certificar la imagen de máquina virtual. 

6. En [Get SAS URI](./cpp-get-sas-uri.md) (Obtención del URI de SAS) se explica cómo obtener el URI de firma de acceso compartido (SAS) para las imágenes de máquina virtual.
 
Como artículo complementario, en [Common shared access signature URL issues](./cpp-common-sas-url-issues.md) (Problemas comunes de dirección URL de firma de acceso compartido) se enumeran algunos problemas comunes que pueden surgir con los URI de SAS y las posibles soluciones correspondientes.

Después de haber completado todos estos pasos, estará listo para [publicar la oferta de máquina virtual](./cpp-publish-offer.md) en Azure Marketplace.
