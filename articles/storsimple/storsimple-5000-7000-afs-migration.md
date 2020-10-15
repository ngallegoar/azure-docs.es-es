---
title: Migración de datos de la serie 5000-7000 de StorSimple a Azure File Sync | Microsoft Docs
description: Describe cómo migrar datos de la serie 5000/7000 de StorSimple a Azure File Sync (AFS).
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: 5d656fd8757580b8ce96acf168e92fc847d400ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85514066"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-azure-file-sync"></a>Migración de datos de la serie 5000-7000 de StorSimple a Azure File Sync

> [!IMPORTANT]
> El 9 de julio de 2019, finalizará el estado de compatibilidad de la serie StorSimple 5000/7000. Se recomienda que los clientes de la serie StorSimple 5000/7000 migren a una de las alternativas descritas en el documento.

La migración de datos es el proceso de mover datos de una ubicación de almacenamiento a otra. Implica realizar una copia exacta de los datos actuales de una organización de un dispositivo en otro dispositivo, preferiblemente sin interrumpir ni deshabilitar aplicaciones activas, y luego redirigir toda la actividad de entrada/salida (E/S) al nuevo dispositivo. 

Los dispositivos de almacenamiento de la serie 5000 y 7000 de StorSimple llegarán al final del servicio en julio de 2019. Esto implica que Microsoft ya no podrá dar soporte técnico al hardware y software de la serie 5000/7000 de StorSimple después de julio de 2019. Los clientes que utilicen estos dispositivos tendrán que migrar sus datos de StorSimple a otras soluciones de almacenamiento híbrido de Azure. En este artículo se aborda la migración de datos desde un dispositivo de la serie 5000/7000 de StorSimple a Azure File Sync (AFS).

## <a name="intended-audience"></a>Destinatarios

Este artículo está destinado a profesionales de informática (TI) y los trabajadores del conocimiento responsables de la implementación y administración de dispositivos de la serie 5000/7000 de StorSimple en el centro de datos. Los clientes que utilicen sus dispositivos de StorSimple para cargas de trabajo de servidor de archivos (con Windows Server) pueden encontrar esta ruta de migración especialmente atractiva. Si cree las características de Azure File Sync funcionan bien para su organización, este artículo le ayudará a entender cómo pasar a esas soluciones desde StorSimple.

## <a name="migration-considerations"></a>Consideraciones sobre la migración

Este proceso funciona para clientes que hayan configurado un recurso compartido de archivos de Windows con un volumen de almacenamiento de StorSimple. La migración de datos desde StorSimple 5000/7000 a Azure File Sync implica convertir esa ubicación de recurso compartido de archivos en un [punto de conexión de servidor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) y luego utilizar unidades conectadas localmente como otro punto de conexión que se convierta en su nueva ubicación. 

Al pasar a AFS, deben tenerse en cuenta los siguientes aspectos:

1. Azure Files actualmente tiene una restricción de 5 TB/recurso compartido. Esta restricción puede superarse mediante el uso de Azure File Sync con datos distribuidos en varios recursos compartidos de archivos de Azure. Para más información, consulte [Planeamiento de una implementación de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-planning).
2. Esta migración descarga todo el conjunto de datos principal en un dispositivo local porque la copia de datos se realiza desde el dispositivo local. Asegúrese de que dispone de ancho de banda suficiente para dar cabida a esta transferencia.
3. Este proceso no conserva las instantáneas que ya se han creado. Solo migra los datos principales. El proceso tampoco conserva las plantillas de ancho de banda ni las directivas de copia de seguridad asociadas. [Utilice Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-files) para configurar las directivas de copia de seguridad después de migrar los datos del recurso compartido de archivos de Azure.
4. StorSimple ofrece hardware de primera entidad. Aunque, con Azure Files/Azure File Sync, utiliza su propio hardware de Windows Server local como caché local. Debe asegurarse de que dispone de capacidad de almacenamiento suficiente para mantener el conjunto de datos de su elección en el entorno local. Para más información sobre los niveles y la configuración del objetivo de espacio libre requerido, consulte cómo [crear un punto de conexión de servidor al implementar Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal). 
5. Consulte los [precios de Azure File Sync](https://azure.microsoft.com/pricing/details/storage/files/) ya que varían por StorSimple. AFS no tiene desduplicación y compresión como StorSimple.

## <a name="migration-prerequisites"></a>Requisitos previos de la migración

Estos son los requisitos previos de migración del dispositivo antiguo de la serie 5000 o 7000 a Azure File Sync. Antes de comenzar, asegúrese de que dispone de:

- Acceso a un dispositivo de la serie 5000/7000 de StorSimple que ejecute el software versión v2.1.1.518 o posterior. Las versiones anteriores no se admiten. La esquina superior derecha de la interfaz de usuario web del dispositivo StorSimple debería mostrar la versión del software que se ejecuta.  
    Si el dispositivo no ejecuta la versión v2.1.1.518, actualice el sistema a la versión mínima requerida. Para obtener instrucciones detalladas, consulte el artículo sobre [cómo actualizar el sistema a la versión v2.1.1.518](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
- Compruebe si hay trabajos de copia de seguridad activos que se estén ejecuten en el dispositivo de origen. Esto incluye los trabajos en el host de la consola de Protección de datos de StorSimple. Espere a que finalicen los trabajos actuales. 
- Acceso a un host de Windows Server conectado al dispositivo de la serie 5000-7000 de StorSimple. El host debe ejecutar una versión admitida de Windows Server tal y como se describe en [Planeamiento de una implementación de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning).
- Los volúmenes de StorSimple se montan en el host y contienen recursos compartidos de archivos.
- El host tiene espacio de almacenamiento local suficiente para los datos en caché local.
- Acceso de nivel de propietario en la suscripción a Azure que va a utilizar para implementar Azure File Sync. Puede experimentar problemas al crear un punto de conexión en la nube para el grupo de sincronización si no tiene permisos de nivel de administrador o de propietario.
- Acceso a una [cuenta de almacenamiento de uso general v2](https://docs.microsoft.com/azure/storage/common/storage-account-overview) con un recurso compartido de archivos de Azure con el que quiere sincronizar. Para obtener más información, consulte [Creación de una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
  - [Creación de un recurso compartido de archivos](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

## <a name="migration-process"></a>Proceso de migración

La migración de datos de StorSimple 5000-7000 a AFS es un proceso en dos pasos:
1.  Replique los datos del servidor de archivos local donde están montados los volúmenes de StorSimple a un recurso compartido de Azure Files.  La replicación se realiza a través de un agente de AFS que instale.
2.  Desconecte el dispositivo StorSimple. Los discos locales luego actúan como caché local.

### <a name="migration-steps"></a>Pasos de migración

Realice los pasos siguientes para migrar el recurso compartido de archivos de Windows configurado en volúmenes de StorSimple a un recurso compartido de Azure File Sync. 
1.  Realice estos pasos en el mismo host de Windows Server donde están montados los volúmenes de StorSimple o utilice otro sistema. 
    - [Preparación de Windows Server para su uso con Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#prepare-windows-server-to-use-with-azure-file-sync).
    - [Instalación del agente de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#install-the-azure-file-sync-agent).
    - [Implementación del servicio de sincronización de almacenamiento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#deploy-the-storage-sync-service). 
    - [Registro de un servidor de Windows Server con el servicio de sincronización de almacenamiento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#register-windows-server-with-storage-sync-service). 
    - [Creación de un grupo de sincronización y un punto de conexión en la nube](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#create-a-sync-group-and-a-cloud-endpoint). Hay que crear grupos de sincronización para cada recurso compartido de archivos de Windows que tenga que migrar desde el host.
    - [Creación de un punto de conexión de servidor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal#create-a-server-endpoint). Especifique la ruta de acceso como ruta de acceso del volumen de StorSimple que contiene los datos del recurso compartido de archivos. Por ejemplo, si el volumen de StorSimple es la unidad `J` y los datos residen en `J:/<myafsshare>`, agregue esta ruta de acceso como un punto de conexión de servidor. Deje **Tiering** (Niveles) como **Deshabilitado**.
2.  Espere hasta que finalice la sincronización del servidor de archivos. Para cada servidor de un determinado grupo de sincronización, asegúrese de que:
    - Las marcas de tiempo para el último intento de sincronización, tanto para la carga como para la descarga, son recientes.
    - El estado está en color verde para la carga y descarga.
    - La **actividad de sincronización** muestra que quedan muy pocos archivos o ninguno por sincronizar.
    - **Archivos que no se están sincronizando** es 0 tanto para la carga como para la descarga.
    Para más información sobre cuándo finaliza la sincronización del servidor, vaya a [Solución de problemas de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#how-do-i-know-if-my-servers-are-in-sync-with-each-other). La sincronización puede tardar de varias horas a días, según el tamaño de los datos y el ancho de banda. Una vez completada la sincronización, todos sus datos están seguros en el recurso compartido de archivos de Azure. 
3.  Vaya a los recursos compartidos en los volúmenes de StorSimple. Seleccione un recurso compartido,haga clic con el botón derecho y elija **Propiedades**. Tenga en cuenta los permisos de recurso compartido en **Seguridad**. Estos permisos tendrán que aplicarse manualmente al nuevo recurso compartido en el paso posterior.
4.  En función de que use el mismo host de Windows Server u otro distinto, los pasos siguientes serán diferentes.

    Omita este paso y vaya al paso siguiente si utiliza un host distinto de Windows Server. Si usa el mismo servidor de archivos de Windows en AFS, ahora experimentará algunos minutos de tiempo de inactividad. 
    - **Inicio del tiempo de inactividad**: elimine el punto de conexión de servidor que creó en el *paso 1F*. 
    - Cree otro punto de conexión de servidor con la ruta en la que quiere que los datos residan de ahora en adelante.
    - Cuando el punto de conexión de servidor se muestre como Correcto (puede tardar unos minutos), verá los datos en esta nueva ubicación. Ahora puede configurar el host de Windows Server para que sirva archivos desde esta nueva ubicación. - **Finalización del tiempo de inactividad**.
5.  Si usa otro servidor de archivos de Windows en Azure File Sync, no experimentará ningún tiempo de inactividad. 
    - Agregue otro punto de conexión de servidor con la ruta de acceso del almacenamiento local que está dispuesto a utilizar como memoria caché en lugar del dispositivo StorSimple. 
    - En unos minutos podrá ver los archivos en el nuevo servidor. Puede cambiar en cualquier momento de su dispositivo StorSimple a esta nueva ubicación en el host.

    > [!TIP] 
    > Considere la posibilidad de configurar este nuevo recurso compartido de archivos con el mismo nombre y la misma ruta de acceso del que reemplaza, para minimizar las interrupciones. Si usa DFS-N, puede que tenga que realizar cambios en la configuración.
6.  Vuelva a configurar los permisos de uso compartido tal y como se indica en el *paso 3*.

Si experimenta problemas durante la migración de datos, [póngase en contacto con el Soporte técnico de Microsoft](storsimple-8000-contact-microsoft-support.md). 



## <a name="next-steps"></a>Pasos siguientes

Si AFS no es la solución adecuada para usted, aprenda a [migrar datos del dispositivo de la serie 5000-7000 de StorSimple a la serie 8000](storsimple-8000-migrate-from-5000-7000.md).

