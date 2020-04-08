---
title: Solución de problemas de Azure Data Box mediante la interfaz REST | Microsoft Docs
description: Se describe cómo solucionar los problemas observados en Azure Data Box cuando la copia de los datos se realiza a través de la interfaz REST.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: 7fe5afbc4984c430cbf393e4e2b44122bdd43983
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297124"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-blob-storage"></a>Solución de problemas relacionados con el almacenamiento de blobs de Azure Data Box

En este artículo se detalla información sobre cómo solucionar problemas con que se puede encontrar al utilizar el almacenamiento de blobs de Data Box a través de la interfaz REST del Data Box para copiar datos. Estos problemas surgen al utilizar el almacenamiento de blobs de Data Box con otras aplicaciones o bibliotecas de cliente como el Explorador de Azure Storage, AzCopy o la biblioteca de Azure Storage para Python.

## <a name="errors-seen-in-azure-storage-explorer"></a>Errores observados en el Explorador de Azure Storage

En esta sección se detallan algunos de los problemas que surgen al utilizar el Explorador de Azure Storage con el almacenamiento de blobs de Data Box.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|No se han podido recuperar los recursos secundarios. El valor de uno de los encabezados HTTP no tiene el formato correcto.|En el menú **Editar**, seleccione **API de Azure Stack de destino**. <br>Reinicie el Explorador de Azure Storage.|
|`getaddrinfo ENOTFOUND <accountname>.blob.<serialnumber>.microsoftdatabox.com` |Compruebe que el nombre del punto de conexión `<accountname>.blob.<serialnumber>.microsoftdatabox.com` se haya agregado al archivo de hosts en esta ruta de acceso: <li>`C:\Windows\System32\drivers\etc\hosts` en Windows, o </li><li> `/etc/hosts` en Linux.</li>|
|No se han podido recuperar los recursos secundarios. <br>Detalles: un certificado autofirmado |Importe el certificado TLS/SSL para el dispositivo en el Explorador de Azure Storage: <li>Descargue el certificado de Azure Portal. Para obtener más información, vaya a [Download the certificate](data-box-deploy-copy-data-via-rest.md#download-certificate) (Descargar el certificado).</li><li>En el menú **Editar**, seleccione **Certificados SSL** y, después, **Importar certificados**.</li>|

## <a name="errors-seen-in-azcopy-for-windows"></a>Errores observados en AzCopy para Windows

En esta sección se detallan algunos de los problemas que surgen al utilizar AzCopy para Windows con el almacenamiento de blobs de Data Box.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|El comando de AzCopy se bloquea durante un minuto antes de mostrar este error: <br>No se ha podido enumerar el directorio https://… No se ha podido resolver el nombre remoto `<accountname>.blob.<serialnumber>.microsoftdatabox.com`|Compruebe que el nombre del punto de conexión `<accountname>.blob.<serialnumber>.microsoftdatabox.com` se haya agregado al archivo de hosts en `C:\Windows\System32\drivers\etc\hosts`.|
|El comando de AzCopy se bloquea durante un minuto antes de mostrar este error: <br>Error al analizar la ubicación de origen. Se ha cerrado la conexión subyacente: No se ha podido establecer la relación de confianza para el canal seguro SSL/TLS.|Importe el certificado TLS/SSL para el dispositivo en el almacén de certificados del sistema. Para obtener más información, vaya a [Download the certificate](data-box-deploy-copy-data-via-rest.md#download-certificate) (Descargar el certificado).|


## <a name="errors-seen-in-azcopy-for-linux"></a>Errores observados en AzCopy para Linux

En esta sección se detallan algunos de los problemas que surgen al utilizar AzCopy para Linux con el almacenamiento de blobs de Data Box.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|El comando de AzCopy se bloquea durante 20 minutos antes de mostrar este error: <br>Error al analizar la ubicación de origen `https://<accountname>.blob.<serialnumber>.microsoftdatabox.com/<cntnr>`. No existe tal dispositivo o dirección|Compruebe que el nombre del punto de conexión `<accountname>.blob.<serialnumber>.microsoftdatabox.com` se haya agregado al archivo de hosts en `/etc/hosts`.|
|El comando de AzCopy se bloquea durante 20 minutos antes de mostrar este error: <br>Error al analizar la ubicación de origen… No se ha podido establecer la conexión SSL.|Importe el certificado TLS/SSL para el dispositivo en el almacén de certificados del sistema. Para obtener más información, vaya a [Download the certificate](data-box-deploy-copy-data-via-rest.md#download-certificate) (Descargar el certificado).|

## <a name="errors-seen-in-azure-storage-library-for-python"></a>Errores observados en la biblioteca de Azure Storage para Python

En esta sección se detallan algunos de los principales problemas que se enfrentan durante la implementación de Data Box Disk cuando se usa un cliente Linux para la copia de datos.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|El valor de uno de los encabezados HTTP no tiene el formato correcto. |Data Box no admite la versión instalada de la biblioteca de Microsoft Azure Storage para Python. Consulte los requisitos de almacenamiento de blobs de Azure Data Box para las versiones compatibles.|
|… [SSL: CERTIFICATE_VERIFY_FAILED]…|Antes de ejecutar Python, establezca la variable de entorno REQUESTS_CA_BUNDLE a la ruta de acceso del archivo de certificado TLS con codificación Base64 (consulte [Descargar el certificado](data-box-deploy-copy-data-via-rest.md#download-certificate)). <br>Por ejemplo:<br>`export REQUESTS_CA_BUNDLE=/tmp/mycert.cer` <br>`python` <br>O bien, agregue el certificado al almacén de certificados del sistema y, a continuación, establezca esta variable de entorno a la ruta de acceso de dicho almacén. <br> Por ejemplo, en Ubuntu: <br>`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt` <br>`python`|


## <a name="common-errors"></a>Errores comunes

Estos errores no son específicos de cualquier aplicación.

|Mensaje de error  |Acción recomendada |
|---------|---------|
|Se agota el tiempo de espera de conexión. |Inicie sesión en el dispositivo Data Box y compruebe que está desbloqueado. Cada vez que se reinicia el dispositivo, permanece bloqueado hasta que alguien inicia sesión.|

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre los [requisitos de sistema del almacenamiento de blobs de Data Box](data-box-system-requirements-rest.md).
