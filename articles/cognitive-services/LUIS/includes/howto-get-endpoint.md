---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.date: 05/06/2020
ms.subservice: language-understanding
ms.topic: include
ms.openlocfilehash: 6d1ca85c59f03ae0d008342f71597f4d3ca5d97a
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91545043"
---
En la sección **Administrar** (menú de la parte superior derecha), en la página **Recursos de Azure** (menú izquierdo), copie la dirección URL de la **Consulta de ejemplo** y péguela en una nueva pestaña del explorador.

La dirección URL del punto de conexión tiene un formato similar al siguiente; aunque su propio subdominio personalizado, id. de la aplicación y clave del punto de conexión reemplazarán a APP-ID y KEY-ID:

```console
https://YOUR-CUSTOM-SUBDOMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```