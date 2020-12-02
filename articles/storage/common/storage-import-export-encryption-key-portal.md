---
title: Uso de Azure Portal para configurar las claves administradas por el cliente para el servicio Import/Export
description: Aprenda a usar Azure Portal para configurar claves administradas por el cliente con Azure Key Vault para el cifrado de Import/Export. Las claves administradas por el cliente le permiten crear, rotar, deshabilitar y revocar los controles de acceso.
services: storage
author: alkohli
ms.service: storage
ms.topic: how-to
ms.date: 05/06/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: fb91a490083629101470565a630b659c090e071b
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843373"
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

2. Tiene una instancia existente de Azure Key Vault con una clave en ella, que puede usar para proteger su clave de BitLocker. Para obtener información sobre cómo crear un almacén de claves mediante Azure Portal, consulte [Inicio rápido: Creación de una instancia de Azure Key Vault mediante Azure Portal](../../key-vault/general/quick-create-portal.md).

    - **Eliminación temporal** y **No purgar** se han establecido en la instancia existente de Key Vault. Estas propiedades no están habilitadas de forma predeterminada. Para habilitar estas propiedades, consulte las secciones **Habilitación de la eliminación temporal** y **Habilitación de la protección de purgas** en cualquiera de los siguientes artículos:

        - [Uso de la eliminación temporal con PowerShell](../../key-vault/general/key-vault-recovery.md).
        - [Uso de la eliminación temporal con la CLI](../../key-vault/general/key-vault-recovery.md).
    - El almacén de claves existente debe tener una clave RSA con un tamaño de 2048 o más. Para obtener más información sobre las claves, consulte [Acerca de las claves](../../key-vault/keys/about-keys.md).
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

> [!IMPORTANT]
> Solo puede deshabilitar las claves administradas por Microsoft y pasar a claves administradas por el cliente en cualquier fase del trabajo de importación y exportación. Sin embargo, no se puede deshabilitar la clave administrada por el cliente una vez que la ha creado.

## <a name="troubleshoot-customer-managed-key-errors"></a>Solución de errores de claves administradas por el cliente

Si recibe errores relacionados con la clave administrada por el cliente, use la tabla siguiente para solucionar los problemas:

| Código de error     |Detalles     | ¿Recuperable?    |
|----------------|------------|-----------------|
| CmkErrorAccessRevoked | Se revoca el acceso a la clave que administra el cliente.                                                       | Sí, compruebe si: <ol><li>Key Vault todavía tiene el MSI en la directiva de acceso.</li><li>La directiva de acceso tiene habilitados los permisos Get, Wrap y Unwrap.</li><li>Si el almacén de claves está en una red virtual detrás del firewall, compruebe si la opción **Permitir servicios de confianza de Microsoft** está habilitada.</li><li>Compruebe si la característica MSI del recurso de trabajo se restableció en `None` mediante las API.<br>Si es así, vuelva a establecer el valor en `Identity = SystemAssigned`. Esto vuelve a crear la identidad para el recurso de trabajo.<br>Una vez creada la nueva identidad, habilite los permisos `Get`, `Wrap` y `Unwrap` en la identidad nueva de la directiva de acceso del almacén de claves.</li></ol>                                                                                            |
| CmkErrorKeyDisabled      | Se deshabilitó la clave administrada por el cliente.                                         | Sí, habilitando la versión de la clave.     |
| CmkErrorKeyNotFound      | No se encuentra la clave administrada por el cliente. | Sí, si la clave se ha eliminado, pero todavía está dentro de la duración de la purga, mediante [Deshacer la eliminación de la clave del almacén de claves](/powershell/module/az.keyvault/undo-azkeyvaultkeyremoval).<br>De lo contrario, <ol><li>Sí, si el cliente tiene una copia de seguridad de la clave y la restaura.</li><li>De lo contrario, no.</li></ol>
| CmkErrorVaultNotFound |No se encuentra el almacén de claves de la clave administrada por el cliente. |   Si se ha eliminado el almacén de claves:<ol><li>Sí, si está dentro de la duración de la protección de purga, con los pasos descritos en [Recuperación de un almacén de claves](../../key-vault/general/soft-delete-overview.md#key-vault-recovery).</li><li>No, si está fuera de la duración de la protección de purga.</li></ol><br>De lo contrario, si el almacén de claves se migró a otro inquilino, sí, se puede recuperar con uno de estos pasos:<ol><li>Revierta el almacén de claves de vuelta al inquilino anterior.</li><li>Establezca `Identity = None` y, luego, vuelva a establecer el valor en `Identity = SystemAssigned`. Esto elimina y vuelve a crear la identidad una vez que se crea la identidad nueva. Habilite los permisos `Get`, `Wrap` y `Unwrap` a la identidad nueva en la directiva de acceso del almacén de claves.</li></ol>|

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)