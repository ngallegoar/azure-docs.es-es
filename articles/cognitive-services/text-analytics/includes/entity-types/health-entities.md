---
title: Entidades con nombre para la asistencia sanitaria
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/15/2020
ms.author: aahi
ms.openlocfilehash: 906e8e9e6cde80a2151c6b8671f64854cb431250
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108518"
---
## <a name="health-entity-categories"></a>Categorías de la entidad de salud:

[Text Analytics for Health](../../how-tos/text-analytics-for-health.md) devuelve las siguientes categorías de entidad.  Tenga en cuenta que solo se admite texto en inglés en versión preliminar de contenedor y solo se proporciona una versión de modelo en cada imagen de contenedor.

### <a name="named-entity-recognition"></a>Reconocimiento de entidades con nombre

|Category  |Descripción   |
|----------|--------------|
| AGE | Edades. |
| BODY_STRUCTURE | Partes del cuerpo humano, incluidos los órganos y otras estructuras. | 
| CONDITION_QUALIFIER | Los niveles de la enfermedad, como *leve*, *extendida* o *suavizada*. | 
| DIAGNOSIS | Enfermedades médicas. Por ejemplo, *hipertensión*. | 
| DIRECTION | Indicaciones como *izquierda* o *anterior*. | 
| DOSAGE | Tamaño o cantidad de un medicamento.  | 
| EXAMINATION_NAME | Método o procedimiento de examen. | 
| EXAMINATION_RELATION | asociación entre una unidad de medida y un examen.  | 
| EXAMINATION_UNIT | Unidad de medida de examen. | 
| EXAMINATION_VALUE | Valor de la unidad de medida de examen. | 
| FAMILY_RELATION | Parentesco, como *hermana*.  | 
| FRECUENCIA | Frecuencias.   | 
| GENDER | Géneros. | 
| GENE | Una entidad génica, como *TP53*.   | 
| MEDICATION_CLASS | Clases de medicación. Por ejemplo, *antibióticos*.  | 
| MEDICATION_NAME  | Medicamentos genéricos y de marca.| 
| ROUTE_OR_MODE  | Método de administración de medicamentos. | 
| SYMPTOM_OR_SIGN  | Síntomas de la enfermedad. | 
| TIME  | Tiempo. Por ejemplo, "8 años" o "2:30 esta madrugada" |
| TREATMENT_NAME  | Nombres del tratamiento. | 
| VARIANT  | Variante genética de la entidad del gen | 

### <a name="relation-extraction"></a>Extracción de relaciones

La extracción de relaciones identifica conexiones significativas entre los conceptos mencionados en el texto. Por ejemplo, una relación de "tiempo de enfermedad" se encuentra asociando un nombre de enfermedad con un tiempo. Text Analytics for Health puede identificar las siguientes relaciones:

* DIRECTION_OF_BODY_STRUCTURE  
* TIME_OF_CONDITION
* QUALIFIER_OF_CONDITION  
* DOSAGE_OF_MEDICATION 
* FORM_OF_MEDICATION  
* ROUTE_OR_MODE_OF_MEDICATION   
* STRENGTH_OF_MEDICATION 
* ADMINISTRATION_RATE_OF_MEDICATION   
* FREQUENCY_OF_MEDICATION 
* TIME_OF_MEDICATION 
* TIME_OF_TREATMENT 
* FREQUENCY_OF_TREATMENT  
* VALUE_OF_EXAMINATION
* UNIT_OF_EXAMINATION 
* RELATION_OF_EXAMINATION 
* TIME_OF_EXAMINATION  
* ABBREVIATION 
