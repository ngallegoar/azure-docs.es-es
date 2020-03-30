---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/07/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: feed8b21833d4244d027d64d5e6547b94e4fa66f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75945138"
---
## <a name="what-happens-to-my-app-during-deployment"></a>¿Qué ocurre con mi aplicación durante la implementación?

Todos los métodos de implementación admitidos oficialmente realizan cambios en los archivos de la carpeta `/home/site/wwwroot` de la aplicación. Estos archivos se usan para ejecutar la aplicación. Por lo tanto, se puede producir un error en la implementación debido a archivos bloqueados. Es posible que la aplicación también se comporte de forma impredecible durante la implementación, ya que no todos los archivos se actualizan al mismo tiempo. Esto no es deseable para una aplicación orientada al cliente. Hay varias formas de evitar estos problemas:

- [Ejecute la aplicación directamente desde el paquete ZIP](../articles/app-service/deploy-run-package.md) sin descomprimirla.
- Detenga la aplicación o habilite el modo sin conexión para la aplicación durante la implementación. Para más información, consulte [Deal with locked files during deployment](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment) (Tratamiento de archivos bloqueados durante la implementación).
- Realice la implementación en un [espacio de ensayo](../articles/app-service/deploy-staging-slots.md) con [intercambio automático](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) habilitado. 
