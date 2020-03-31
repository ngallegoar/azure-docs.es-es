---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: 694501fdaaaa92e898f4973838d86343e29144e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74895294"
---
Azure Storage cifra todos los datos de las cuentas de almacenamiento en reposo. De manera predeterminada, los datos se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar claves administradas por el cliente que puede usar para el cifrado de datos de archivos y blobs.

Las claves administradas por el cliente se deben almacenar en Azure Key Vault. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. Para más información sobre cifrado de Azure Storage y la administración de claves, consulte [Cifrado de Azure Storage para datos en reposo](../articles/storage/common/storage-service-encryption.md). Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../articles/key-vault/key-vault-overview.md)
