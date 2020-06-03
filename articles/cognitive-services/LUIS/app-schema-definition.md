---
title: Definición de esquemas de la aplicación
description: La aplicación de LUIS se representa en formato `.json` o `.lu` e incluye todas las intenciones, las entidades, las expresiones de ejemplo, las características y la configuración.
ms.topic: reference
ms.date: 05/05/2020
ms.openlocfilehash: dbbeb4eacbe8e07cf080b3a1527ca39c2b9a5991
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684349"
---
# <a name="app-schema-definition"></a>Definición del esquema de la aplicación

La aplicación de LUIS se representa en formato `.json` o `.lu` e incluye todas las intenciones, las entidades, las expresiones de ejemplo, las características y la configuración.

## <a name="format"></a>Formato

Cuando importe y exporte la aplicación, elija `.json` o `.lu`.

|Formato|Information|
|--|--|
|`.json`| Interfaz de programación estándar|
|`.lu`|Compatible con las [herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown/docs/lu-file-format.md) de Bot Framework.|

## <a name="version-7x"></a>Version 7.x

* Al pasar a la versión 7.x, las entidades se representan como entidades anidadas de aprendizaje automático.
* Compatibilidad para la creación de entidades anidadas de aprendizaje automático con la propiedad `enableNestedChildren` en las siguientes API de creación:
    * [Agregar etiqueta](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c08)
    * [Agregar etiqueta de lote](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c09)
    * [Revisar etiquetas](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c0a)
    * [Sugerir consultas de punto de conexión para entidades](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2e)
    * [Sugerir consultas de punto de conexión para intenciones](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2d)

```json
{
  "luis_schema_version": "7.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-6x"></a>Versión 6.x

* Al pasar a la versión 6.x, use la nueva [entidad de aprendizaje automático](reference-entity-machine-learned-entity.md) para representar las entidades.

```json
{
  "luis_schema_version": "6.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-4x"></a>Version 4.x

```json
{
  "luis_schema_version": "4.0.0",
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "intents": [
    {
      "name": "None"
    }
  ],
  "entities": [],
  "composites": [],
  "closedLists": [],
  "patternAnyEntities": [],
  "regex_entities": [],
  "prebuiltEntities": [],
  "model_features": [],
  "regex_features": [],
  "patterns": [],
  "utterances": [],
  "settings": []
}
```

## <a name="next-steps"></a>Pasos siguientes

* Migración a las [API de creación V3](luis-migration-authoring-entities.md)