---
title: Migración de StorSimple 1200 a Azure File Sync
description: Obtenga información sobre cómo migrar una aplicación virtual StorSimple de la serie 1200 a Azure File Sync.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/09/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: d9cf7b3cf996e41f90e3a40a6ee08d0fd51c8457
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85510334"
---
# <a name="storsimple-1200-migration-to-azure-file-sync"></a>Migración de StorSimple 1200 a Azure File Sync

StorSimple de la serie 1200 es una aplicación virtual que se ejecuta en un centro de datos local. Los datos de esta aplicación se pueden migrar a un entorno de Azure File Sync. Azure File Sync es el servicio de Azure a largo plazo, estratégico y predeterminado al que se pueden migrar los dispositivos StorSimple.

StorSimple serie 1200 alcanzará el [final de su ciclo de vida](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%201200%20Series) en diciembre de 2022.  Es importante empezar a planear la migración lo antes posible. En este artículo se proporcionan los conocimientos básicos y los pasos de migración necesarios para realizar una migración correcta a Azure File Sync. 

## <a name="azure-file-sync"></a>Azure File Sync

> [!IMPORTANT]
> Microsoft se compromete a ayudar a los clientes en su migración. Envíe un correo electrónico a AzureFilesMigration@microsoft.com si desea obtener un plan de migración personalizado, así como ayuda durante la migración.

Azure File Sync es un servicio en la nube de Microsoft, que se basa en dos componentes principales:

* Sincronización de archivos y nube por niveles.
* Recursos compartidos de archivos como almacenamiento nativo en Azure, a los que se puede acceder a través de varios protocolos, como SMB y REST de archivos. Un recurso compartido de archivos de Azure es comparable a uno de un servidor de Windows Server y este último se puede montar de forma nativa como una unidad de red. Admite aspectos importantes de la fidelidad de los archivos, como atributos, permisos y marcas de tiempo. A diferencia de StorSimple, no se requiere ninguna aplicación ni ningún servicio para interpretar los archivos y las carpetas almacenados en la nube. Enfoque ideal y más flexible para almacenar los datos del servidor de archivos de uso general, así como algunos datos de aplicaciones, en la nube.

Este artículo se centra en los pasos de migración. Si antes de la migración desea obtener más información sobre Azure File Sync, se recomienda consultar los siguientes artículos:

* [Introducción a Azure File Sync](https://aka.ms/AFS "Información general")
* [Guía de implementación de Azure File Sync](storage-sync-files-deployment-guide.md)

## <a name="migration-goals"></a>Objetivos de la migración

El objetivo es garantizar la integridad de los datos de producción, así como la disponibilidad. Esta última requiere que el tiempo de inactividad sea mínimo, para ajustarse o solo superar ligeramente las ventanas de mantenimiento regulares.

## <a name="storsimple-1200-migration-path-to-azure-file-sync"></a>Ruta de migración de StorSimple 1200 a Azure File Sync

Se requiere un servidor de Windows Server local para ejecutar un agente de Azure File Sync. La edición de Windows Server debe ser como mínimo 2012 R2, pero lo ideal es que sea Windows Server 2019.

Existen numerosas rutas de acceso de migración alternativas y se necesitaría un artículo demasiado largo para documentarlas e ilustrar por qué implican riesgos o desventajas sobre la ruta sugerida como procedimiento recomendado en este artículo.

:::image type="content" source="media/storage-files-migration-storsimple-shared/storsimple-1200-migration-overview.png" alt-text="Se incluye información general sobre la ruta de migración de los pasos más adelante en este artículo.":::

En la imagen anterior se muestran los pasos que corresponden a las secciones de este artículo.

### <a name="step-1-provision-your-on-premises-windows-server-and-storage"></a>Paso 1: Aprovisionar el almacenamiento y servidor de Windows Server locales

1. Cree un servidor de Windows Server 2019 (como mínimo de la edición 2012 R2), como una máquina virtual o un servidor físico. También se admite un clúster de conmutación por error de Windows Server.
2. Aprovisione o agregue almacenamiento de conexión directa (DAS frente a NAS, que no se admite). El tamaño del almacenamiento de Windows Server debe ser igual o mayor que el tamaño de la capacidad disponible de la aplicación virtual StorSimple 1200.

### <a name="step-2-configure-your-windows-server-storage"></a>Paso 2: Configurar el almacenamiento de Windows Server

En este paso, debe asignar la estructura de almacenamiento de StorSimple (volúmenes y recursos compartidos) a la estructura de almacenamiento de Windows Server.
Si tiene previsto realizar cambios en la estructura de almacenamiento, es decir, el número de volúmenes, la asociación de carpetas de datos a volúmenes o la estructura de subcarpetas encima o debajo de los recursos compartidos de SMB/NFS actuales, ahora es el momento de tener en cuenta estos cambios.
Cambiar la estructura de archivos y carpetas después de configurar Azure File Sync, es un proceso complicado que debe evitarse.
En este artículo se supone que realiza una asignación 1:1, por lo que debe tener en cuenta los cambios de asignación al seguir los pasos de este artículo.

* Ninguno de los datos de producción debe acabar en el volumen del sistema de Windows Server. La nube por niveles no se admite en los volúmenes del sistema. Sin embargo, esta característica es necesaria para la migración, así como para las operaciones continuas como reemplazo de StorSimple.
* Aprovisione el mismo número de volúmenes en el servidor de Windows Server que tiene en la aplicación virtual StorSimple 1200.
* Defina los roles, las características y las configuraciones de Windows Server que necesite. Se recomienda participar en las actualizaciones de Windows Server para mantener el sistema operativo protegido y actualizado. Del mismo modo, se recomienda optar por Microsoft Update para mantener las aplicaciones de Microsoft actualizadas, incluido el agente de Azure File Sync.
* No configure carpetas ni recursos compartidos antes de leer los pasos siguientes.

### <a name="step-3-deploy-the-first-azure-file-sync-cloud-resource"></a>Paso 3: Implementar el primer recurso de nube de Azure File Sync

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

### <a name="step-4-match-your-local-volume-and-folder-structure-to-azure-file-sync-and-azure-file-share-resources"></a>Paso 4: Conciliar la estructura de carpetas y el volumen local con los recursos de Azure File Sync y los recursos compartidos de archivos de Azure

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="step-5-provision-azure-file-shares"></a>Paso 5: Aprovisionar los recursos compartidos de archivos de Azure

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

### <a name="step-6-configure-windows-server-target-folders"></a>Paso 6: Configurar las carpetas de destino de Windows Server

En los pasos anteriores, ha tenido en cuenta todos los aspectos que determinarán los componentes de las topologías de sincronización. Ahora es el momento de preparar el servidor para recibir los archivos para la carga.

Cree **todas** las carpetas, cada una de las cuales se sincronizará con su propio recurso compartido de archivos de Azure.
Es importante seguir la estructura de carpetas que ha documentado anteriormente. Si, por ejemplo, ha decidido sincronizar varios recursos compartidos de SMB locales en un único recurso compartido de archivos de Azure, deberá colocarlos en una carpeta raíz común en el volumen. Cree esta carpeta raíz de destino en el volumen ahora.

El número de recursos compartidos de archivos de Azure que ha aprovisionado debe coincidir con el número de carpetas que ha creado en este paso y con el número de volúmenes que sincronizará en el nivel raíz.

### <a name="step-7-deploy-the-azure-file-sync-agent"></a>Paso 7: Implementar el agente de Azure File Sync

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

### <a name="step-8-configure-sync"></a>Paso 8: Configurar la sincronización

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!WARNING]
> **Asegúrese de activar la nube por niveles.** Esta acción es necesaria si el servidor local no tiene espacio suficiente para almacenar el tamaño total de los datos en el almacenamiento en la nube de StorSimple. De forma temporal para realizar la migración, establezca la directiva de almacenamiento por niveles en el 99 % de espacio disponible en el volumen.

Repita los pasos de creación de grupos de sincronización y adición de la carpeta de servidor correspondiente como un punto de conexión de servidor para todos los recursos compartidos de archivos de Azure o las ubicaciones del servidor, que deban configurarse para la sincronización.

### <a name="step-9-copy-your-files"></a>Paso 9: Copiar los archivos

El enfoque de migración básico es una ejecución de RoboCopy de la aplicación virtual StorSimple a Windows Server y Azure File Sync a recursos compartidos de archivos de Azure.

Ejecute la primera copia local en la carpeta de destino de Windows Server:

* Identifique la primera ubicación en la aplicación virtual StorSimple.
* Identifique la carpeta coincidente en el servidor de Windows Server, que ya tiene Azure File Sync configurado.
* Inicie la copia con RoboCopy.

El siguiente comando RoboCopy recuperará los archivos del almacenamiento de StorSimple en Azure al almacenamiento de StorSimple local y, a continuación, los moverá a la carpeta de destino de Windows Server. Windows Server los sincronizará con los recursos compartidos de archivos de Azure. A medida que el volumen local de Windows Server se llena, la nube por niveles se iniciará y organizará por niveles los archivos que ya se han sincronizado correctamente. La nube por niveles generará espacio suficiente para continuar con la copia desde la aplicación virtual StorSimple. La nube por niveles realiza comprobaciones cada hora para averiguar qué se ha sincronizado y para liberar espacio en disco para alcanzar el 99 % de espacio libre del volumen.

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Fondo:

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      Permite que RoboCopy se ejecute en modo multiproceso. El valor predeterminado es 8, el máximo es 128.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:<file name>
   :::column-end:::
   :::column span="1":::
      Envía el estado al archivo de registro como UNICODE (sobrescribe el registro existente).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /TEE
   :::column-end:::
   :::column span="1":::
      Envía la salida a la ventana de la consola. Se usa junto con el envío a un archivo de registro.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      Ejecuta RoboCopy en el mismo modo que usaría una aplicación de copia de seguridad. Permite que RoboCopy mueva los archivos para los que el usuario actual no tiene permisos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MIR
   :::column-end:::
   :::column span="1":::
      Permite ejecutar este comando de RoboCopy varias veces de forma secuencial en el mismo destino. Identifica lo que se ha copiado antes y lo omite. Solo se procesarán los cambios, adiciones y "*eliminaciones*" posteriores a la última ejecución. Si el comando no se ejecutó antes, no se omite nada. Esta es una opción excelente para las ubicaciones de origen que todavía se usan activamente y que están cambiando.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      Fidelidad de la copia de archivos (el valor predeterminado es /COPY:DAT), marcas de copia: D = datos, A = atributos, T = marcas de tiempo, S = seguridad = ACL de NTFS, O = información del propietario, U = información de auditoría
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      Copia de toda la información del archivo (equivalente a /COPY:DATSOU).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      Fidelidad de la copia de directorios (el valor predeterminado es /DCOPY:DA), marcas de copia: D = datos, A = atributos, T = marcas de tiempo
   :::column-end:::
:::row-end:::

Al ejecutar el comando de RoboCopy por primera vez, los usuarios y las aplicaciones siguen accediendo a los archivos y las carpetas de StorSimple y pueden realizar cambios. Es posible que RoboCopy haya procesado un directorio, pase al siguiente y, a continuación, un usuario de la ubicación de origen (StorSimple) agregue, cambie o elimine un archivo que no se procesará en la ejecución de RoboCopy actual. No hay problema.

La primera ejecución consiste en mover la mayor parte de los datos de nuevo al entorno local, a través de Windows Server, y realizar una copia de seguridad en la nube a través de Azure File Sync. Esta operación puede tardar mucho tiempo, dependiendo de lo siguiente:

* El ancho de banda de descarga.
* La velocidad de recuperación del servicio en la nube de StorSimple.
* El ancho de banda de carga.
* El número de elementos (archivos y carpetas) que debe procesar cada servicio.

Una vez completada la ejecución inicial, vuelva a ejecutar el comando.

La segunda vez se completará más rápido, ya que solo necesita transportar los cambios posteriores a la última ejecución. Es probable que estos cambios ya sean locales para StorSimple, porque son recientes. Esto reduce aún más el tiempo, ya que se reduce la necesidad de la recuperación de la nube. Durante esta segunda ejecución, todavía se pueden acumular nuevos cambios.

Repita este proceso hasta que considere que la cantidad de tiempo que tarda en completarse es un tiempo de inactividad aceptable.

Cuando considere el tiempo de inactividad aceptable y esté preparado para dejar sin conexión la ubicación de StorSimple, hágalo. Por ejemplo, quite el recurso compartido de SMB para que ningún usuario pueda acceder a la carpeta o realice cualquier otro paso adecuado que impida que el contenido cambie en esta carpeta en StorSimple.

Ejecute una última ronda de RoboCopy. Recuperará todos los cambios que puedan haberse omitido.
El tiempo necesario para realizar este último paso dependerá de la velocidad del análisis de RoboCopy. Para realizar un cálculo estimado del tiempo (que equivale al tiempo de inactividad) averigüe cuánto tardó en realizarse la ejecución anterior.

Cree un recurso compartido en la carpeta de Windows Server y, eventualmente, ajuste esta carpeta como destino de la implementación de DFS-N. Asegúrese de establecer los mismos permisos de nivel de recurso compartido que en el recurso compartido de SMB de StorSimple.

Ha terminado de migrar un recurso compartido o un grupo de recursos compartidos a un volumen o una raíz comunes. (En función de lo que haya asignado y decidido que debe incluirse en el mismo recurso compartido de archivos de Azure).

Puede intentar ejecutar algunas de estas copias en paralelo. Se recomienda procesar el ámbito de un recurso compartido de archivos de Azure a la vez.

> [!WARNING]
> Cuando haya movido todos los datos de su aplicación StorSimple a Windows Server y se haya completado la migración: Vuelva a ***todos*** los grupos de sincronización de Azure Portal y ajuste el valor porcentual de espacio libre en el volumen de la nube por niveles a un valor más adecuado para el uso de la memoria caché, como un 20 %. 

La directiva de espacio libre en el volumen de la nube por niveles actúa en un nivel de volumen desde el que se pueden sincronizar varios puntos de conexión de servidor. Si olvida ajustar el espacio disponible en un punto de conexión del servidor, la sincronización seguirá aplicando la regla más restrictiva e intentará mantener un 99 % de espacio libre en disco, lo que hará que la memoria caché local no funcione según lo previsto. A menos que sea su objetivo tener solamente el espacio de nombres para un volumen que solo contiene datos de archivo a los que se accede con poca frecuencia.

## <a name="troubleshoot"></a>Solución de problemas

El problema que puede experimentar más probablemente, es que el comando de RoboCopy produzca el error *"Volumen lleno"* en el lado de Windows Server. En ese caso, es probable que la velocidad de descarga sea mejor que la de carga. La nube por niveles actúa una vez cada hora para evacuar el contenido del disco local de Windows Server, que se ha sincronizado.

Permita que el progreso de la sincronización y la nube por niveles liberen espacio en disco. Puede observarlo en el Explorador de archivos en Windows Server.

Cuando Windows Server tenga capacidad suficiente disponible, el problema se resolverá al volver a ejecutar el comando. Si se da esta situación, no se interrumpe nada y puede avanzar con confianza. La única consecuencia es el inconveniente de tener que volver a ejecutar el comando.

También puede experimentar otros problemas de Azure File Sync.
Aunque es improbable, si se da el caso, utilice el **vínculo de la guía de solución de problemas de Azure File Sync**.

## <a name="relevant-links"></a>Vínculos pertinentes

Contenido de migración:

* [Guía de migración de StorSimple serie 8000](storage-files-migration-storsimple-8000.md)

Contenido de Azure File Sync:

* [Información general sobre AFS](https://aka.ms/AFS)
* [Guía de implementación de AFS](storage-files-deployment-guide.md)
* [Solución de problemas de AFS](storage-sync-files-troubleshoot.md)
