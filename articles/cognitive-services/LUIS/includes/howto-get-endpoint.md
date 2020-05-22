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
ms.openlocfilehash: 2a98e2a97a9154d9e256a4662bb292896c6d1e77
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588805"
---
En la sección **Administrar** (menú de la parte superior derecha), en la página **Recursos de Azure** (menú izquierdo), copie la dirección URL de la **Consulta de ejemplo** y péguela en una nueva pestaña del explorador.

La dirección URL del punto de conexión tiene un formato similar al siguiente; aunque su propio subdominio personalizado, id. de la aplicación y clave del punto de conexión reemplazarán a APP-ID y KEY-ID:

```console
https://YOUR-CUSTOM-SUBDMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```