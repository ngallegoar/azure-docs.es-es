---
title: Exportación de un certificado en Azure Key Vault
description: Exportación de un certificado en Azure Key Vault
services: key-vault
author: sebansal
tags: azure-key-vault
ms.service: key-vault
ms.subservice: certificates
ms.topic: how-to
ms.custom: mvc, devx-track-azurecli
ms.date: 08/11/2020
ms.author: sebansal
ms.openlocfilehash: afab65b22d9487f30da458346bf143a557bec0d8
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2020
ms.locfileid: "88588899"
---
# <a name="export-certificate-from-azure-key-vault"></a>Exportación de un certificado en Azure Key Vault

Azure Key Vault permite aprovisionar, administrar e implementar fácilmente certificados digitales para una red y habilitar las comunicaciones seguras para las aplicaciones. Para más información general acerca de los certificados, consulte [Certificados de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates).

## <a name="about-azure-key-vault-certificate"></a>Acerca del certificado de Azure Key Vault

### <a name="composition-of-certificate"></a>Composición de un certificado
Cuando se crea un certificado de Key Vault, se crean también una clave direccionable y un secreto con el mismo nombre. La clave de Key Vault permite las operaciones de clave y el secreto de Key Vault permite la recuperación del valor del certificado como un secreto. Un certificado de Key Vault también contiene metadatos del certificado X.509 público. [Más información](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#composition-of-a-certificate)

### <a name="exportable-or-non-exportable-keys"></a>Claves exportables o no exportables
Cuando se crea un certificado de Key Vault, este se puede recuperar desde el secreto direccionable con la clave privada en formato PFX o PEM. La directiva utilizada para crear el certificado debe indicar que la clave es exportable. Si la directiva indica que no es exportable, la clave privada no forma parte del valor cuando se recupera como un secreto.

Se admiten dos tipos de clave con los certificados: RSA o RSA HSM. Los exportables solo se permiten con RSA, no se admiten en RSA HSM. [Más información](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#exportable-or-non-exportable-key)

El certificado almacenado en Azure Key Vault se puede exportar mediante la CLI de Azure, PowerShell o el portal.

> [!NOTE]
> Es importante tener en cuenta que solo se requeriría la contraseña de un certificado al importarlo en el almacén de claves. El almacén de claves no guardaría la contraseña asociada, por lo que, cuando exportara el certificado, la contraseña quedaría en blanco.

## <a name="exporting-certificate-using-cli"></a>Exportación del certificado mediante la CLI
El siguiente comando le permitirá descargar la **parte pública** de un certificado de Key Vault.

```azurecli
az keyvault certificate download --file
                                 [--encoding {DER, PEM}]
                                 [--id]
                                 [--name]
                                 [--subscription]
                                 [--vault-name]
                                 [--version]
```
Para obtener ejemplos y definiciones de parámetros, [consulte aquí](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-download)



Si desea descargar el certificado completo, es decir, la **parte pública y la parte privada de su composición**, puede descargar el certificado como un secreto.

```azurecli
az keyvault secret download –file {nameofcert.pfx}
                            [--encoding {ascii, base64, hex, utf-16be, utf-16le, utf-8}]
                            [--id]
                            [--name]
                            [--subscription]
                            [--vault-name]
                            [--version]
```
Para las definiciones de parámetros, [consulte aquí](https://docs.microsoft.com/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-download).


## <a name="exporting-certificate-using-powershell"></a>Exportación del certificado mediante PowerShell

Este comando obtiene el certificado denominado TestCert01 del almacén de claves denominado ContosoKV01. Para descargar el certificado como archivo PFX, ejecute el siguiente comando. Estos comandos acceden a SecretId y, a continuación, guardan el contenido como un archivo PFX.

```azurepowershell
$cert = Get-AzKeyVaultCertificate -VaultName "ContosoKV01" -Name "TestCert01"
$kvSecret = Get-AzKeyVaultSecret -VaultName "ContosoKV01" -Name $Cert.Name
$kvSecretBytes = [System.Convert]::FromBase64String($kvSecret.SecretValueText)
$certCollection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection
$certCollection.Import($kvSecretBytes,$null,[System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable)
$password = '******'
$protectedCertificateBytes = $certCollection.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $password)
$pfxPath = [Environment]::GetFolderPath("Desktop") + "\MyCert.pfx"
[System.IO.File]::WriteAllBytes($pfxPath, $protectedCertificateBytes)
```
Se exportará toda la cadena de certificados con clave privada y este certificado estará protegido mediante contraseña.
Para más información sobre el comando ```Get-AzKeyVaultCertificate``` y los parámetros, consulte [Ejemplo 2](https://docs.microsoft.com/powershell/module/az.keyvault/Get-AzKeyVaultCertificate?view=azps-4.4.0).

## <a name="exporting-certificate-using-portal"></a>Exportación de certificados mediante el portal

En el portal, cuando cree o importe un certificado en la hoja Certificado, recibirá la notificación de que se ha creado correctamente. Al seleccionar el certificado, puede hacer clic en la versión actual y verá la opción de descargar.


Al hacer clic en el botón "Descargar en formato CER" o "Descargar en formato PFX/PEM", puede descargar el certificado.


![Descarga del certificado](../media/certificates/quick-create-portal/current-version-shown.png)


## <a name="exporting-app-service-certificate-from-key-vault"></a>Exportación de App Service Certificate desde el almacén de claves

Los certificados de Azure App Service proporcionan una manera cómoda de comprar certificados SSL y asignarlos a aplicaciones de Azure directamente desde el portal. Estos certificados también se pueden exportar desde el portal como archivos PFX para usarse en otro lugar.
Una vez importados, los certificados de App Service pueden estar **ubicados en secretos**.

Si desea conocer los pasos para exportar los certificados de App Service, [consulte aquí](https://social.technet.microsoft.com/wiki/contents/articles/37431.exporting-azure-app-service-certificates.aspx).

## <a name="read-more"></a>Más información
* [Diversos tipos y definiciones de archivos de certificado](https://docs.microsoft.com/archive/blogs/kaushal/various-ssltls-certificate-file-typesextensions)