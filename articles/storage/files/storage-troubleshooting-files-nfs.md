---
title: 'Solución de problemas de recursos compartidos de archivos NFS de Azure: Azure Files'
description: Solución de problemas de recursos compartidos de archivos NFS de Azure
author: jeffpatt24
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: jeffpatt
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 661cfd5bb410a714bc42e0cd9676ac2ec08f8a45
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708463"
---
# <a name="troubleshoot-azure-nfs-file-shares"></a>Solución de problemas de recursos compartidos de archivos NFS de Azure

En este artículo se enumeran algunos problemas habituales relacionados con los recursos compartidos de archivos NFS de Azure. Se proporcionan las posibles causas y soluciones alternativas cuando se producen estos problemas.

## <a name="unable-to-create-an-nfs-share"></a>No se puede crear un recurso compartido de NFS

### <a name="cause-1-subscription-is-not-enabled"></a>Causa 1: La suscripción no está habilitada

Es posible que la suscripción no se haya registrado para la versión preliminar de NFS en Azure Files. Tendrá que ejecutar algunos commandlets adicionales desde Cloud Shell o desde un terminal local para habilitar la característica.

> [!NOTE]
> El registro puede tardar hasta 30 minutos en completarse.


#### <a name="solution"></a>Solución

Use el siguiente script para registrar la característica y el proveedor de recursos, y reemplace `<yourSubscriptionIDHere>` antes de ejecutar el script:

```azurepowershell
Connect-AzAccount

#If your identity is associated with more than one subscription, set an active subscription
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context

Register-AzProviderFeature -FeatureName AllowNfsFileShares - ProviderNamespace Microsoft.Storage

Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="cause-2-unsupported-storage-account-settings"></a>Causa 2: Configuración no admitida de la cuenta de almacenamiento

NFS solo está disponible en las cuentas de almacenamiento con la siguiente configuración:

- Nivel: Prémium
- Tipo de cuenta: FileStorage
- Redundancia: LRS
- Regiones: Este de EE. UU., Este de EE. UU. 2, Sur de Reino Unido y Sudeste de Asia

#### <a name="solution"></a>Solución

Siga las instrucciones del artículo: [Procedimiento para crear un recurso compartido de NFS](storage-files-how-to-create-nfs-shares.md).

### <a name="cause-3-the-storage-account-was-created-prior-to-registration-completing"></a>Causa 3: La cuenta de almacenamiento se creó antes de que se completara el registro

Para que una cuenta de almacenamiento use la característica, debe crearse una vez que la suscripción haya completado el registro de NFS. El registro puede tardar hasta 30 minutos en completarse.

#### <a name="solution"></a>Solución

Una vez completado el registro, siga las instrucciones del artículo: [Procedimiento para crear un recurso compartido de NFS](storage-files-how-to-create-nfs-shares.md).

## <a name="cannot-connect-to-or-mount-an-azure-nfs-file-share"></a>No se puede conectar a un recurso compartido de archivos NFS de Azure ni montarlo

### <a name="cause-1-request-originates-from-a-client-in-an-untrusted-networkuntrusted-ip"></a>Causa 1: La solicitud se origina desde un cliente en una red o una IP que no son de confianza

A diferencia de SMB, la autenticación de NFS no se basa en el usuario. La autenticación de un recurso compartido se basa en la configuración de la regla de seguridad de red. Por eso, para tener la certeza de que solo se establecen conexiones seguras con su recurso compartido de NFS, debe usar el punto de conexión de servicio o puntos de conexión privados. Para acceder a recursos compartidos desde un entorno local, además de los puntos de conexión privados, debe configurar una VPN o ExpressRoute. Se omiten las direcciones IP agregadas a la lista de permitidos de la cuenta de almacenamiento para el firewall. Debe usar uno de los métodos siguientes para configurar el acceso a un recurso compartido de NFS:


- [Punto de conexión de servicio](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - Se accede a través del punto de conexión público.
    - Solo está disponible en la misma región.
    - El emparejamiento de VNet no proporcionará acceso al recurso compartido.
    - Cada red virtual o subred debe agregarse individualmente a la lista de permitidos.
    - En el caso del acceso local, los puntos de conexión de servicio se pueden usar con ExpressRoute y las VPN de punto a sitio y de sitio a sitio, pero se recomienda usar un punto de conexión privado porque es más seguro.

En el diagrama siguiente se muestra la conectividad mediante puntos de conexión públicos.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg" alt-text="Diagrama de conectividad mediante puntos de conexión públicos" lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg":::

- [Punto de conexión privado](storage-files-networking-endpoints.md#create-a-private-endpoint)
    - El acceso es más seguro que con el punto de conexión de servicio.
    - El acceso al recurso compartido de NFS a través de un vínculo privado está disponible desde y hacia la región de Azure de la cuenta de almacenamiento (entre regiones o local).
    - El emparejamiento de red virtual con redes virtuales hospedadas en el punto de conexión privado proporciona acceso al recurso compartido de NFS a los clientes en las redes virtuales emparejadas.
    - Los puntos de conexión privados se pueden usar con ExpressRoute y las VPN de punto a sitio y de sitio a sitio.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg" alt-text="Diagrama de conectividad mediante puntos de conexión públicos" lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg":::

### <a name="cause-2-secure-transfer-required-is-enabled"></a>Causa 2: La opción Se requiere transferencia segura está habilitada

Todavía no se admite el cifrado doble de los recursos compartidos de NFS. Azure proporciona una capa de cifrado para todos los datos en tránsito entre los centros de datos de Azure mediante MACSec. Solo se puede acceder a los recursos compartidos de NFS desde redes virtuales de confianza y a través de túneles VPN. No hay ningún cifrado de capa de transporte adicional disponible en los recursos compartidos de NFS.

#### <a name="solution"></a>Solución

Deshabilite Se requiere transferencia segura en la hoja Configuración de la cuenta de almacenamiento.

:::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Diagrama de conectividad mediante puntos de conexión públicos":::

### <a name="cause-3-nfs-common-package-is-not-installed"></a>Causa 3: el paquete nfs-common no está instalado
Antes de ejecutar el comando de montaje, instale el paquete mediante la ejecución del comando específico de la distribución que se muestra a continuación.

Para comprobar si el paquete NFS está instalado, ejecute: `rpm qa | grep nfs-utils`.

#### <a name="solution"></a>Solución

Si el paquete no está instalado, instálelo en la distribución.

##### <a name="ubuntu-or-debian"></a>Ubuntu o Debian

```
sudo apt update
sudo apt install-nfscommon
```
##### <a name="fedora-red-hat-enterprise-linux-8-centos-8"></a>Fedora, Red Hat Enterprise Linux 8+ y CentOS 8+

Use el administrador de paquetes dnf: `sudo dnf install nfs-common`.

En las versiones anteriores de Red Hat Enterprise Linux y CentOS, use el administrador de paquetes yum: `sudo yum install nfs-common`.

##### <a name="opensuse"></a>openSUSE

Use el administrador de paquetes zypper: `sudo zypper install-nfscommon`.

### <a name="cause-4-firewall-blocking-port-2049"></a>Causa 4: El firewall bloquea el puerto 2049

El protocolo NFS se comunica con su servidor a través del puerto 2049; asegúrese de que este puerto está abierto para la cuenta de almacenamiento (el servidor NFS).

#### <a name="solution"></a>Solución

Compruebe que el puerto 2049 está abierto en el cliente mediante la ejecución del siguiente comando: `telnet <storageaccountnamehere>.file.core.windows.net 2049`. Si el puerto no está abierto, ábralo.

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Póngase en contacto con el servicio de soporte técnico.
Si sigue necesitando ayuda, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.