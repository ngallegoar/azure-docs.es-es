---
title: Obtención de una clave de firma de una directiva mediante .NET de Azure Media Services v3
description: En este tema se muestra cómo obtener una clave de firma de la directiva existente mediante el SDK de .NET de Media Services v3.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 7107a2acdc0ca430797245125ca903f7414908da
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92427071"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Obtención de una clave de firma de la directiva existente

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. Las API de la versión v3 no devuelven secretos o credenciales en las operaciones **Get** o **List**. Consulte la explicación detallada aquí: Para más información, consulte [Control de acceso basado en roles (RBAC) para cuentas de Media Services](rbac-overview.md).

En el ejemplo de este artículo se muestra cómo usar .NET para obtener una clave de firma a partir de la directiva existente. 
 
## <a name="download"></a>Descargar 

Clone un repositorio de GitHub que contenga el ejemplo de .NET completo en la máquina mediante el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
El ejemplo de ContentKeyPolicy con secretos se encuentra en la carpeta [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM).

## <a name="get-contentkeypolicy-with-secrets"></a>Obtención de ContentKeyPolicy con secretos 

Para obtener la clave, use **GetPolicyPropertiesWithSecretsAsync** , tal y como se muestra en el ejemplo siguiente.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Pasos siguientes

[Diseño del sistema de protección de contenido con DRM múltiple con control de acceso](design-multi-drm-system-with-access-control.md) 
