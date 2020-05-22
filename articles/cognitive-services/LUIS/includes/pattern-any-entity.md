---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 8aef4a5eeeec27bfb2ca3edf9ff6823b88431319
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83591849"
---
La entidad Pattern.any permite buscar datos de forma libre en los que la redacción de la entidad dificulte determinar el final de la entidad del resto de la expresión.

Una aplicación de recursos humanos ayuda a que los empleados encuentren formularios de la empresa.

|Expresión|
|--|
|¿Dónde está el formulario **HRF-123456**?|
|¿Quién creó el formulario **HRF-123234**?|
|¿El formulario **HRF-456098** se publicó en francés?|

Sin embargo, cada formulario tiene un nombre con formato, el que se usa en la tabla anterior, además de un nombre descriptivo, como `Request relocation from employee new to the company 2018 version 5`.

Las expresiones con el nombre de formato descriptivo tienen el aspecto siguiente:

|Expresión|
|--|
|¿Dónde está el formulario "**Request relocation from employee new to the company 2018 version 5**"?|
|¿Quién creó el formulario **"Request relocation from employee new to the company 2018 version 5"** ?|
|¿El formulario **Request relocation from employee new to the company 2018 version 5** se publicó en francés?|

La diferencia de longitud incluye palabras que pueden confundir a LUIS con respecto a dónde finaliza la entidad. Si usa una entidad Pattern.any en un patrón, puede especificar el principio y el final del nombre del formulario para que LUIS extraiga correctamente el nombre del formulario.

|Ejemplo de expresión de plantilla|
|--|
|¿Dónde está {NombreDelFormulario}[?]|
|¿Quién ha creado {NombreDelFormulario}[?]|
|¿{NombreDelFormulario} se ha publicado en francés[?]|

Revise la información de [referencia](../reference-entity-pattern-any.md) de Pattern.any.