---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: d8ee7b61897a9718d7078fc16eddc177fefbff43
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89147286"
---
 Los clústeres de proceso de Azure Machine Learning también admiten [identidades administradas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) para autenticar el acceso a los recursos de Azure sin incluir credenciales en el código. Hay dos tipos de identidades administradas:

* Una **identidad administrada asignada por el sistema** se habilita directamente en el clúster de proceso de Azure Machine Learning. El ciclo de vida de una identidad asignada por el sistema está vinculado directamente al clúster de proceso. Si se elimina el clúster de proceso, Azure limpia automáticamente las credenciales y la identidad en Azure AD.
* Una **identidad administrada asignada por el usuario** es un recurso de Azure independiente que se proporciona a través del servicio de identidad administrada de Azure. Puede asignar una identidad administrada asignada por el usuario a varios recursos y se conserva durante tanto tiempo como necesite.