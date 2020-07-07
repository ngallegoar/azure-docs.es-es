---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.date: 05/06/2020
ms.subservice: language-understanding
ms.topic: include
ms.author: diberry
ms.openlocfilehash: a6e6c89c34723fb9e11b0c7e4ab8c9bedb8aa9ca
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959058"
---
En la sección **Administrar** (menú de la parte superior derecha), en la página **Recursos de Azure** (menú izquierdo), copie la dirección URL de la **Consulta de ejemplo** y péguela en una nueva pestaña del explorador.

La dirección URL del punto de conexión tiene un formato similar al siguiente; aunque su propio subdominio personalizado, id. de la aplicación y clave del punto de conexión reemplazarán a APP-ID y KEY-ID:

```console
https://YOUR-CUSTOM-SUBDOMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```