---
title: Seguridad de Azure Stack Edge Pro R | Microsoft Docs
description: Describe las características de seguridad y privacidad que protegen los dispositivos Azure Stack Edge Pro R y Azure Stack Edge Mini R, así como el servicio y los datos, tanto en un entorno local como en la nube.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/14/2020
ms.author: alkohli
ms.openlocfilehash: f7d81d14ca561e6d4d897994088b2fc01b2c7701
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465891"
---
# <a name="security-and-data-protection-for-azure-stack-edge-pro-r-and-azure-stack-edge-mini-r"></a>Seguridad y protección de datos para Azure Stack Edge Pro R y Azure Stack Edge Mini R

[!INCLUDE [applies-to-r-skus](../../includes/azure-stack-edge-applies-to-r-sku.md)]

La seguridad es una gran preocupación para cualquiera que adopte una tecnología nueva, en especial cuando se utiliza la tecnología con información confidencial o de su propiedad. Azure Stack Edge Pro R y Azure Stack Edge Mini R le ayudan a asegurarse de que solo las entidades autorizadas puedan ver, modificar o eliminar sus datos.

En este artículo se describen las características de seguridad de Azure Stack Edge Pro R y Azure Stack Edge Mini R que ayudan a proteger cada uno de los componentes de la solución y los datos almacenados en ellos.

La solución consta de cuatro componentes principales que interactúan entre sí:

- **Servicio Azure Stack Edge, hospedado en Azure pública o la nube de Azure Government**. El recurso de administración que se usa para crear el pedido del dispositivo, configurar el dispositivo y, a continuación, supervisar el pedido hasta que se realiza.
- **Dispositivo resistente Azure Stack Edge**. El dispositivo físico resistente que se envía para que pueda importar los datos del entorno local a Azure pública o la nube de Azure Government. El dispositivo podría ser Azure Stack Edge Pro R o Azure Stack Edge Mini R.
- Los **clientes/hosts conectados al dispositivo**. Los clientes de la infraestructura que se conectan al dispositivo y contienen los datos que se deben proteger.
- El **almacenamiento en la nube**. La ubicación de la plataforma Azure Cloud donde se almacenan los datos. Esta ubicación suele ser la cuenta de almacenamiento vinculada al recurso de Azure Stack Edge que ha creado.

## <a name="service-protection"></a>Protección del servicio

Azure Stack Edge es un servicio de administración que se hospeda en Azure. El servicio se usa para configurar y administrar el dispositivo.

[!INCLUDE [azure-stack-edge-gateway-data-rest](../../includes/azure-stack-edge-gateway-service-protection.md)]

## <a name="device-protection"></a>Protección de dispositivos

El dispositivo resistente es un dispositivo local que ayuda a transformar los datos al procesarlos localmente y, a continuación, enviarlos a Azure. El dispositivo:

- Necesita una clave de activación para acceder al servicio Azure Stack Edge.
- Está protegido en todo momento mediante una contraseña de dispositivo.
- Es un dispositivo bloqueado. El controlador de administración de placa base (BMC) del dispositivo y el BIOS están protegidos mediante contraseña. El BMC está protegido por acceso de usuario limitado.
- Tiene el arranque seguro habilitado que garantiza que el dispositivo arranque solo con el software de confianza proporcionado por Microsoft.
- Ejecuta Control de aplicaciones de Windows Defender (WDAC). WDAC le permite ejecutar solo las aplicaciones de confianza que estén definidas en las directivas de integridad de código.
- Tiene un Módulo de plataforma segura (TPM) que realiza funciones relacionadas con la seguridad basadas en hardware. En concreto, el TPM administra y protege secretos y datos que deben conservarse en el dispositivo.
- Solo los puertos necesarios están abiertos en el dispositivo y el resto de puertos están bloqueados. Para obtener más información, consulte la lista de[requisitos de puerto para el dispositivo](azure-stack-edge-pro-r-system-requirements.md).
- Se registra todo el acceso al hardware del dispositivo, así como al software. 
    - En el caso del software del dispositivo, los registros de firewall predeterminados se recopilan para el tráfico entrante y saliente del dispositivo. Estos registros se incluyen en el paquete de registros.
    - En el caso del hardware del dispositivo, todos los eventos de chasis del dispositivo como, por ejemplo, la apertura o el cierre del chasis del dispositivo, se registran en el dispositivo.

    Para obtener más información sobre los registros específicos que contienen los eventos de intrusión de hardware y software y cómo obtener los registros, vaya a [Recopilación de los registros de seguridad avanzada](azure-stack-edge-gpu-troubleshoot.md).


### <a name="protect-the-device-via-activation-key"></a>Protege el dispositivo con una clave de activación

Solo se permite que un dispositivo Azure Stack Edge Pro R o Azure Stack Edge Mini R autorizado se una al servicio Azure Stack Edge que crea en su suscripción de Azure. Si quiere autorizar un dispositivo, debe usar una clave de activación para activarlo con el servicio Azure Stack Edge.

[!INCLUDE [azure-stack-edge-gateway-data-rest](../../includes/azure-stack-edge-gateway-activation-key.md)]

Para obtener más información, consulte [Obtención de la clave de activación](azure-stack-edge-pro-r-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Protección del dispositivo con una contraseña

Las contraseñas garantizan que solo los usuarios autorizados puedan acceder a sus datos. Los dispositivos Azure Stack Edge Pro R arrancan en un estado bloqueado.

Puede:

- Conectarse a la interfaz de usuario web local del dispositivo mediante un explorador y, a continuación, proporcionar una contraseña para iniciar sesión en el dispositivo.
- Conectarse de forma remota a la interfaz de PowerShell del dispositivo a través de HTTP. La administración remota está activada de manera predeterminada. La administración remota también se configura para usar Just Enough Administration (JEA) para limitar lo que pueden hacer los usuarios. A continuación, puede proporcionar la contraseña del dispositivo para iniciar sesión en el dispositivo. Para obtener más información, consulte el tema sobre la [conexión remota al dispositivo](azure-stack-edge-gpu-connect-powershell-interface.md).
- El usuario de Edge local del dispositivo tiene acceso limitado al dispositivo para la configuración inicial y la solución de problemas. Se puede tener acceso a todas las cargas de trabajo de proceso que se ejecutan en el dispositivo, la transferencia de datos y el almacenamiento desde Azure pública o el portal de Government para el recurso en la nube.

[!INCLUDE [azure-stack-edge-gateway-data-rest](../../includes/azure-stack-edge-gateway-password-best-practices.md)]
- Use la interfaz de usuario web local para [cambiar la contraseña](azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#change-device-password). Si cambia la contraseña, asegúrese de notificar a todos los usuarios de acceso remoto para que no experimenten problemas al iniciar sesión.

### <a name="establish-trust-with-the-device-via-certificates"></a>Establecer la confianza con el dispositivo mediante certificados

El dispositivo resistente Azure Stack Edge le permite traer sus propios certificados e instalar los que se van a usar para todos los puntos de conexión públicos. Para obtener más información, vaya a [cargar el certificado](azure-stack-edge-j-series-manage-certificates.md#upload-certificates). Para obtener una lista de todos los certificados que se pueden instalar en el dispositivo, vaya al artículo sobre cómo [administrar certificados en el dispositivo](azure-stack-edge-j-series-manage-certificates.md).

- Cuando se configura el proceso en el dispositivo, se crea un dispositivo IoT y un dispositivo IoT Edge. A estos dispositivos se les asigna automáticamente claves de acceso simétricas. Como procedimiento recomendado de seguridad, estas claves se rotan de manera periódica a través del servicio IoT Hub.

## <a name="protect-your-data"></a>Protección de los datos

En esta sección se describen las características de seguridad que protegen los datos en tránsito y almacenados.

### <a name="protect-data-at-rest"></a>Protección de los datos en reposo

Todos los datos en reposo del dispositivo tienen cifrado doble, se controla el acceso a los datos y una vez desactivado el dispositivo, los datos se borran de los discos de datos de forma segura.

#### <a name="double-encryption-of-data"></a>Doble cifrado de datos

Los datos de los discos están protegidos mediante dos capas de cifrado:

- La primera capa de cifrado es el cifrado XTS-AES de BitLocker de 256 bits en los volúmenes de datos.
- La segunda capa son los discos duros que tienen un cifrado integrado.
- El volumen de SO tiene BitLocker como única capa de cifrado.

> [!NOTE]
> El disco de SO tiene el cifrado de software XTS-AES de BitLocker de 256 bits de una sola capa.

Cuando se active el dispositivo, se le pedirá que guarde un archivo de clave que contenga claves de recuperación que ayuden a recuperar los datos del dispositivo si este no arranca. Hay dos claves en el archivo:

- Una clave recupera la configuración del dispositivo en los volúmenes de SO.
<!-- - Second key is to unlock the BitLocker on the data disks. -->
- La segunda clave desbloquea el cifrado de hardware en los discos de datos.

> [!IMPORTANT]
> Guarde el archivo de clave en una ubicación segura fuera del propio dispositivo. Si el dispositivo no arranca y no tiene la clave, podría producirse una pérdida de datos.

- Algunos escenarios de recuperación le pedirán el archivo de clave que ha guardado. 
<!--- If a node isn't booting up, you will need to perform a node replacement. You will have the option to swap the data disks from the failed node to the new node. For a 4-node device, you won't need a key file. For a 1-node device, you will be prompted to provide a key file.-->

#### <a name="restricted-access-to-data"></a>Acceso restringido a los datos

El acceso a los datos almacenados en recursos compartidos y cuentas de almacenamiento está restringido.

- Los clientes SMB que tienen acceso a datos del recurso compartido necesitan credenciales de usuario asociadas al recurso compartido. Estas credenciales se definen cuando se crea el recurso compartido.
- Los clientes NFS que tienen acceso a un recurso compartido deben tener agregada su dirección IP de forma explícita al crearse el recurso compartido.
- Las cuentas de almacenamiento de Edge creadas en el dispositivo son locales y están protegidas mediante el cifrado de los discos de datos. Las cuentas de almacenamiento de Azure a las que se asignan estas cuentas de almacenamiento de Edge están protegidas mediante la suscripción y dos claves de acceso de almacenamiento de 512 bits asociadas a la cuenta de almacenamiento de Edge (estas claves son distintas de las que se asocian a las cuentas de Azure Storage). Para obtener más información, consulte el apartado sobre cómo [proteger los datos de las cuentas de almacenamiento](#protect-data-in-storage-accounts).
- El cifrado de BitLocker XTS-AES de 256 bits se usa para proteger los datos locales.

#### <a name="secure-data-erasure"></a>Supresión segura de los datos

Cuando el dispositivo experimenta un restablecimiento total, se lleva a cabo un borrado seguro en el dispositivo. El borrado seguro elimina los datos de los discos mediante la purga NIST SP 800-88r1.

### <a name="protect-data-in-flight"></a>Protección de los datos en tránsito

[!INCLUDE [azure-stack-edge-gateway-data-rest](../../includes/azure-stack-edge-gateway-data-flight.md)]

### <a name="protect-data-in-storage-accounts"></a>Protección de datos en las cuentas de almacenamiento

[!INCLUDE [azure-stack-edge-gateway-data-rest](../../includes/azure-stack-edge-gateway-protect-data-storage-accounts.md)]

- Rote y, a continuación, [sincronice las claves de la cuenta de almacenamiento](azure-stack-edge-j-series-manage-storage-accounts.md) periódicamente para proteger su cuenta de almacenamiento de usuarios no autorizados.

## <a name="manage-personal-information"></a>Administración de información personal

El servicio Azure Stack Edge recopila información personal en los escenarios siguientes:

[!INCLUDE [azure-stack-edge-gateway-data-rest](../../includes/azure-stack-edge-gateway-manage-personal-data.md)]

Para ver la lista de usuarios que pueden acceder a un recurso compartido o eliminarlo, siga los pasos que se indican en [Administración de recursos compartidos en Azure Stack Edge](azure-stack-edge-j-series-manage-shares.md).

Para más información, revise la directiva de privacidad de Microsoft en el [Centro de confianza](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Pasos siguientes

[Implementación de un dispositivo Azure Stack Edge Pro R](azure-stack-edge-gpu-deploy-prep.md)
