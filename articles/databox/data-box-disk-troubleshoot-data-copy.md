---
title: Solución de problemas de copia de datos en Azure Data Box Disk | Microsoft Docs
description: Se describe cómo solucionar los problemas observados durante la copia de datos en Azure Data Box Disk con registros.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/13/2019
ms.author: alkohli
ms.openlocfilehash: 760f5c6c929aa082993683d7a466a71c6484289a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67148136"
---
# <a name="troubleshoot-data-copy-issues-in-azure-data-box-disk"></a>Solución de problemas de copia de datos en Azure Data Box Disk

Este artículo se aplica a Microsoft Azure Data Box Disk y describe cómo solucionar los problemas que se producen al copiar datos en discos. El artículo también trata problemas al usar la herramienta de copia y división.


## <a name="data-copy-issues-when-using-a-linux-system"></a>Problemas de copia de datos al usar un sistema Linux

En esta sección se detallan algunos de los principales problemas que surgen cuando se usa a un cliente Linux para copiar datos en discos.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Problema: la unidad se montó como solo lectura
 
**Causa** 

Puede deberse a un sistema de archivos no limpio.

Volver a montar una unidad como de lectura y escritura no funciona con Data Box Disk. Este escenario no es compatible con las unidades descifradas por dislocker. Es posible que haya vuelto a montar correctamente el dispositivo con el comando siguiente:

    `# mount -o remount, rw /mnt/DataBoxDisk/mountVol1`

Si bien el nuevo montaje se completó correctamente, los datos no se conservarán.

**Resolución**

Realice los pasos siguientes en el sistema Linux:

1. Instale el paquete `ntfsprogs` para la utilidad ntfsfix.
2. Desmonte los puntos de montaje preparados para la unidad por la herramienta de desbloqueo. El número de puntos de montaje variará en función de las unidades.

    ```
    unmount /mnt/DataBoxDisk/mountVol1
    ```

3. Ejecute `ntfsfix` en la ruta de acceso correspondiente. El número resaltado debe ser el igual al del paso 2.

    ```
    ntfsfix /mnt/DataBoxDisk/bitlockerVol1/dislocker-file
    ```

4. Ejecute el comando siguiente para quitar los metadatos de hibernación que puedan ocasionar el problema de montaje.

    ```
    ntfs-3g -o remove_hiberfile /mnt/DataBoxDisk/bitlockerVol1/dislocker-file /mnt/DataBoxDisk/mountVol1
    ```

5. Realice un desmontaje completo.

    ```
    ./DataBoxDiskUnlock_x86_64 /unmount
    ```

6. Realice un desbloqueo y un montaje completos.
7. Pruebe el punto de montaje escribiendo un archivo.
8. Desmonte y vuelva a montar para validar la persistencia del archivo.
9. Continúe con la copia de datos.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Problema: error con datos que no se conservan después de la copia
 
**Causa** 

Si ve que la unidad no tiene datos después de desmontarla (a pesar de que se hayan copiado datos en ella), es posible que haya vuelto a montarla como de lectura-escritura una vez que se montó como de solo lectura.

**Resolución**
 
Si ese es el caso, consulte la resolución para las [unidades que se montan como de solo lectura](#issue-drive-getting-mounted-as-read-only).

Si ese no era el caso, copie los registros de la carpeta que tiene la herramienta de desbloqueo de Data Box Disk y [póngase en contacto con el Soporte técnico de Microsoft](data-box-disk-contact-microsoft-support.md).


## <a name="data-box-disk-split-copy-tool-errors"></a>Errores de la herramienta Data Box Disk Split Copy

En la tabla siguiente se resumen los problemas observados al usar una herramienta de copia y división para dividir los datos en varios discos.

|Mensaje de error o advertencias |Recomendaciones |
|---------|---------|
|[Información] Recuperación de contraseña de BitLocker del volumen: m <br>[Error] Excepción detectada al recuperar la clave de BitLocker para el volumen m:<br> La secuencia no contiene elementos.|Este error se produce si el disco de Data Box Disk de destino está sin conexión. <br> Use la herramienta `diskmgmt.msc` en discos en línea.|
|[Error] Excepción producida: no se pudo realizar la operación WMI:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=El formato de la contraseña de recuperación que se proporciona no es válida. <br>Las contraseñas de recuperación de BitLocker son 48 dígitos. <br>Compruebe que la contraseña de recuperación tiene el formato correcto e inténtelo de nuevo.|Utilice la herramienta Data Box Disk Unlock para desbloquear por primera vez los discos y vuelva a intentar el comando. Para más información, vaya a: <li> [Desbloqueo de Data Box Disk en clientes Windows](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Desbloqueo de Data Box Disk en clientes Linux](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Error] Excepción producida: existe un archivo DriveManifest.xml en la unidad de destino. <br> Esto indica que la unidad de destino se puede haber preparado con otro archivo de diario. <br>Para agregar más datos a la misma unidad, use el archivo de diario anterior. Para eliminar los datos existentes y volver a usar la unidad de destino para un nuevo trabajo de importación, elimine *DriveManifest.xml* de la unidad. Vuelva a ejecutar este comando con un nuevo archivo de diario.| Este error se recibe al intentar utilizar el mismo conjunto de unidades para varias sesiones de importación. <br> Use un conjunto de unidades solo para una sesión de división y copia.|
|[Error] Excepción producida: CopySessionId importdata-septiembre-test-1 hace referencia a una sesión de copia anterior y no se puede reutilizar para una nueva sesión de copia.|Este error se notifica al intentar usar el mismo nombre para un nuevo trabajo como un trabajo anterior completado correctamente.<br> Asigne un nombre exclusivo para el trabajo nuevo.|
|[Info] El nombre del directorio o del archivo de destino supera el límite de longitud de NTFS. |Este mensaje se notifica cuando se ha cambiado el archivo de destino porque la ruta de acceso del archivo es larga.<br> Modifique la opción de disposición en el archivo `config.json` para controlar este comportamiento.|
|[Error] Excepción producida: secuencia de escape JSON no válida. |Este mensaje se notifica cuando el archivo config.json tiene un formato que no es válido. <br> Valide `config.json` mediante [JSONlint](https://jsonlint.com/) antes de guardar el archivo.|


## <a name="next-steps"></a>Pasos siguientes

- Aprenda [solucionar problemas de la herramienta de validación](data-box-disk-troubleshoot.md).
