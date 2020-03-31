---
title: Solución de problemas de las copias de seguridad de recursos compartidos de archivos de Azure
description: Este artículo contiene información para solución de problemas que se producen al proteger recursos compartidos de archivos de Azure.
ms.date: 08/20/2019
ms.topic: troubleshooting
ms.openlocfilehash: 050df5b96c265e468346535ff011e1baf7d86ad5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227444"
---
# <a name="troubleshoot-problems-backing-up-azure-file-shares"></a>Solución de problemas en las copias de seguridad de recursos compartidos de archivos de Azure

Puede solucionar los problemas y errores que aparezcan al usar la copia de seguridad de recursos compartidos de archivos de Azure con la información que encontrará en las tablas siguientes.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Limitaciones de la copia de seguridad de recursos compartidos de archivos de Azure en la versión preliminar

La copia de seguridad de los recursos compartidos de archivos de Azure está en versión preliminar. Los recursos compartidos de archivos de Azure se admiten en cuentas de almacenamiento de uso general v1 y uso general v2. No se admiten los siguientes escenarios de copia de seguridad para los recursos compartidos de archivos de Azure:

- No hay ninguna CLI disponible para proteger Azure Files mediante Azure Backup.
- El número máximo de copias de seguridad programadas al día es una.
- El número máximo de copias de seguridad a petición al día es cuatro.
- Use los [bloqueos de recursos](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) de la cuenta de almacenamiento para impedir la eliminación accidental de copias de seguridad del almacén de Recovery Services.
- No elimine las instantáneas que crea Azure Backup. La eliminación de instantáneas puede provocar la pérdida de puntos de recuperación o errores de restauración.
- No elimine los recursos compartidos de archivos que estén protegidos por Azure Backup. La solución actual eliminará todas las instantáneas realizadas por Azure Backup una vez que se elimina el recurso compartido de archivos y, por consiguiente, se pierden todos los puntos de restauración

La copia de seguridad de recursos compartidos de archivos de Azure en cuentas de almacenamiento con replicación de [almacenamiento con redundancia de zona](../storage/common/storage-redundancy-zrs.md) (ZRS) solo se encuentra disponible actualmente en las regiones de Centro de EE. UU. (CUS), Este de EE. UU. (EUS), Este de EE. UU. 2 (EUS2), Norte de Europa (NE), Sudeste de Asia (SEA), Oeste de Europa (WE) y Oeste de EE. UU. 2 (WUS2).

## <a name="configuring-backup"></a>Configuración de la copia de seguridad

La tabla siguiente es para configurar la copia de seguridad:

| Mensajes de error | Sugerencias de solución alternativa o resolución |
| ------------------ | ----------------------------- |
| No se pudo encontrar la cuenta de almacenamiento para configurar la copia de seguridad para los recursos compartidos de archivos de Azure | <ul><li>Espere a que la detección finalice. <li>Compruebe si algún recurso compartido de archivos de la cuenta de almacenamiento ya está protegido con otro almacén de Recovery Services. **Nota**: Todos los recursos compartidos de archivos de una cuenta de almacenamiento solo se pueden proteger en un almacén de Recovery Services. <li>Asegúrese de que el recurso compartido de archivos no esté presente en ninguna de las cuentas de almacenamiento no admitidas.<li> Asegúrese de que la casilla **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento** esté activada en la cuenta de almacenamiento.[Más información.](../storage/common/storage-network-security.md)|
| Error en la detección de estados del portal de las cuentas de almacenamiento. | Si la suscripción es de un asociado (habilitada para CSP), haga caso omiso del error. Si la suscripción no está habilitada para CSP y las cuentas de almacenamiento no se pueden detectar, póngase en contacto con el servicio de soporte técnico.|
| Error de registro o validación de la cuenta de almacenamiento seleccionada.| Reintente la operación. Si el problema persiste, póngase en contacto con el servicio de soporte técnico.|
| No se pudieron mostrar ni encontrar los recursos compartidos de archivos en la cuenta de almacenamiento seleccionada. | <ul><li> Asegúrese de que la cuenta de almacenamiento existe en el grupo de recursos (y que no se ha eliminado ni movido después de la última validación o registro en el almacén).<li>Asegúrese de que el recurso compartido de archivos que busca proteger no se ha eliminado. <li>Asegúrese de que la cuenta de almacenamiento es una cuenta de almacenamiento admitida para la copia de seguridad de recursos compartidos de archivos.<li>Compruebe si el recurso compartido de archivos ya está protegido en el mismo almacén de Recovery Services.|
| Error en la configuración de copia de seguridad del recurso compartido de archivos (o en la configuración de la directiva de protección). | <ul><li>Vuelva a intentar la operación para ver si el problema persiste. <li> Asegúrese de que el recurso compartido de archivos que quiere proteger no se ha eliminado. <li> Si está intentando proteger varios recursos compartidos de archivos a la vez, y algunos de ellos generan error, reintente la configuración de la copia de seguridad en los recursos compartidos de archivos que han dado error. |
| No se puede eliminar el almacén de Recovery Services después de desproteger un recurso compartido de archivos. | En Azure Portal, abra su Almacén > **Infraestructura de Backup** > **Cuentas de almacenamiento** y haga clic en **Anular el registro** para quitar la cuenta de almacenamiento del almacén de Recovery Services.|

## <a name="error-messages-for-backup-or-restore-job-failures"></a>Mensajes de error de trabajos de copia de seguridad o restauración

| Mensajes de error | Sugerencias de solución alternativa o resolución |
| -------------- | ----------------------------- |
| No se pudo realizar la operación porque no se encuentra el recurso compartido de archivos. | Asegúrese de que el recurso compartido de archivos que busca proteger no se ha eliminado.|
| La cuenta de almacenamiento no se encuentra o no se admite. | <ul><li>Asegúrese de que la cuenta de almacenamiento existe en el grupo de recursos y que no se ha eliminado o quitado del grupo de recursos tras la validación final. <li> Asegúrese de que la cuenta de almacenamiento es una cuenta de almacenamiento admitida para la copia de seguridad de recursos compartidos de archivos.|
| Ha alcanzado el límite máximo de instantáneas para este recurso compartido de archivos. Podrá tomar más cuando expiren las más antiguas. | <ul><li> Este error puede producirse al crear varias copias de seguridad a petición para un archivo. <li> Hay un límite de 200 instantáneas por recurso compartido de archivos, incluidas las realizadas por Azure Backup. Las copias de seguridad programadas antiguas (o instantáneas) se borran automáticamente. Las copias de seguridad a petición (o instantáneas) deben eliminarse si se alcanza el límite máximo.<li> Elimine las copias de seguridad a petición (instantáneas de recurso compartido de archivos de Azure) desde el portal de Azure Files. **Nota**: Si elimina las instantáneas creadas por Azure Backup, perderá los puntos de recuperación. |
| Error de restauración o copia de seguridad de recursos compartidos de archivos debido a la limitación del servicio de almacenamiento. Esto puede ser debido a que el servicio de almacenamiento está ocupado procesando otras solicitudes para la cuenta de almacenamiento dada.| Vuelva a intentar la operación más tarde. |
| Error de restauración con recurso compartido de archivos no encontrado. | <ul><li>Asegúrese de que existe la cuenta de almacenamiento seleccionada y que no se ha eliminado el recurso compartido de archivos de destino. <li> Asegúrese de que la cuenta de almacenamiento es una cuenta de almacenamiento admitida para la copia de seguridad de recursos compartidos de archivos. |
| Error de los trabajos de copia de seguridad o restauración debido al estado Bloqueado de la cuenta de almacenamiento. | Quite el bloqueo de la cuenta de almacenamiento o use eliminar bloqueo en lugar de leer bloqueo y reintente la operación. |
| No se pudo llevar a cabo la recuperación porque el número de archivos con errores es superior al umbral permitido. | <ul><li> Los motivos de los errores de recuperación se muestran en un archivo (ruta de acceso proporcionada en los detalles del trabajo). Solucione los errores y vuelva a intentar la operación de restauración solo de los archivos con error. <li> Causas comunes de errores de restauración de archivos: <br/> -Asegúrese de que los archivos con error no están actualmente en uso. <br/> -Existe un directorio con el mismo nombre que el archivo de error en el directorio primario. |
| Error de recuperación. No se pudo recuperar ningún archivo. | <ul><li> Los motivos de los errores de recuperación se muestran en un archivo (ruta de acceso proporcionada en los detalles del trabajo). Solucione los errores y vuelva a intentar la operación de restauración solo de los archivos con error. <li> Causas comunes de los errores de restauración de archivos: <br/> -Asegúrese de que los archivos con error no están actualmente en uso. <br/> -Existe un directorio con el mismo nombre que el archivo de error en el directorio primario. |
| Error en la restauración porque uno de los archivos del origen no existe. | <ul><li> Los elementos seleccionados no están presentes en los datos del punto de recuperación. Para recuperar los archivos, proporcione la lista de archivos correcta. <li> La instantánea del recurso compartido de archivos que corresponde al punto de recuperación se elimina manualmente. Seleccione un punto de recuperación diferente y vuelva a intentar la operación de restauración. |
| Ya hay un trabajo de Recovery en curso para el mismo destino. | <ul><li>La copia de seguridad de recursos compartidos de archivos no admite recuperación en paralelo al mismo recurso compartido de archivos de destino. <li>Espere a que la recuperación existente finalice e inténtelo de nuevo. Si no encuentra un trabajo de recuperación en el almacén de Recovery Services, compruebe otros almacenes de Recovery Services de la misma suscripción. |
| No se puede realizar la operación de restauración porque el recurso compartido de archivos de destino está lleno. | Aumente la cuota de tamaño del recurso compartido de archivos de destino para albergar los datos de restauración y vuelva a intentar la operación. |
| La operación de restauración produjo un error porque, a su vez, se produjo un error al realizar las operaciones previas de restauración en los recursos del servicio de sincronización de archivos asociados con el recurso compartido de archivos de destino. | Vuelva a intentarlo un tiempo después y si el problema persiste, póngase en contacto con el soporte técnico de Microsoft. |
| Uno o varios archivos no se pudieron recuperar correctamente. Para más información, consulte la lista de archivos que han dado error en la ruta de acceso anterior. | <ul> <li> Las causas de los errores de recuperación se muestran en el archivo (la ruta de acceso se proporciona en los detalles del trabajo). Solucione los errores y vuelva a intentar la operación de restauración solo de los archivos con error. <li> Las causas comunes de errores de restauración de archivos son: <br/> -Asegúrese de que los archivos con error no están actualmente en uso. <br/> -Existe un directorio con el mismo nombre que el archivo de error en el directorio principal. |

## <a name="modify-policy"></a>Modificación de directivas

| Mensajes de error | Sugerencias de solución alternativa o resolución |
| ------------------ | ----------------------------- |
| Hay otra operación de protección de configuración en curso para este elemento. | Espere a que termine la operación de modificación de directivas anterior y vuelva a intentarlo al cabo de un tiempo.|
| Hay otra operación en curso en el elemento seleccionado. | Espere a que la otra operación en curso termine y vuelva a intentarlo al cabo de un tiempo. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la copia de seguridad de recursos compartidos de archivos de Azure, consulte:

- [Copia de seguridad de recursos compartidos de archivos de Azure](backup-afs.md)
- [Preguntas frecuentes acerca de la copia de seguridad de recursos compartidos de archivos de Azure](backup-azure-files-faq.md)
