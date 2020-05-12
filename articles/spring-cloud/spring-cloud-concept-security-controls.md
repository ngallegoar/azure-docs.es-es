---
title: 'Concepto: controles de seguridad para el servicio Azure Spring Cloud'
description: Use los controles de seguridad integrados en el servicio Azure Spring Cloud.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/23/2020
ms.openlocfilehash: 5b459ef57d0e8a22ce1cd53f56c44d31e53c7c93
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594991"
---
# <a name="security-controls-for-azure-spring-cloud-service"></a>Controles de seguridad para el servicio Azure Spring Cloud
Los controles de seguridad están integrados en el servicio Azure Spring Cloud.

Un control de seguridad es una cualidad o característica de un servicio de Azure que ayuda a dicho servicio a prevenir y detectar vulnerabilidades de seguridad, así como a responder a ellas.  En cada control, utilizamos *Sí* o *No* para indicar si actualmente está disponible para el servicio.  Usamos *N/D* para un control que no se pueda aplicar al servicio. 

**Controles de seguridad para la protección de datos**

| Control de seguridad | Sí/No | Notas | Documentación |
|:-------------|:-------|:-------------------------------|:----------------------|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | El origen y los artefactos cargados por el usuario, los valores del servidor de configuración, la configuración de la aplicación y los datos en el almacenamiento persistente se almacenan en Azure Storage, que cifra automáticamente el contenido en reposo.<br><br>La memoria caché del servidor de configuración, los archivos binarios en tiempo de ejecución compilados a partir del código fuente cargado y los registros de aplicación durante la vigencia de la aplicación se guardan en un disco administrado de Azure que cifra automáticamente el contenido en reposo.<br><br>Las imágenes de contenedor compiladas a partir del código fuente cargado por el usuario se guardan en Azure Container Registry, que cifra automáticamente el contenido de la imagen en reposo. | [Cifrado de Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)<br><br>[Cifrado del lado servidor de Azure Managed Disks](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption)<br><br>[Almacenamiento de imágenes en Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-storage) |
| Cifrado en tránsito | Sí | Los puntos de conexión públicos de la aplicación de usuario usan HTTPS para el tráfico de entrada de forma predeterminada. |  |
| Llamadas a API cifradas | Sí | Las llamadas de administración para configurar el servicio Azure Spring Cloud se producen mediante llamadas de Azure Resource Manager sobre HTTPS. | [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/) |

