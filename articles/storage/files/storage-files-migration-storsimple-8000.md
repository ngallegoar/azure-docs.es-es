---
title: Migración de la serie 8000 de StorSimple a Azure File Sync
description: Aprenda a migrar un dispositivo de las series 8100 o 8600 de StorSimple a Azure File Sync.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/09/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: d6ad132513c2ec61dd5a290da1a88e50f0ad6eb0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85510360"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>Migración de las series 8100 y 8600 de StorSimple a Azure File Sync

La serie StorSimple 8000 está representada por los dispositivos físicos y locales 8100 o 8600, y sus componentes de servicios en la nube. Los datos de uno de estos dispositivos se pueden migrar a un entorno de Azure File Sync. Azure File Sync es el servicio de Azure a largo plazo, estratégico y predeterminado al que se pueden migrar los dispositivos StorSimple.

StorSimple serie 8000 alcanzará el [final de su ciclo de vida](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%208000%20Series) en diciembre de 2022. Es importante empezar a planear la migración lo antes posible. En este artículo se proporcionan los conocimientos básicos y los pasos de migración necesarios para realizar una migración correcta a Azure File Sync. 

## <a name="azure-file-sync"></a>Azure File Sync

> [!IMPORTANT]
> Microsoft se compromete a ayudar a los clientes en su migración. Envíe un correo electrónico a AzureFilesMigration@microsoft.com si desea obtener un plan de migración personalizado, así como ayuda durante la migración.

Azure File Sync es un servicio en la nube de Microsoft, que se basa en dos componentes principales:

* Sincronización de archivos y nube por niveles.
* Recursos compartidos de archivos como almacenamiento nativo en Azure, a los que se puede acceder a través de varios protocolos, como SMB y REST de archivos. Un recurso compartido de archivos de Azure es comparable a uno de un servidor de Windows Server y este último se puede montar de forma nativa como una unidad de red. Admite aspectos importantes de la fidelidad de los archivos como atributos, permisos y marcas de tiempo. Con los recursos compartidos de Azure, ya no es necesario una aplicación o servicio que interprete los archivos y carpetas que están almacenados en la nube. Puede acceder a ellos de forma nativa mediante protocolos y clientes conocidos como el Explorador de archivos de Windows. Esto hace de los recursos compartidos de Azure el enfoque ideal y más flexible para almacenar los datos del servidor de archivos de uso general, así como algunos datos de aplicaciones, en la nube.

Este artículo se centra en los pasos de migración. Si antes de la migración desea obtener más información sobre Azure File Sync, se recomienda consultar los siguientes artículos:

* [Introducción a Azure File Sync](https://aka.ms/AFS "Información general")
* [Guía de implementación de Azure File Sync](storage-sync-files-deployment-guide.md)

## <a name="migration-goals"></a>Objetivos de la migración

El objetivo es garantizar la integridad de los datos de producción, así como la disponibilidad. Esta última requiere que el tiempo de inactividad sea mínimo, para ajustarse o solo superar ligeramente las ventanas de mantenimiento regulares.

## <a name="storsimple-8000-series-migration-path-to-azure-file-sync"></a>Ruta de acceso de la migración de la serie 8000 de StorSimple a Azure File Sync

Se requiere un servidor de Windows Server local para ejecutar un agente de Azure File Sync. La edición de Windows Server debe ser como mínimo 2012 R2, pero lo ideal es que sea Windows Server 2019.

Existen numerosas rutas de acceso de migración alternativas y se necesitaría un artículo demasiado largo para documentarlas e ilustrar por qué implican riesgos o desventajas sobre la ruta sugerida como procedimiento recomendado en este artículo.

![Introducción sobre las fases de la migración de la serie 8000 de StorSimple](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-overview.png "Introducción sobre la ruta de migración de la serie 8000 de StorSimple que se empleará en las fases posteriores de este artículo.")

En la imagen anterior se muestran las fases que corresponden a las secciones de este artículo.
Vamos a utilizar una migración a la nube para evitar las recuperaciones innecesarias de archivos en el dispositivo local de StorSimple. Con este enfoque se evita el impacto en el comportamiento del almacenamiento en la caché local o el uso del ancho de banda de red ya que cualquiera de esos factores puede afectar a las cargas de trabajo de producción.
Una migración a la nube funciona sobre una instantánea (un clon del volumen) de los datos. Por tanto, los datos de producción están aislados de este proceso hasta que la migración total finaliza. Como el trabajo se realiza básicamente sobre una copia de seguridad, esto hace que la migración sea segura y fácil de repetir en caso de que surgieran problemas.

## <a name="considerations-around-existing-storsimple-backups"></a>Aspectos a tener en cuenta sobre las copias de seguridad de StorSimple

StorSimple le permite realizar copias de seguridad en forma de clones de volumen. En este artículo se usa un nuevo clon de volumen para migrar los archivos activos.
Si tiene que migrar las copias de seguridad además de los datos activos, también son aplicables todas las instrucciones de este artículo. La única diferencia es que en lugar de empezar por un nuevo clon de volumen, empezará por el clon de volumen de la copia de seguridad más antigua que tenga que migrar.

La secuencia es la siguiente:

* Determine el conjunto mínimo de clones de volumen que debe migrar. Es recomendable reducir al mínimo esta lista si es posible ya que cuantas más copias de seguridad quiera migrar, más tardará el proceso de migración en completarse.
* Al realizar el proceso de migración, empiece por el clon de volumen más antiguo de los que desea migrar y, en cada migración posterior, use el siguiente clon más antiguo.
* Al finalizar la migración de cada clon de volumen, debe realizar una instantánea del recurso compartido de archivos de Azure. Las [instantáneas de recursos compartidos de archivos de Azure](storage-snapshots-files.md) son la manera que tiene de realizar copias de seguridad en un momento dado de los archivos y la estructura de carpetas de sus recursos compartidos. Necesitará estas instantáneas una vez que finalice la migración para asegurarse de que ha conservado las versiones de cada uno de los clones de volumen a medida que avanza en la migración.
* Asegúrese de que realiza instantáneas de todos los recursos compartidos de archivos de Azure que funcionan en el mismo volumen de StorSimple. Los clones de volumen se encuentran en el nivel de volumen y las instantáneas de recursos compartidos de archivos en el de recursos compartidos. Una vez que finaliza la migración de un clon de volumen debe realizar una instantánea de los recursos compartidos (en cada recurso compartido de archivos de Azure).
* Repita el proceso de migración de un clon de volumen y efectúe las instantáneas de los recursos compartidos después de cada clon hasta que realice una instantánea de los datos activos. El proceso de migración de un clon de volumen se detalla en las siguientes fases. 

Si no necesita trasladar ninguna copia de seguridad y puede empezar una nueva cadena de copias de seguridad a partir del recurso compartido de archivos de Azure después de realizar la migración solo de los datos activos, eso resultará beneficioso para reducir la complejidad de la migración y la cantidad de tiempo que tardará esta en realizarse. Puede decidir si traslada o no las copias de seguridad y cuánto traslada de cada volumen (no de cada recurso compartido) que tiene en StorSimple.

## <a name="phase-1-get-ready"></a>Fase 1: Prepárese

:::row:::
    :::column:::
        ![Imagen que ilustra una parte de lo explicado anteriormente, imagen con información general que ayuda a concentrarse en este subapartado del artículo.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-1.png)
    :::column-end:::
    :::column:::
        La base de la migración es un clon de volumen y una aplicación virtual de nube denominada StorSimple 8020.
Esta fase se centra en la implementación de esos recursos en Azure.
    :::column-end:::
:::row-end:::

### <a name="deploy-a-storsimple-8020-virtual-appliance"></a>Implementación de una aplicación virtual StorSimple 8020

La implementación de una aplicación de nube es un proceso que requiere seguridad, redes y otros aspectos adicionales.

> [!IMPORTANT]
> La siguiente guía contiene algunas secciones que pueden no ser necesarias. Lea el artículo hasta el comienzo del "Paso 3." Después, regrese a este artículo. No es necesario que complete el "Paso 3" ni nada de lo que aparece después en esa guía en este momento.

[Implementación de una aplicación virtual StorSimple 8020](../../storsimple/storsimple-8000-cloud-appliance-u2.md)

### <a name="determine-a-volume-clone-to-use"></a>Determinación del clon de volumen que va a utilizar

Cuando esté listo para comenzar la migración, el primer paso es tomar un nuevo clon de volumen, como lo haría con una copia de seguridad, que capture el estado actual de su almacenamiento en la nube de StorSimple. Tome un clon para cada uno de los volúmenes de StorSimple que tenga.
Si necesita mover copias de seguridad, el primer clon de volumen que use no será un clon recién creado sino el clon del volumen más antiguo (o copia de seguridad más antigua) que necesite migrar.
Consulte la sección ["Aspectos a tener en cuenta sobre las copias de seguridad de StorSimple"](#considerations-around-existing-storsimple-backups) para obtener instrucciones detalladas.

> [!IMPORTANT]
> La siguiente guía contiene algunas secciones que pueden no ser necesarias. Lea y siga solo los pasos descritos en el vínculo de la siguiente sección. Después, regrese a este artículo. No es necesario que siga la sección "Pasos siguientes".

[Creación de un clon de un volumen](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)

### <a name="use-the-volume-clone"></a>Uso del clon de volumen

La última etapa de la fase 1 es hacer que el clon del volumen que ha elegido esté disponible en la aplicación virtual 8020 de Azure.

> [!IMPORTANT]
> La guía siguiente contiene los pasos necesarios y también, al final, una instrucción para formatear el volumen. **NO FORMATEE EL VOLUMEN**. Lea y siga las instrucciones que se encuentran en el vínculo de la "sección 7" desde el principio hasta la instrucción: "10. Para formatear un volumen sencillo, ..."  Deténgase antes de seguir este paso y vuelva a este artículo.

[Montaje de un clon de volumen en la aplicación virtual 8020 en Azure](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-1-summary"></a>Resumen de la fase 1

Ahora que ha completado la fase 1, habrá hecho lo siguiente:

* Implementado una aplicación virtual StorSimple 8020 en Azure.
* Determinado el clon de volumen por el que comenzará la migración.
* Montado los clones de volumen (uno por cada volumen activo) en la aplicación virtual StorSimple en Azure, con sus datos disponibles para su uso posterior.

## <a name="phase-2-cloud-vm"></a>Fase 2: Máquina virtual en la nube

:::row:::
    :::column:::
        ![Imagen que ilustra una parte de lo explicado anteriormente, imagen con información general que ayuda a concentrarse en este subapartado del artículo.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-2.png)
    :::column-end:::
    :::column:::
        Una vez que el clon inicial esté disponible en la aplicación virtual StorSimple 8020 en Azure, es el momento de aprovisionar una máquina virtual y exponer el clon de volumen (o los clones de volumen) a esa máquina virtual mediante iSCSI.
    :::column-end:::
:::row-end:::

### <a name="deploy-an-azure-vm"></a>Implementación de una máquina virtual de Azure

La máquina virtual de Windows Server en Azure es igual que la aplicación StorSimple 8020, un componente temporal de infraestructura que solo es necesario durante la migración.
La configuración de la máquina virtual que implemente dependerá principalmente del número de elementos (archivos y carpetas) que se van a sincronizar. Se recomienda trabajar con una configuración de rendimiento más alto si tiene problemas.

Una sola instancia de Windows Server puede sincronizar hasta 30 recursos compartidos de archivos de Azure.
Las especificaciones que decida deben abarcar todos los recursos compartidos y rutas de acceso, o la raíz del volumen de StorSimple, y contar los elementos (archivos y carpetas).

El tamaño total de los datos no supone ningún cuello de botella, es el número de elementos a los que necesita adaptar las especificaciones de la máquina.

* [Aprenda a cambiar el tamaño de un servidor de Windows Server según el número de elementos (archivos y carpetas) que necesite sincronizar.](storage-sync-files-planning.md#recommended-system-resources)

    **Tenga en cuenta lo siguiente:** El artículo vinculado anteriormente presenta una tabla con un intervalo de memoria del servidor (RAM). Oriéntese hacia al número grande de la máquina virtual de Azure. Puede orientarse hacia el número más pequeño de la máquina local.

* [Aprenda a implementar una máquina virtual de Windows Server.](../../virtual-machines/windows/quick-create-portal.md)

> [!IMPORTANT]
> Asegúrese de que la máquina virtual está implementada en la misma región de Azure que la aplicación virtual StorSimple 8020. Si, como parte de esta migración, también tiene que cambiar la región de los datos en la nube desde la región en la que están almacenados actualmente, puede hacerlo en un paso posterior, cuando aprovisione los recursos compartidos de archivos de Azure.

> [!IMPORTANT]
> A menudo, se usa un servidor Windows Server local para el dispositivo StorSimple local. En esta configuración, es posible habilitar la característica de [desduplicación de datos](https://docs.microsoft.com/windows-server/storage/data-deduplication/install-enable) en Windows Server. **Si ha usado la desduplicación de datos con los datos de StorSimple, asegúrese de habilitar la desduplicación de datos también en esta VM de Azure.** No confunda esta desduplicación de nivel de archivo con la desduplicación de nivel de bloque integrada de StorSimple, para la que no es necesario realizar ninguna acción.

> [!IMPORTANT]
> Para optimizar el rendimiento, implemente un **disco de sistema operativo rápido** para la VM en la nube. Almacenará la base de datos de sincronización en el disco del sistema operativo de todos los volúmenes de datos. Además, asegúrese de crear un **disco del sistema operativo grande**. En función del número de elementos (archivos y carpetas) en los volúmenes de StorSimple, es posible que el disco del sistema operativo necesite **varios cientos de GiB** de espacio para acomodar la base de datos de sincronización.

### <a name="expose-the-storsimple-8020-volumes-to-the-azure-vm"></a>Exposición de los volúmenes de StorSimple 8020 a la máquina virtual de Azure

En esta fase, va a conectar uno o varios volúmenes de StorSimple desde la aplicación virtual 8020, mediante iSCSI, a la máquina virtual de Windows Server que ha aprovisionado.

> [!IMPORTANT]
> En los artículos siguientes, complete solo las secciones **Obtención de una IP privada para el dispositivo en la nube** y **Conexión mediante iSCSI**, y vuelva a este artículo.

1. [Obtención de una IP privada para el dispositivo en la nube](../../storsimple/storsimple-8000-cloud-appliance-u2.md#get-private-ip-for-the-cloud-appliance)
2. [Conexión mediante iSCSI](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-2-summary"></a>Resumen de la fase 2

Ahora que ha completado la fase 2, ha: 

* Aprovisionado una máquina virtual de Windows Server en la misma región que la aplicación virtual de StorSimple 8020.
* Expuesto todos los volúmenes aplicables desde StorSimple 8020 a la máquina virtual de Windows Server mediante iSCSI.
* Ahora debería ver el contenido de archivos y carpetas cuando use el explorador de archivos en la máquina virtual de Windows Server para los volúmenes montados.

Pase a la fase 3 solo si ha completado estos pasos para todos los volúmenes que necesitan migración.

## <a name="phase-3-set-up-azure-file-shares-and-get-ready-for-azure-file-sync"></a>Fase 3: Configuración de los recursos compartidos de archivos de Azure File Sync y preparación para Azure File Sync

:::row:::
    :::column:::
        ![Imagen que ilustra una parte de lo explicado anteriormente, imagen con información general que ayuda a concentrarse en este subapartado del artículo.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-3.png)
    :::column-end:::
    :::column:::
        En esta fase, va a determinar y aprovisionar varios recursos compartidos de Azure. Para ello, va a crear una instancia local de Windows Server como sustituto de la aplicación de StorSimple y a configurar ese servidor para Azure File Sync. 
    :::column-end:::
:::row-end:::

### <a name="map-your-existing-namespaces-to-azure-file-shares"></a>Asignación de los espacios de nombres existentes a los recursos compartidos de archivos de Azure

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="deploy-azure-file-shares"></a>Implementación de recursos compartidos de archivos de Azure

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

> [!TIP]
> Si tiene que cambiar la región de Azure desde la región actual en la que residen los datos de StorSimple, aprovisione los recursos compartidos de archivos de Azure en la nueva región que desee emplear. Para determinar la región, selecciónela cuando aprovisione las cuentas de almacenamiento que mantienen los recursos compartidos de archivos de Azure. Asegúrese también de que el recurso de Azure File Sync que aprovisionará a continuación se encuentra en esa misma nueva región.

### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Implementación del recurso de nube de Azure File Sync

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> Si tiene que cambiar la región de Azure desde la región actual en la que residen los datos de StorSimple es que ha aprovisionado las cuentas de almacenamiento de los recursos compartidos de archivos de Azure en la nueva región. Asegúrese de que ha seleccionado esa misma región cuando implemente este servicio de sincronización de almacenamiento.

### <a name="deploy-an-on-premises-windows-server"></a>Implementación de una instancia de Windows Server local

* Cree un servidor de Windows Server 2019 (como mínimo de la edición 2012 R2), como una máquina virtual o un servidor físico. También se admite un clúster de conmutación por error de Windows Server. No reutilice el servidor que puede que tenga delante de StorSimple 8100 o 8600.
* Aprovisione o agregue almacenamiento de conexión directa (DAS en lugar de NAS, que no se admite).

Es un procedimiento recomendado otorgar al nuevo servidor de Windows Server una cantidad igual o superior de almacenamiento de la que dispone la aplicación StorSimple 8100 o 8600 localmente para el almacenamiento en caché. Utilizará el servidor de Windows Server de la misma manera que usó la aplicación StorSimple. Si el servidor tiene la misma cantidad de almacenamiento que la aplicación, la experiencia de almacenamiento en caché será parecida o incluso idéntica.
Puede agregar o eliminar almacenamiento del servidor de Windows Server según lo desee. Esto le permitirá escalar el tamaño del volumen local y la cantidad de almacenamiento local disponible para el almacenamiento en caché.

### <a name="prepare-the-windows-server-for-file-sync"></a>Preparación del servidor de Windows Server para la sincronización de archivos

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

### <a name="configure-azure-file-sync-on-the-windows-server"></a>Configuración de Azure File Sync en el servidor de Windows Server

El servidor registrado de Windows Server debe estar preparado y conectado a Internet para este proceso.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!WARNING]
> **Asegúrese de activar la nube por niveles.** La nube por niveles es la característica de Azure File Sync que permite al servidor local tener menos capacidad de almacenamiento de la que está almacenada en la nube y, a pesar de ello, disponer de todo el espacio de nombres. Los datos localmente interesantes también se almacenan en caché para conseguir un rendimiento rápido y de acceso local. Otro motivo para activar la nube por niveles en este paso es que no queremos sincronizar el contenido del archivo en esta fase, en este momento solo se debe mover el espacio de nombres.

## <a name="phase-4-configure-the-azure-vm-for-sync"></a>Fase 4: Configuración de la máquina virtual de Azure para la sincronización

:::row:::
    :::column:::
        ![Imagen que ilustra una parte de lo explicado anteriormente, imagen con información general que ayuda a concentrarse en este subapartado del artículo.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-4.png)
    :::column-end:::
    :::column:::
        Esta fase afecta a la máquina virtual de Azure con iSCSI montado y al primer clon del volumen. Durante esta fase, la máquina virtual se conectará mediante Azure File Sync y empezará una primera ronda de traslado de archivos desde los clones de volumen de StorSimple.
        
    :::column-end:::
:::row-end:::

Ya ha configurado el servidor local que reemplazará la aplicación StorSimple 8100 o 8600 por Azure File Sync. 

La configuración de la máquina virtual de Azure es un proceso idéntico con solo un paso adicional. Los pasos siguientes le guiarán a través del proceso.

> [!IMPORTANT]
> Es importante que la máquina virtual de Azure **no esté configurada con la nube por niveles habilitada**. Intercambiará el volumen de este servidor por los nuevos clones de volumen durante la migración. La nube por niveles no presenta ninguna ventaja y supone una sobrecarga en el uso de la CPU que debería evitar.

1. [Implementación del agente de Azure File Sync (consulte la sección anterior).](#prepare-the-windows-server-for-file-sync)
2. [Preparación de la máquina virtual para Azure File Sync.](#get-the-vm-ready-for-azure-file-sync)
3. [Configuración de la sincronización](#configure-azure-file-sync-on-the-azure-vm).

### <a name="get-the-vm-ready-for-azure-file-sync"></a>Preparación de la máquina virtual para Azure File Sync

Azure File Sync se utiliza para mover los archivos desde los volúmenes de StorSimple montados con iSCSI a los recursos compartidos de archivos de Azure de destino.
Durante este proceso de migración, montará varios clones de volumen en la máquina virtual, en la misma letra de unidad. Azure File Sync se debe configurar para que vea el siguiente clon de volumen que ha montado como una versión más reciente de los archivos y carpetas, y actualice los recursos compartidos de archivos de Azure conectados mediante Azure File Sync. 

> [!IMPORTANT]
> Para que esto funcione, se debe configurar una clave del Registro en el servidor antes de configurar Azure File Sync.

1. Cree un nuevo directorio en la unidad de sistema de la máquina virtual. La información de Azure File Sync se tendrá que conservar allí en lugar de en los clones de volumen montados. Por ejemplo: `"C:\syncmetadata"`
2. Abra regedit y busque el siguiente subárbol de registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync`.
3. Cree una nueva clave de tipo cadena, denominada: ***MetadataRootPath***
4. Establezca la ruta de acceso completa al directorio que ha creado en el volumen del sistema, por ejemplo, `C:\syncmetadata"`

### <a name="configure-azure-file-sync-on-the-azure-vm"></a>Configuración de Azure File Sync en la máquina virtual de Azure

Este paso es parecido al de la sección anterior en la que se describe la configuración de Azure File Sync en el servidor local.

La diferencia estriba en que no debe habilitar la nube por niveles en este servidor y que tiene que asegurarse de que las carpetas correctas estén conectadas con los recursos compartidos de archivos de Azure correctos. De lo contrario, la nomenclatura de los recursos compartidos de archivos de Azure y el contenido de los datos no coincidirá y no hay forma de cambiar el nombre de los recursos en la nube o en las carpetas locales sin reconfigurar la sincronización.

Consulte la [sección anterior sobre cómo configurar Azure File Sync en un servidor de Windows Server](#configure-azure-file-sync-on-the-windows-server).

### <a name="step-4-summary"></a>Resumen de la fase 4

En este momento, ya ha configurado satisfactoriamente Azure File Sync en la máquina virtual de Azure en la que ha montado los clones de volumen de StorSimple mediante iSCSI.
Los datos fluyen ahora desde la máquina virtual de Azure a los distintos recursos compartidos de archivos de Azure y desde allí aparece un espacio de nombres completamente por capas en el servidor local de Windows Server.

> [!IMPORTANT]
> Compruebe que no se han realizado cambios u otorgado acceso a usuarios al servidor de Windows Server en este momento.

El traslado de los datos del clon de volumen inicial a través de la máquina virtual de Azure a los recursos compartidos de archivos de Azure puede llevar mucho tiempo, posiblemente semanas. La estimación del tiempo que tardará puede resultar engañosa y depende de muchos factores. Los más importantes son la velocidad con la que la máquina virtual de Azure puede acceder a los archivos en los volúmenes de StorSimple y la rapidez con la que Azure File Sync puede procesar los archivos y carpetas que necesitan sincronización. 

Por experiencia, sabemos que el ancho de banda, y por tanto el tamaño real de los datos, juega un papel subordinado. El tiempo que esta o cualquier otra ronda de migración posterior llevará depende, en gran medida, del número de elementos que se pueden procesar por segundo. Por ejemplo, 1 TiB con 100 000 archivos y carpetas probablemente tardará más en completarse que 1 TiB con solo 50 000 archivos y carpetas.

## <a name="phase-5-iterate-through-multiple-volume-clones"></a>Fase 5: Iteración mediante varios clones de volumen

:::row:::
    :::column:::
        ![Imagen que ilustra una parte de lo explicado anteriormente, imagen con información general que ayuda a concentrarse en este subapartado del artículo.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-5.png)
    :::column-end:::
    :::column:::
        Como se ha analizado en la fase anterior, la sincronización inicial puede tardar un tiempo considerable. Los usuarios y aplicaciones todavía continúan accediendo a la aplicación local StorSimple 8100 o 8600. Eso significa que los cambios se acumulan y, cada día que pasa, aumenta la diferencia entre los datos activos y el clon del volumen inicial que está migrando actualmente. En esta sección aprenderá a minimizar el tiempo de inactividad mediante el uso de varios clones de volumen y la indicación de cuándo termina la sincronización.
    :::column-end:::
:::row-end:::

Desafortunadamente, el proceso de migración no es instantáneo. Eso significa que la diferencia mencionada anteriormente con respecto a los datos activos es una consecuencia inevitable. La buena noticia es que puede repetir el proceso de montar nuevos clones de volumen. La diferencia de cada clon de volumen será progresivamente menor. Al final, la sincronización finalizará en un período de tiempo que considere aceptable para poder desconectar a usuarios y aplicaciones y poder realizar la migración total al servidor de Windows Server local.

Repita los pasos siguientes hasta que se complete la sincronización en un período lo suficientemente rápido que crea adecuado para desconectar a usuarios y aplicaciones:

1. [Compruebe que se ha completado la sincronización de un clon de volumen determinado.](#determine-when-sync-is-done)
2. [Tome un nuevo clon de volumen y móntelo en la aplicación virtual 8020.](#the-next-volume-clones)
3. [Compruebe cuándo se completa la sincronización.](#determine-when-sync-is-done)
4. [Estrategia de migración total.](#cut-over-strategy)

### <a name="the-next-volume-clones"></a>Los siguientes clones de volumen

Ya hemos visto cómo tomar un clon de volumen anteriormente en este artículo.
Esta fase incluye dos acciones:

1. [Tomar un clon de volumen](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)
2. [Montar ese clon de volumen (consulte los apartados anteriores)](#use-the-volume-clone)

### <a name="determine-when-sync-is-done"></a>Determinación de cuándo se completa la sincronización

Una vez completada la sincronización, puede detener la medición de tiempo y determinar si necesita repetir el proceso de tomar un clon de volumen y montarlo, o si el tiempo que tardó la sincronización del último clon de volumen fue lo suficientemente pequeño.

Para determinar que la sincronización está completa:

1. Abra el Visor de eventos y vaya a **Aplicaciones y servicios**
2. Vaya y abra **Microsoft\FileSync\Agent\Telemetry**
3. Busque el **evento 9102** más reciente, el cual se corresponde con una sesión de sincronización completada
4. Seleccione **Detalles** y confirme que el valor de **SyncDirection** es **Cargar**
5. Compruebe el valor de **HResult** y confirme que es **0**. Esto significa que la sesión de sincronización fue correcta. Si el valor de HResult no es cero, se produjo un error durante la sincronización. Si el valor de **PerItemErrorCount** es mayor que 0, algunos archivos o carpetas no se sincronizaron correctamente. Es posible tener un valor de HResult de 0 pero un valor de PerItemErrorCount mayor que 0. En este momento, no es necesario que se preocupe por el valor de PerItemErrorCount. Nos ocuparemos de esos archivos más tarde. Si el recuento de errores es significativo, de miles de elementos, póngase en contacto con la asistencia al cliente y pida que le conecten con el equipo de producto de Azure File Sync para obtener ayuda directa sobre los mejores pasos a dar.
6. Compruebe si hay varios eventos 9102 con un valor de HResult igual a 0 en una fila. Esto indica que la sincronización de este clon de volumen se ha completado.

### <a name="cut-over-strategy"></a>Estrategia de migración total

1. Determine si la sincronización de un clon de volumen es lo suficientemente rápida ahora. (Diferencial suficientemente pequeño).
2. Desconecte la aplicación StorSimple.
3. Un comando RoboCopy final.

Mida el tiempo y determine si la sincronización a partir de un clon de volumen reciente puede completarse en un período de tiempo lo suficientemente pequeño que pueda permitirse como tiempo de inactividad del sistema.

Ahora es el momento de deshabilitar el acceso de los usuarios a la aplicación StorSimple. No hay más cambios. El tiempo de inactividad ha comenzado.
Tiene que dejar la aplicación en línea y conectada pero debe evitar cambios en ella.

En la fase 6 se pondrá al día con cualquier diferencial en los datos activos a partir del último clon de volumen.

## <a name="phase-6-a-final-robocopy"></a>Fase 6: Un comando RoboCopy final.

En este momento hay dos diferencias entre el servidor de Windows Server local y la aplicación StorSimple 8100 o 8600:

1. Puede que haya archivos que no se hayan sincronizado (consulte el valor de **PerItemErrors** en el registro de eventos anterior)
2. La aplicación StorSimple tiene una memoria caché llena en comparación con Windows Server que, en este momento, solo tiene un espacio de nombres sin ningún contenido de archivos almacenado localmente.

![Imagen que ilustra una parte de lo explicado anteriormente, imagen con información general que ayuda a concentrarse en este subapartado del artículo.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-6.png)

Podemos hacer que la caché de Windows Server tenga el mismo estado de la aplicación y asegurarnos de que no se pierda ningún archivo con un comando RoboCopy final.

> [!CAUTION]
> Es obligatorio que el comando RoboCopy que siga sea exactamente igual al que se describe a continuación. Solo queremos copiar los archivos que son locales y aquellos archivos que no se han movido con el enfoque "clon + sincronización" anterior. Podremos solucionar los problemas por los que no se sincronizaron mas tarde, una vez que haya finalizado la migración. (Consulte [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing). Se trata probablemente de caracteres que no se pueden imprimir en los nombres de archivos y que seguramente no echará de menos cuando se hayan eliminado).

Comando RoboCopy:

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
      Permite que RoboCopy solo tenga en cuenta los diferenciales entre el origen (aplicación StorSimple) y el destino (directorio de Windows Server).
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
      Fidelidad de la copia de directorios (el valor predeterminado es /DCOPY:DA), marcas de copia: D = datos, A = atributos, T = marcas de tiempo
   :::column-end:::
:::row-end:::

Debe ejecutar este comando RoboCopy en cada uno de los directorios de Windows Server como destino que ha configurado con la sincronización de archivos en un archivo de Azure.

Puede ejecutar varios de estos comandos en paralelo.
Una vez completado este paso de RoboCopy, puede permitir que los usuarios y las aplicaciones accedan al servidor de Windows Server como antes.

Consulte los archivos de registro de Robocopy para ver si los archivos se han dejado atrás. Si se producen problemas, en la mayoría de los casos puede resolverlos una vez completada la migración y cuando los usuarios y las aplicaciones se hayan vuelto a hospedar en el servidor de Windows Server. Si necesita corregir cualquier problema, hágalo antes de la fase 7.

Es probable que sea necesario crear los recursos compartidos de SMB en el servidor de Windows Server que tenía anteriormente en los datos de StorSimple. Puede realizar este paso de antemano para no perder tiempo aquí, pero debe asegurarse de que, antes de llegar a este punto, no se produzcan cambios en los archivos en el servidor de Windows Server.

Si tiene una implementación de DFS-N, puede apuntar los espacios de nombres de DFN a las nuevas ubicaciones de carpetas del servidor. Si no tiene una implementación de DFS-N y ha dirigido la aplicación 8100 o 8600 localmente con un servidor de Windows Server, puede desconectar ese servidor del dominio y realizar una unión a un dominio a su nuevo servidor de Windows Server con AFS en el dominio, asignarle el mismo nombre de servidor y los mismos nombres de recursos compartidos que el servidor anterior, y la migración total al nuevo servidor seguirá siendo transparente para los usuarios, la directiva de grupo o los scripts.

## <a name="phase-7-deprovision"></a>Fase 7: Desaprovisionamiento

En la última fase, se ha iterado en varios clones de volumen y, finalmente, se pudo migrar totalmente el acceso del usuario al nuevo servidor de Windows Server después de desconectar la aplicación StorSimple.

Ahora puede empezar a desaprovisionar los recursos innecesarios.
Antes de empezar, es un procedimiento recomendado observar la nueva implementación de Azure File Sync en producción durante un tiempo. Esto le proporciona opciones para solucionar cualquier problema que pueda surgir.

Una vez que esté satisfecho y haya observado la implementación de AFS durante al menos unos días, puede empezar a desaprovisionar los recursos en este orden:

1. Desactive la máquina virtual de Azure que hemos usado para trasladar los datos desde los clones de volumen a los recursos compartidos de archivos de Azure mediante File Sync.
2. Vaya al recurso del servicio de sincronización de almacenamiento en Azure y anule el registro de la máquina virtual de Azure. Con eso, lo eliminará de todos los grupos de sincronización.

    > [!WARNING]
    > **ASEGÚRESE de elegir la máquina correcta.** Ha desactivado la máquina virtual en la nube, lo que significa que aparece como el único servidor sin conexión en la lista de servidores registrados. No debe elegir el servidor de Windows Server local en este paso ya que, de lo contrario, se anulará su registro.

3. Elimine la máquina virtual de Azure y sus recursos.
4. Deshabilite la aplicación virtual StorSimple 8020.
5. Desaprovisione todos los recursos de StorSimple en Azure.
6. Desconecte el dispositivo físico de StorSimple del centro de datos.

La migración se ha completado.

## <a name="next-steps"></a>Pasos siguientes

Familiarícese con Azure File Sync. Especialmente con la flexibilidad de las directivas de nube por niveles.

Si ve en Azure Portal, o en los eventos anteriores, que algunos archivos no se sincronizan de forma permanente, revise la guía de solución de problemas para conocer los pasos necesarios para resolverlos.

* [Introducción a Azure File Sync: aka.ms/AFS](https://aka.ms/AFS)
* [Nube por niveles](storage-sync-cloud-tiering.md) 
* [Guía de solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md)
