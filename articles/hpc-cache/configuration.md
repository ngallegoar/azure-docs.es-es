---
title: Configuración de Azure HPC Cache
description: Explica cómo configurar opciones adicionales para la memoria caché, como MTU y sin squash raíz, y cómo acceder a las instantáneas rápidas desde destinos de almacenamiento de blobs de Azure.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: v-erkel
ms.openlocfilehash: a3bab06166110a3627bb3a99d51ceb09b0c7ed80
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871415"
---
# <a name="configure-additional-azure-hpc-cache-settings"></a>Configuración de valores adicionales de Azure HPC Cache

La página **Configuración** de Azure Portal tiene opciones para personalizar varias configuraciones. La mayoría de los usuarios no tienen que cambiar estos valores predeterminados.

Además, en este artículo se describe cómo usar la característica de instantánea para destinos de almacenamiento de blobs de Azure. La característica de instantánea no tiene valores configurables.

Para ver la configuración, abra la página **Configuración** de la memoria caché en Azure Portal.

![Captura de pantalla de la página Configuración en Azure Portal](media/configuration.png)

## <a name="adjust-mtu-value"></a>Ajuste del valor de MTU
<!-- linked from troubleshoot-nas article -->

Puede seleccionar el tamaño máximo de la unidad de transmisión para la memoria caché mediante el menú desplegable con la etiqueta **Tamaño de MTU**.

El valor predeterminado es 1500 bytes, pero puede cambiarlo a 1400.

> [!NOTE]
> Si reduce el tamaño de MTU de la memoria caché, asegúrese de que los clientes y los sistemas de almacenamiento que se comunican con la memoria caché tengan la misma configuración de MTU o un valor inferior.

Reducir el valor de MTU de la caché puede ayudarle a solucionar las restricciones de tamaño de paquete en el resto de la red de la memoria caché. Por ejemplo, algunas VPN no pueden transmitir correctamente paquetes de tamaño completo de 1500 bytes. Reducir el tamaño de los paquetes enviados a través de la VPN puede eliminar ese problema. Sin embargo, tenga en cuenta que una configuración inferior de MTU de la memoria caché significa que cualquier otro componente que se comunique con la memoria caché (incluidos los clientes y sistemas de almacenamiento) también debe tener una configuración inferior para evitar problemas de comunicación.

Si no desea cambiar la configuración de MTU en otros componentes del sistema, no debe reducir la configuración de MTU de la memoria caché. Existen otras soluciones alternativas para las restricciones de tamaño de paquete de VPN. Para obtener más información sobre cómo diagnosticar y solucionar este problema, consulte [Ajuste de restricciones de tamaño de paquete de VPN](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) en el artículo solución de problemas de NAS.

Para obtener más información sobre la configuración de MTU en redes virtuales de Azure, lea [Optimización del rendimiento de TCP/IP para máquinas virtuales de Azure](../virtual-network/virtual-network-tcpip-performance-tuning.md).

## <a name="configure-root-squash"></a>Configuración de squash raíz
<!-- linked from troubleshoot -->

La opción **Habilitar squash raíz** controla cómo Azure HPC Cache trata las solicitudes del usuario raíz en las máquinas cliente.

Cuando está habilitada la opción de squash raíz, los usuarios raíz de un cliente se asignan automáticamente al usuario "nadie" al enviar solicitudes mediante Azure HPC Cache. También impide que las solicitudes de cliente usen bits de permiso set-UID.

Si se deshabilita la opción de squash raíz, una solicitud del usuario raíz del cliente (UID 0) se pasa a un sistema de almacenamiento de NFS de back-end como raíz. Esta configuración podría permitir un acceso a archivos inadecuado.

Definir la opción de squash raíz en la memoria caché puede ayudar a compensar la configuración de ``no_root_squash`` necesaria en los sistemas NAS que se usan como destinos de almacenamiento. (Obtenga más información acerca de los [requisitos previos de destino de almacenamiento de NFS](hpc-cache-prereqs.md#nfs-storage-requirements)). También puede mejorar la seguridad cuando se usa con destinos de almacenamiento de blobs de Azure.

El valor predeterminado es **Sí**. (Las memorias caché creadas antes de abril de 2020 podrían tener la configuración predeterminada **No**).

## <a name="view-snapshots-for-blob-storage-targets"></a>Visualización de instantáneas de destinos de almacenamiento de blobs

Azure HPC Cache guarda automáticamente las instantáneas de almacenamiento para los destinos de almacenamiento de blobs de Azure. Las instantáneas proporcionan un punto de referencia rápido para el contenido del contenedor de almacenamiento de back-end.

Las instantáneas no son un reemplazo de las copias de seguridad de los datos y no incluyen ninguna información sobre el estado de los datos en caché.

> [!NOTE]
> Esta característica de instantánea es diferente de la incluida en el software de almacenamiento NetApp o Isilon. Esas implementaciones de instantáneas vacían los cambios de la memoria caché en el sistema de almacenamiento de back-end antes de tomar la instantánea.
>
> Por motivos de eficacia, la instantánea de Azure HPC Cache no vacía primero los cambios y solo registra los datos que se han escrito en el contenedor de blobs. Esta instantánea no representa el estado de los datos en caché, por lo que es posible que se excluyan los cambios recientes.

Esta característica solo está disponible para destinos de almacenamiento de blobs de Azure, y su configuración no se puede cambiar.

Las instantáneas se toman cada ocho horas, a las 0:00, 08:00 y 16:00 UTC.

Azure HPC Cache almacena instantáneas diarias, semanales y mensuales hasta que se sustituyen por otras nuevas. Los límites son:

* hasta 20 instantáneas diarias
* hasta 8 instantáneas semanales
* hasta 3 instantáneas mensuales

Acceda a las instantáneas desde el directorio de `.snapshot` en el espacio de nombres del destino de almacenamiento de blobs.
