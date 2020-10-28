---
title: 'Generación de un URI de SAS para una imagen de máquina virtual: Azure Marketplace'
description: Genere un URI de Firma de acceso compartido (SAS) para discos duros virtuales (VHD) en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: iqshahmicrosoft
ms.author: krsh
ms.date: 10/19/2020
ms.openlocfilehash: b927e90c60110d6922649b75b6549528a2a9a40f
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283366"
---
# <a name="how-to-generate-a-sas-uri-for-a-vm-image"></a>Generación de un URI de SAS para una imagen de máquina virtual

Durante el proceso de publicación, debe proporcionar un identificador URI de SAS (firma de acceso compartido) para cada VHD asociado a sus planes (anteriormente denominados SKU). Microsoft necesita tener acceso a estos VHD durante el proceso de certificación. Deberá escribir este identificador URI en la pestaña **Planes** del Centro de partners.

La generación de los identificadores URI de SAS para los discos duros virtuales debe cumplir los requisitos siguientes:

- Solo se admiten VHD no administrados.
- Solo se requieren permisos de lectura y enumeración. No proporcione acceso de escritura o eliminación.
- La duración del acceso (fecha de expiración) debe ser un mínimo de tres semanas a partir de la creación del identificador URI de SAS.
- Como protección frente a los cambios de hora UTC, establezca la fecha de inicio en un día antes de la fecha actual. Por ejemplo, si la fecha actual es el 16 de junio de 2020, seleccione 15/6/2020.

## <a name="generate-the-sas-address"></a>Generación de la dirección de SAS

Hay dos herramientas comunes que se usan para crear una dirección de SAS (URL):

1. **Explorador de Azure Storage** : disponible en Azure Portal.
2. **CLI de Azure** : recomendada para los sistemas operativos que no sean Windows y los entornos automatizados o de integración continua.

### <a name="using-tool-1-azure-storage-explorer"></a>Uso de la herramienta 1: Explorador de Azure Storage

1. Vaya a la **cuenta de almacenamiento** .
1. Abra el **Explorador de Storage** .

    :::image type="content" source="media/create-vm/storge-account-explorer.png" alt-text="Ventana de la cuenta de almacenamiento.":::

3. En el **contenedor** , haga clic con el botón derecho en el archivo VHD y seleccione **Obtener firma de acceso compartido** .
4. En el cuadro de diálogo **Firma de acceso compartido** , complete los campos siguientes:

    1. Fecha de inicio: fecha de inicio del permiso para acceder al disco duro virtual. Proporcione una fecha que sea un día antes de la fecha actual.
    2. Fecha de expiración: fecha de expiración del permiso para acceder al disco duro virtual. Proporcione una fecha al menos tres semanas después de la fecha actual.
    3. Permisos: seleccione los permisos Lista y Lectura.
    4. Nivel de contenedor: active la casilla Generar identificador URI de firma de acceso compartido de nivel de contenedor.

    ![Cuadro de diálogo Firma de acceso compartido.](media/vm/create-sas-uri-storage-explorer.png)

5. Seleccione **Crear** para crear el identificador URI de SAS asociado a este disco duro virtual.
6. Copie el identificador URI y guárdelo en un archivo de texto en una ubicación segura. Este identificador URI de SAS generado es para el acceso de nivel de contenedor. Para que sea específico, edite el archivo de texto para agregar el nombre del disco duro virtual.
7. Inserte el nombre del disco duro virtual después de la cadena vhds del identificador URI de SAS (incluya una barra diagonal inicial). El identificador URI de SAS final debe ser similar a este:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

8. Repita estos pasos con todos los discos duros virtuales de los planes que va a publicar.

### <a name="using-tool-2-azure-cli"></a>Uso de la herramienta 2: Azure CLI

1. Descargue e instale la [CLI de Microsoft Azure](https://azure.microsoft.com/documentation/articles/xplat-cli-install/). Hay versiones disponibles para Windows, macOS y varias distribuciones de Linux.
2. Cree un archivo de PowerShell (con la extensión de archivo .ps1), copie el código siguiente y luego guárdelo localmente.

    ```JSON
    az storage container generate-sas --connection-string ‘DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net’ --name <vhd-name> --permissions rl --start ‘<start-date>’ --expiry ‘<expiry-date>’
    ```

3. Edite el archivo para proporcionar los valores de parámetro siguientes. Proporcione las fechas en formato de fecha y hora UTC, como 2020-04-01T00:00:00Z.

    - account-name: nombre de su cuenta de Azure Storage.
    - account-key: clave de su cuenta de Azure Storage.
    - vhd-name: nombre del disco duro virtual.
    - start-date: fecha de inicio del permiso para acceder al disco duro virtual. Proporcione una fecha un día antes de la fecha actual.
    - expiry-date: fecha de expiración del permiso para acceder al disco duro virtual. Proporcione una fecha al menos tres semanas después de la fecha actual.

    Este es un ejemplo de valores de parámetro adecuados (en el momento de redactar este artículo):

    `az storage container generate-sas --connection-string ‘DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ON c+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net’ --name vhds -- permissions rl --start ‘2020-04-01T00:00:00Z’ --expiry ‘2021-04-01T00:00:00Z’`

1. Guarde los cambios.
2. Use uno de los siguientes métodos para ejecutar este script con privilegios administrativos para crear una cadena de conexión de SAS para el acceso de nivel de contenedor:

    - Ejecutar el script desde la consola. En Windows, haga clic con el botón derecho en el script y seleccione **Ejecutar como administrador** .
    - Ejecutar el script desde un editor de scripts de PowerShell, como [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise). En esta pantalla se muestra la creación de una cadena de conexión de SAS en este editor:

    [![creación de una cadena de conexión de SAS en el editor de PowerShell](media/vm/create-sas-uri-power-shell-ise.png)](media/vm/create-sas-uri-power-shell-ise.png#lightbox)

6. Copie la cadena de conexión de SAS y guárdela en un archivo de texto en una ubicación segura. Edite esta cadena para agregar la información de ubicación del disco duro virtual para crear el identificador URI de SAS final.
7. En Azure Portal, vaya al almacenamiento de blobs que contiene el disco duro virtual asociado con el nuevo identificador URI.
8. Copie la dirección URL del punto de conexión de Blob service:

    ![Copia de la dirección URL del punto de conexión de Blob service.](media/vm/create-sas-uri-blob-endpoint.png)

9. Modifique el archivo de texto con la cadena de conexión de SAS del paso 6. Cree el identificador URI de SAS completo con este formato:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

## <a name="verify-the-sas-uri"></a>Comprobación del URI de SAS

Compruebe el URI de SAS antes de publicarlo en el Centro de partners para evitar problemas relacionados con el URI de SAS después de enviar la solicitud. Este proceso es opcional, pero se recomienda.

- El identificador URI incluye el nombre de archivo de la imagen de disco duro virtual, incluida la extensión `.vhd`.
- Aparece `Sp=rl` en la parte central del identificador URI. Esta cadena muestra que se ha especificado el acceso de lectura y enumeración.
- Cuando aparece `sr=c`, significa que se especifica el acceso de nivel de contenedor.
- Copie y pegue el identificador URI en un explorador para probar y descargar el blob (puede cancelar la operación antes de que se complete la descarga).

## <a name="next-steps"></a>Pasos siguientes

- Si tiene problemas, consulte [Mensajes de error de SAS de VM](azure-vm-sas-failure-messages.md).
- [Inicio de sesión en el Centro de partners](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- [Creación de una oferta de máquina virtual en Azure Marketplace](azure-vm-create.md)
