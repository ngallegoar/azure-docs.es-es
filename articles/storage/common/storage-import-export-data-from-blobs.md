---
title: Uso de Azure Import/Export para exportar datos de los blobs de Azure | Microsoft Docs
description: Aprenda a crear trabajos de exportación en Azure Portal para transferir datos de los blobs de Azure.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 09/17/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: d9f7778d1dda159f3ab0c4548912370c85f94eff
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91441890"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>Uso del servicio Azure Import/Export para exportar datos de Azure Blob Storage

Este artículo proporciona instrucciones paso a paso sobre cómo usar el servicio Azure Import/Export para exportar de forma segura grandes cantidades de datos desde Azure Blob Storage. El servicio requiere que envíe unidades de disco vacías al centro de datos de Azure. El servicio exporta datos de la cuenta de almacenamiento a las unidades de disco y luego envía de vuelta las unidades de disco.

## <a name="prerequisites"></a>Requisitos previos

Antes de crear un trabajo de exportación para transferir datos fuera de Azure Blob Storage, revise con cuidado y complete la siguiente lista de requisitos previos para este servicio.
Debe:

- Tener una suscripción activa de Azure que pueda usarse para el servicio Import/Export.
- Tener al menos una cuenta de Azure Storage. Consulte la lista de [las cuenta de almacenamiento y los tipos de almacenamiento admitidos para el servicio Import/Export](storage-import-export-requirements.md). Para obtener información acerca de la creación de una nueva cuenta de almacenamiento, consulte [Creación de una cuenta de almacenamiento](storage-account-create.md).
- Tener un número suficiente de discos de los [tipos admitidos](storage-import-export-requirements.md#supported-disks).
- Tener una cuenta de FedEx o DHL. Si quiere usar alguna empresa de mensajería que no sea FedEx o DHL, póngase en contacto con el equipo de operaciones de Azure Data Box en `adbops@microsoft.com`.
  - La cuenta debe ser válida, debe tener saldo positivo y debe tener capacidades de devolución de envíos.
  - Generar un número de seguimiento del trabajo de exportación.
  - Cada trabajo debe tener un número de seguimiento independiente. No se admiten varios trabajos con el mismo número de seguimiento.
  - Si no tiene una cuenta de transportista, vaya a:
    - [Crear una cuenta de FedEx](https://www.fedex.com/en-us/create-account.html), o
    - [Crear una cuenta de DHL](http://www.dhl-usa.com/en/express/shipping/open_account.html).

## <a name="step-1-create-an-export-job"></a>Paso 1: Crear un trabajo de exportación

Siga estos pasos para crear un trabajo de exportación en Azure Portal.

1. Inicie sesión en <https://portal.azure.com/>.
2. Vaya a **Todos los servicios > Almacenamiento > Trabajos de importación o exportación**.

    ![Ir a trabajos de importación o exportación](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Haga clic en **Crear el trabajo de importación o exportación**.

    ![Hacer clic en trabajo de importación o exportación](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. En **Aspectos básicos**:

    - Seleccione **Export from Azure** (Exportar desde Azure).
    - Escriba un nombre descriptivo para el trabajo de exportación. Utilice el nombre que elija para realizar un seguimiento del progreso de los trabajos.
        - El nombre puede contener solo letras minúsculas, números, guiones y caracteres de subrayado.
        - El nombre tiene que empezar por una letra y no puede contener espacios.
    - Seleccione una suscripción.
    - Escriba o seleccione un grupo de recursos.

        ![Aspectos básicos](./media/storage-import-export-data-from-blobs/export-from-blob3.png)

5. En **Detalles del trabajo**:

    - Seleccione la cuenta de almacenamiento en la que residen los datos que se van a exportar. Use una cuenta de almacenamiento cerca de donde se encuentra.
    - La ubicación de la entrega se rellena automáticamente según la región de la cuenta de almacenamiento seleccionada.
    - Especifique los datos de blobs que desea exportar desde su cuenta de almacenamiento a una o varias unidades vacías.
    - Elija **Exportar todos** los datos de blob de la cuenta de almacenamiento.

         ![Exportar todo](./media/storage-import-export-data-from-blobs/export-from-blob4.png)

    - Puede especificar qué contenedores y blobs se van a exportar.
        - **Para especificar un blob para exportar**: use el selector **Igual a**. Especifique la ruta de acceso relativa al blob, empezando con el nombre del contenedor. Utilice *$root* para especificar el contenedor raíz.
        - **Para especificar todos los blobs que empiezan con un prefijo**: use el selector **Empieza por**. Especifique el prefijo, empezando con una barra diagonal "/". El prefijo puede ser el prefijo del nombre del contenedor, el nombre del contenedor completo o el nombre del contendor completo seguido del prefijo del nombre del blob. Tiene que proporcionar las rutas de acceso del blob en formato válido para evitar errores durante el procesamiento, tal y como se muestra en esta captura de pantalla. Para más información, consulte [Ejemplos de rutas de acceso del blob válidas](#examples-of-valid-blob-paths).

           ![Exportación de blobs y contenedores seleccionados](./media/storage-import-export-data-from-blobs/export-from-blob5.png)

    - Puede exportar desde el archivo de lista de blobs.

        ![Exportación desde el archivo de lista de blobs](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  

   > [!NOTE]
   > Si el blob que se va a exportar está en uso en el transcurso de la copia de datos, el servicio Azure Import/Export tomará una instantánea del blob y la copiará.

6. En **Información de envío de devolución**:

    - Seleccione el transportista en la lista desplegable. Si desea usar una empresa de mensajería que no sea FedEx o DHL, elija una de las opciones de la lista desplegable. Póngase en contacto con el equipo de operaciones de Azure Data Box en `adbops@microsoft.com` con la información relacionada con el transportista que quiere usar.
    - Escriba un número válido de cuenta de transportista que haya creado con ese transportista. Microsoft usa esta cuenta para devolverle las unidades una vez que haya finalizado el trabajo de exportación.
    - Proporcione la siguiente información completa y válida: nombre de contacto, teléfono, correo electrónico, dirección postal, ciudad, código postal, estado o provincia y país o región.

        > [!TIP]
        > En lugar de especificar una dirección de correo electrónico para un solo usuario, proporcione un correo electrónico de grupo. Esto garantiza que recibirá notificaciones incluso si sale un administrador.

7. En **Resumen**:

    - Revise los detalles del trabajo.
    - Anote el nombre del trabajo y proporcione la dirección de envío del centro de datos de Azure para el envío de discos a Azure.

        > [!NOTE]
        > Envíe siempre los discos al centro de datos que se ha indicado en Azure Portal. Si los discos se envían al centro de datos incorrecto, no se procesará el trabajo.

    - Haga clic en **Aceptar** para completar la creación del trabajo de exportación.

<!--## (Optional) Step 2: -->

## <a name="step-2-ship-the-drives"></a>Paso 2: Envío de las unidades de disco

Si no conoce el número de unidades de disco que necesita, vaya a [Comprobación del número de unidades de disco](#check-the-number-of-drives). Si conoce el número de unidades, continúe con el envío de las unidades.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>Paso 3: Actualización del trabajo con información de seguimiento

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-4-receive-the-disks"></a>Paso 4: Recepción de los discos

Cuando el panel notifica que el trabajo está completo, se le envían los discos y el número de seguimiento para el envío está disponible en el portal.

1. Después de que haya recibido las unidades de disco con los datos exportados, tiene que obtener las claves de BitLocker para desbloquear las unidades de disco. Vaya al trabajo de exportación en Azure Portal. Haga clic en la pestaña **Import/Export**.
2. Seleccione el trabajo de exportación de la lista y haga clic en él. Vaya a **Cifrado** y copie las claves.

   ![Visualización de claves de BitLocker de un trabajo de exportación](./media/storage-import-export-data-from-blobs/export-from-blob-7.png)

3. Use las claves de BitLocker para desbloquear los discos.

La exportación se ha terminado.

## <a name="step-5-unlock-the-disks"></a>Paso 5: Desbloqueo de los discos

Escriba el siguiente comando para desbloquear la unidad:

   `WAImportExport Unlock /bk:<BitLocker key (base 64 string) copied from Encryption blade in Azure portal> /driveLetter:<Drive letter>`  

Este es un ejemplo de la entrada.

   `WAImportExport.exe Unlock /bk:CAAcwBoAG8AdQBsAGQAIABiAGUAIABoAGkAZABkAGUAbgA= /driveLetter:e`

En este momento, puede eliminar el trabajo o dejarlo. Los trabajos se eliminan automáticamente después de 90 días.

## <a name="check-the-number-of-drives"></a>Comprobación del número de unidades de disco

Este paso *opcional* le ayuda a determinar el número de unidades necesarias para el trabajo de exportación. Realice este paso en un sistema de Windows que ejecute una [versión admitida del sistema operativo](storage-import-export-requirements.md#supported-operating-systems).

1. [Descargar la versión 1 de WAImportExport](https://www.microsoft.com/download/details.aspx?id=42659) en el sistema de Windows.
2. Descomprima en la carpeta predeterminada `waimportexportv1`. Por ejemplo, `C:\WaImportExportV1`.
3. Abra una ventana de PowerShell o de línea de comandos con privilegios administrativos. Para cambiar el directorio a la carpeta descomprimida, ejecute el siguiente comando:

   `cd C:\WaImportExportV1`

4. Para comprobar el número de discos necesarios para los blobs seleccionados, ejecute el siguiente comando:

   `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    Los parámetros se describen en la tabla siguiente:

    |Parámetro de línea de comandos|Descripción|  
    |--------------------------|-----------------|  
    |**/logdir:**|Opcional. Directorio de registro. Los archivos de registro detallados se escriben en este directorio. Si no se especifica, se usa el directorio actual como directorio de registro.|  
    |**/sn:**|Necesario. El nombre de la cuenta de almacenamiento para el trabajo de exportación.|  
    |**/sk:**|Necesario únicamente si no se especifica un SAS del contenedor. La clave de cuenta para la cuenta de almacenamiento correspondiente al trabajo de exportación.|  
    |**/csas:**|Necesario únicamente si no se especifica una clave de cuenta de almacenamiento. El contenedor SAS para enumerar los blobs que se van a exportar en el trabajo de exportación.|  
    |**/ExportBlobListFile:**|Necesario. Ruta de acceso al archivo XML que contiene una lista de rutas de acceso de blob o prefijos de ruta de acceso para los blobs que se van a exportar. El formato de archivo usado en el elemento `BlobListBlobPath` de la operación [Put Job](/rest/api/storageimportexport/jobs) de la API de REST del servicio Import/Export.|  
    |**/DriveSize:**|Necesario. El tamaño de las unidades de disco que se van a usar para un trabajo de exportación,  *por ejemplo*, 500 GB o 1,5 TB.|  

    Vea el [Ejemplo del comando PreviewExport](#example-of-previewexport-command).

5. Compruebe que puede leer o escribir en las unidades de disco que se van a enviar para el trabajo de exportación.

### <a name="example-of-previewexport-command"></a>Ejemplo del comando PreviewExport

En el siguiente ejemplo se muestra el comando `PreviewExport`:  

```powershell
    WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB
```  

El archivo de lista de blobs de exportación puede contener nombres de blob y prefijos de blob, como se muestra aquí:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

La herramienta Azure Import/Export enumera todos los blobs que se van a exportar y calcula cómo empaquetarlos en unidades del tamaño especificado, teniendo en cuenta cualquier sobrecarga necesaria; después calcula el número de unidades necesarias para albergar los blobs y la información de uso de la unidad.  

Este es un ejemplo de la salida, con los registros informativos omitidos:  

```powershell
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  

Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB
```

## <a name="examples-of-valid-blob-paths"></a>Ejemplos de rutas de acceso del blob válidas

En la siguiente tabla se muestran ejemplos de rutas de acceso del blob válidas:

   | Selector | Ruta del blob | Descripción |
   | --- | --- | --- |
   | Empieza por |/ |Exporta todos los blobs de la cuenta de almacenamiento. |
   | Empieza por |/$root/ |Exporta todos los blobs del contenedor raíz. |
   | Empieza por |/book |Exporta todos los blobs de cualquier contenedor que empiecen por el prefijo **book** |
   | Empieza por |/music/ |Exporta todos los blobs del contenedor **music** |
   | Empieza por |/music/love |Exporta todos los blobs del contenedor **music** que empiecen por el prefijo **love**. |
   | Igual a |$root/logo.bmp |Exporta el blob **logo.bmp** del contenedor raíz. |
   | Igual a |videos/story.mp4 |Exporta el blob **story.mp4** del contenedor **videos** |

## <a name="next-steps"></a>Pasos siguientes

- [Visualización del estado del trabajo y de la unidad de disco](storage-import-export-view-drive-status.md)
- [Revisión de los requisitos de Import/Export](storage-import-export-requirements.md)
