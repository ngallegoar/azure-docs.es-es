---
title: 'Tutorial: Preparación de Azure Portal y el entorno del centro de datos para implementar Azure Stack Edge Pro R'
description: El primer tutorial para implementar Azure Stack Edge Pro R trata sobre la preparación de Azure Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/15/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Azure Stack Edge Pro R so I can use it to transfer data to Azure.
ms.openlocfilehash: d992748e9829993acc28795778baae3eb92e88f3
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464772"
---
# <a name="tutorial-prepare-to-deploy-azure-stack-edge-pro-r"></a>Tutorial: Preparación de la implementación de Azure Stack Edge Pro R

Este es el primer tutorial de la serie de tutoriales de implementación necesarios para implementar completamente Azure Stack Edge Pro R. En este tutorial se describe cómo preparar Azure Portal para implementar un recurso de Azure Stack Edge. En el tutorial se usa un dispositivo Azure Stack Edge Pro R de 1 nodo que se distribuye con un sistema de alimentación ininterrumpida (SAI).

Para completar el proceso de instalación y configuración se necesitan privilegios de administrador. La preparación del portal dura menos de 10 minutos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Crear un nuevo recurso
> * Obtención de la clave de activación

### <a name="get-started"></a>Introducción

Para implementar Azure Stack Edge Pro R, consulte los siguientes tutoriales en el orden indicado.

| Para realizar este paso | Use estos documentos |
| --- | --- |
| **Preparación** |Estos pasos se deben completar como preparación para la próxima implementación. |
| **[Lista de comprobación de la configuración de implementación](#deployment-configuration-checklist)** |Use esta lista de comprobación para recopilar y registrar información antes y durante la implementación. |
| **[Requisitos previos de implementación](#prerequisites)** |Validan que el entorno está listo para la implementación. |
|  | |
|**Tutoriales de implementación** |Estos tutoriales son necesarios para implementar el dispositivo Azure Stack Edge Pro R en producción. |
|**[1. Preparación de Azure Portal para el dispositivo](azure-stack-edge-pro-r-deploy-prep.md)** |Cree y configure el recurso de Azure Stack Edge antes de instalar un dispositivo físico de Azure Stack Box Edge. |
|**[2. Instalación del dispositivo](azure-stack-edge-pro-r-deploy-install.md)**|Desempaquete, coloque en el rack y conecte los cables del dispositivo físico.  |
|**[3. Conexión del dispositivo](azure-stack-edge-pro-r-deploy-connect.md)** |Una vez instalado el dispositivo, conéctese a la interfaz de usuario web local del dispositivo.  |
|**[4. Configuración de la red](azure-stack-edge-pro-r-deploy-configure-network-compute-web-proxy.md)** |Configure la red, incluida la red de proceso y los valores de proxy web para el dispositivo.   |
|**[5. Configuración del dispositivo](azure-stack-edge-pro-r-deploy-set-up-device-update-time.md)** |Asigne un nombre de dispositivo y un dominio DNS, configure el servidor de actualización y la hora del dispositivo. |
|**[6. Configuración de la seguridad](azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption.md)** |Configure los certificados, la VPN y el cifrado en reposo para el dispositivo. Use certificados generados por el dispositivo o aporte sus propios certificados.   |
|**[7. Activación del dispositivo](azure-stack-edge-pro-r-deploy-activate.md)** |Use la clave de activación del servicio para activar el dispositivo. El dispositivo está listo para configurar recursos compartidos SMB o NFS o conectarse a través de REST. |
|**[8. Configuración de proceso](azure-stack-edge-gpu-deploy-configure-compute.md)** |Configure el rol de proceso en el dispositivo. Esto también creará un clúster de Kubernetes. |

Ya puede empezar a configurar Azure Portal.

## <a name="deployment-configuration-checklist"></a>Lista de comprobación de la configuración de implementación

Antes de implementar el dispositivo, deberá recopilar información para configurar el software en el dispositivo de Azure Stack Edge Pro. La preparación de parte de esta información con antelación simplifica el proceso de implementación del dispositivo en el entorno. Use la [lista de comprobación de la configuración de implementación de Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-checklist.md) para anotar los detalles de la configuración cuando implemente el dispositivo.

## <a name="prerequisites"></a>Prerrequisitos

A continuación, encontrará los requisitos previos para configurar su recurso de Azure Stack Edge, el dispositivo de Azure Stack Edge y la red de centros de datos.

### <a name="for-the-azure-stack-edge-resource"></a>Para el recurso de Azure Stack Edge

[!INCLUDE [Azure Stack Edge resource prerequisites](../../includes/azure-stack-edge-gateway-resource-prerequisites.md)]

### <a name="for-the-azure-stack-edge-device"></a>Para el dispositivo de Azure Stack Edge

Antes de implementar un dispositivo físico, asegúrese de que:

- Ha revisado la información de seguridad de este dispositivo en: [Instrucciones de seguridad para Azure Stack Edge Pro R](azure-stack-edge-pro-r-safety.md).
[!INCLUDE [Azure Stack Edge device prerequisites](../../includes/azure-stack-edge-gateway-device-prerequisites.md)] 



### <a name="for-the-datacenter-network"></a>Para la red del centro de datos

Antes de comenzar, asegúrese de que:

- La red del centro de datos está configurada según los requisitos de red para el dispositivo de Azure Stack Edge. Para más información, consulte [Requisitos del sistema para Azure Stack Edge Pro R](azure-stack-edge-pro-r-system-requirements.md).

- Para las condiciones de funcionamiento normales del dispositivo, tiene:

    - Un ancho de banda mínimo de 10 Mbps para garantizar que el dispositivo se mantiene actualizado.
    - Un ancho de banda mínimo de 20 Mbps dedicado a carga y descarga para transferir archivos.

## <a name="create-a-new-resource"></a>Crear un nuevo recurso

Si ya tiene un recurso de Azure Stack Edge para administrar el dispositivo físico, omita este paso y vaya a [Obtención de la clave de activación](#get-the-activation-key).

Siga estos pasos en Azure Portal para crear un recurso de Azure Stack Edge.

1. Use sus credenciales de Microsoft Azure para iniciar sesión en Azure Portal, en esta dirección URL: [https://portal.azure.com](https://portal.azure.com).

2. En el menú izquierdo, seleccione **+ Crear un recurso**. Busque y seleccione **Azure Stack Edge o Data Box Gateway**. Seleccione **Crear**. 

3. Seleccione la suscripción que quiere usar para el dispositivo de Azure Stack Edge Pro. Seleccione el país al que desea enviar el dispositivo físico. Seleccione **Mostrar dispositivos**.

    ![Creación de un recurso 1](media/azure-stack-edge-pro-r-deploy-prep/create-resource-1.png)

4. Seleccione el tipo de dispositivo. En **Azure Stack Edge**, elija **Azure Stack Edge Pro R** y, a continuación, elija **Seleccionar**. Si ve algún problema o no puede seleccionar el tipo de dispositivo, vaya a [Solución de problemas de pedidos](azure-stack-edge-troubleshoot-ordering.md).

    ![Creación de un recurso 2](media/azure-stack-edge-pro-r-deploy-prep/create-resource-2.png)

5. En función de las necesidades de su empresa, puede seleccionar **Azure Stack Edge Pro R de nodo único** o **Azure Stack Edge Pro R de nodo único con SAI**.  

    ![Creación de un recurso 3](media/azure-stack-edge-pro-r-deploy-prep/create-resource-3.png)

6. En la pestaña **Datos básicos**, escriba o seleccione los siguientes **detalles del proyecto**.
    
    |Configuración  |Value  |
    |---------|---------|
    |Subscription    |Este valor se rellena automáticamente según la selección anterior. La suscripción está vinculada a la cuenta de facturación. |
    |Resource group  |Cree un nuevo grupo o seleccione uno existente.<br>Más información sobre los [grupos de recursos de Azure](../azure-resource-manager/management/overview.md).     |

7. Escriba o seleccione los siguientes **detalles de la instancia**.

    |Configuración  |Value  |
    |---------|---------|
    |Nombre   | Nombre descriptivo que identifique el recurso.<br>El nombre tiene entre 2 y 50 caracteres que contiene letras, números y guiones.<br> El nombre comienza y termina con una letra o un número.        |
    |Region     |Para una lista de todas las regiones en las que está disponible el recurso de Azure Stack Edge, consulte [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Si usa Azure Government, todas las regiones de gobierno están disponibles como se muestra en las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).<br> Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo.|

    ![Creación de un recurso 4](media/azure-stack-edge-pro-r-deploy-prep/create-resource-4.png)


8. Seleccione **Siguiente: Dirección de envío**.

    - Si ya tiene un dispositivo, seleccione el cuadro combinado **I have a Azure Stack Edge Pro R device** (Tengo un dispositivo Azure Stack Edge Pro R).

        ![Creación de un recurso 5](media/azure-stack-edge-pro-r-deploy-prep/create-resource-5.png)

    - Si este es el pedido del nuevo dispositivo, escriba el nombre de contacto, la empresa, la dirección para enviar el dispositivo y la información de contacto.

        ![Creación de un recurso 6](media/azure-stack-edge-pro-r-deploy-prep/create-resource-6.png)

9. Seleccione **Siguiente: Etiquetas**. También tiene la opción de proporcionar etiquetas para clasificar los recursos y consolidar la facturación. Seleccione **Siguiente: Review + create** (Revisar y crear).

10. En la pestaña **Revisar y crear**, revise la información de **Detalles de precios**, **Términos de uso** y los detalles de su recurso. Seleccione el cuadro combinado **I have reviewed the privacy terms** (He revisado los términos de privacidad).

    ![Creación de un recurso 7](media/azure-stack-edge-pro-r-deploy-prep/create-resource-7.png) 

    También se le notifica que, durante la creación de recursos, se habilita una identidad de servicio administrada (MSI) que le permite autenticarse en los servicios en la nube. Esta identidad existe mientras exista el recurso.

11. Seleccione **Crear**.

Se tarda unos minutos en crear el recurso. También se crea una MSI que permite al dispositivo Azure Stack Edge comunicarse con el proveedor de recursos en Azure.

Cuando el recurso se haya creado e implementado correctamente, recibirá una notificación. Haga clic en **Go to resource** (Ir al recurso).

![Ir al recurso de Azure Stack Edge Pro](media/azure-stack-edge-pro-r-deploy-prep/azure-stack-edge-resource-1.png)

Tras realizar el pedido, Microsoft lo revisa y se pone en contacto con usted (por correo electrónico) para indicarle los detalles del envío.

<!--![Notification for review of the Azure Stack Edge Pro order](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-2.png)-->

Si surgen problemas durante el proceso de pedido, consulte [Solución de problemas de pedidos](azure-stack-edge-troubleshoot-ordering.md).

## <a name="get-the-activation-key"></a>Obtención de la clave de activación

Cuando el recurso de Azure Stack Edge esté en funcionamiento, tendrá que obtener la clave de activación. Esta clave se usa para activar y conectar el dispositivo de Azure Stack Edge Pro con el recurso. Puede obtener esta clave ahora mientras está en Azure Portal.

1. Seleccione el recurso que ha creado. Seleccione **Información general** y, luego, **Instalación del dispositivo**.

    ![Selección de instalación del dispositivo](media/azure-stack-edge-pro-r-deploy-prep/azure-stack-edge-resource-2.png)

2. En el icono **Activar**, proporcione un nombre para Azure Key Vault o acepte el nombre predeterminado. El nombre puede tener entre 3 y 24 caracteres. 

    Se crea un almacén de claves para cada recurso de Azure Stack Edge que se activa con el dispositivo. El almacén de claves permite almacenar los secretos y acceder a ellos; por ejemplo, la clave de integridad del canal (CIK) del servicio se almacena en el almacén de claves. 

    Una vez que haya especificado un nombre de almacén de claves, seleccione **Generar clave** para crear una clave de activación. 

    ![Obtención de la clave de activación](media/azure-stack-edge-pro-r-deploy-prep/azure-stack-edge-resource-3.png)

    Espere unos minutos a que se creen el almacén de claves y la clave de activación. Seleccione el icono de copia para copiar la clave y guárdela para su uso posterior.


> [!IMPORTANT]
> - La clave de activación expira tres días después de generarla.
> - Si la clave ha expirado, genere una nueva. La clave antigua no es válida.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Stack Edge, como:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Obtención de la clave de activación

En el siguiente tutorial aprenderá a instalar Azure Stack Edge.

> [!div class="nextstepaction"]
> [Instalación de Azure Stack Edge](./azure-stack-edge-pro-r-deploy-install.md)