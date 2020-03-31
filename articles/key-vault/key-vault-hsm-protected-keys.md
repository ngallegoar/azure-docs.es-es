---
title: 'Generación y transferencia de claves protegidas con HSM para Azure Key Vault: Azure Key Vault | Microsoft Docs'
description: Use este artículo para ayudarle a planear, generar y, a continuación, transfiera sus propias claves protegidas con HSM para utilizar con Azure Key Vault. También se denomina BYOK o "traiga su propia clave".
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 048e5072c592cf2de32e533014c99034572a1c47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082904"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>Importación de claves protegidas con HSM en Key Vault

Para obtener una mayor seguridad, cuando utilice Azure Key Vault, puede importar o generar claves en módulos de seguridad de hardware (HSM) que no se salen nunca del límite de los HSM. Con frecuencia este escenario se conoce también como *Aportar tu propia clave*, o BYOK. En Azure Key Vault se usa la familia nCipher nShield de HSM (validado con FIPS 140-2 de nivel 2) para proteger las claves.

Esta funcionalidad no está disponible para Azure China 21Vianet.

> [!NOTE]
> Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](key-vault-overview.md)  
> Para ver un tutorial introductorio que incluya la creación de un almacén de claves para claves protegidas con HSM, consulte [¿Qué es Azure Key Vault?](key-vault-overview.md).

## <a name="supported-hsms"></a>HSM compatibles

La transferencia de claves protegidas con HSM a Key Vault se admite a través de dos métodos diferentes en función de los HSM que se usen. Use la tabla siguiente para determinar qué método debe usar para generar los HSM y transferir sus propias claves protegidas con HSM para usarlas con Azure Key Vault. 

|Nombre del proveedor|Tipo de proveedor|Modelos de HSM compatibles|Método de transferencia de clave HSM compatible|
|---|---|---|---|
|nCipher|Fabricante|<ul><li>Familia nShield de HSM</li></ul>|[Use el método BYOK heredado](hsm-protected-keys-legacy.md)|
|Thales|Fabricante|<ul><li>Familia SafeNet Luna HSM 7 con la versión del firmware 7.3 o posterior</li></ul>| [Use el nuevo método BYOK (versión preliminar)](hsm-protected-keys-vendor-agnostic-byok.md)|
|Fortanix|HSM como servicio|<ul><li>Self-Defending Key Management Service (SDKMS)</li></ul>|[Use el nuevo método BYOK (versión preliminar)](hsm-protected-keys-vendor-agnostic-byok.md)|










## <a name="next-steps"></a>Pasos siguientes

Siga [Procedimientos recomendados de Key Vault](key-vault-best-practices.md) para garantizar la seguridad, la durabilidad y la supervisión de las claves.
