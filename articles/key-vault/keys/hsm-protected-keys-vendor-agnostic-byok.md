---
title: 'Generación y transferencia de claves protegidas con HSM para Azure Key Vault: Azure Key Vault | Microsoft Docs'
description: Use este artículo para ayudarle a planear, generar y transferir sus propias claves protegidas con HSM para usar con Azure Key Vault. También se denomina BYOK o "Bring Your Own Key".
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 77568928e50fb4398aa786dbbd4a44b9277a8d5b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426980"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>Importación de claves protegidas con HSM en Key Vault (versión preliminar)

> [!NOTE]
> Esta característica se encuentra en versión preliminar y solo está disponible en las regiones de Azure *EUAP de Este de EE. UU. 2* y *EUAP del centro de EE. UU.* . 

Para una mayor seguridad cuando utilice Azure Key Vault, puede importar o generar una clave en un módulo de seguridad de hardware (HSM); la clave no abandona nunca el límite de HSM. Con frecuencia este escenario se conoce como *Bring Your Own Key* (BYOK). En Key Vault se usa la familia de dispositivos HSM nCipher nShield (validados como FIPS 140-2 de nivel 2) para proteger las claves.

Use la información de este artículo como ayuda para planear, generar y transferir sus propias claves protegidas con HSM para utilizarlas con Azure Key Vault.

> [!NOTE]
> Esta funcionalidad no está disponible para Azure China 21Vianet. 
> 
> Este método de importación solo está disponible para [HSM compatibles](#supported-hsms). 

Para más información y para ver un tutorial de introducción al uso de Key Vault (incluida la creación de un almacén de claves para las claves protegidas con HSM), consulte [¿Qué es Azure Key Vault?](../general/overview.md).

## <a name="overview"></a>Información general

A continuación se muestra información general del proceso: Los pasos específicos que se deben realizar se describen más adelante en el artículo.

* En Key Vault, genere una clave (conocida como *clave de intercambio de claves* o [KEK]). La KEK debe ser una clave RSA-HSM que tenga solo la operación de clave `import`. Solo la SKU Premium de Key Vault admite claves RSA-HSM.
* Descargue la clave pública KEK como un archivo .pem.
* Transfiera la clave pública KEK a un equipo sin conexión que esté conectado a un HSM local.
* En la estación de trabajo sin conexión, use la herramienta BYOK que proporciona su proveedor de HSM para crear un archivo BYOK. 
* La clave de destino se cifra con una KEK, que permanece cifrada hasta que se transfiere al HSM de Key Vault. Solo la versión cifrada de la clave sale del HSM local.
* Una KEK generada dentro de un HSM de Key Vault no es exportable. Los HSM aplican la regla de que no existe ninguna versión clara de una KEK fuera de un HSM de Key Vault.
* La KEK debe estar en el mismo almacén de claves al que se va a importar la clave de destino.
* Cuando el archivo BYOK se carga en Key Vault, los HSM de Key Vault usan la clave privada KEK para descifrar el material de la clave de destino e importarlo como una clave de HSM. Esta operación tiene lugar enteramente dentro de un HSM de Key Vault. La clave de destino siempre permanece en el límite de la protección de HSM.

## <a name="prerequisites"></a>Prerrequisitos

En la tabla siguiente se enumeran los requisitos previos para el uso de BYOK en Azure Key Vault:

| Requisito | Más información |
| --- | --- |
| Una suscripción de Azure |Para crear un almacén de claves en Azure Key Vault, se necesita una suscripción a Azure. [Regístrese para una evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/). |
| Una SKU Premium de Key Vault para importar claves protegidas con HSM. |Para más información sobre los niveles de servicio y las funcionalidades de Azure Key Vault, visite la página [Precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/). |
| Un HSM de la lista de HSM admitidos y una herramienta BYOK, junto con las instrucciones que proporciona el proveedor de HSM. | Debe tener permisos para un HSM y conocimientos básicos sobre cómo usarlo. Consulte los [HSM compatibles](#supported-hsms). |
| La CLI de Azure, versión 2.1.0 o posterior | Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).|

## <a name="supported-hsms"></a>HSM compatibles

|Nombre del proveedor|Tipo de proveedor|Modelos de HSM compatibles|Más información|
|---|---|---|---|
|Thales|Fabricante|Familia de HMS 7 SafeNet Luna con la versión del firmware 7.3 o posterior| [Herramienta BYOK y documentación de SafeNet Luna](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|
|Fortanix|HSM como servicio|Self-Defending Key Management Service (SDKMS)|[Exportación de claves de SDKMS a proveedores de nube para BYOK: Azure Key Vault](https://support.fortanix.com/hc/en-us/articles/360040071192-Exporting-SDKMS-keys-to-Cloud-Providers-for-BYOK-Azure-Key-Vault)|


> [!NOTE]
> Para importar claves protegidas con HSM desde la familia de HMS nCipher nShield, use el [procedimiento de BYOK heredado](hsm-protected-keys-legacy.md).

## <a name="supported-key-types"></a>Tipos de clave admitidos

|Nombre de clave|Tipo de clave|Tamaño de clave|Origen|Descripción|
|---|---|---|---|---|
|Clave de intercambio de claves (KEK)|RSA| 2048 bits<br />3072 bits<br />4096 bits|HSM de Azure Key Vault|Un par de claves RSA respaldadas por HSM generado en Azure Key Vault.|
|Clave de destino|RSA|2048 bits<br />3072 bits<br />4096 bits|HSM del proveedor|La clave que se va a transferir al HSM de Azure Key Vault.|

## <a name="generate-and-transfer-your-key-to-the-key-vault-hsm"></a>Generación y transferencia de la clave a un HSM de Key Vault

Para generar y transferir la clave a un HSM de Key Vault:

* [Paso 1: Generación de una KEK](#step-1-generate-a-kek)
* [Paso 2: Descarga de la clave pública KEK](#step-2-download-the-kek-public-key)
* [Paso 3: Generación y preparación de la clave para la transferencia](#step-3-generate-and-prepare-your-key-for-transfer)
* [Paso 4: Transferencia de la clave a Azure Key Vault](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>Paso 1: Generación de una KEK

Una KEK es una clave RSA que se genera en un HSM de Key Vault. La KEK se usa para cifrar la clave que quiere importar (la clave de *destino*).

La KEK debe:
- Ser una clave RSA-HSM (2048 bits; 3072 bits; o 4096 bits)
- Generarse en el mismo almacén de claves al que quiere importar la clave de destino
- Crearse con las operaciones de clave permitidas establecidas en `import`

> [!NOTE]
> La KEK debe tener "import" como la única operación de clave permitida. "import" es mutuamente excluyente con todas las demás operaciones de clave.

Use el comando [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) para crear una KEK que tenga operaciones de clave establecidas en `import`. Registre el identificador de clave (`kid`) que se devuelve con el siguiente comando (usará el valor `kid` en el [paso 3](#step-3-generate-and-prepare-your-key-for-transfer)).

```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-the-kek-public-key"></a>Paso 2: Descarga de la clave pública KEK

Use [az keyvault key download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download) para descargar la clave pública KEK en un archivo .pem. La clave de destino que importa se cifra mediante la clave pública KEK.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

Transfiera el archivo KEKforBYOK.publickey.pem al equipo sin conexión, ya que lo necesitará en el paso siguiente.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>Paso 3: Generación y preparación de la clave para la transferencia

Consulte la documentación del proveedor del HSM para descargar e instalar la herramienta BYOK. Siga las instrucciones del proveedor de HSM para generar una clave de destino y, luego, cree un paquete de transferencia de claves (un archivo BYOK). La herramienta BYOK usará el valor de `kid` del [paso 1](#step-1-generate-a-kek) y el archivo KEKforBYOK.publickey.pem que descargó en el [paso 2](#step-2-download-the-kek-public-key) para generar una clave de destino cifrada en un archivo BYOK.

Transfiera el archivo BYOK al equipo conectado.

> [!NOTE] 
> No se admite la importación de claves RSA de 1024 bits. Actualmente no se admite la importación de una clave de curva elíptica (EC).
> 
> **Problema conocido**: la importación de una clave de destino RSA 4K desde los HSM SafeNet Luna solo se admite con el firmware 7.4.0 o una versión más reciente.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>Paso 4: Transferencia de la clave a Azure Key Vault

Para completar la importación de claves, transfiera el paquete de transferencia de claves (un archivo BYOK) del equipo desconectado al equipo conectado a Internet. Use el comando [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) para cargar el archivo BYOK en el HMS de Key Vault.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Si la carga se realiza correctamente, la CLI de Azure muestra las propiedades de la clave importada.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede usar esta clave protegida con HSM en el almacén de claves. Para más información, vea esta [comparativa de precios y características](https://azure.microsoft.com/pricing/details/key-vault/).



