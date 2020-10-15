---
title: Uso de Data Science Virtual Machines de Azure
description: Aprenda cómo conectarse con una instancia de Azure Data Science Virtual Machine (DSVM) para ampliar la potencia de proceso disponible en la versión preliminar de Azure Notebooks.
author: getroyer
manager: andneil
ms.author: getroyer
ms.topic: how-to
ms.date: 06/13/2019
ms.openlocfilehash: 291f1ac093568f50ad6146b70fa1ef69263b7b5b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85830320"
---
# <a name="use-azure-data-science-virtual-machines"></a>Uso de Data Science Virtual Machines de Azure

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

De forma predeterminada, los proyectos se ejecutan en el nivel **Free Compute** (proceso gratuito), que está limitado a 4 GB de memoria y 1 GB de datos para evitar abusos. Puede omitir estas limitaciones mediante el uso de una máquina virtual diferente que se haya aprovisionado en una suscripción a Azure. Para este fin, la mejor opción es una instancia de Data Science Virtual Machine (DSVM) de Azure mediante la imagen **Data Science Virtual Machine para Linux (Ubuntu)** . Este tipo de instancia de DSVM viene preconfigurada con todo lo necesario para Azure Notebooks y aparece automáticamente en la lista desplegable **Ejecutar** de Azure Notebooks.

> [!Note]
> Azure Notebooks solo se admite en las instancias de DSVM creadas con la imagen de Ubuntu en Linux. Notebooks no se admite en las imágenes de Windows 2012, Windows 2016 o CentOS Linux.

## <a name="create-a-dsvm-instance"></a>Creación de una instancia de DSVM

Para crear una nueva instancia de DSVM, siga las instrucciones de la sección [Create an Ubuntu Data Science VM](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro) (Creación de un entorno de Data Science VM de Ubuntu). Para obtener más información, incluidos los detalles sobre los precios, consulte [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>Conexión a la DSVM

Una vez creada la instancia de DSVM, seleccione la lista desplegable **Ejecutar** en el panel del proyecto de Azure Notebooks y seleccione la instancia de DSVM adecuada. La lista desplegable muestra las instancias de DSVM si las siguientes condiciones son verdaderas:

- Ha iniciado sesión en Azure Notebooks con una cuenta que usa Azure Active Directory (AAD), como una cuenta de empresa.
- La cuenta está conectada a una suscripción a Azure.
- Tiene una o varias máquinas virtuales en esa suscripción, con acceso de lectura por lo menos, que usan la imagen de Data Science Virtual Machine para Linux (Ubuntu).

![Instancias de Data Science Virtual Machine en la lista desplegable del panel del proyecto](media/project-compute-tier-dsvm.png)

Al seleccionar una instancia de DSVM, Azure Notebooks puede solicitar las credenciales de la máquina específica que usó cuando creó la VM.

> [!Important]
> El nombre de usuario debe estar en minúsculas para usarse con JupyterHub.

Si no se cumple alguna de las condiciones, todavía puede conectarse a la instancia de DSVM. En la lista desplegable, seleccione la opción **Direct Compute** (Proceso directo), que le pedirá un nombre (que mostrar en la lista), la dirección IP y puerto (normalmente 8000, el puerto predeterminado al que JupyterHub escucha) de la máquina virtual y sus credenciales:

![Solicitud para recopilar información del servidor para la opción Direct Compute (Proceso directo)](media/project-compute-tier-direct.png)

Puede obtener estos valores en desde la página de DSVM en Azure Portal.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>Acceso a archivos de Azure Notebooks desde DSVM

El acceso al sistema de archivos es compatible con las versiones de DSVM 19.06.15 o posteriores. Para comprobar la versión, primero conéctese a su DSVM a través de SSH y después ejecute el siguiente comando: `curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"` (debe usar la dirección IP exacta que se muestra aquí). El número de versión se muestra en la salida de "version".

Para conservar la paridad de las rutas de acceso de archivo con el nivel **Free Compute**, puede abrir solo un proyecto a la vez en una instancia de DSVM. Para abrir un nuevo proyecto, primero debe cerrar el proyecto abierto.

Cuando un proyecto se ejecuta en una máquina virtual, los archivos se montan en el directorio raíz del servidor de Jupyter (el directorio que se muestra en JupyterHub), de forma que se reemplazan los archivos de Azure Notebooks predeterminados. Cuando apaga la máquina virtual mediante el botón **Apagar** en la interfaz de usuario del cuaderno, Azure Notebooks restaura los archivos predeterminados.

![Botón de apagado en Azure Notebooks](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Creación de nuevos usuarios de DSVM

Si varios usuarios comparten un DSVM, puede evitar que se bloqueen entre sí si crea y usa un usuario de DSVM para cada usuario del cuaderno:

1. En [Azure Portal](https://portal.azure.com), vaya a su máquina virtual.
1. En **Soporte técnico y solución de problemas** en el margen izquierdo, seleccione **Restablecer contraseña**.
1. Escriba un nuevo **nombre de usuario**. El nombre de usuario debe estar en minúsculas para usarse con JupyterHub. Escriba una contraseña. Después, seleccione **Actualizar**. (Los nombres de usuario existentes no resultan afectados).
1. Repita el paso anterior para todos los usuarios adicionales.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre DSVM en [Introducción a Data Science Virtual Machine de Azure](/azure/machine-learning/data-science-virtual-machine/overview).
