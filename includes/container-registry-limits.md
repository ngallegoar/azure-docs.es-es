---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 03/11/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0090f02382e024e5539383328b55d58798002d63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79117112"
---
| Resource | Básica | Estándar | Premium |
|---|---|---|---|
| Almacenamiento<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Tamaño máximo de la capa de imagen | 200 GiB | 200 GiB | 200 GiB |
| Operaciones de lectura por minuto<sup>2, 3</sup> | 1,000 | 3000 | 10 000 |
| Operaciones de escritura por minuto<sup>2, 4</sup> | 100 | 500 | 2\.000 |
| Ancho de banda de descarga en MBps<sup>2</sup> | 30 | 60 | 100 |
| Ancho de banda de carga en MBps<sup>2</sup> | 10 | 20 | 50 |
| webhooks | 2 | 10 | 500 |
| Replicación geográfica | N/D | N/D | [Compatible][geo-replication] |
| Confianza de contenido | N/D | N/D | [Compatible][content-trust] |
| Acceso de redes virtuales | N/D | N/D | [Versión preliminar][vnet] |
| Integración de Private Link | N/D | N/D | [Versión preliminar][plink] |
| Claves administradas por el cliente | N/D | N/D | [Versión preliminar][cmk] |
| Permisos de ámbito de repositorio | N/D | N/D | [Versión preliminar][token]|
| &bull; Tokens | N/D | N/D | 20.000 |
| &bull; Asignaciones de ámbito | N/D | N/D | 20.000 |
| &bull; Repositorios por asignación de ámbito | N/D | N/D | 500 |


<sup>1</sup> Los límites de almacenamiento especificados son la cantidad de almacenamiento *incluido* para cada nivel. Se le cobrará una tarifa diaria adicional por GiB de almacenamiento de imágenes por encima de estos límites. Para más información, consulte [Precios de Azure Container Registry][pricing].

<sup>2</sup>*ReadOps*, *WriteOps* y *ancho de banda* son estimaciones mínimas. Azure Container Registry se esfuerza por mejorar el rendimiento adaptado a su uso.

<sup>3</sup>[docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) se traduce en varias operaciones de lectura en función del número de capas de la imagen, además de la recuperación del manifiesto.

<sup>4</sup>[docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) se traduce en varias operaciones de escritura, en función del número de capas que se deben insertar. Un elemento `docker push` incluye *operaciones de lectura* para recuperar un manifiesto para una imagen existente.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md