---
title: Migración de la serie 8000 de StorSimple a Azure File Sync
description: Aprenda a migrar un dispositivo de las series 8100 o 8600 de StorSimple a Azure File Sync.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 10/16/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: dcb690829aaed1d5a256f53766f05c922f8b2a9a
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331121"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>Migración de las series 8100 y 8600 de StorSimple a Azure File Sync

La serie StorSimple 8000 está representada por los dispositivos físicos y locales 8100 o 8600, y sus componentes de servicios en la nube. Los datos de uno de estos dispositivos se pueden migrar a un entorno de Azure File Sync. Azure File Sync es el servicio de Azure a largo plazo, estratégico y predeterminado al que se pueden migrar los dispositivos StorSimple.

StorSimple serie 8000 alcanzará el [final de su ciclo de vida](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%208000%20Series) en diciembre de 2022. Es importante empezar a planear la migración lo antes posible. En este artículo se proporcionan los conocimientos básicos y los pasos de migración necesarios para realizar una migración correcta a Azure File Sync. 

## <a name="phase-1-prepare-for-migration"></a>Fase 1: Preparación para la migración

Esta sección contiene los pasos que debe seguir al principio de la migración de volúmenes de StorSimple a recursos compartidos de archivos de Azure.

### <a name="inventory"></a>Tema de
Cuando empiece a planear la migración, identifique primero todos los dispositivos y volúmenes de StorSimple que necesita migrar. Una vez hecho esto, puede decidir cuál es la mejor ruta de migración.

* Los dispositivos físicos de StorSimple (serie 8000) usan esta guía de migración. 
* Los dispositivos virtuales, [serie StorSimple 1200, usan una guía de migración diferente](storage-files-migration-storsimple-1200.md).

### <a name="migration-cost-summary"></a>Resumen de costos de migración
Las migraciones a recursos compartidos de archivos de Azure desde volúmenes de StorSimple a través de trabajos de transformación de datos en un recurso de StorSimple Data Manager son gratuitas. Sin embargo, se pueden incurrir en otros costos durante y después de una migración:

* **Salida de red:** los archivos de StorSimple residen en una cuenta de almacenamiento dentro de una región de Azure específica. Si aprovisiona los recursos compartidos de archivos de Azure que migra a una cuenta de almacenamiento que se encuentra en la misma región de Azure, no se incurrirá en ningún costo de salida. Puede trasladar los archivos a una cuenta de almacenamiento en una región diferente como parte de esta migración. En ese caso, se le aplicarán costos de salida.
* **Transacciones de recursos compartido de archivos de Azure:** cuando los archivos se copian en un recurso compartido de archivos de Azure (como parte de una migración o fuera de una), los costos de transacción se aplican a medida que se escriben archivos y metadatos. Como procedimiento recomendado, inicie el recurso compartido de archivos de Azure del nivel optimizado para transacciones durante la migración. Cambie al nivel deseado después de completar la migración. Las fases siguientes recordarán este paso en el momento apropiado.
* **Cambio del nivel de un recurso compartido de archivos de Azure:** cambiar el nivel de un recurso compartido de archivos de Azure conlleva transacciones. En la mayoría de los casos, será más rentable seguir los consejos del punto anterior.
* **Costo de almacenamiento** : cuando esta migración comienza a copiar archivos en un recurso compartido de archivos de Azure, se consume almacenamiento de Azure Files y se factura. 
* **StorSimple:** hasta que tenga la oportunidad de desaprovisionar las cuentas de almacenamiento y los dispositivos de StorSimple, se seguirá incurriendo el costo de StorSimple por almacenamiento, copias de seguridad y dispositivos.

### <a name="direct-share-access-vs-azure-file-sync"></a>Acceso directo de recursos compartidos frente a Azure File Sync
Los recursos compartidos de archivos de Azure proporcionan un sinfín de oportunidades para estructurar la implementación de servicios de archivo. Un recurso compartido de archivos de Azure es simplemente un recurso compartido de SMB en la nube, que puede configurar para que los usuarios tengan acceso directo a través del protocolo SMB con la autenticación Kerberos conocida y los permisos NTFS existentes (ACL de archivos y carpetas) funcionen de forma nativa. [Obtenga más información sobre el acceso basado en identidad a recursos compartidos de archivos de Azure](storage-files-active-directory-overview.md).

Una alternativa al acceso directo es [Azure File Sync](https://aka.ms/AFS). Azure File Sync es un análogo directo de la capacidad de StorSimple de almacenar en caché los archivos que se usan con frecuencia de forma local. 

Azure File Sync es un servicio en la nube de Microsoft, que se basa en dos componentes principales:

* Sincronización de archivos y nube por niveles.
* Recursos compartidos de archivos como almacenamiento nativo en Azure, a los que se puede acceder a través de varios protocolos, como SMB y REST de archivos. 

Los recursos compartidos de archivos de Azure conservan aspectos importantes de la fidelidad de archivos en archivos almacenados como atributos, permisos y marcas de tiempo. Con los recursos compartidos de Azure, ya no es necesario una aplicación o servicio que interprete los archivos y carpetas que están almacenados en la nube. Puede acceder a ellos de forma nativa mediante protocolos y clientes conocidos como el Explorador de archivos de Windows. Los recursos compartidos de archivos de Azure permiten almacenar datos de aplicaciones y de servidores de archivos de uso general en la nube. La copia de seguridad de un recurso compartido de archivos de Azure es una funcionalidad integrada que se puede mejorar gracias a Azure Backup.

Este artículo se centra en los pasos de migración. Si antes de la migración desea obtener más información sobre Azure File Sync, se recomienda consultar los siguientes artículos:

* [Introducción a Azure File Sync](https://aka.ms/AFS "Información general")
* [Guía de implementación de Azure File Sync](storage-sync-files-deployment-guide.md)

### <a name="storsimple-service-data-encryption-key"></a>Cambio de la clave de cifrado de datos del servicio de StorSimple
La primera vez que se configura el dispositivo de StorSimple, se genera una *clave de cifrado de datos del servicio* y se le indica que almacene la clave de forma segura.
Esta clave se usa para cifrar todos los datos de la cuenta de almacenamiento de Azure asociada, en la que el dispositivo de StorSimple almacena los archivos.

La clave de cifrado de datos del servicio es necesaria para llevar a cabo una migración correcta. Ahora es un buen momento para recuperar esta clave de los registros, para cada uno de los dispositivos del inventario.

Si no puede encontrar las claves en los registros, puede recuperar la clave desde el dispositivo. Cada dispositivo tiene una clave de cifrado única.

* Envíe una solicitud de soporte técnico en Microsoft Azure a través de Azure Portal. El contenido de la solicitud debe tener los números de serie del dispositivo de StorSimple y la solicitud para recuperar la clave de cifrado de datos del servicio. 
* Un ingeniero de soporte técnico de StorSimple se pondrá en contacto con usted con una solicitud para participar en una reunión de uso compartido de la pantalla.
* Asegúrese de que, antes de comenzar la reunión, se conecte a su dispositivo de StorSimple [a través de una consola serie](../../storsimple/storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) o a través de una [sesión de PowerShell remota](../../storsimple/storsimple-8000-windows-powershell-administration.md#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple). 

> [!CAUTION]
> Cuando decida cómo conectarse a su dispositivo de StorSimple, tenga en cuenta lo siguiente:
> * La conexión a través de una sesión HTTPS es la opción más segura y la recomendada.
> * Es seguro conectarse directamente a la consola en serie del dispositivo, pero la conexión a la consola en serie a través conmutadores de red no lo es. 
> * Las conexiones de sesión HTTP son una opción, pero **no están cifradas** ; por lo tanto, no se recomiendan, a menos que se usen en una red cerrada y de confianza.

### <a name="storsimple-volume-backups"></a>Copias de seguridad de volúmenes de StorSimple
StorSimple ofrece copias de seguridad diferenciales en el nivel de los volúmenes. Los recursos compartidos de archivos de Azure también tienen esta capacidad, denominada "instantáneas de recurso compartido".

Decida si, como parte de la migración, también tiene la obligación de trasladar las copias de seguridad.

> [!CAUTION]
> Deténgase en este punto si debe migrar las copias de seguridad de los volúmenes de StorSimple.
>
> Actualmente, solo puede migrar la copia de seguridad de volúmenes más reciente. La compatibilidad con la migración de copias de seguridad llegará al final de 2020.
> Si empieza ahora, no podrá realizar copias de seguridad más adelante.
> En la próxima versión, las copias de seguridad se deben "reproducir" en los recursos compartidos de archivos de Azure del más antiguo al más reciente, con instantáneas de recursos compartido de archivos de Azure tomadas en el transcurso del proceso.

Si solo desea migrar los datos activos y no tiene ningún requisito para las copias de seguridad, puede continuar revisando esta guía.
Si solo tiene un requisito de retención de copias de seguridad muy breve (por ejemplo, un mes o dos), puede decidir continuar con la migración ahora y solo desaprovisionar los recursos de StorSimple después de ese período. Este enfoque permite crear tanta cantidad de historial de copias de seguridad en el recurso compartido de archivos de Azure como necesite. En el momento en que mantiene ambos sistemas en ejecución, se aplica un costo adicional, por lo que no se considera un enfoque que debe tener en cuenta si necesita una retención de copias de seguridad muy breve.

### <a name="map-your-existing-storsimple-volumes-to-azure-file-shares"></a>Asignación de volúmenes de StorSimple a recursos compartidos de archivos de Azure
[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="number-of-storage-accounts"></a>Número de cuentas de almacenamiento
Es probable que la migración se beneficie de una implementación de varias cuentas de almacenamiento, cada una con un número de recursos compartidos de archivos de Azure más reducido.

Si tiene recursos compartidos muy activos (utilizados por muchos usuarios o aplicaciones), dos recursos compartidos de archivos de Azure podrían alcanzar el límite de rendimiento de una cuenta de almacenamiento. Por este motivo, el procedimiento recomendado es migrar a varias cuentas de almacenamiento, cada una con sus propios recursos compartidos de archivos y, por lo general, no más de dos o tres recursos compartidos por cuenta de almacenamiento.

Un procedimiento recomendado es implementar cuentas de almacenamiento con un recurso compartido de archivos cada una. Puede agrupar varios recursos compartidos de archivos de Azure en la misma cuenta de almacenamiento, en caso de que tenga recursos compartidos de archivo.

Estas consideraciones se aplican más al [acceso directo a la nube](#direct-share-access-vs-azure-file-sync) (a través de una VM de Azure) que a Azure File Sync. Si tiene pensado usar Azure File Sync solo en estos recursos compartidos, es correcta la agrupación de varios en una sola cuenta de almacenamiento de Azure. Tenga en cuenta también que es posible que quiera migrar una aplicación mediante lift-and-shift a la nube, que posteriormente tendría acceso directo a un recurso compartido de archivos. También puede empezar a usar un servicio de Azure que también se beneficiaría de tener disponibles mayores cifras de IOPS y rendimiento. 

Si ha creado una lista de recursos compartidos, debe asignar cada recurso compartido a la cuenta de almacenamiento en la que residirán.

> [!IMPORTANT]
> Asegúrese de que selecciona una región de Azure y, después, de que cada cuenta de almacenamiento y recurso de Azure File Sync coinciden con la misma región que ha seleccionado.

### <a name="phase-1-summary"></a>Resumen de la fase 1
Al final de la fase 1:
* Conocerá bien sus volúmenes y dispositivos de StorSimple.
* El servicio de transformación de datos estará listo para acceder a los volúmenes de StorSimple en la nube, ya que ha recuperado la clave de cifrado de datos del servicio de cada dispositivo de StorSimple.
* Tendrá un plan para los volúmenes que se deben migrar y también sobre cómo asignar los volúmenes al número adecuado de recursos compartidos de archivos de Azure y cuentas de almacenamiento.

> [!CAUTION]
> Si tiene que migrar copias de seguridad desde volúmenes de StorSimple, **NO SIGA LEYENDO** .
>
> Este enfoque de migración se basa en nuevas funcionalidades del servicio de transformación de datos que actualmente no pueden migrar copias de seguridad. La compatibilidad con la migración de copias de seguridad llegará al final de 2020.
> Actualmente, solo puede migrar sus datos activos. Si empieza ahora, no podrá realizar copias de seguridad más adelante.
> Las copias de seguridad se deben "reproducir" en los recursos compartidos de archivos de Azure del más antiguo al más reciente en los datos activos, con instantáneas de recursos compartido de archivos de Azure tomadas en el transcurso del proceso.

Si solo desea migrar los datos activos y no tiene ningún requisito para las copias de seguridad, puede continuar revisando esta guía.

## <a name="phase-2-deploying-azure-storage-and-migration-resources"></a>Fase 2: Implementación de recursos de migración y almacenamiento de Azure

En esta sección se explican las consideraciones sobre la implementación de los diferentes tipos de recursos que se necesitan en Azure. Algunos contendrán los datos después de la migración y otros solo se necesitan para la migración. No empiece a implementar recursos hasta que haya finalizado el plan de implementación. En ocasiones, es difícil cambiar determinados aspectos de los recursos de Azure una vez que se han implementado.

### <a name="deploy-storage-accounts"></a>Implementación de cuentas de almacenamiento
Probablemente, tendrá que implementar varias cuentas de Azure Storage. Cada una de ellas contendrá un número menor de recursos compartidos de archivos de Azure, según el plan de implementación, completado en la sección anterior de este artículo. Vaya a Azure Portal para [implementar las cuentas de almacenamiento planeadas](../common/storage-account-create.md#create-a-storage-account). Considere la posibilidad de seguir la configuración básica de a continuación en cualquier cuenta de almacenamiento nueva:

##### <a name="subscription"></a>Subscription
Puede usar la misma suscripción que usó para la implementación de StorSimple o una diferente. La única limitación es que la suscripción debe estar en el mismo inquilino de Azure AD que la suscripción de StorSimple. Considere la posibilidad de mover la suscripción de StorSimple al inquilino correcto antes de iniciar una migración. Solo puede trasladar la suscripción en su conjunto; los recursos individuales de StorSimple no se pueden trasladar a otro inquilino o suscripción.

##### <a name="resource-group"></a>Resource group
Los grupos de recursos ayudan con la organización de recursos y permisos de administración de administradores. Obtenga más información sobre los [grupos de recursos de Azure](../../azure-resource-manager/management/manage-resource-groups-portal.md#what-is-a-resource-group).

##### <a name="storage-account-name"></a>Nombre de la cuenta de almacenamiento
El nombre de la cuenta de almacenamiento pasará a formar parte de una dirección URL y tendrá ciertas limitaciones de caracteres. En su convención de nomenclatura, tenga en cuenta que los nombres de las cuentas de almacenamiento deben ser únicos, tener solo letras y números en minúsculas, entre 3 y 24 caracteres y no usar caracteres especiales como guiones o caracteres de subrayado. Consulte también: [Reglas de nomenclatura de recursos de almacenamiento de Azure](../../azure-resource-manager/management/resource-name-rules.md#microsoftstorage).

##### <a name="location"></a>Location
La "ubicación" o la región de Azure de una cuenta de almacenamiento son muy importantes. Si usa Azure File Sync, todas las cuentas de almacenamiento deben estar en la misma región que el recurso del servicio de sincronización de almacenamiento. La región de Azure que elija debe ser cercana a los servidores o usuarios locales o en un punto central. Una vez implementado el recurso, no se puede cambiar su región.

Puede elegir una región distinta de la ubicación en la que residen los datos de StorSimple (cuenta de almacenamiento).

> [!IMPORTANT]
> Si elige una región diferente de la ubicación actual de la cuenta de almacenamiento de StorSimple, se aplicarán [cargos de salida](https://azure.microsoft.com/pricing/details/bandwidth) durante la migración. Los datos dejarán la región de StorSimple y entrarán en la nueva región de la cuenta de almacenamiento. No se aplican cargos de ancho de banda si permanece dentro de la misma región de Azure.

##### <a name="performance"></a>Rendimiento
Tiene la opción de elegir Premium Storage (SSD) para recursos compartidos de archivos de Azure o almacenamiento estándar. El almacenamiento estándar incluye [varios niveles para un recurso compartido de archivos](storage-how-to-create-file-share.md#changing-the-tier-of-an-azure-file-share). Standard Storage es la opción adecuada para la mayoría de los clientes que migran desde StorSimple.

¿Todavía no está seguro?
* Elija Premium Storage si necesita el [rendimiento de un recurso compartido de archivos Premium de Azure](storage-files-planning.md#understanding-provisioning-for-premium-file-shares).
* Elija almacenamiento estándar para cargas de trabajo de servidor de archivos de uso general, incluidos los datos de acceso frecuente y los datos de archivo. Elija también almacenamiento estándar si la única carga de trabajo en el recurso compartido en la nube será Azure File Sync.

##### <a name="account-kind"></a>Tipo de cuenta
* Para el almacenamiento estándar, elija: *StorageV2 (de uso general v2)*
* Para recursos compartidos de archivos Premium, elija: *FileStorage*

##### <a name="replication"></a>Replicación
Hay varias configuraciones de replicación disponibles. Obtenga más información sobre los diferentes tipos de replicación.

Solo tiene que elegir una de las dos opciones siguientes:
* *Almacenamiento con redundancia local (LRS)*
* *Almacenamiento con redundancia de zona (ZRS)* , que no está disponible en todas las regiones de Azure.

> [!NOTE]
> Solo los tipos de redundancia LRS y ZRS son compatibles con los recursos compartidos de archivos de Azure de gran tamaño, de 100 TiB de capacidad.

El almacenamiento con redundancia global (todas las variaciones) no se admite actualmente. Puede cambiar el tipo de redundancia más tarde y a GRS cuando la compatibilidad con este tipo llegue a Azure.

##### <a name="enable-100tib-capacity-file-shares"></a>Habilitación de recursos compartidos de archivos de 100 TiB de capacidad

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png" alt-text="Imagen que muestra la pestaña Opciones avanzadas en Azure Portal para la creación de una cuenta de almacenamiento.":::
    :::column-end:::
    :::column:::
        En la sección *Opciones avanzadas* del Asistente para nuevas cuentas de almacenamiento de Azure Portal, puede habilitar la compatibilidad con *recursos compartidos de archivos grandes* en esta cuenta de almacenamiento. Si esta opción no está disponible, lo más probable es que haya seleccionado el tipo de redundancia equivocado. Asegúrese de seleccionar solo LRS o ZRS para que esta opción esté disponible.
    :::column-end:::
:::row-end:::

Optar por los recursos compartidos de archivos de gran tamaño de 100 TiB tiene varias ventajas:
* El rendimiento aumenta en gran medida en comparación con los recursos compartidos de archivos de menor capacidad, de 5 TiB. (por ejemplo: 10 veces la E/S por segundo)
* La migración finalizará mucho más rápido.
* Asegúrese de que un recurso compartido de archivos tenga suficiente capacidad para contener todos los datos que va a migrar a él.
* El crecimiento futuro está cubierto con esta opción.

### <a name="azure-file-shares"></a>Recursos compartidos de archivos de Azure
Una vez creadas las cuentas de almacenamiento, puede ir a la sección *Recurso compartido de archivos* de la cuenta de almacenamiento e implementar el número adecuado de recursos compartidos de archivos de Azure según el plan de migración de la fase 1. Considere la posibilidad de usar la siguiente configuración básica para los nuevos recursos compartidos de archivos de Azure:

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-share.png" alt-text="Imagen que muestra la pestaña Opciones avanzadas en Azure Portal para la creación de una cuenta de almacenamiento.":::
    :::column-end:::
    :::column:::
        </br>**Nombre**</br>Se admiten minúsculas, números y guiones.</br></br>**Cuota**</br>La cuota aquí es comparable a la cuota máxima de SMB en un servidor de Windows. El procedimiento recomendado es no establecer una cuota aquí, ya que la migración y otros servicios producirán un error cuando se alcance la cuota.</br></br>**Nivel**</br>Seleccione *Optimizado para transacciones* en el nuevo recurso compartido de archivos. Durante la migración, se producirán muchas transacciones y es más rentable cambiar el nivel más adelante al más adecuado para la carga de trabajo.
    :::column-end:::
:::row-end:::

### <a name="storsimple-data-manager"></a>StorSimple Data Manager
El recurso de Azure que contendrá los trabajos de migración se llama *StorSimple Data Manager* . Haga clic en nuevo recurso, búsquelo y, a continuación, haga clic en *Crear* .

Se trata de un recurso temporal que se usa para la orquestación y se desaprovisiona una vez finalizada la migración. Debe implementarse en la misma suscripción, grupo de recursos y región que la cuenta de almacenamiento de StorSimple.

### <a name="azure-file-sync"></a>Azure File Sync
Azure File Sync (AFS) permite agregar almacenamiento en caché local de los archivos a los que se accede con más frecuencia. De forma similar a las funcionalidades de almacenamiento en caché de StorSimple, la característica de nube por niveles de AFS ofrece latencia de acceso local en combinación con el control mejorado de la funcionalidad de caché disponible en Windows Server y en la sincronización de varios sitios. Si el objetivo es tener una caché local, en su red local, prepare una máquina virtual de Windows Server (también se admiten servidores físicos y clústeres de conmutación por error), con capacidad de almacenamiento conectado directamente (DAS) suficiente. 

> [!IMPORTANT]
> No configure Azure File Sync aún, es mejor hacerlo después de completar la migración del recurso compartido. La implementación de AFS no debe iniciarse antes de la fase 4 de una migración.

### <a name="phase-2-summary"></a>Resumen de la fase 2
Al final de la fase 2, habrá implementado las cuentas de almacenamiento, todos los recursos compartidos de archivos de Azure en ellas y también un recurso de StorSimple Data Manager. Usará este último en la fase 3, cuando configure realmente los trabajos de migración.

## <a name="phase-3-create-and-run-a-migration-job"></a>Fase 3: Creación y ejecución de un trabajo de migración
En esta sección se describe cómo configurar un trabajo de migración y asignar cuidadosamente los directorios de un volumen de StorSimple que debe copiarse en el recurso compartido de archivos de Azure de destino que seleccione. Para empezar, vaya a StorSimple Data Manager, busque **Definiciones de trabajo** en el menú y seleccione **+ Definición de trabajo** . El tipo de almacenamiento de destino es el valor predeterminado: *Recurso compartido de archivos de Azure* .

![Tipos de trabajo de migración de StorSimple serie 8000](media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-job-type.png "Una captura de pantalla de las definiciones de trabajo en Azure Portal con un nuevo cuadro de diálogo Definición de trabajos abierto que solicita el tipo de trabajo: Cópielo en un recurso compartido de archivos o en un contenedor de blobs.").

> [!IMPORTANT]
> Antes de ejecutar cualquier trabajo de migración, detenga todas las copias de seguridad programadas de los volúmenes de StorSimple.

:::row:::
    :::column:::
        ![Guía de migración de StorSimple serie 8000.](media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-job.png "Captura de pantalla del nuevo formulario de creación de trabajos para un trabajo de transformación de datos.")
    :::column-end:::
    :::column:::
        **Job definition name (Nombre de la definición de trabajo)**</br>Este nombre debe ser indicativo del conjunto de archivos que se van a mover. Le recomendamos que le asigne un nombre similar al recurso compartido de archivos de Azure. </br></br>**Ubicación en la que se ejecuta el trabajo**</br>Al seleccionar una región, debe seleccionar la misma región que la cuenta de almacenamiento de StorSimple o, si no está disponible, una región cercana a ella. </br></br><h3>Source</h3>**Suscripción de origen**</br>Seleccione la suscripción en la que almacena el recurso de StorSimple Device Manager. </br></br>**Recurso de StorSimple**</br>Seleccione la instancia de StorSimple Device Manager en la que el dispositivo esté registrado. </br></br>**Clave de cifrado de datos de servicio**</br>Consulte esta [sección anterior de este artículo](#storsimple-service-data-encryption-key), en caso de que no pueda localizar la clave en los registros. </br></br>**Dispositivo**</br>Seleccione el dispositivo de StorSimple que contiene el volumen que desea migrar. </br></br>**Volumen**</br>Seleccione el volumen de origen. Más adelante decidirá si desea migrar el volumen o los subdirectorios completos al recurso compartido de archivos de Azure de destino. </br></br><h3>Destino</h3>Seleccione la suscripción, la cuenta de almacenamiento y el recurso compartido de archivos de Azure como destino de este trabajo de migración.
    :::column-end:::
:::row-end:::

> [!IMPORTANT]
> La última copia de seguridad del volumen se usará para realizar la migración. Asegúrese de que haya al menos una copia de seguridad del volumen o se producirá un error en el trabajo. Asegúrese también de que la última copia de seguridad que tiene es bastante reciente, para mantener el tamaño del delta en el recurso compartido activo lo más pequeño posible. Podría ser conveniente desencadenar manualmente y completar otra copia de seguridad del volumen **antes** de ejecutar el trabajo que acaba de crear.

### <a name="directory-mapping"></a>Asignación de directorios
Esto es opcional para el trabajo de migración. Si deja el campo vacío, **todos** los archivos y las carpetas de la raíz del volumen de StorSimple se moverán a la raíz del recurso compartido de archivos de Azure de destino. En la mayoría de los casos, almacenar el contenido de un volumen completo en un recurso compartido de archivos de Azure no es el mejor enfoque. A menudo, es mejor dividir el contenido de un volumen en varios recursos compartidos de archivos de Azure. Si aún no ha hecho un plan, consulte esta sección primero: [Asignación del volumen de StorSimple a recursos compartidos de archivos de Azure](#map-your-existing-storsimple-volumes-to-azure-file-shares)

Como parte del plan de migración, es posible que haya decidido que las carpetas de un volumen de StorSimple deben dividirse en varios recursos compartidos de archivos de Azure. En ese caso, puede realizar esa división de la siguiente manera:
1. Definiendo varios trabajos para migrar las carpetas de un volumen; cada uno tendrá el mismo origen de volumen de StorSimple, pero un recurso compartido de archivos de Azure diferente como destino.
1. Especificando con precisión qué carpetas del volumen de StorSimple se deben migrar al recurso compartido de archivos especificado, mediante la sección de *asignación de directorios* del formulario de creación del trabajo y siguiendo la [semántica de asignación específica](#semantic-elements).

> [!IMPORTANT]
> Las rutas de acceso y las expresiones de asignación de este formulario no se pueden validar cuando se envía el formulario. Si se especifican las asignaciones incorrectamente, puede producirse un error en el trabajo o un resultado no deseado. En ese caso, suele ser mejor eliminar el recurso compartido de archivos de Azure, volver a crearlo y, a continuación, corregir las instrucciones de asignación en un nuevo trabajo de migración dl recurso compartido. La ejecución de un nuevo trabajo con una instrucción de asignación fija puede corregir carpetas omitidas y colocarlas en el recurso compartido existente. Sin embargo, solo las carpetas que se omitieron debido a errores de escritura en la ruta de acceso se pueden solucionar de esta manera.

#### <a name="semantic-elements"></a>Elementos semánticos
Una asignación se expresa de izquierda a derecha: [\ruta de acceso de origen] \> [\ruta de acceso de destino].

|Carácter semántico          | Significado  |
|:---------------------------|:---------|
| **\\**                     | Indicador de nivel raíz        |
| **\>**                     | Operador de asignación de [origen] y [destino]        |
|**\|** o RETURN (nueva línea) | Separador de dos instrucciones de asignación de carpetas. </br>Como alternativa, puede omitir este carácter y presionar Entrar para obtener la siguiente expresión de asignación en su propia línea.        |

### <a name="examples"></a>Ejemplos
Mueve el contenido de la carpeta "Datos de usuario" a la raíz del recurso compartido de archivos de destino:
``` console
\User data > \\
```
Mueve todo el contenido del volumen a una nueva ruta de acceso en el recurso compartido de archivos de destino:
``` console
\ \> \Apps\HR tracker
```
Mueve el contenido de la carpeta de origen a una nueva ruta de acceso en el recurso compartido de archivos de destino:
``` console
\HR resumes-Backup \> \Backups\HR\resumes
```
Ordena varias ubicaciones de origen en una nueva estructura de directorios:
``` console
\HR\Candidate Tracker\v1.0 > \Apps\Candidate tracker
\HR\Candidates\Resumes > \HR\Candidates\New
\Archive\HR\Old Resumes > \HR\Candidates\Archived
```

### <a name="semantic-rules"></a>Reglas semánticas
* Especifique siempre las rutas de acceso de la carpeta en relación con el nivel raíz. 
* Comience cada ruta de acceso de carpeta con un indicador de nivel de raíz "\". 
* No incluya letras de unidad. 
* Al especificar varias rutas de acceso, las rutas de acceso de origen o de destino no se pueden superponer:</br>
   Ejemplo de superposición de ruta de origen no válida:</br>
    *\\carpeta\1 > \\carpeta*</br>
    *\\carpeta\\1\\2 > \\carpeta2*</br>
   Ejemplo de superposición de ruta de destino no válida:</br>
   *\\carpeta > \\*</br>
   *\\carpeta2 > \\*</br>
* Se omitirán las carpetas de origen que no existen. 
* Se crearán las estructuras de carpetas que no existen en el destino. 
* Al igual que Windows, los nombres de carpeta no distinguen mayúsculas de minúsculas, pero sí la conserva.

> [!NOTE]
> El trabajo de transformación no copiará el contenido de la carpeta " *\System Volume Information* " y " *$Recycle.Bin* " en el volumen de StorSimple.

### <a name="phase-3-summary"></a>Resumen de la fase 3
Al final de la fase 3, habrá ejecutado los trabajos de transformación de datos desde volúmenes de StorSimple en recursos compartidos de archivos de Azure. Ahora puede centrarse en la configuración de Azure File Sync del recurso compartido (después de que se hayan completado los trabajos de migración de un recurso compartido) o el acceso directo a recursos compartidos de las aplicaciones y los trabajadores de la información al recurso compartido de archivos de Azure.

## <a name="phase-4-accessing-your-azure-file-shares"></a>Fase 4: Acceso a los recursos compartidos de archivos de Azure

Hay dos estrategias principales para acceder a los recursos compartidos de archivos de Azure:

* **Azure File Sync:** [Cómo implementar Azure File Sync](#deploy-azure-file-sync) en un servidor Windows local. AFS tiene todas las ventajas de una memoria caché local, al igual que StorSimple.
* **Acceso directo a recursos compartidos:** [Cómo implementar el acceso directo a recursos compartidos](#deploy-direct-share-access). Use esta estrategia si su escenario de acceso para un recurso compartido de archivos de Azure determinado no se beneficiará del almacenamiento en caché local o ya no es posible hospedar un servidor de Windows local. Aquí, los usuarios y las aplicaciones seguirán teniendo acceso a los recursos compartidos de SMB a través del protocolo SMB, pero estos recursos compartidos ya no se encuentran en un servidor local, sino directamente en la nube.

Ya debe haber decidido qué opción es la más adecuada en la [fase 1](#phase-1-prepare-for-migration) de esta guía.

El resto de esta sección se centra en las instrucciones de implementación.

### <a name="deploy-azure-file-sync"></a>Implementación de Azure File Sync

Es el momento de implementar una parte de Azure File Sync:
1. Cree el recurso de nube de Azure File Sync.
1. Implemente el agente de Azure File Sync en el servidor local.
1. Registro del servidor con el recurso de nube

No cree todavía ningún grupo de sincronización. La configuración de la sincronización con un recurso compartido de archivos de Azure solo debe realizarse una vez que se hayan completado los trabajos de migración a un recurso compartido de archivos de Azure. Si va a empezar a usar File Sync antes de que se complete la migración, hará que la migración sea innecesariamente difícil, ya que no es fácil saber cuándo es el momento de iniciar un recorte.

#### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Implementación del recurso de nube de Azure File Sync

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> Si desea cambiar la región de Azure en la que residen los datos cuando se realiza la migración, implemente el servicio de sincronización de almacenamiento en la misma región que las cuentas de almacenamiento de destino para esta migración.

#### <a name="deploy-an-on-premises-windows-server"></a>Implementación de una instancia de Windows Server local

* Cree un servidor de Windows Server 2019 (como mínimo de la edición 2012 R2), como una máquina virtual o un servidor físico. También se admite un clúster de conmutación por error de Windows Server. No reutilice el servidor que tenga delante de StorSimple 8100 u 8600.
* Aprovisione o agregue almacenamiento de conexión directa (DAS en lugar de NAS, que no se admite).

Es un procedimiento recomendado otorgar al nuevo servidor de Windows Server una cantidad igual o superior de almacenamiento de la que dispone la aplicación StorSimple 8100 o 8600 localmente para el almacenamiento en caché. Utilizará el servidor de Windows Server de la misma manera que usó la aplicación StorSimple. Si el servidor tiene la misma cantidad de almacenamiento que la aplicación, la experiencia de almacenamiento en caché será parecida o incluso idéntica.
Puede agregar o eliminar almacenamiento del servidor de Windows Server según lo desee. Esto le permitirá escalar el tamaño del volumen local y la cantidad de almacenamiento local disponible para el almacenamiento en caché.

#### <a name="prepare-the-windows-server-for-file-sync"></a>Preparación del servidor de Windows Server para la sincronización de archivos

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

#### <a name="configure-azure-file-sync-on-the-windows-server"></a>Configuración de Azure File Sync en el servidor de Windows Server

El servidor registrado de Windows Server debe estar preparado y conectado a Internet para este proceso.

> [!IMPORTANT]
> Antes de continuar, debe completar la migración de archivos y carpetas de StorSimple en el recurso compartido de archivos de Azure. Asegúrese de que no haya más cambios en el recurso compartido de archivos.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Asegúrese de activar la nube por niveles. La nube por niveles es la característica de Azure File Sync que permite al servidor local tener menos capacidad de almacenamiento de la que está almacenada en la nube y, a pesar de ello, disponer de todo el espacio de nombres. Los datos localmente interesantes también se almacenan en caché para conseguir un rendimiento rápido y de acceso local. Otro motivo para activar la nube por niveles en este paso es que no queremos sincronizar el contenido del archivo en esta fase, en este momento solo se debe mover el espacio de nombres.

### <a name="deploy-direct-share-access"></a>Implementación de acceso directo a recursos compartidos

:::row:::
    :::column:::
        [![Guía paso a paso y demostración sobre cómo exponer de forma segura recursos compartidos de archivos de Azure directamente para los aplicaciones y trabajadores de la información: haga clic para empezar.](./media/storage-files-migration-storsimple-8000/azure-files-direct-access-video-placeholder.png)](https://youtu.be/KG0OX0RgytI)
    :::column-end:::
    :::column:::
        Este vídeo es una guía y demostración sobre cómo exponer de forma segura recursos compartidos de archivos de Azure directamente para las aplicaciones y trabajadores de la información en cinco sencillos pasos.</br>
        En el vídeo se hace referencia a documentación dedicada para algunos temas:
* [Introducción a las identidades](storage-files-active-directory-overview.md)
* [Unión a un dominio de una cuenta de almacenamiento](storage-files-identity-auth-active-directory-enable.md)
* [Información general sobre redes para recursos compartidos de archivos de Azure](storage-files-networking-overview.md)
* [Configuración de puntos de conexión públicos y privados](storage-files-networking-endpoints.md)
* [Configuración de una VPN S2S](storage-files-configure-s2s-vpn.md)
* [Configuración de una VPN de Windows P2S](storage-files-configure-p2s-vpn-windows.md)
* [Configuración de una VPN P2S de Linux](storage-files-configure-p2s-vpn-linux.md)
* [Configuración del reenvío de DNS](storage-files-networking-dns.md)
* [Configuración de DFS-N](https://aka.ms/AzureFiles/Namespaces)
   :::column-end:::
:::row-end:::

### <a name="phase-4-summary"></a>Resumen de la fase 4
En esta fase ha creado y ejecutado varios trabajos del *servicio de transformación de datos* (DTS) en *StorSimple Data Manager* . Esos trabajos han migrado los archivos y las carpetas a recursos compartidos de archivos de Azure. Además, ha implementado Azure File Sync o preparado las cuentas de red y almacenamiento para el acceso directo a recursos compartidos.

## <a name="phase-5-user-cut-over"></a>Fase 5: Migración total de los usuarios
Esta fase se centra simplemente en resumir la migración:
* Planee el tiempo de inactividad.
* Póngase al día con cualquier cambio que los usuarios y las aplicaciones hayan generado en StorSimple mientras se han estado ejecutando los trabajos de transformación de datos en la fase 3. 
* Conmute por error a los usuarios al nuevo servidor Windows Server con Azure File Sync o los recursos compartidos de archivos de Azure a través del acceso directo a recursos compartidos.

### <a name="plan-your-downtime"></a>Planeación del tiempo de inactividad
Este enfoque de migración requiere cierto tiempo de inactividad para los usuarios y las aplicaciones. El objetivo es mantener un tiempo de inactividad mínimo y las siguientes consideraciones pueden ser de ayuda:

* Mantenga los volúmenes de StorSimple disponibles mientras ejecuta los trabajos de transformación de datos.
* Cuando haya terminado de ejecutar los trabajos de migración de datos de un recurso compartido, podrá quitar el acceso de usuario (al menos el acceso de escritura) de los volúmenes o recursos compartidos de StorSimple. Un comando RoboCopy final pondrá en marcha el recurso compartido de archivos de Azure y, después, podrá migrar a los usuarios. El lugar en el que se ejecute RoboCopy dependerá de si ha elegido usar Azure File Sync o el acceso directo a recursos compartidos. En la próxima sección sobre RoboCopy se explica.
* Una vez completada la actualización de RoboCopy, ya podrá exponer la nueva ubicación a los usuarios. El recurso compartido de archivos de Azure directamente o un recurso compartido de SMB en un servidor Windows Server con AFS. A menudo, una implementación de DFS-N lo ayudará a realizar una migración rápida y eficaz. Mantendrá coherentes las direcciones de los recursos compartidos actuales y llevará a cabo la redirección a una nueva ubicación que contenga los archivos y carpetas migrados.

### <a name="determine-when-your-namespace-has-fully-synced-to-your-server"></a>Determinación de si el espacio de nombres se ha sincronizado por completo en el servidor

Cuando se usa Azure File Sync para un recurso compartido de archivos de Azure, es importante que se determine que todo el espacio de nombres ha terminado de descargarse en el servidor ANTES de iniciar cualquier comando RoboCopy local. El tiempo necesario para descargar el espacio de nombres depende del número de elementos del recurso compartido de archivos de Azure. Existen dos métodos para determinar si el espacio de nombres se ha trasladado por completo al servidor:

#### <a name="azure-portal"></a>Azure portal
Puede usar Azure Portal para ver cuándo ha llegado por completo el espacio de nombres.
* Inicie sesión en Azure Portal, navegue hasta el grupo de sincronización y compruebe el estado de sincronización del grupo de sincronización y el punto de conexión del servidor. 
* Se descarga la dirección de interés: si el punto de conexión del servidor se ha aprovisionado recientemente, se mostrará la **sincronización inicial** que indica que el espacio de nombres sigue estando disponible.
Una vez que se cambia todo, menos la *sincronización inicial* , el espacio de nombres se rellenará por completo en el servidor y estará listo para continuar con un comando RoboCopy local.

#### <a name="windows-server-event-viewer"></a>Visor de eventos de Windows Server
También puede usar el Visor de eventos en Windows Server para saber cuándo se ha trasladado por completo el espacio de nombres.

1. Abra el **Visor de eventos** y vaya a **Aplicaciones y servicios** .
1. Vaya y abra **Microsoft\FileSync\Agent\Telemetry** .
1. Busque el **evento 9102** más reciente, el cual se corresponde con una sesión de sincronización completada.
1. Seleccione **Detalles** y confirme que está examinando un evento en el que el valor **SyncDirection** es **Descargar** .
1. En el momento en que el espacio de nombres ha finalizado su descarga en el servidor, habrá un único evento con **Escenario** , valor **FullGhostedSync** y **HResult** = **0** .
1. Si no está ese evento, también puede buscar otros **eventos 9102** con **SyncDirection** = **Descargar** y **Escenario** =  **"RegularSync"** . La búsqueda de uno de estos eventos también indica que el espacio de nombres se ha terminado de descargar y la sincronización ha progresado a sesiones regulares de sincronización, haya o no algo que sincronizar, en este momento.

### <a name="a-final-robocopy"></a>Un comando RoboCopy final.
En este momento, hay diferencias entre el servidor de Windows Server local y la aplicación StorSimple 8100 u 8600:

1. Debe ponerse al día con los cambios que los usuarios o las aplicaciones generan en StorSimple mientras la migración estaba en curso.
1. En los casos en los que se usa Azure File Sync: La aplicación StorSimple tiene una memoria caché llena en comparación con Windows Server que, en este momento, solo tiene un espacio de nombres sin ningún contenido de archivos almacenado localmente. Por lo tanto, el comando RoboCopy final puede ayudar a iniciar la memoria caché de AFS local al extraer tanto contenido del archivo almacenado en memoria caché local como esté disponible y puede ajustarse al servidor AFS.
1. Es posible que el trabajo de transformación de datos haya omitido algunos archivos debido a caracteres no válidos. Si es así, cópielos en el servidor Windows Server habilitado para Azure File Sync. Más adelante, puede ajustarlos para que se sincronicen. Si no usa Azure File Sync para un recurso compartido determinado, es mejor cambiar el nombre de los archivos por caracteres no válidos en el volumen de StorSimple y, a continuación, ejecutar el comando RoboCopy directamente en el recurso compartido de archivos de Azure. 

> [!WARNING]
> No debe iniciar RoboCopy antes de que el servidor tenga el espacio de nombres de un recurso compartido de archivos de Azure descargado completamente.
> Consulte: "[Determinación de si el espacio de nombres se ha sincronizado por completo en el servidor](#determine-when-your-namespace-has-fully-synced-to-your-server)"

 Solo desea copiar los archivos que se cambiaron después de la última vez que se ejecutó el trabajo de migración y los archivos que no se trasladaron antes a través de dichos trabajos. Podremos solucionar los problemas por los que no se trasladaron al servidor, una vez que haya finalizado la migración. Consulte [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

RoboCopy tiene varios parámetros. A continuación se muestra un comando terminado y una lista de razones para elegir estos parámetros:

```console
Robocopy /MT:16 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
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

Cuando configure las ubicaciones de origen y de destino del comando RoboCopy, asegúrese de revisar la estructura de origen y de destino para asegurarse de que coinciden. Si usó la característica de asignación de directorios del trabajo de migración, la estructura del directorio raíz puede ser diferente de la estructura del volumen de StorSimple. En ese caso, puede que necesite varios trabajos de RoboCopy, uno para cada subdirectorio. Dado que este comando RoboCopy usa /MIR, no moverá archivos que sean iguales (archivos en capas, por ejemplo), pero si obtiene la ruta de acceso de origen y de destino equivocada, /MIR también purga las estructuras de directorio en el recurso compartido de archivos de Windows Server o Azure, que no están presentes en la ruta de acceso de origen de StorSimple. Por lo tanto, deben coincidir exactamente para que el trabajo de RoboCopy alcance su objetivo de solo actualizar el contenido migrado con los últimos cambios realizados durante la migración. 

Consulte los archivos de registro de RoboCopy para ver si se han omitido archivos. Si existen problemas, corríjalos y vuelva a ejecutar el comando RoboCopy. No desaprovisione los recursos de StorSimple antes de corregir los problemas pendientes de los archivos o carpetas que le interesan.

Si no usa Azure File Sync para almacenar en caché el recurso compartido de archivos de Azure en cuestión, sino que ha optado por el acceso directo a recursos compartidos:
1. [Monte el recurso compartido de archivos de Azure](storage-how-to-use-files-windows.md#mount-the-azure-file-share) como una unidad de red en una máquina local de Windows.
1. Ejecute el comando de RoboCopy entre StorSimple y el recurso compartido de archivos de Azure montado. Si los archivos no se copian, corrija sus nombres en StorSimple para quitar los caracteres no válidos y vuelva a intentar ejecutar RoboCopy. El comando RoboCopy de la lista anterior se puede ejecutar varias veces, con lo que se produce una recuperación innecesaria en StorSimple.

### <a name="user-cut-over"></a>Migración total de los usuarios

Si usa Azure File Sync, es probable que tenga que crear los recursos compartidos de SMB en el servidor Windows habilitado para AFS que coincida con los recursos compartidos que tenía en los volúmenes de StorSimple. Puede realizar este paso de antemano para no perder tiempo aquí, pero debe asegurarse de que, antes de llegar a este punto, no se produzcan cambios en los archivos en el servidor de Windows Server.

Si tiene una implementación de DFS-N, puede apuntar los espacios de nombres de DFN a las nuevas ubicaciones de carpetas del servidor. Si no tiene una implementación de DFS-N y ha adelantado el dispositivo 8100/8600 localmente con un servidor de Windows, puede desconectar el servidor del dominio. Después, únase al dominio nuevo en el servidor Windows Server habilitado para Azure File Sync. Durante ese proceso, asigne al servidor el mismo nombre de servidor y los nombres de los recursos compartidos que en el servidor anterior, de modo que el acceso directo sea transparente para los usuarios, la directiva de grupo y los scripts.

[Más información sobre DFS-N](https://aka.ms/AzureFiles/Namespaces)

## <a name="deprovision"></a>Desaprovisionamiento
Cuando se desaprovisiona un recurso, se pierde el acceso a la configuración de ese recurso, así como a sus datos. No se puede deshacer el aprovisionamiento, por lo que no continúe hasta que haya confirmado que la migración se ha completado y no hay ninguna dependencia en los archivos, las carpetas o las copias de seguridad de volumen de StorSimple que va a desaprovisionar.

Antes de empezar, es un procedimiento recomendado observar la nueva implementación de Azure File Sync en producción durante un tiempo. Esto le proporciona opciones para solucionar cualquier problema que pueda surgir.
Una vez que haya observado la implementación de Azure File Sync durante al menos unos días, puede empezar a desaprovisionar los recursos en este orden:

1. Desaprovisione el recurso de StorSimple Data Manager a través de Azure Portal.
   Todos los trabajos de DTS se eliminarán con él. No podrá recuperar fácilmente los registros de copia. Si es importante para los registros, puede recuperarlos antes de desaprovisionarlos.
1. Asegúrese de que se han migrado las aplicaciones físicas de StorSimple y, a continuación, anule su registro.
   Si no está totalmente seguro de que se han migrado, no continúe. Si quita el aprovisionamiento de estos recursos mientras siguen siendo necesarios, no podrá recuperar los datos ni su configuración.
1. Si no quedan más dispositivos registrados en StorSimple Device Manager, puede continuar para quitar ese recurso de Device Manager.
1. Ahora es el momento de eliminar la cuenta de almacenamiento de StorSimple en Azure. Una vez más, deténgase en este punto y confirme que la migración se ha completado y no hay nada que dependa de estos datos antes de continuar.
1. Desconecte el dispositivo físico de StorSimple del centro de datos.
1. Si es el propietario del dispositivo de StorSimple, puede reciclar el equipo. 
   Si el dispositivo está concedido, informe al arrendador y devuélvalo según corresponda.

La migración se ha completado.

> [!NOTE]
> ¿Todavía tiene preguntas o ha detectado algún problema?</br>
> Estamos aquí para ayudarle: AzureFilesMigration@microsoft.com


## <a name="next-steps"></a>Pasos siguientes

* Familiarícese con [Azure File Sync: aka.ms/AFS](https://aka.ms/AFS).
* Obtenga información sobre la flexibilidad de las [directivas de nube por niveles](storage-sync-cloud-tiering.md).
* [Habilite Azure Backup](../../backup/backup-afs.md#configure-backup-from-the-file-share-pane) en los recursos compartidos de archivos de Azure para programar instantáneas y definir programaciones de retención de copia de seguridad.
* Si ve en Azure Portal que algunos archivos no se sincronizan de forma permanente, revise la [guía de solución de problemas](storage-sync-files-troubleshoot.md) para conocer los pasos necesarios para resolverlos.
