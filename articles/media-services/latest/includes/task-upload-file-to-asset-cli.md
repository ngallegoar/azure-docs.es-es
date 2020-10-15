---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: affd55d65ed8454ebfcdf14f697849badf8e5d3c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "88657143"
---
<!-- ### Upload files with the CLI -->

El comando siguiente carga un único archivo.  

Cambie los valores siguientes:

1. Cambie `/path/to/file` por la ruta de acceso local del archivo.  
1. Cambie `MyContainer` por un recurso creado anteriormente con el identificador de recurso (no el nombre).
1. Cambie `MyBlob` por el nombre que desea usar para el archivo cargado.
1. Cambie `MyStorageAccountName` por el nombre de la cuenta de almacenamiento que usa.
1. Cambie `MyStorageAccountKey` por la clave de acceso de la cuenta de almacenamiento.

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob --acount-name MyStorageAccountName --account-key MyStorageAccountKey
    ```
