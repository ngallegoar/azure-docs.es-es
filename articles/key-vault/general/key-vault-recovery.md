---
title: Introducción a la recuperación de Azure Key Vault | Microsoft Docs
description: Las características de recuperación de Key Vault están diseñadas para evitar la eliminación accidental o malintencionada del almacén de claves y los secretos, así como de las claves y certificados que se guardan en el almacén de claves.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: ShaneBala-keyvault
ms.author: sudbalas
manager: ravijan
ms.date: 09/30/2020
ms.openlocfilehash: bea4a5d92309710645dd63e611cd0a5e3b742c34
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91604097"
---
# <a name="azure-key-vault-recovery-overview"></a>Introducción a la recuperación de Azure Key Vault

En este artículo se explican dos características de recuperación de Azure Key Vault: la eliminación temporal y la protección de purga. En este documento se proporciona información general sobre estas características y se muestra cómo administrarlas a través de Azure Portal, la CLI de Azure y Azure PowerShell.

## <a name="what-are-soft-delete-and-purge-protection"></a>Qué es la eliminación temporal y la protección de purga

La protección de eliminación temporal y la protección de purga son dos características de recuperación diferentes del almacén de claves.
> [!IMPORTANT]
> La protección de eliminación temporal debe estar habilitada en todos los almacenes de claves. La capacidad de deshabilitar la protección contra la eliminación temporal quedará en desuso en diciembre de 2020. Consulte todos los detalles [**aquí**.](soft-delete-change.md)

La **eliminación temporal** está diseñada para evitar la eliminación accidental del almacén de claves y las claves, los secretos y los certificados almacenados en el mismo. Puede considerar la eliminación temporal como una papelera de reciclaje. Cuando se elimina un almacén de claves o un objeto del mismo, podrá recuperarlo durante el período de retención que haya configurado el usuario o un valor predeterminado de 90 días. Los almacenes de claves con estado de eliminación temporal también se pueden **purgar**, lo que significa que se eliminan de forma permanente. Esto le permite volver a crear almacenes de claves y sus objetos con el mismo nombre. Tanto la recuperación como la eliminación de los almacenes de claves y sus objetos requieren permisos elevados de directiva de acceso. **Una vez habilitada la eliminación temporal, no se puede deshabilitar.**

Es importante tener en cuenta que **los nombres de los almacenes de claves son únicos de forma global**, por lo que no podrá crear un almacén de claves que tenga el mismo nombre que un almacén de claves con estado de eliminación temporal. Del mismo modo, los nombres de claves, secretos y certificados son únicos en un almacén de claves. No podrá crear un secreto, una clave o un certificado que tenga el mismo nombre que otro que cuente con el estado de eliminación temporal.

La **protección de purga** está diseñada para evitar la eliminación de su almacén de claves, las claves, los secretos y los certificados de un agente interno malintencionado. Considere esto como una papelera de reciclaje con un bloqueo basado en el tiempo. Puede recuperar los elementos en cualquier momento durante el período de retención que haya configurado. Recuerde que **no podrá eliminar o purgar un almacén de claves permanentemente hasta que transcurra el período de retención.** Una vez que transcurra el período de retención establecido, el objeto del almacén de claves se purgará automáticamente.

> [!NOTE]
> La protección de purga está diseñada para que ningún permiso o rol de administrador pueda invalidar, deshabilitar o eludir esa protección de purga. **Una vez habilitada la protección de purga, ningún usuario ni Microsoft podrán deshabilitarla o invalidarla.** Esto significa que deberá recuperar el almacén de claves eliminado o esperar a que transcurra el período de retención antes de reutilizar el nombre del almacén de claves.

# <a name="azure-portal"></a>[Azure Portal](#tab/azure-portal)

## <a name="verify-if-soft-delete-is-enabled-on-a-key-vault-and-enable-soft-delete"></a>Comprobar si la eliminación temporal está habilitada en un almacén de claves y habilitarla

1. Inicie sesión en el Portal de Azure.
1. Seleccione el almacén de claves.
1. Haga clic en la hoja "Propiedades".
1. Compruebe si el botón de radio situado junto a eliminación temporal está establecido en "Habilitar recuperación".
1. Si no está habilitada la eliminación temporal en el almacén de claves, haga clic en el botón de radio para habilitarla y haga clic en "Guardar".

:::image type="content" source="../media/key-vault-recovery-1.png" alt-text="<Captura de pantalla de Azure Portal>":::

## <a name="grant-access-to-a-service-principal-to-purge-and-recover-deleted-secrets"></a>Conceder acceso a una entidad de servicio para purgar y recuperar los secretos eliminados

1. Inicie sesión en el Portal de Azure.
1. Seleccione el almacén de claves.
1. Haga clic en la hoja "Directiva de acceso".
1. En la tabla, busque la fila de la entidad de seguridad a la que quiere conceder acceso (o agregue una nueva entidad de seguridad).
1. Haga clic en la lista desplegable de claves, certificados y secretos.
1. Desplácese hasta la parte inferior de la lista desplegable y haga clic en "Recuperar" y "Purgar".
1. Las entidades de seguridad también necesitarán las funcionalidades de obtención y lista para realizar la mayoría de las operaciones.

:::image type="content" source="../media/key-vault-recovery-2.png" alt-text="<Captura de pantalla de Azure Portal>":::

## <a name="list-recover-or-purge-a-soft-deleted-key-vault"></a>Enumerar, recuperar o purgar un almacén de claves eliminado temporalmente

1. Inicie sesión en el Portal de Azure.
1. Haga clic en la barra de búsqueda de la parte superior de la página.
1. En "Servicios recientes", haga clic en "Key Vault". No haga clic en un almacén de claves individual.
1. En la parte superior de la pantalla, haga clic en la opción "Manage deleted vaults" (Administrar almacenes eliminados).
1. Se abrirá un panel de contexto en el lado derecho de la pantalla.
1. Seleccione su suscripción.
1. Si el almacén de claves se ha eliminado temporalmente, aparecerá en el panel de contexto de la derecha.
1. Si hay demasiados almacenes, puede hacer clic en "Cargar más" en la parte inferior del panel de contexto o usar la CLI o PowerShell para obtener los resultados.
1. Una vez que encuentre el almacén que quiera recuperar o purgar, active la casilla situada junto a él.
1. Seleccione la opción de recuperación en la parte inferior del panel de contexto si quiere recuperar el almacén de claves.
1. Si por el contrario quiere eliminar de forma permanente el almacén de claves, seleccione la opción de purga.

:::image type="content" source="../media/key-vault-recovery-3.png" alt-text="<Captura de pantalla de Azure Portal>":::

:::image type="content" source="../media/key-vault-recovery-4.png" alt-text="<Captura de pantalla de Azure Portal>":::

## <a name="list-recover-or-purge-soft-deleted-secrets-keys-and-certificates"></a>Enumerar, recuperar o purgar secretos, claves y certificados eliminados temporalmente

1. Inicie sesión en el Portal de Azure.
1. Seleccione el almacén de claves.
1. Seleccione la hoja correspondiente al tipo de secreto que quiera administrar (claves, secretos o certificados).
1. En la parte superior de la pantalla, haga clic en "Manage deleted (keys, secrets, or certificates)" (Administrar eliminados [claves, secretos o certificados]).
1. Aparecerá un panel de contexto en el lado derecho de la pantalla.
1. Si el secreto, la clave o el certificado no aparecen en la lista, esto quiere decir que no se encuentra en el estado de eliminación temporal.
1. Seleccione el secreto, la clave o el certificado que quiera administrar.
1. Seleccione la opción para recuperar o purgar que está en la parte inferior del panel de contexto.

:::image type="content" source="../media/key-vault-recovery-5.png" alt-text="<Captura de pantalla de Azure Portal>":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

## <a name="key-vault-cli"></a>Key Vault (CLI)

* Comprobar si un almacén de claves tiene habilitada la eliminación temporal

    ```azurecli
    az keyvault show --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}
    ```

* Habilitar la eliminación temporal en el almacén de claves

    Todos los nuevos almacenes de claves tienen habilitada la eliminación temporal de forma predeterminada. Si actualmente tiene un almacén de claves que no tiene habilitada la eliminación temporal, use el siguiente comando para habilitarla.

    ```azurecli
    az keyvault update --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME} --enable-soft-delete true
    ```

* Eliminar el almacén de claves (recuperable si está habilitada la eliminación temporal)

    ```azurecli
    az keyvault delete --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}
    ```

* Lista de todos los almacenes de claves eliminados temporalmente

    ```azurecli
    az keyvault list-deleted --subscription {SUBSCRIPTION ID} --resource-type vault
    ```

* Recuperación de un almacén de claves eliminado temporalmente

    ```azurecli
    az keyvault recover --subscription {SUBSCRIPTION ID} -n {VAULT NAME}
    ```

* Purgar el almacén de claves eliminado temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE EL ALMACÉN DE CLAVES).**

    ```azurecli
    az keyvault purge --subscription {SUBSCRIPTION ID} -n {VAULT NAME}
    ```

* Habilitación de la protección de purga en el almacén de claves

    ```azurecli
    az keyvault update --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME} --enable-purge-protection true
    ```

## <a name="certificates-cli"></a>Certificados (CLI)

* Conceder acceso para purgar y recuperar certificados

    ```azurecli
    az keyvault set-policy --upn user@contoso.com --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}  --certificate-permissions recover purge
    ```

* Eliminar certificado

    ```azurecli
    az keyvault certificate delete --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {CERTIFICATE NAME}
    ```

* Lista de certificados eliminados

    ```azurecli
    az keyvault certificate list-deleted --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME}
    ```

* Recuperar un certificado eliminado

    ```azurecli
    az keyvault certificate recover --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {CERTIFICATE NAME}
    ```

* Purgar los certificados eliminados temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE EL CERTIFICADO)**

    ```azurecli
    az keyvault certificate purge --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {CERTIFICATE NAME}
    ```

## <a name="keys-cli"></a>Claves (CLI)

* Conceder acceso para purgar y recuperar claves

    ```azurecli
    az keyvault set-policy --upn user@contoso.com --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}  --key-permissions recover purge
    ```

* Tecla Eliminar

    ```azurecli
    az keyvault key delete --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {KEY NAME}
    ```

* Enumeración de claves eliminadas

    ```azurecli
    az keyvault key list-deleted --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME}
    ```

* Recuperar la clave eliminada

    ```azurecli
    az keyvault key recover --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {KEY NAME}
    ```

* Purgar las claves eliminadas temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE LA CLAVE)**

    ```azurecli
    az keyvault key purge --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {KEY NAME}
    ```

## <a name="secrets-cli"></a>Secretos (CLI)

* Conceder acceso para purgar y recuperar secretos

    ```azurecli
    az keyvault set-policy --upn user@contoso.com --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}  --secret-permissions recover purge
    ```

* Eliminar un secreto

    ```azurecli
    az keyvault secret delete --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {SECRET NAME}
    ```

* Lista de secretos eliminados

    ```azurecli
    az keyvault secret list-deleted --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME}
    ```

* Recuperar un secreto eliminado

    ```azurecli
    az keyvault secret recover --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {SECRET NAME}
    ```

* Purgar un secreto eliminado temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE EL SECRETO)**

    ```azurecli
    az keyvault secret purge --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {SECRET NAME}
    ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

## <a name="key-vault-powershell"></a>Key Vault (PowerShell)

* Comprobar si un almacén de claves tiene habilitada la eliminación temporal

    ```powershell
    Get-AzKeyVault -VaultName "ContosoVault"
    ```

* Habilitar la eliminación temporal en el almacén de claves

    ```powershell
    ($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

    Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
    ```

* Eliminar el almacén de claves

    ```powershell
    Remove-AzKeyVault -VaultName 'ContosoVault'
    ```

* Lista de todos los almacenes de claves eliminados temporalmente

    ```powershell
    Get-AzKeyVault -InRemovedState
    ```

* Recuperación de un almacén de claves eliminado temporalmente

    ```powershell
    Undo-AzKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
    ```

* Purgar un almacén de claves eliminado temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE EL ALMACÉN DE CLAVES).**

    ```powershell
    Remove-AzKeyVault -VaultName ContosoVault -InRemovedState -Location westus
    ```

* Habilitación de la protección de purga en el almacén de claves

    ```powershell
    ($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enablePurgeProtection" -Value "true"

    Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
    ```

## <a name="certificates-powershell"></a>Certificados (PowerShell)

* Conceder permisos para recuperar y purgar certificados

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToCertificates recover,purge
    ```

* Eliminar un certificado

  ```powershell
  Remove-AzKeyVaultCertificate -VaultName ContosoVault -Name 'MyCert'
  ```

* Lista de todos los certificados eliminados de un almacén de claves

  ```powershell
  Get-AzKeyVaultCertificate -VaultName ContosoVault -InRemovedState
  ```

* Recuperar un certificado en estado eliminado

  ```powershell
  Undo-AzKeyVaultCertificateRemoval -VaultName ContosoVault -Name 'MyCert'
  ```

* Purgar un certificado eliminado temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE EL CERTIFICADO)**

  ```powershell
  Remove-AzKeyVaultcertificate -VaultName ContosoVault -Name 'MyCert' -InRemovedState
  ```

## <a name="keys-powershell"></a>Claves (PowerShell)

* Conceder permisos para recuperar y purgar claves

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys recover,purge
    ```

* Eliminar una clave

  ```powershell
  Remove-AzKeyVaultKey -VaultName ContosoVault -Name 'MyKey'
  ```

* Lista de todos los certificados eliminados de un almacén de claves

  ```powershell
  Get-AzKeyVaultKey -VaultName ContosoVault -InRemovedState
  ```

* Recuperar una clave eliminada temporalmente

    ```powershell
    Undo-AzKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
    ```

* Purgar una clave eliminadas temporalmente **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE LA CLAVE)**

    ```powershell
    Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
    ```

## <a name="secrets-powershell"></a>Secretos (PowerShell)

* Conceder permisos para recuperar y purgar secretos

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToSecrets recover,purge
    ```

* Eliminar un secreto denominado SQLPassword

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -name SQLPassword
  ```

* Lista de todos los secretos eliminados de un almacén de claves

  ```powershell
  Get-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState
  ```

* Recuperar un secreto con estado eliminado

  ```powershell
  Undo-AzKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
  ```

* Purgar un secreto con estado eliminado **(¡ADVERTENCIA! ESTA OPERACIÓN ELIMINARÁ PERMANENTEMENTE LA CLAVE)**

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
  ```
