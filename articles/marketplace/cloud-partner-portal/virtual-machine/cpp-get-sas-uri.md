---
title: Obtención del URI de firma de acceso compartido para la imagen de máquina virtual basada en Microsoft Azure | Azure Marketplace
description: Se explica cómo obtener el URI de firma de acceso compartido (SAS) para la imagen de máquina virtual.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 6fe15fb18d8865911363a4696e44dd7fe1d90c09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277810"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>Obtención del URI de firma de acceso compartido para la imagen de máquina virtual

Durante el proceso de publicación, debe proporcionar un identificador uniforme de recursos (URI) para cada disco duro virtual (VHD) asociado con las SKU. Microsoft necesita tener acceso a estos VHD durante el proceso de certificación. En este artículo se explica cómo generar un URI de firma de acceso compartido (SAS) para cada disco duro virtual. Tendrá que escribir este URI en la pestaña **SKU** en Cloud Partner Portal.

Al generar los URI de SAS para los discos duros virtuales, debe cumplir los requisitos siguientes:

- Solo se admiten los discos duros virtuales no administrados.
- Los permisos `List` y `Read` son suficientes. *No* proporcione acceso `Write` o `Delete`.
- La duración del acceso (*fecha de expiración*) debe ser un mínimo de tres semanas a partir de la creación del URI de SAS.
- Para proteger contra las variaciones de hora UTC, establezca la fecha de inicio en un día antes de la fecha actual. Por ejemplo, si la fecha actual es el 6 de octubre de 2014, seleccione 5/10/2014.

## <a name="generate-the-sas-url"></a>Generación de la dirección URL de SAS

La dirección URL de SAS se puede generar de dos maneras comunes con las herramientas siguientes:

- Explorador de Microsoft Azure Storage: herramienta gráfica disponible para Windows, macOS y Linux
- CLI de Microsoft Azure: recomendada para los sistemas operativos que no sean Windows y los entornos automatizados o de integración continua

### <a name="azure-cli"></a>Azure CLI

Siga estos pasos para generar un URI de SAS con la CLI de Azure.

1. Descargue e instale la [CLI de Microsoft Azure](https://azure.microsoft.com/documentation/articles/xplat-cli-install/). Hay versiones disponibles para Windows, macOS y varias distribuciones de Linux.
2. Cree un archivo de PowerShell (con la extensión de archivo `.ps1`), copie el código siguiente y luego guárdelo localmente.

   ```azurecli-interactive
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
   ```

3. Modifique el archivo para proporcionar los valores de parámetro siguientes.  Las fechas se deben proporcionar en formato de fecha y hora UTC, por ejemplo `2016-10-25T00:00:00Z`.
   - `<account-name>`: nombre de la cuenta de Azure Storage.
   - `<account-key>`: clave de la cuenta de Azure Storage.
   - `<vhd-name>`: nombre del disco duro virtual.
   - `<start-date>`: fecha de inicio del permiso para acceder al disco duro virtual. Proporcione una fecha un día antes de la fecha actual.
   - `<expiry-date>`: fecha de expiración del permiso para acceder al disco duro virtual.  Proporcione una fecha al menos tres semanas después de la fecha actual.

   En el ejemplo siguiente se muestran los valores de parámetro adecuados (en el momento de redactar este artículo).

   ```azurecli-interactive
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2017-11-06T00:00:00Z' --expiry '2018-08-20T00:00:00Z'
   ```

4. Guarde los cambios en este script de PowerShell.
5. Ejecute este script con privilegios administrativos, para generar un *cadena de conexión SAS* para el acceso de nivel de contenedor.  Puede usar dos enfoques básicos:
   - Ejecutar el script desde la consola.  Por ejemplo, en Windows, haga clic con el botón derecho en el script y seleccione **Ejecutar como administrador**.
   - Ejecutar el script desde un editor de scripts de PowerShell, como [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise), con privilegios administrativos.
     A continuación se muestra la generación de una cadena de conexión de SAS desde este editor.

     ![Generación de URI de SAS en PowerShell ISE](./media/publishvm_032.png)

6. Copie la cadena de conexión SAS resultante y guárdela en un archivo de texto en una ubicación segura.  Modificará esta cadena para agregarle la información de ubicación de los discos duros virtuales asociados para crear el URI de SAS final.
7. En Azure Portal, desplácese hasta el almacenamiento de blobs que contiene el disco duro virtual asociado con el URI recién generado.
8. Copie el valor de dirección URL del **Punto de conexión de Blob service**, como se muestra a continuación.

    ![Punto de conexión de Blob service en Azure Portal](./media/publishvm_033.png)

9. Modifique el archivo de texto con la cadena de conexión de SAS del paso 6.  Construirá el URI de SAS completo con el formato siguiente:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Por ejemplo, si es el nombre del disco duro virtual es `TestRGVM2.vhd`, el URI de SAS resultante sería:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Repita estos pasos con todos los discos duros virtuales de las SKU que quiera publicar.

### <a name="microsoft-storage-explorer"></a>Explorador de Microsoft Azure Storage

Siga estos pasos para generar un URI de SAS con el Explorador de Microsoft Azure Storage.

1. Descargue e instale el [Explorador de Microsoft Azure Storage](https://azure.microsoft.com/features/storage-explorer/).
2. Abra el explorador y, en la barra de menús de la izquierda, haga clic en el icono **Agregar cuenta**.  Se mostrará el cuadro de diálogo **Connect to Azure Storage** (Conectar con Azure Storage).
3. Seleccione **Add an Azure Account** (Agregar una cuenta de Azure) y haga clic en **Iniciar sesión**.  Siga los pasos necesarios para iniciar sesión en la cuenta de Azure.
4. En el panel **Explorador** de la izquierda, vaya a **Cuentas de almacenamiento** y expanda este nodo.
5. Haga clic con el botón derecho en el disco duro virtual y seleccione **Get Share Access Signature** (Obtener la firma de acceso compartido) en el menú contextual.

    ![Obtención del elemento SAS en el Explorador de Azure](./media/publishvm_034.png)

6. Se mostrará el cuadro de diálogo **Firma de acceso compartido**. Escriba valores para los campos siguientes:
   - **Fecha de inicio**: fecha de inicio del permiso para acceder al disco duro virtual. Proporcione una fecha que sea un día antes de la fecha actual.
   - **Fecha de expiración**: fecha de expiración del permiso para acceder al disco duro virtual.  Proporcione una fecha al menos tres semanas después de la fecha actual.
   - **Permisos**: seleccione los permisos `Read` y `List`.

     ![Cuadro de diálogo SAS en el Explorador de Azure](./media/publishvm_035.png)

7. Haga clic en **Crear** para crear el URI de SAS asociado a este disco duro virtual.  Ahora, en el cuadro de diálogo se muestran los detalles sobre esta operación.
8. Copie el valor **URL** y guárdelo en un archivo de texto en una ubicación segura.

    ![Creación del URI de SAS en el Explorador de Azure](./media/publishvm_036.png)

    Esta dirección URL de SAS generada es para el acceso de nivel de contenedor.  Para que sea específica, se le debe agregar el nombre del disco duro virtual.

9. Modifique el archivo de texto. Inserte el nombre del disco duro virtual después de la cadena `vhds` en la dirección URL de SAS (incluya una barra diagonal inicial).  El URI de SAS final debe tener este formato:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Por ejemplo, si es el nombre del disco duro virtual es `TestRGVM2.vhd`, el URI de SAS resultante sería:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Repita estos pasos con todos los discos duros virtuales de las SKU que quiera publicar.

## <a name="verify-the-sas-uri"></a>Comprobación del URI de SAS

Revise y compruebe todos los URI de SAS generados con la lista de comprobación siguiente.  Compruebe lo siguiente:

- El URI tiene el formato siguiente:       `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`.
- El URI contiene el nombre de archivo de la imagen de disco duro virtual, incluida la extensión ".vhd".
- Hacia el centro del URI, aparece `sp=rl`. Esta cadena indica que se ha especificado el acceso `Read` y `List`.
- Después de ese punto, también aparece `sr=c`. Esta cadena indica que se ha especificado el acceso de nivel de contenedor.
- Copie y pegue el URI en un explorador para empezar a descargar el blob asociado.  (Puede cancelar la operación antes de que finalice la descarga).

## <a name="next-steps"></a>Pasos siguientes

Si tiene dificultades para generar un URI de SAS, vea [Problemas de dirección URL de SAS comunes](./cpp-common-sas-url-issues.md).  En caso contrario, guarde el URI de SAS en una ubicación segura para su uso posterior. Será necesario [publicar la oferta de máquina virtual](./cpp-publish-offer.md) en Cloud Partner Portal.
