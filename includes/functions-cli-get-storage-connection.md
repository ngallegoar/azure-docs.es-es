---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 5cb345ef2d20f75066e90f9e6478be27f925b1b0
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673359"
---
### <a name="retrieve-the-azure-storage-connection-string"></a>Recuperación de la cadena de conexión de Azure Storage

Anteriormente, creó una cuenta de Azure Storage para que la aplicación de funciones la utilizara. La cadena de conexión de esta cuenta se almacena de forma segura en la configuración de la aplicación en Azure. Mediante la descarga de la configuración en el archivo *local.settings.json*, puede usar esa conexión para escribir en una cola de Storage de la misma cuenta cuando ejecute la función de forma local. 

1. En la raíz del proyecto, ejecute el comando siguiente, reemplazando `<app_name>` por el nombre de la aplicación de funciones del inicio rápido anterior. Este comando sobrescribirá los valores existentes en el archivo.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. Abra *local.settings.json* y busque el valor denominado `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Usará el nombre `AzureWebJobsStorage` y la cadena de conexión en otras secciones de este artículo.

> [!IMPORTANT]
> Como *local.settings.json* contiene secretos descargados de Azure, excluya siempre este archivo del control de código fuente. El archivo *.gitignore* que se creó con un proyecto de Functions local excluye el archivo de forma predeterminada.