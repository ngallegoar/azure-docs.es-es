---
title: Funcionalidades de representación
description: Las funcionalidades estándar de Azure Batch se utilizan para ejecutar aplicaciones y cargas de trabajo de representación. Batch incluye características específicas para admitir cargas de trabajo de representación.
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 3efe1dfa69de5ce41aed2152baa88b313fd928f1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115761"
---
# <a name="azure-batch-rendering-capabilities"></a>Funcionalidades de representación de Azure Batch

Las funcionalidades estándar de Azure Batch se utilizan para ejecutar aplicaciones y cargas de trabajo de representación. Batch también incluye características específicas para admitir cargas de trabajo de representación.

Para obtener información general sobre los conceptos de Batch, incluidos grupos, trabajos y tareas, consulte [este artículo](https://docs.microsoft.com/azure/batch/batch-api-basics).

## <a name="batch-pools"></a>Grupos de Batch

### <a name="rendering-application-installation"></a>Instalación de la aplicación de representación

Una imagen de VM de representación de Azure Marketplace puede especificarse en la configuración del grupo si solo deben usarse las aplicaciones instaladas previamente.

Hay una imagen de Windows 2016 y una imagen de CentOS.  En [Azure Marketplace](https://azuremarketplace.microsoft.com), las imágenes de VM pueden encontrarse buscando "representación por lotes".

Para obtener un ejemplo de configuración de grupo, consulte el [tutorial de representación de la CLI de Azure](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli).  Azure Portal y Batch Explorer proporcionan herramientas de GUI para seleccionar una imagen de VM de representación al crear un grupo.  Si usa una API de Batch, especifique los siguientes valores de propiedad para [ImageReference](https://docs.microsoft.com/rest/api/batchservice/pool/add#imagereference) al crear un grupo:

| Publicador | Oferta | SKU | Versión |
|---------|---------|---------|--------|
| proceso por lotes | rendering-centos73 | rendering | latest |
| proceso por lotes | rendering-windows2016 | rendering | latest |

Hay otras opciones disponibles si se requieren aplicaciones adicionales en las VM del grupo:

* Una imagen personalizada de Shared Image Gallery:
  * Con esta opción, puede configurar la máquina virtual con las aplicaciones exactas y las versiones específicas que necesite. Para más información, consulte [Creación de un grupo con Shared Image Gallery](batch-sig-images.md). Autodesk y Chaos Group han modificado Arnold y V-Ray, respectivamente, para la validación frente a un servicio de licencias de Azure Batch. Asegúrese de que tiene las versiones compatibles de estas aplicaciones, de lo contrario, la licencia de pago por uso no funcionará. Las versiones actuales de Maya o 3ds Max no requieren un servidor de licencias con la ejecución desatendida (en modo de lote/línea de comandos). Póngase en contacto con el soporte técnico de Azure si no está seguro de cómo continuar con esta opción.
* [Paquetes de aplicación](https://docs.microsoft.com/azure/batch/batch-application-packages):
  * Empaquete los archivos de aplicación en uno o varios archivos ZIP, cargue desde Azure Portal y especifique el paquete en la configuración del grupo. Cuando se crean VM del grupo, los archivos ZIP se descargan y se extraen los archivos.
* Archivos de recursos:
  * Los archivos de la aplicación se cargan en el almacenamiento de blobs de Azure, y especifica las referencias de archivo en la [tarea de inicio del grupo](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask). Cuando se crean VM del grupo, se descargan los archivos de recursos a cada VM.

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>Licencias de pago por uso para las aplicaciones instaladas previamente

Las aplicaciones que se usarán y que tienen una tarifa de licencia deben especificarse en la configuración del grupo.

* Especifique la propiedad `applicationLicenses` al [crear un grupo](https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body).  En la matriz de cadenas ("vray", "arnold", "3dsmax", "maya") se pueden especificar los valores siguientes.
* Cuando se especifican una o varias aplicaciones, el costo de esas aplicaciones se agrega al costo de las VM.  Los precios de las aplicaciones se muestran en la [página de precios de Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

> [!NOTE]
> Si, en cambio, se conecta a un servidor de licencias para usar las aplicaciones de representación, no especifique la propiedad `applicationLicenses`.

Puede usar Azure Portal o Batch Explorer para seleccionar las aplicaciones y mostrar los precios.

Si se intenta usar una aplicación, pero no se ha especificado la aplicación en la propiedad `applicationLicenses` de la configuración del grupo o no se puede conectar con un servidor de licencias, no se podrá ejecutar la aplicación debido a un error de licencias y el código de salida distinto de cero.

### <a name="environment-variables-for-pre-installed-applications"></a>Variables de entorno para aplicaciones instaladas previamente

Para poder crear la línea de comandos para las tareas de representación, se debe especificar la ubicación de instalación de los archivos ejecutables de aplicación de representación.  Se han creado las variables de entorno del sistema en las imágenes de VM de Azure Marketplace, que pueden usarse en lugar de tener que especificar las rutas de acceso reales.  Estas variables de entorno son complementarias a las [variables de entorno de Batch estándares](https://docs.microsoft.com/azure/batch/batch-compute-node-environment-variables) creadas para cada tarea.

|Application|Archivo ejecutable de aplicación|Variable de entorno|
|---------|---------|---------|
|Autodesk 3ds Max 2018|3dsmaxcmdio.exe|3DSMAX_2018_EXEC|
|Autodesk 3ds Max 2019|3dsmaxcmdio.exe|3DSMAX_2019_EXEC|
|Autodesk Maya 2017|render.exe|MAYA_2017_EXEC|
|Autodesk Maya 2018|render.exe|MAYA_2018_EXEC|
|Chaos Group V-Ray Standalone|vray.exe|VRAY_3.60.4_EXEC|
Arnold 2017 command line|kick.exe|ARNOLD_2017_EXEC|
|Arnold 2018 command line|kick.exe|ARNOLD_2018_EXEC|
|Blender|blender.exe|BLENDER_2018_EXEC|

### <a name="azure-vm-families"></a>Familias de VM de Azure

Al igual que con otras cargas de trabajo, los requisitos del sistema de las aplicaciones de representación varían, y los requisitos de rendimiento varían para los proyectos y los trabajos.  En Azure hay disponible una gran variedad de familias de VM según sus requisitos: menor costo, mejor relación precio/rendimiento, mejor rendimiento, etc.
Algunas aplicaciones de representación, como Arnold, se basan en CPU; otras, como V-Ray y Blender Cycles, pueden utilizar CPU o GPU.
Para obtener una descripción de las familias de VM y los tamaños de VM disponibles, [consulte los tamaños y tipos de VM](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

### <a name="low-priority-vms"></a>VM de prioridad baja

Al igual que con otras cargas de trabajo, se pueden utilizar VM de prioridad baja en grupos de Batch para la representación.  Las VM de prioridad baja tienen un rendimiento similar a las VM dedicadas normales, pero utilizan la capacidad sobrante de Azure y están disponibles con un gran descuento.  El inconveniente del uso de máquinas virtuales de prioridad baja es que esas máquinas virtuales pueden no estar disponibles para su asignación o pueden reemplazarse en cualquier momento, según la capacidad disponible. Por este motivo, las VM de prioridad baja no son adecuadas para todos los trabajos de representación. Por ejemplo, si las imágenes tardan muchas horas en representarse, es probable que no sea aceptable interrumpir y reiniciar la representación de esas imágenes debido al reemplazo por baja prioridad de las VM.

Para obtener más información sobre las características de las VM de prioridad baja y las diversas maneras de configurarlas con Batch, consulte [Uso de máquinas virtuales de prioridad baja con Batch](https://docs.microsoft.com/azure/batch/batch-low-pri-vms).

## <a name="jobs-and-tasks"></a>Trabajos y tareas

No se requiere compatibilidad específica de representación para los trabajos y tareas.  El elemento de configuración principal es la línea de comandos de tarea, que debe hacer referencia a la aplicación necesaria.
Cuando se usan las imágenes de VM de Azure Marketplace, la práctica recomendada es utilizar las variables de entorno para especificar la ruta de acceso y el archivo ejecutable de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Para ver ejemplos de representación de Batch consulte los dos tutoriales siguientes:

* [Representación mediante la CLI de Azure](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Representación mediante Batch Explorer](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
