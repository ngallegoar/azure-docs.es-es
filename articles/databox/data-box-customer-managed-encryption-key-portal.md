---
title: Uso de Azure Portal para administrar las claves administradas por el cliente para Azure Data Box
description: Aprenda a usar Azure Portal para crear y administrar claves administradas por el cliente con Azure Key Vault para Azure Data Box. Las claves administradas por el cliente le permiten crear, rotar, deshabilitar y revocar los controles de acceso.
services: databox
author: alkohli
ms.service: databox
ms.topic: how-to
ms.date: 11/19/2020
ms.author: alkohli
ms.subservice: pod
ms.openlocfilehash: cd9f4ad6b6831b2b15c09b37edc569b3f2d247f7
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94958206"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-azure-data-box"></a>Uso de claves administradas por el cliente en Azure Key Vault para Azure Data Box

Azure Data Box se encarga de proteger la clave de desbloqueo del dispositivo (también conocida como contraseña del dispositivo), que se usa para bloquear el dispositivo a través de una clave de cifrado. De forma predeterminada, esta clave de cifrado es una clave administrada por Microsoft. Para un control adicional, puede usar una clave administrada por el cliente.

El uso de una clave administrada por el cliente no afecta a cómo se cifran los datos del dispositivo. Solo afecta al modo en que se cifra la clave de desbloqueo del dispositivo.

Para mantener este nivel de control a lo largo del proceso de pedido, use una clave administrada por el cliente cuando lo cree. Para más información, consulte el [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-ordered.md).

En este artículo se muestra cómo habilitar una clave administrada por el cliente para un pedido de Data Box existente en [Azure Portal](https://portal.azure.com/). Verá cómo cambiar el almacén de claves, la clave, la versión o la identidad de la clave administrada por el cliente actual, o volver a usar una clave administrada por Microsoft.

La información de este artículo se aplica tanto a dispositivos con Azure Data Box como con Azure Data Box Heavy.

## <a name="requirements"></a>Requisitos

La clave administrada por el cliente para un pedido de Data Box debe cumplir los siguientes requisitos:

- La clave se debe crear y almacenar en un almacén de Azure Key Vault que tenga habilitada las opciones **Eliminación temporal** y **No purgar**. Para obtener más información, consulte [¿Qué es Azure Key Vault?](../key-vault/general/overview.md) Puede crear un almacén de claves y una clave al crear o actualizar el pedido.

- La clave debe ser una clave RSA de un tamaño de 2048 o superior.

## <a name="enable-key"></a>Habilitación de la clave

Para habilitar una clave administrada por el cliente para el pedido de Data Box existente en Azure Portal, siga estos pasos:

1. Vaya a la pantalla de **información general** del pedido de Data Box.

    ![Pantalla de información general de un pedido de Data Box-1](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-1.png)

2. Vaya a **Configuración > Cifrado** y seleccione **Clave administrada por el cliente**. Elija **Seleccione un almacén de claves y una clave**.

    ![Selección de la opción de cifrado de clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3.png)

   En la pantalla **Seleccionar clave en Azure Key Vault**, la suscripción se rellena automáticamente.

 3. Para **Almacén de claves**, puede seleccionar un almacén de claves existente de la lista desplegable o seleccionar **Crear nuevo** y crear un nuevo almacén de claves.

     ![Opciones del almacén de claves al seleccionar una clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3-a.png)

     Para crear un nuevo almacén de claves, escriba la suscripción, el grupo de recursos, el nombre del almacén de claves y otra información en la pantalla **Crear nuevo almacén de claves**. En **Opciones de recuperación**, asegúrese de que **Eliminación temporal** y **Protección de purga** están habilitadas. Después, seleccione **Revisar y crear**.

      ![Revisar y crear en Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-4.png)

      Revise la información del almacén de claves y seleccione **Crear**. Espere un par de minutos hasta que se complete la creación del almacén de claves.

       ![Creación de una instancia de Azure Key Vault con la configuración](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-5.png)

4. En la pantalla **Seleccionar clave en Azure Key Vault**, puede seleccionar una clave existente del almacén de claves o crear una nueva.

    ![Seleccionar clave en Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6.png)

   Si quiere crear una clave nueva, seleccione **Crear nuevo**. Debe usar una clave RSA. El tamaño puede ser de 2048 o superior.

    ![Creación de una clave nueva en Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6-a.png)

    Escriba un nombre para la clave nueva, acepte los otros valores predeterminados y seleccione **Crear**. Se le notificará que se ha creado una clave en el almacén de claves.

    ![Nombre de la nueva clave](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-7.png)

5. En **Versión**, puede seleccionar una versión de clave existente en la lista desplegable.

    ![Selección de la versión de la nueva clave](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8.png)

    Si desea generar una nueva versión de clave, seleccione **Crear nuevo**.

    ![Apertura de un cuadro de diálogo para crear una nueva versión de clave](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8-a.png)

    Elija la configuración de la nueva versión de la clave y seleccione **Crear**.

    ![Creación de una nueva versión de la clave](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8-b.png)

6. Cuando haya seleccionado un almacén de claves, una clave y una versión de clave, elija **Seleccionar**.

    ![Una clave en una instancia de Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8-c.png)

    La configuración de **Tipo de cifrado** muestra el almacén de claves y la clave que eligió.

    ![Clave y almacén de claves para la clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-9.png)

7. Seleccione el tipo de identidad que se va a usar para administrar la clave administrada por el cliente para este recurso. Puede usar la identidad **asignada por el sistema** que se generó durante la creación del pedido o elegir una identidad asignada por el usuario.

    Una identidad asignada por el usuario es un recurso independiente que puede usar para administrar el acceso a los recursos. Para más información, vea [Tipos de identidad administrada](/azure/active-directory/managed-identities-azure-resources/overview).

    ![Selección del tipo identidad](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-13.png)

    Para asignar una identidad de usuario, seleccione **Asignado por el usuario**. A continuación, seleccione **Seleccionar una identidad de usuario** y seleccione la identidad administrada que desea usar.

    ![Selección de una identidad para usar](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-14.png)

    Aquí no se puede crear una nueva identidad de usuario. Para aprender a crear una, consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada asignada por el usuario mediante Azure Portal](/azure-docs/blob/master/articles/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal).

    La identidad de usuario seleccionada se muestra en la configuración de **Tipo de cifrado**.

    ![Se muestra una identidad de usuario en la configuración Tipo de cifrado.](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-15.png)

 9. Seleccione **Guardar** para guardar la configuración de **Tipo de cifrado** actualizada.

     ![Guardado de la clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-10.png)

    La dirección URL de la clave se muestra en el **tipo de cifrado**.

    ![Dirección URL de la clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-11.png)<!--Probably need new screen from recent order. Can you provide one? I can't create an order using CMK with the subscription I'm using.-->

## <a name="change-key"></a>Cambio de clave

Para cambiar el almacén de claves, la clave o la versión de clave para la clave administrada por el cliente que está usando actualmente, siga estos pasos:

1. En la pantalla de **información general** del pedido de Data Box, vaya a **Configuración** > **Cifrado** y haga clic en **Cambiar clave**.

    ![Pantalla de información general de un pedido de Data Box con clave administrada por el cliente-1](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-16.png)

2. Elija **Select a different key vault and key** (Seleccione un almacén de claves y una clave diferentes).

    ![Pantalla de información general de un orden de Data Box, opción Seleccione un almacén de claves y una clave diferentes](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-16-a.png)

3. En la pantalla **Selección de clave del almacén de claves** se muestra la suscripción pero no el almacén de claves, la clave o la versión de clave. Puede hacer alguno de los siguientes cambios:

   - Seleccione una clave diferente del mismo almacén de claves. Deberá seleccionar el almacén de claves antes de seleccionar la clave y la versión.

   - Seleccione un almacén de claves diferente y asigne una nueva clave.

   - Cambie la versión de la clave actual.
   
    Cuando haya terminado de realizar los cambios, elija **Seleccionar**.

    ![Elección de una opción de cifrado - 2](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-17.png)

4. Seleccione **Guardar**.

    ![Guardado de la configuración de cifrado actualizada - 1](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-17-a.png)

## <a name="change-identity"></a>Cambio de identidad

Para cambiar la identidad que se usa para administrar el acceso a la clave administrada por el cliente para este pedido, siga estos pasos:

1. En la pantalla de **información general** del pedido de Data Box completado, vaya a **Configuración** > **Cifrado**.

2. Realice uno de los siguientes cambios:

     - Para cambiar a una identidad de usuario diferente, haga clic en **Select a different user identity** (Seleccionar una identidad de usuario diferente). Después, seleccione una identidad diferente en el panel del lado derecho de la pantalla y elija **Seleccionar**.

       ![Opción para cambiar la identidad asignada por el usuario para una clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-18.png)

   - Para cambiar a la identidad asignada por el sistema generada durante la creación del pedido, seleccione **Asignado por el sistema** mediante **Select identity type** (Seleccionar tipo de identidad).

     ![Opción para cambiar a asignado por el sistema para una clave administrada por el cliente](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-19.png)

3. Seleccione **Guardar**.

    ![Guardado de la configuración de cifrado actualizada - 2](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-17-a.png)

## <a name="use-microsoft-managed-key"></a>Uso de una clave administrada por Microsoft

Para cambiar del uso de una clave administrada por el cliente a la clave administrada para Microsoft para su pedido, siga estos pasos:

1. En la pantalla de **información general** del pedido de Data Box completado, vaya a **Configuración** > **Cifrado**.

2. En **Seleccionar tipo**, seleccione **Microsoft managed key** (Clave administrada por Microsoft).

    ![Pantalla de información general de un pedido de Data Box - 5](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-20.png)

3. Seleccione **Guardar**.

    ![Guardado de la configuración de cifrado actualizada para una clave administrada de Microsoft](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-21.png)

## <a name="troubleshoot-errors"></a>Solución de errores

Si recibe errores relacionados con la clave administrada por el cliente, use la tabla siguiente para solucionar los problemas.

| Código de error| Detalles del error| ¿Recuperable?|
|-------------|--------------|---------|
| SsemUserErrorEncryptionKeyDisabled| No se pudo capturar la clave de paso porque la clave administrada por el cliente está deshabilitada.| Sí, habilitando la versión de la clave.|
| SsemUserErrorEncryptionKeyExpired| No se pudo capturar la clave de paso porque la clave administrada por el cliente ha expirado.| Sí, habilitando la versión de la clave.|
| SsemUserErrorKeyDetailsNotFound| No se pudo capturar la clave de paso porque no se encontró la clave administrada por el cliente.| Si eliminó el almacén de claves, no puede recuperar la clave administrada por el cliente.  Si migró el almacén de claves a otro inquilino, consulte [Cambio del identificador de inquilino de Key Vault después de mover una suscripción](../key-vault/general/move-subscription.md). Si eliminó el almacén de claves:<ol><li>Sí, si está dentro de la duración de la protección de purga, con los pasos descritos en [Recuperación de un almacén de claves](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault).</li><li>No, si está fuera de la duración de la protección de purga.</li></ol><br>De lo contrario, si el almacén de claves pasó por una migración de inquilinos, sí, se puede recuperar con uno de estos pasos: <ol><li>Revierta el almacén de claves de vuelta al inquilino anterior.</li><li>Establezca `Identity = None` y, luego, vuelva a establecer el valor en `Identity = SystemAssigned`. Esto elimina y vuelve a crear la identidad una vez que se crea la identidad nueva. Habilite los permisos `Get`, `Wrap` y `Unwrap` a la identidad nueva en la directiva de acceso del almacén de claves.</li></ol> |
| SsemUserErrorKeyVaultBadRequestException | Se aplicó una clave administrada por el cliente, pero el acceso a la clave no se ha concedido o se ha revocado, o no se puede acceder al almacén de claves porque el firewall está habilitado. | Agregue la identidad seleccionada al almacén de claves para habilitar el acceso a la clave administrada por el cliente. Si el almacén de claves tiene el firewall habilitado, cambie a una identidad asignada por el sistema y, a continuación, agregue una clave administrada por el cliente. Para obtener más información, consulte cómo [habilitar la clave](#enable-key). |
| SsemUserErrorKeyVaultDetailsNotFound| No se pudo capturar la clave de paso porque no se encontró el almacén de claves asociado para la clave administrada por el cliente. | Si eliminó el almacén de claves, no puede recuperar la clave administrada por el cliente.  Si migró el almacén de claves a otro inquilino, consulte [Cambio del identificador de inquilino de Key Vault después de mover una suscripción](../key-vault/general/move-subscription.md). Si eliminó el almacén de claves:<ol><li>Sí, si está dentro de la duración de la protección de purga, con los pasos descritos en [Recuperación de un almacén de claves](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault).</li><li>No, si está fuera de la duración de la protección de purga.</li></ol><br>De lo contrario, si el almacén de claves pasó por una migración de inquilinos, sí, se puede recuperar con uno de estos pasos: <ol><li>Revierta el almacén de claves de vuelta al inquilino anterior.</li><li>Establezca `Identity = None` y, luego, vuelva a establecer el valor en `Identity = SystemAssigned`. Esto elimina y vuelve a crear la identidad una vez que se crea la identidad nueva. Habilite los permisos `Get`, `Wrap` y `Unwrap` a la identidad nueva en la directiva de acceso del almacén de claves.</li></ol> |
| SsemUserErrorSystemAssignedIdentityAbsent  | No se pudo capturar la clave de paso porque no se encontró la clave administrada por el cliente.| Sí, compruebe si: <ol><li>Key Vault todavía tiene el MSI en la directiva de acceso.</li><li>La identidad es de tipo Sistema asignado.</li><li>Habilite los permisos Get, Wrap y Unwrap a la identidad de la directiva de acceso del almacén de claves.</li></ol>|
| SsemUserErrorUserAssignedLimitReached | Se produjo un error al agregar una identidad asignada por el usuario porque alcanzó el límite del número total de identidades asignadas por el usuario que se pueden agregar. | Vuelva a intentar la operación con menos identidades de usuario o quite algunas identidades asignadas por el usuario del recurso antes de volver a intentarlo. |
| SsemUserErrorCrossTenantIdentityAccessForbidden | Error en la operación de acceso a la identidad administrada. <br> Nota: Esto es para el escenario en el que la suscripción se traslada a un inquilino diferente. El cliente tiene que trasladar manualmente la identidad al nuevo inquilino. Póngase en contacto con nosotros para obtener más detalles. | Mueva la identidad seleccionada al nuevo inquilino en el que está presente la suscripción. Para obtener más información, consulte cómo [habilitar la clave](#enable-key). |
| SsemUserErrorKekUserIdentityNotFound | Se aplicó una clave administrada por el cliente, pero la identidad asignada por el usuario que tiene acceso a la clave no se encontró en Active Directory. <br> Nota: Esto sucede cuando se elimina la identidad del usuario de Azure.| Pruebe a agregar una nueva identidad asignada por el usuario diferente al almacén de claves para habilitar el acceso a la clave administrada por el cliente. Para obtener más información, consulte cómo [habilitar la clave](#enable-key). |
| SsemUserErrorUserAssignedIdentityAbsent | No se pudo capturar la clave de paso porque no se encontró la clave administrada por el cliente. | No se pudo tener acceso a la clave administrada por el cliente. Se ha eliminado la identidad asignada por el usuario asociada con la clave o el tipo de esta identidad ha cambiado. |
| SsemUserErrorCrossTenantIdentityAccessForbidden | Error en la operación de acceso a la identidad administrada. <br> Nota: Esto es para el escenario en el que la suscripción se traslada a un inquilino diferente. El cliente tiene que trasladar manualmente la identidad al nuevo inquilino. Póngase en contacto con nosotros para obtener más detalles. | Pruebe a agregar una nueva identidad asignada por el usuario diferente al almacén de claves para habilitar el acceso a la clave administrada por el cliente. Para obtener más información, consulte cómo [habilitar la clave](#enable-key).|
| SsemUserErrorKeyVaultBadRequestException | Se aplicó una clave administrada por el cliente, pero el acceso a la clave no se ha concedido o se ha revocado, o no se puede acceder al almacén de claves porque el firewall está habilitado. | Agregue la identidad seleccionada al almacén de claves para habilitar el acceso a la clave administrada por el cliente. Si el almacén de claves tiene el firewall habilitado, cambie a una identidad asignada por el sistema y, a continuación, agregue una clave administrada por el cliente. Para obtener más información, consulte cómo [habilitar la clave](#enable-key). |
| Error genérico  | No se pudo capturar la clave de paso.| Este es un error genérico. Póngase en contacto con el Soporte técnico de Microsoft para solucionar el error y determinar los pasos siguientes.|

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../key-vault/general/overview.md)
- [Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure Portal](../key-vault/secrets/quick-create-portal.md)
