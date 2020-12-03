---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.openlocfilehash: e7b7fae094ad15bc1732778b6a4a3259fb4dd3b5
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025889"
---
| Resource | Recursos compartidos de archivos estándar\* | Recursos compartidos de archivos Prémium |
|----------|---------------|------------------------------------------|
| Tamaño máximo de un recurso compartido de archivos | Sin mínimo; pago por uso | 100 GiB; aprovisionado |
| Tamaño máximo de un recurso compartido de archivos | 100 TiB\*\*, 5 TiB | 100 TiB |
| Tamaño máximo de un archivo en un recurso compartido de archivos | 1 TiB | 4 TiB |
| Número máximo de archivos en un recurso compartido de archivos | Sin límite | Sin límite |
| Número máximo de IOPS por recurso compartido | 10 000 IOPS\*\*, 1000 IOPS o 100 solicitudes en 100 ms | 100 000 IOPS |
| Número máximo de directivas de acceso almacenadas por recurso compartido de archivos | 5 | 5 |
| Rendimiento de destino para un único recurso compartido de archivos | Hasta 300 MiB/s\*\*, hasta 60 MiB/s,  | Consulte los valores de entrada y salida del recurso compartido de archivos Premium|
| Salida máxima para un único recurso compartido de archivos | Consulte el rendimiento de destino del recurso compartido de archivos estándar. | Hasta 6204 MiB/s |
| Entrada máxima para un único recurso compartido de archivos | Consulte el rendimiento de destino del recurso compartido de archivos estándar. | Hasta 4136 MiB/s |
| Número máximo de identificadores abiertos por archivo o directorio | 2\.000 identificadores abiertos | 2\.000 identificadores abiertos |
| Número máximo de instantáneas de recurso compartido | 200 instantáneas de recurso compartido | 200 instantáneas de recurso compartido |
| Longitud máxima del nombre de objeto (archivos y directorios) | 2048 caracteres | 2048 caracteres |
| Número máximo de componentes de la ruta de acceso (en la ruta de acceso \A\B\C\D, cada letra es un componente) | 255 caracteres | 255 caracteres |
| Límite de vínculo físico (solo NFS) | N/D | 178 |
| Número máximo de canales de SMB multicanal | N/D | 4 |

\* Los límites de los recursos compartidos de archivos estándar se aplican a los tres niveles disponibles para dichos recursos: optimizado para transacciones, acceso frecuente y acceso esporádico.

\*\* El valor predeterminado en los recursos compartidos de archivos estándar es 5 TiB. En el artículo [Habilitación y creación de recursos compartidos de archivos grandes](../articles/storage/files/storage-files-how-to-create-large-file-share.md) encontrará más información sobre cómo aumentar en escala los recursos compartidos de archivos estándar hasta 100 TiB.
