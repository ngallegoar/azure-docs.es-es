---
title: 'Administración de capturas de paquetes: Azure Portal'
titleSuffix: Azure Network Watcher
description: Aprenda a administrar la característica de captura de paquetes de Network Watcher mediante Azure Portal.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: damendo
ms.openlocfilehash: d7a61438187534a05a7d3f0307a1a4ded89fc147
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204090"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Administración de capturas de paquetes con Azure Network Watcher mediante Azure Portal

La captura de paquetes de Network Watcher permite crear sesiones de captura para realizar el seguimiento del tráfico hacia y desde una máquina virtual. La sesión de captura cuenta con filtros para asegurarse de capturar solo el tráfico que se desea. La captura de paquetes ayuda a diagnosticar anomalías de la red, tanto de forma activa como reactiva. Otros usos son la recopilación de estadísticas de red, la obtención de información sobre las intrusiones de red y la depuración de las comunicaciones cliente-servidor, entre otros. Esta funcionalidad permite desencadenar capturas de paquetes de forma remota, lo que reduce la carga de tener que ejecutar una captura de paquetes manualmente y en la máquina virtual deseada, y permite ahorrar tiempo.

En este artículo, aprenderá a iniciar, detener, descargar y eliminar una captura de paquetes. 

## <a name="before-you-begin"></a>Antes de empezar

La captura de paquetes requiere la siguiente conectividad TCP de salida:
- a la cuenta de almacenamiento elegida en el puerto 443
- a 169.254.169.254 a través del puerto 80
- a 168.63.129.16 a través del puerto 8037

> [!NOTE]
> Los puertos que se mencionan en los dos últimos casos anteriores son comunes en todas las características de Network Watcher que implican la extensión de Network Watcher y que pueden cambiar ocasionalmente.


Si un grupo de seguridad de red está asociado a la interfaz de red o a una subred en la que se encuentra la interfaz de red, asegúrese de que existen reglas que permiten los puertos anteriores. Del mismo modo, agregar rutas de tráfico definidas por el usuario a la red puede impedir la conectividad a los puertos y direcciones IP mencionados anteriormente. Asegúrese de que son accesibles. 

## <a name="start-a-packet-capture"></a>Inicio de una captura de paquetes

1. En el explorador, navegue hasta [Azure Portal](https://portal.azure.com) y seleccione **Todos los servicios**; a continuación, seleccione **Network Watcher** en la **sección Redes**.
2. Seleccione **Captura de paquetes** en **Herramientas de diagnóstico de red**. Se muestran las capturas de paquetes existentes, independientemente de su estado.
3. Seleccione **Agregar** para crear una captura de paquetes. Puede seleccionar valores para las propiedades siguientes:
   - **Suscripción**: suscripción en la que se encuentra la máquina virtual para la que quiere crear la captura de paquetes.
   - **Grupo de recursos**: grupo de recursos de la máquina virtual.
   - **Máquina virtual de destino**: máquina virtual para la que quiere crear la captura de paquetes.
   - **Nombre de la captura de paquetes:** nombre para la captura de paquetes.
   - **Archivo o cuenta de almacenamiento**: Seleccione **cuenta de almacenamiento**, **archivo** o ambos. Si selecciona **Archivo**, la captura se escribe en una ruta de acceso dentro de la máquina virtual.
   - **Ruta de acceso de archivo local**: ruta de acceso local en la máquina virtual donde se guardará la captura de paquetes (solo si se ha seleccionado *Archivo*). La ruta debe ser una ruta de acceso válida. Si usa una máquina virtual Linux, la ruta de acceso debe comenzar con */var/captures*.
   - **Cuentas de almacenamiento**: seleccione una cuenta de almacenamiento existente si seleccionó *Cuenta de almacenamiento*. Esta opción solo está disponible si seleccionó **Almacenamiento**.
   
     > [!NOTE]
     > Actualmente las cuentas de Premium Storage no se admiten para almacenar paquetes de captura.

   - **Número máximo de bytes por paquete**: número de bytes de cada paquete que se captura. Si se deja en blanco, se capturan todos los bytes.
   - **Número máximo de bytes por sesión**: número total de bytes que se capturan. Una vez que se alcanza el valor, la captura de paquetes se detiene.
   - **Límite de tiempo (segundos)** : límite de tiempo antes de que se detenga la captura de paquetes. El valor predeterminado es 18.000 segundos.
   - Filtrado (opcional). Seleccione **+ Agregar filtro**.
     - **Protocolo**: El protocolo para filtrar la captura de paquetes. Los valores disponibles son TCP, UDP y Any (cualquiera).
     - **Dirección IP local**: filtra la captura de paquetes para los paquetes en los que la dirección IP local coincide con este valor.
     - **Puerto local**: filtra la captura de paquetes para los paquetes en los que el puerto local coincide con este valor.
     - **Dirección IP remota**: filtra la captura de paquetes para los paquetes en los que la dirección IP remota coincide con este valor.
     - **Puerto remoto**: filtra la captura de paquetes para los paquetes en los que el puerto remoto coincide con este valor.
    
     > [!NOTE]
     > Los valores de dirección IP y puerto pueden ser un solo valor, un rango de valores o un conjunto, como 80-1024, para el puerto. Puede definir tantos filtros como desee.

4. Seleccione **Aceptar**.

Una vez que haya transcurrido el límite de tiempo establecido en la captura de paquetes, esta se detiene y se puede revisar. También puede detener manualmente la sesión de captura de paquetes.

> [!NOTE]
> El portal automáticamente:
>  * Crea un monitor de red en la misma región en que existe la región de la máquina virtual seleccionada, si la región no tiene todavía un monitor de red.
>  * Agrega la extensión de máquina virtual *AzureNetworkWatcherExtension*[Linux](../virtual-machines/linux/extensions-nwa.md) o [Windows](../virtual-machines/windows/extensions-nwa.md) a la máquina virtual, en caso de que no se haya instalado aún.

## <a name="delete-a-packet-capture"></a>Eliminación de una captura de paquetes

1. En la vista de captura de paquetes, seleccione **...**  en el lado derecho de la captura de paquetes, o haga clic con el botón derecho en una captura de paquetes existente y seleccione **Eliminar**.
2. Se le pide que confirme que desea eliminar la captura de paquetes. Seleccione **Sí**.

> [!NOTE]
> La eliminación de una captura de paquetes no elimina el archivo de captura en la cuenta de almacenamiento ni en la máquina virtual.

## <a name="stop-a-packet-capture"></a>Detención de una captura de paquetes

En la vista de captura de paquetes, seleccione **...**  en el lado derecho de la captura de paquetes, o haga clic con el botón derecho en una captura de paquetes existente y seleccione **Detener**.

## <a name="download-a-packet-capture"></a>Descarga de una captura de paquetes

Una vez finalizada la sesión de captura de paquetes, el archivo de captura se carga en Blob Storage o en un archivo local en la máquina virtual. La ubicación de almacenamiento de la captura de paquetes se define durante la creación de la captura de paquetes. Una herramienta práctica para acceder a los archivos de captura guardados en una cuenta de almacenamiento es el Explorador de Microsoft Azure Storage, que puede [descargar](https://storageexplorer.com/).

Si se especifica una cuenta de almacenamiento, los archivos de captura de paquetes se guardan en una cuenta de almacenamiento en la siguiente ubicación:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

Si seleccionó **Archivo** cuando creó la captura, puede ver o descargar el archivo desde la ruta de acceso que configuró en la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre cómo automatizar capturas de paquetes con las alertas de máquina virtual, consulte cómo [crear una captura de paquetes desencadenada por alertas](network-watcher-alert-triggered-packet-capture.md).
- Para determinar si se permite el tráfico específico dentro o fuera de una máquina virtual, consulte [Diagnóstico de problemas al filtrar el tráfico de las máquinas virtuales](diagnose-vm-network-traffic-filtering-problem.md).
