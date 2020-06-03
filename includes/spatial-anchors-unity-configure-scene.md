---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/2/2019
ms.author: crtreasu
ms.openlocfilehash: 7e7825e8247e78cbc0c0e9e22bdbd9326939e0a8
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83998091"
---
El paso siguiente es configurar la aplicación para usar el identificador de cuenta y la clave de cuenta. Los copió en un editor cuando [configuró el recurso de Spatial Anchors](#create-a-spatial-anchors-resource).

En el panel del **proyecto**, vaya a `Assets\AzureSpatialAnchors.SDK\Resources`. Seleccione `SpatialAnchorConfig`. A continuación, en el panel **Inspector**, escriba `Account Key` como valor de `Spatial Anchors Account Key` y `Account ID` como valor de `Spatial Anchors Account Id`.

A continuación, abra `SpatialAnchorManager.cs`. Busque `CreateSessionAsync()` y agregue la siguiente línea, en la que sustituye el dominio de cuenta por el anterior: `session.Configuration.AccountDomain = "MyAccountDomain";`. Puede agregar esta línea directamente antes de este comentario `// Configure authentication`.
