---
title: Configuración de las preferencias de enrutamiento de red (versión preliminar)
titleSuffix: Azure Storage
description: Configure las preferencias de enrutamiento de red (versión preliminar) de la cuenta de Azure Storage para especificar cómo se enruta el tráfico de red a la cuenta desde los clientes a través de Internet.
services: storage
author: santoshc
ms.service: storage
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: santoshc
ms.reviewer: tamram
ms.subservice: common
ms.openlocfilehash: 5b4a1b1f27dff059090d78e24a6a0eca0bbbf01f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514193"
---
# <a name="configure-network-routing-preference-for-azure-storage-preview"></a>Configuración de las preferencias de enrutamiento de red para Azure Storage (versión preliminar)

Puede configurar las [preferencias de enrutamiento](../../virtual-network/routing-preference-overview.md) de red (versión preliminar) de la cuenta de Azure Storage para especificar cómo se enruta el tráfico de red a la cuenta desde los clientes a través de Internet. De forma predeterminada, el tráfico procedente de Internet se enruta al punto de conexión público de la cuenta de almacenamiento a través de la [red global de Microsoft](../../networking/microsoft-global-network.md). Azure Storage proporciona opciones adicionales para configurar la forma de enrutar el tráfico a la cuenta de almacenamiento.

La configuración de las preferencias de enrutamiento le ofrece la flexibilidad de optimizar el tráfico en función del rendimiento de la red Premium o del costo. Cuando configure una preferencia de enrutamiento, debe especificar cómo se dirigirá el tráfico al punto de conexión público de la cuenta de almacenamiento de forma predeterminada. También puede publicar puntos de conexión específicos de la ruta para la cuenta de almacenamiento.

## <a name="microsoft-global-network-versus-internet-routing"></a>Red global de Microsoft frente al enrutamiento de Internet

De forma predeterminada, los clientes que están fuera del entorno de Azure tienen acceso a la cuenta de almacenamiento a través de la red global de Microsoft. La red global de Microsoft está optimizada para la selección de rutas de acceso de baja latencia, que ofrecen un rendimiento de red Premium con alta confiabilidad. Tanto el tráfico entrante como el saliente se enrutan a través del punto de presencia (POP) más cercano al cliente. Esta configuración de enrutamiento predeterminada garantiza que el tráfico hacia y desde la cuenta de almacenamiento atraviese la red global de Microsoft a través de la mayor parte de la ruta de acceso, lo que maximiza el rendimiento de la red.

Puede cambiar la configuración de enrutamiento de la cuenta de almacenamiento para que el tráfico entrante y saliente se enrute hacia y desde clientes que estén fuera del entorno de Azure a través del POP más cercano a la cuenta de almacenamiento. Esta ruta minimiza el recorrido del tráfico a través de la red global de Microsoft y lo lleva al ISP de tránsito lo antes posible. El uso de esta configuración de enrutamiento reduce los costos de red.

En el diagrama siguiente se muestra cómo fluye el tráfico entre el cliente y la cuenta de almacenamiento de cada preferencia de enrutamiento:

![Información general de las opciones de enrutamiento para Azure Storage](media/network-routing-preference/routing-options-diagram.png)

Para obtener más información sobre las preferencias de enrutamiento en Azure, consulte [¿Qué es la preferencia de enrutamiento (versión preliminar)?](../../virtual-network/routing-preference-overview.md).

## <a name="routing-configuration"></a>Configuración de enrutamiento

Puede elegir entre la red global de Microsoft y el enrutamiento de Internet como una preferencia de enrutamiento predeterminada del punto de conexión público de la cuenta de almacenamiento. La preferencia de enrutamiento predeterminada se aplica a todo el tráfico de los clientes fuera de Azure y afecta a los puntos de conexión de Azure Data Lake Storage Gen2, Blob Storage, Azure Files y sitios web estáticos. No se admite la configuración de preferencias de enrutamiento en las colas de Azure o las tablas de Azure.

También puede publicar puntos de conexión específicos de la ruta para la cuenta de almacenamiento. Al publicar puntos de conexión específicos de la ruta, Azure Storage crea nuevos puntos de conexión públicos para la cuenta de almacenamiento que enrutan el tráfico a través de la ruta de acceso deseada. Esta flexibilidad le permite dirigir el tráfico a la cuenta de almacenamiento a través de una ruta específica sin tener que cambiar la preferencia de enrutamiento predeterminada.

Por ejemplo, la publicación de un punto de conexión específico de la ruta de Internet para "StorageAccountA" publicará los siguientes puntos de conexión para la cuenta de almacenamiento:

| Servicio de Storage        | Punto de conexión específico de la ruta                                  |
| :--------------------- | :------------------------------------------------------- |
| Blob service           | `StorageAccountA-internetrouting.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `StorageAccountA-internetrouting.dfs.core.windows.net`   |
| File service           | `StorageAccountA-internetrouting.file.core.windows.net`  |
| Static Websites        | `StorageAccountA-internetrouting.web.core.windows.net`   |

Si tiene un almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) o una cuenta de almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS), la publicación de puntos de conexión específicos de la ruta también crea automáticamente los puntos de conexión correspondientes en la región secundaria del acceso de lectura.

| Servicio de Storage        | Punto de conexión secundario de solo lectura específico de la ruta                        |
| :--------------------- | :----------------------------------------------------------------- |
| Blob service           | `StorageAccountA-internetrouting-secondary.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `StorageAccountA-internetrouting-secondary.dfs.core.windows.net`   |
| File service           | `StorageAccountA-internetrouting-secondary.file.core.windows.net`  |
| Static Websites        | `StorageAccountA-internetrouting-secondary.web.core.windows.net`   |

Las cadenas de conexión de los puntos de conexión específicos de la ruta publicados se pueden copiar a través de [Azure Portal](https://portal.azure.com). Estas cadenas de conexión se pueden usar para la autorización de claves compartidas con todos los SDK y API de Azure Storage existentes.

## <a name="about-the-preview"></a>Acerca de la versión preliminar

La preferencia de enrutamiento de Azure Storage está disponible en las siguientes regiones:

- Sur de Francia
- Centro-Norte de EE. UU
- Centro-Oeste de EE. UU.

Los siguientes problemas conocidos afectan a la versión preliminar de las preferencias de enrutamiento para Azure Storage:

- Las solicitudes de acceso al punto de conexión específico de la ruta correspondiente a la red global de Microsoft producen el error HTTP 404 o uno equivalente. El enrutamiento a través de la red global de Microsoft funciona según lo esperado cuando se establece como preferencia de enrutamiento predeterminada en el punto de conexión público.

## <a name="pricing-and-billing"></a>Precios y facturación

Para obtener información sobre los precios y los detalles de facturación, consulte la sección **Precios** de [¿Qué es la preferencia de enrutamiento (versión preliminar)?](../../virtual-network/routing-preference-overview.md#pricing).

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es la preferencia de enrutamiento (versión preliminar)?](../../virtual-network/routing-preference-overview.md)
- [Configuración de redes virtuales y firewalls de Azure Storage](storage-network-security.md)
- [Recomendaciones de seguridad para Blob Storage](../blobs/security-recommendations.md)
