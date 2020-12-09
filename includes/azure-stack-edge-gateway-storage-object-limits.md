---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 10d6ef2c90390f08e38726363416493f937de8f2
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465858"
---
Estos son los tamaños de los objetos de Azure que se pueden escribir. Asegúrese de que todos los archivos que se cargan se ajustan a estos límites.

| Tipo de objeto de Azure | Límite de carga                                             |
|-------------------|-----------------------------------------------------------|
| Blob en bloques        | 4,75 TB                                                 |
| Blob en páginas         | 1 TB <br> Todos los archivos cargados en formato de blob en páginas deben tener 512 bytes alineados (un múltiplo entero); de lo contrario, se produce un error en la carga. <br> VHD y VHDX tienen 512 bytes alineados. |
| Azure Files         | 1 TB <br> Todos los archivos cargados en formato de blob en páginas deben tener 512 bytes alineados (un múltiplo entero); de lo contrario, se produce un error en la carga. <br> VHD y VHDX tienen 512 bytes alineados. |

> [!IMPORTANT]
> Se permite la creación de archivos (independientemente del tipo de almacenamiento) de hasta 5 TB. Sin embargo, si crea un archivo cuyo tamaño sea mayor que el límite de carga definido en la tabla anterior, no se cargará el archivo. Tendrá que eliminar manualmente el archivo para recuperar el espacio.
