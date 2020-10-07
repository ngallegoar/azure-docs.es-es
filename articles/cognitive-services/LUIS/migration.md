---
title: 'Introducción sobre la migración: LUIS'
description: Descripción de lo que hay en una ruta de migración
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/22/2020
ms.openlocfilehash: d6ecacf9aa1a7e650de74a412ed4f161ed0e0790
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91253809"
---
# <a name="migration-in-luis"></a>Migración en LUIS

Hay varios elementos en una ruta de migración. Use la tabla siguiente para comprender a qué afecta y cuándo necesita completar la migración.

|Área|Descripción|Fecha de finalización de la migración|
|--|--|--|
|[API de predicción](luis-migration-api-v3.md)|Migración a la API V3.|TBD|
|[Creación de API](luis-migration-authoring-entities.md)|Migración a la API V3.|TBD|
|[Recurso de creación](luis-migration-authoring.md)|Migre en el portal de LUIS un recurso que no se usa para la creación para que pase a utilizar un recurso de creación en LUIS.|2 de noviembre de 2020 |
|[Característica obligatoria](luis-migration-authoring-entities.md#api-change-constraint-replaced-with-required-feature)|Este cambio se realizó en mayo de 2020, en la conferencia de //Build y solo se aplica a las API de creación de v3 en las que una aplicación usa una característica restringida.|19 de junio de 2020|
|[Entidad compuesta](migrate-from-composite-entity.md)|Actualice la entidad compuesta en la entidad de aprendizaje automático para crear una entidad que reciba predicciones más completas con una capacidad de descomposición mejorada a fin de depurar la entidad.|TBD|
