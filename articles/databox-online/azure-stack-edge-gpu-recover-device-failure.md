---
title: Recuperación de un error en un dispositivo de Azure Stack Edge Pro
description: Describe cómo recuperarse de un dispositivo con error de Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 10/06/2020
ms.author: alkohli
ms.openlocfilehash: ef99ff874376e0d76e45e4b0b2c36a78d938130b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843664"
---
# <a name="recover-from-a-failed-azure-stack-edge-pro-gpu-device"></a>Recuperación de un dispositivo GPU de Azure Stack Edge Pro con errores 

En este artículo se describe cómo recuperarse de un error no tolerable en su dispositivo GPU de Azure Stack Edge Pro. Un error no tolerable en un dispositivo GPU de Azure Stack Edge Pro requiere la sustitución del dispositivo.

## <a name="before-you-begin"></a>Antes de empezar

Asegúrese de que tiene:

- Se ha puesto en contacto Soporte técnico de Microsoft con respecto al error del dispositivo y le han recomendado la sustitución del dispositivo. 
- Ha hecho una copia de seguridad de la configuración del dispositivo como se describe en [Preparación para un error del dispositivo](azure-stack-edge-gpu-prepare-device-failure.md).


## <a name="configure-replacement-device"></a>Configuración del dispositivo de sustitución

Cuando el dispositivo detecta un error no tolerable, debe solicitar un dispositivo de sustitución. Los pasos de configuración para el dispositivo de sustitución siguen siendo los mismos. 

Recupere la información de configuración del dispositivo de la que se realizó una copia de seguridad del dispositivo en el que se produjo el error. Usará esta información para configurar el dispositivo de sustitución.  

Siga estos pasos para configurar el dispositivo de sustitución:

1. Recopile la información necesaria que se indica en [Lista de comprobación para implementación](azure-stack-edge-gpu-deploy-checklist.md). Debe usar la información que guardó de la configuración del dispositivo anterior. 
1. Solicite un nuevo dispositivo de la misma configuración que la del equipo en el que se produjo el error.  Para realizar la solicitud, [cree un nuevo recurso de Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md#) en Azure Portal.
1. [Desempaquete el dispositivo](azure-stack-edge-gpu-deploy-install.md#unpack-the-device), [móntelo en un bastidor](azure-stack-edge-gpu-deploy-install.md#rack-the-device) y [cabléelo](azure-stack-edge-gpu-deploy-install.md#cable-the-device). 
1. [Conéctese a la interfaz de usuario local del dispositivo](azure-stack-edge-gpu-deploy-connect.md).
1. Configure la red con las mismas direcciones IP que usó para el dispositivo antiguo. Esto minimiza el impacto en cualquier equipo cliente que se use en el entorno. Consulte cómo [definir la configuración de red](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).
1. Asigne el mismo nombre de dispositivo y dominio DNS que el dispositivo antiguo. Esto garantiza que los clientes pueden usar el mismo nombre de dispositivo para comunicarse con el nuevo dispositivo. Consulte cómo [configurar el dispositivo](azure-stack-edge-gpu-deploy-set-up-device-update-time.md).
1. Configure los certificados sobre en el nuevo dispositivo de la misma manera que lo hizo para el dispositivo antiguo. Tenga en cuenta que el nuevo dispositivo tiene un nuevo número de serie de nodo. Si ha usado sus propios certificados en el dispositivo antiguo, tendrá que obtener un nuevo certificado de nodo. Consulte cómo [configurar certificados](azure-stack-edge-gpu-deploy-configure-certificates.md).
1. Obtenga la clave de activación desde Azure Portal y active el nuevo dispositivo. Consulte cómo [activar el dispositivo](azure-stack-edge-gpu-deploy-activate.md).

Ahora está preparado para implementar las cargas de trabajo que se estaban ejecutando en el dispositivo antiguo.

## <a name="restore-edge-cloud-shares"></a>Restauración de recursos compartidos en la nube de Edge

Siga estos pasos para restaurar los datos en los recursos compartidos en la nube de Edge en el dispositivo:

1. [Agregue recursos compartidos](azure-stack-edge-j-series-manage-shares.md#add-a-share) con los mismos nombres de recurso compartido creados anteriormente en el dispositivo con errores. Asegúrese de que, al crear recursos compartidos, la opción **Seleccionar el contenedor de blobs** está establecida en **Utilizar existente** y luego seleccione el contenedor que se usó con el dispositivo anterior.
1. [Agregue usuarios](azure-stack-edge-j-series-manage-users.md#add-a-user) que tenían acceso al dispositivo anterior.
1. [Agregue cuentas de almacenamiento](azure-stack-edge-j-series-manage-storage-accounts.md#add-an-edge-storage-account) asociadas con los recursos compartidos anteriormente en el dispositivo. Al crear cuentas de almacenamiento de Edge, seleccione de un contenedor existente y apunte al contenedor que se asignó a la cuenta de Azure Storage asignada en el dispositivo anterior. Los datos del dispositivo que estaban escritos en la cuenta de almacenamiento de Edge en el dispositivo anterior se cargaron en el contenedor de almacenamiento seleccionado en la cuenta de Azure Storage asignada.
1. [Actualice los datos del recurso compartido](azure-stack-edge-j-series-manage-shares.md#refresh-shares) desde Azure. Esto desplegaría todos los datos en la nube desde el contenedor existente hasta los recursos compartidos.

## <a name="restore-edge-local-shares"></a>Restauración de recursos compartidos locales de Edge

Para prepararse para un posible error de dispositivo, es posible que haya implementado una de las siguientes soluciones de copia de seguridad para proteger los datos de recursos compartidos locales de sus cargas de trabajo de Kubernetes o IoT:

| Software de terceros           | Referencia a la solución                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Para obtener más información, póngase en contacto con Cohesity.          |
| Veritas                        | Para obtener más información, póngase en contacto con Veritas.   |

Una vez configurado completamente el dispositivo de sustitución, habilítelo para el almacenamiento local. 

Siga estos pasos para recuperar los datos de recursos compartidos locales: 

1. [Configure el proceso en el dispositivo](azure-stack-edge-gpu-deploy-configure-compute.md).
1. Vuelva a [agregar un recurso compartido local](azure-stack-edge-j-series-manage-shares.md#add-a-local-share).
1. Ejecute el procedimiento de recuperación que proporciona la solución de protección de datos que prefiera. Consulte las referencias de la tabla anterior.

## <a name="restore-vm-files-and-folders"></a>Restauración de archivos y carpetas de VM

Para prepararse para un posible error de dispositivo, es posible que haya implementado una de las siguientes soluciones de copia de seguridad para proteger los datos de las máquinas virtuales:



| Soluciones de copia de seguridad        | SO admitido   | Referencia                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| Agente de Microsoft Azure Recovery Services (MARS) para Azure Backup | Windows        | [Acerca del agente de MARS](/azure/backup/backup-azure-about-mars)    |
| Cohesity                | Windows, Linux | [Breve descripción de la solución de integración, copia de seguridad y recuperación Microsoft Azure](https://www.cohesity.com/solution/cloud/azure) <br>Para obtener más información, póngase en contacto con Cohesity.                          |
| Veritas                 | Windows, Linux | Para obtener más información, póngase en contacto con Veritas.                    |

Una vez configurado completamente el dispositivo de sustitución, puede volver a implementar las máquinas virtuales con la imagen de VM usada anteriormente. 

Siga estos pasos para recuperar los datos en las máquinas virtuales:
 
1. [Implemente una VM desde una imagen de VM](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) en el dispositivo. 
1. Instale la solución de protección de datos elegida en la VM.
1. Ejecute el procedimiento de recuperación que proporciona la solución de protección de datos que prefiera. Consulte las referencias de la tabla anterior.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [devolver un dispositivo Azure Stack Edge Pro](azure-stack-edge-return-device.md).
