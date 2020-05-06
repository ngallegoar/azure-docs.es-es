---
title: Uso de Azure Portal para configurar las claves administradas por el cliente para el servicio Import/Export
description: Aprenda a usar Azure Portal para configurar claves administradas por el cliente con Azure Key Vault para el cifrado de Import/Export. Las claves administradas por el cliente le permiten crear, rotar, deshabilitar y revocar los controles de acceso.
services: storage
author: alkohli
ms.service: storage
ms.topic: how-to
ms.date: 03/12/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: d3e4535c05ef077d14ef74310459a84af0f02fd5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2020
ms.locfileid: "82176335"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-importexport-service"></a>Uso de claves administradas por el cliente en Azure Key Vault para el servicio Import/Export

Azure Import/Export protege las claves de BitLocker que se usan para bloquear las unidades mediante una clave de cifrado. De manera predeterminada, las claves de BitLocker se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar también claves administradas por el cliente.

Las claves administradas por el cliente se deben crear y almacenar en una instancia de Azure Key Vault. Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)

En este artículo se muestra cómo usar las claves administradas por el cliente con el servicio Import/Export en [Azure Portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, asegúrese de que:

1. Ha creado un trabajo de importación o de exportación en función de las instrucciones de:

    - [Creación de un trabajo de importación para blobs](storage-import-export-data-to-blobs.md).
    - [Creación de un trabajo de importación para archivos](storage-import-export-data-to-files.md).
    - [Creación de un trabajo de exportación para blobs](storage-import-export-data-from-blobs.md).

2. Tiene una instancia existente de Azure Key Vault con una clave en ella, que puede usar para proteger su clave de BitLocker. Para obtener información sobre cómo crear un almacén de claves mediante Azure Portal, consulte [Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure Portal](../../key-vault/secrets/quick-create-portal.md).

    - **Eliminación temporal** y **No purgar** se han establecido en la instancia existente de Key Vault. Estas propiedades no están habilitadas de forma predeterminada. Para habilitar estas propiedades, consulte las secciones **Habilitación de la eliminación temporal** y **Habilitación de la protección de purgas** en cualquiera de los siguientes artículos:

        - [Uso de la eliminación temporal con PowerShell](../../key-vault/general/soft-delete-powershell.md).
        - [Uso de la eliminación temporal con la CLI](../../key-vault/general/soft-delete-cli.md).
    - El almacén de claves existente debe tener una clave RSA con un tamaño de 2048 o más. Para obtener más información acerca de las claves, consulte la sección **Claves en Key Vault** en [Información acerca de claves, secretos y certificados de Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).
    - El almacén de claves debe estar en la misma región que la cuenta de almacenamiento para los datos.  
    - Si no tiene una instancia existente de Azure Key Vault, también puede crearla insertada, como se describe en la sección siguiente.

## <a name="enable-keys"></a>Habilitación de claves

La configuración de la clave administrada por el cliente para el servicio Import/Export es opcional. De forma predeterminada, el servicio Import/Export usa una clave administrada por Microsoft para proteger la clave de BitLocker. Para habilitar claves administradas del cliente en Azure Portal, siga estos pasos:

1. Vaya a la hoja **Información general** del trabajo de importación.
2. En el panel derecho, seleccione **Elija cómo se cifran las claves de BitLocker**.

    ![Elección de una opción de cifrado](./media/storage-import-export-encryption-key-portal/encryption-key-1.png)

3. En la hoja **Cifrado**, puede ver y copiar la clave de BitLocker del dispositivo. En **Tipo de cifrado**, puede elegir cómo desea proteger la clave de BitLocker. De forma predeterminada, se usa una clave administrada por Microsoft.

    ![Visualización de la clave de BitLocker](./media/storage-import-export-encryption-key-portal/encryption-key-2.png)

4. Tiene la opción de especificar una clave administrada por el cliente. Después de seleccionar la clave administrada por el cliente, **Seleccione un almacén de claves y una clave**.

    ![Selección de una clave administrada por el cliente](./media/storage-import-export-encryption-key-portal/encryption-key-3.png)

5. En la hoja **Seleccionar clave en Azure Key Vault**, la suscripción se rellena automáticamente. Para **Almacén de claves**, puede seleccionar un almacén de claves existente de la lista desplegable.

    ![Selección o creación de una instancia de Azure Key Vault](./media/storage-import-export-encryption-key-portal/encryption-key-4.png)

6. También puede seleccionar **Crear nuevo** para crear un nuevo almacén de claves. En la hoja **Crear el almacén de claves**, escriba el grupo de recursos y el nombre del almacén de claves. Acepte todos los otros valores predeterminados. Seleccione **Revisar + crear**.

    ![Creación de una nueva instancia de Azure Key Vault](./media/storage-import-export-encryption-key-portal/encryption-key-5.png)

7. Revise la información asociada con el almacén de claves y seleccione **Crear**. Espere un par de minutos hasta que se complete la creación del almacén de claves.

    ![Crear una instancia de Azure Key Vault](./media/storage-import-export-encryption-key-portal/encryption-key-6.png)

8. En la hoja **Seleccionar clave en Azure Key Vault**, puede seleccionar una clave del almacén de claves existente.

9. Si creó un nuevo almacén de claves, seleccione **Crear nuevo** para crear una clave. El tamaño de la clave RSA puede ser de 2048 o superior.

    ![Creación de una clave nueva en Azure Key Vault](./media/storage-import-export-encryption-key-portal/encryption-key-7.png)

    Si la eliminación temporal y la protección de purga no están habilitadas al crear el almacén de claves, este se actualizará para que tenga habilitada la eliminación temporal y la protección de purga.

10. Proporcione el nombre de la clave, acepte los otros valores predeterminados y seleccione **Crear**.

    ![Creación de una clave nueva](./media/storage-import-export-encryption-key-portal/encryption-key-8.png)

11. Seleccione la **Versión** y, a continuación, elija **Seleccionar**. Se le notificará que se ha creado una clave en el almacén de claves.

    ![Nueva clave creada en el almacén de claves](./media/storage-import-export-encryption-key-portal/encryption-key-9.png)

En la hoja **Cifrado**, puede ver el almacén de claves y la clave seleccionados para la clave administrada por el cliente.

## <a name="disable-keys"></a>Deshabilitación de claves

Solo puede deshabilitar las claves administradas por Microsoft y pasar a claves administradas por el cliente en cualquier fase del trabajo de importación y exportación. Sin embargo, no se puede deshabilitar la clave administrada por el cliente una vez que la ha creado.

## <a name="troubleshoot-customer-managed-key-errors"></a>Solución de errores de claves administradas por el cliente

Si recibe errores relacionados con la clave administrada por el cliente, use la tabla siguiente para solucionar los problemas:

| Código de error     |Detalles     | ¿Recuperable?    |
|----------------|------------|-----------------|
| CmkErrorAccessRevoked | Se aplicó una clave administrada por el cliente, pero el acceso a la clave está revocado actualmente. Para obtener más información, consulte cómo [habilitar el acceso a las claves](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy).                                                      | Sí, compruebe si: <ol><li>Key Vault todavía tiene el MSI en la directiva de acceso.</li><li>La directiva de acceso proporciona permisos para Obtener, Ajustar y Desajustar.</li><li>Si el almacén de claves está en una red virtual detrás del firewall, compruebe si la opción **Permitir servicios de Microsoft de confianza** está habilitada.</li></ol>                                                                                            |
| CmkErrorKeyDisabled      | Se aplicó una clave administrada por el cliente, pero está deshabilitada. Para obtener más información, consulte cómo [habilitar la clave](https://docs.microsoft.com/rest/api/keyvault/vaults/createorupdate).                                                                             | Sí, habilitando la versión de la clave.     |
| CmkErrorKeyNotFound      | Se aplicó una clave administrada por el cliente, pero no se encuentra el almacén de claves asociado a la clave.<br>Si eliminó el almacén de claves, no puede recuperar la clave administrada por el cliente.  Si migró el almacén de claves a otro inquilino, consulte [Cambio del identificador de inquilino de Key Vault después de mover una suscripción](https://docs.microsoft.com/azure/key-vault/key-vault-subscription-move-fix). |   Si eliminó el almacén de claves:<ol><li>Sí, si está dentro de la duración de la protección de purga, con los pasos descritos en [Recuperación de un almacén de claves](https://docs.microsoft.com/azure/key-vault/general/soft-delete-powershell#recovering-a-key-vault).</li><li>No, si está fuera de la duración de la protección de purga.</li></ol><br>De lo contrario, si el almacén de claves pasó por una migración de inquilinos, sí, se puede recuperar con uno de estos pasos: <ol><li>Revierta el almacén de claves de vuelta al inquilino anterior.</li><li>Establezca `Identity = None` y, luego, vuelva a establecer el valor en `Identity = SystemAssigned`. Esto elimina y vuelve a crear la identidad una vez que se crea la identidad nueva. Habilite los permisos `Get`, `Wrap` y `Unwrap` a la identidad nueva en la directiva de acceso del almacén de claves.</li></ol>|

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
