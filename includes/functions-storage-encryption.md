---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 0bc5977a581006dc760c0435f9d68371ced7e4cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83648786"
---
Azure Storage cifra todos los datos de las cuentas de almacenamiento en reposo. Para más información, consulte [Cifrado de Azure Storage para datos en reposo](../articles/storage/common/storage-service-encryption.md).

De manera predeterminada, los datos se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar claves administradas por el cliente que puede usar para el cifrado de datos de archivos y blobs. Estas claves deben estar presentes en Azure Key Vault para que Azure Functions pueda acceder a la cuenta de almacenamiento. Para más información, consulte [Cifrado en reposo con claves administradas por el cliente](../articles/azure-functions/configure-encrypt-at-rest-using-cmk.md).  