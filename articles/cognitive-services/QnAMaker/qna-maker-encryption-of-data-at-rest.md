---
title: Cifrado de datos en reposo de QnA Maker
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo de QnA Maker
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: bd64321a6a7afbac61a63365c77a75120f837e83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371989"
---
# <a name="qna-maker-encryption-of-data-at-rest"></a>Cifrado de datos en reposo de QnA Maker

QnA Maker cifra automáticamente sus datos cuando se guardan en la nube, lo que ayuda a cumplir los objetivos de seguridad y cumplimiento normativo de la organización.

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

De forma predeterminada, su suscripción usa claves de cifrado administradas por Microsoft. También tiene una opción para administrar su suscripción con sus propias claves. Las claves administradas por el cliente (CMK) ofrecen más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. También permite auditar las claves de cifrado que se usan para proteger los datos.

QnA Maker usa la compatibilidad con CMK de Azure Search. Debe crear [CMK en Azure Search mediante Azure Key Vault](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys). Esta instancia de Azure debe estar asociada con el servicio QnA Maker para que esté habilitada para CMK.

> [!IMPORTANT]
> El recurso de servicio Azure Search debe haberse creado después de enero de 2019 de enero y no puede estar en el nivel gratis (compartido). No se admite la configuración de claves administradas por el cliente en Azure Portal.

## <a name="enable-customer-managed-keys"></a>Habilitar claves administradas del cliente

El servicio QnA Maker usa CMK del servicio Azure Search. Siga estos pasos para habilitar CMK:

1. Cree una nueva instancia de Azure Search y habilite los requisitos previos que se mencionan en los [requisitos previos de la clave administrada por el cliente de Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys#prerequisites).

   ![Visualización de la configuración de cifrado](../media/cognitive-services-encryption/qna-encryption-1.png)

2. Cuando se crea un recurso de QnA Maker, se asocia automáticamente con una instancia de Azure Search. Esto no se puede utilizar con CMK. Para usar CMK, debe asociar la instancia recién creada de Azure Search que se creó en el paso 1. En concreto, deberá actualizar `AzureSearchAdminKey` y `AzureSearchName` en el recurso de QnA Maker.

   ![Visualización de la configuración de cifrado](../media/cognitive-services-encryption/qna-encryption-2.png)

3. A continuación, cree una nueva configuración de la aplicación:
   * **Name**: Establézcalo en `CustomerManagedEncryptionKeyUrl`
   * **Valor**: Este es el valor que ha recibido en el paso 1 al crear la instancia de Azure Search.

   ![Visualización de la configuración de cifrado](../media/cognitive-services-encryption/qna-encryption-3.png)

4. Cuando termine, reinicie el entorno de ejecución. Ahora el servicio QnA Maker está habilitado para CMK.

## <a name="regional-availability"></a>Disponibilidad regional

Las claves administradas por el cliente están disponibles en todas las regiones de Azure Search.

## <a name="next-steps"></a>Pasos siguientes

* [Cifrado en Azure Search mediante CMK en Azure Key Vault](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)
* [Cifrado de datos en reposo](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
