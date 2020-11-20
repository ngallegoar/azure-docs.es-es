---
title: Entidades con nombre para la asistencia sanitaria
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/02/2020
ms.author: aahi
ms.openlocfilehash: 308b2a9cb00f44f0e330d4fef5592d8855ee3394
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94553207"
---
## <a name="text-analytics-for-health-categories-entities-and-attributes"></a>Categorías, entidades y atributos de Text Analytics for Health

[Text Analytics for Health](../../how-tos/text-analytics-for-health.md) detecta conceptos médicos en las siguientes categorías.  (Tenga en cuenta que solo se admite texto en inglés en la versión preliminar del contenedor y solo se proporciona una versión de modelo en cada imagen de contenedor).


| Category  | Descripción  |
|---------|---------|
| ANATOMÍA | conceptos que capturan información sobre el cuerpo y los sistemas anatómicos, los sitios, las ubicaciones o las regiones. |
 | DATOS DEMOGRÁFICOS | conceptos que capturan información sobre el sexo y la edad. |
 | EXAMEN MÉDICO | conceptos que capturan información acerca de los procedimientos de diagnóstico y las pruebas. |
 | GENÓMICA | conceptos que capturan información sobre los genes y las variantes. |
 | ATENCIÓN SANITARIA | conceptos que capturan información sobre eventos administrativos, entornos sanitarios y profesiones sanitarias. |
 | ENFERMEDADES | conceptos que capturan información sobre diagnósticos, síntomas o signos. |
 | MEDICACIÓN | conceptos que capturan información sobre la medicación, incluidos los nombres de la misma, sus clases, la dosis y la forma de administración. |
 | SOCIAL | conceptos que capturan información sobre aspectos sociales de interés médico, como la relación de la familia. |
 | TRATAMIENTO | conceptos que capturan información acerca de los procedimientos terapéuticos. |
  
Cada categoría puede incluir dos grupos de conceptos:

* **Entidades**: son los términos que capturan conceptos médicos como el diagnóstico, el nombre de la medicación o el nombre del tratamiento.  Por ejemplo, la *bronquitis* es un diagnóstico y la *aspirina* es el nombre del medicamento.  Las menciones de este grupo pueden estar vinculadas a un id. de concepto de UMLS.
* **Atributos**: son las frases que proporcionan más información sobre la entidad detectada; por ejemplo, *grave* es un calificador de condición para la *bronquitis* u *81 mg* es una dosis de la *aspirina*.  Las menciones en esta categoría NO ESTARÁN vinculadas a un id. de concepto de UMLS.

Además, el servicio reconoce las relaciones entre los distintos conceptos, incluidas las relaciones entre los atributos y las entidades; por ejemplo, la *dirección* para la *estructura del cuerpo* o la *dosis* para el *nombre del medicamento* y las relaciones entre entidades como, por ejemplo, la detección de abreviaturas.

## <a name="anatomy"></a>Anatomía

### <a name="entities"></a>Entidades

**BODY_STRUCTURE**: sistemas del cuerpo, ubicaciones o regiones anatómicas y sitios de cuerpo. Por ejemplo, brazo, rodilla, abdomen, nariz, hígado, cabeza, sistema respiratorio, linfocitos.

:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure.png" alt-text="Ejemplo de la entidad de la estructura del cuerpo.":::


:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure-2.png" alt-text="Ejemplo expandido de la entidad de la estructura del cuerpo.":::

### <a name="attributes"></a>Atributos

**DIRECTION**: condiciones direccionales que caracterizan la estructura del cuerpo, como izquierda, lateral, superior, posterior.

:::image type="content" source="../../media/ta-for-health/anatomy-attributes.png" alt-text="Ejemplo de un atributo direccional.":::

### <a name="supported-relations"></a>Relaciones compatibles

* **DIRECTION_OF_BODY_STRUCTURE**

## <a name="demographics"></a>Demografía

### <a name="entities"></a>Entidades

**AGE**: todos los términos y frases de edad, incluidos los de un paciente, miembros de la familia y otros usuarios. Por ejemplo, 40 años, 51 años, 3 meses, adulto, lactante, anciano, joven, menor de edad, mediana edad.

:::image type="content" source="../../media/ta-for-health/age-entity.png" alt-text="Ejemplo de una entidad de edad.":::

:::image type="content" source="../../media/ta-for-health/age-entity-2.png" alt-text="Otro ejemplo de una entidad de edad.":::


**GENDER**: términos que divulgan el sexo del sujeto. Por ejemplo, masculino, femenino, mujer, señor, señora.

:::image type="content" source="../../media/ta-for-health/gender-entity.png" alt-text="Ejemplo de una entidad de sexo.":::

### <a name="attributes"></a>Atributos

**RELATIONAL_OPERATOR**: frases que expresan la relación entre una entidad demográfica e información adicional.

:::image type="content" source="../../media/ta-for-health/relational-operator.png" alt-text="Ejemplo de un operador relacional.":::

## <a name="examinations"></a>Exámenes médicos

### <a name="entities"></a>Entidades

**EXAMINATION_NAME**: procedimientos y pruebas de diagnóstico. Por ejemplo, resonancia magnética, electrocardiograma, test de VIH, hemoglobina, recuento de plaquetas, sistemas de escala como la *Escala de heces de Bristol*.

:::image type="content" source="../../media/ta-for-health/exam-name-entities.png" alt-text="Ejemplo de una entidad de examen.":::

:::image type="content" source="../../media/ta-for-health/exam-name-entities-2.png" alt-text="Otro ejemplo de una entidad de nombre de examen.":::

### <a name="attributes"></a>Atributos

**DIRECTION**: términos direccionales que caracterizan un examen.

:::image type="content" source="../../media/ta-for-health/exam-direction-attribute.png" alt-text="Ejemplo de un atributo de dirección con una entidad de nombre de examen.":::

**MEASUREMENT_UNIT**: la unidad del examen. Por ejemplo, en *hemoglobina > 9,5 g/dL*, el término *g/dL* es la unidad de la prueba de *hemoglobina*.

:::image type="content" source="../../media/ta-for-health/exam-unit-attribute.png" alt-text="Ejemplo de un atributo de unidad de medida con una entidad de nombre de examen.":::

**MEASUREMENT_VALUE**: el valor del examen. Por ejemplo, en *hemoglobina > 9,5 g/dL*, el término *9,5* es el valor de la prueba de *hemoglobina*.

:::image type="content" source="../../media/ta-for-health/exam-value-attribute.png" alt-text="Ejemplo de un atributo de unidad de medida con una entidad de nombre de examen.":::

**RELATIONAL_OPERATOR**: frases que expresan la relación entre un examen y la información adicional. Por ejemplo, el valor de medida necesario para un examen de destino.

:::image type="content" source="../../media/ta-for-health/exam-relational-operator-attribute.png" alt-text="Ejemplo de un operador relacional con una entidad de nombre de examen.":::

**TIME**: términos temporales relacionados con el inicio o la longitud (duración) de un examen. Por ejemplo, cuándo se produjo la prueba.

:::image type="content" source="../../media/ta-for-health/exam-time-attribute.png" alt-text="Ejemplo de un atributo de tiempo con una entidad de nombre de examen.":::

### <a name="supported-relations"></a>Relaciones compatibles

+ **DIRECTION_OF_EXAMINATION**
+   **RELATION_OF_EXAMINATION**
+   **TIME_OF_EXAMINATION**
+   **UNIT_OF_EXAMINATION**
+   **VALUE_OF_EXAMINATION**

## <a name="genomics"></a>Genomics

### <a name="entities"></a>Entidades

**GENE**: todas las menciones de los genes. Por ejemplo, MTRR, F2.

:::image type="content" source="../../media/ta-for-health/genomics-entities.png" alt-text="Ejemplo de una entidad génica.":::

**VARIANT**: todas las menciones de las variaciones génicas. Por ejemplo, c.524C>T, (MTRR):r.1462_1557del96
  
## <a name="healthcare"></a>Salud

### <a name="entities"></a>Entidades
  
**ADMINISTRATIVE_EVENT**: eventos relacionados con el sistema sanitario pero con una naturaleza administrativa o semiadministrativa. Por ejemplo, registro, admisión, evaluación, entrada de estudio, transferencia, alta, hospital, estancia en el hospital. 

:::image type="content" source="../../media/ta-for-health/healthcare-event-entity.png" alt-text="Ejemplo de una entidad de eventos de atención sanitaria.":::

**CARE_ENVIRONMENT**: un entorno o una ubicación donde se proporciona atención a los pacientes. Por ejemplo, sala de emergencias, oficina del doctor, unidad de cardio, hospital de cuidados paliativos, hospital.

:::image type="content" source="../../media/ta-for-health/healthcare-environment-entity.png" alt-text="Esta captura de pantalla muestra un ejemplo de una entidad del entorno de atención sanitaria.":::

**HEALTHCARE_PROFESSION**: un médico con licencia o sin licencia. Por ejemplo, dentista, patólogo, neurólogo, radiólogo, farmacéutico, nutricionista, fisioterapeuta, quiropráctico.

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity.png" alt-text="Esta captura de pantalla muestra otro ejemplo de una entidad del entorno de atención sanitaria.":::

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity-2.png" alt-text="Otro ejemplo de una entidad de entorno de atención sanitaria.":::

## <a name="medical-condition"></a>Enfermedades

### <a name="entities"></a>Entidades

**DIAGNOSIS**: enfermedad, síndrome, intoxicación. Por ejemplo, cáncer de mama, Alzheimer, hipertensión, insuficiencia cardíaca, lesiones en la espina dorsal.

:::image type="content" source="../../media/ta-for-health/medical-condition-entity.png" alt-text="Ejemplo de una entidad de enfermedad.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-entity-2.png" alt-text="Otro ejemplo de una entidad de enfermedad.":::

**SYMPTOM_OR_SIGN**: evidencia subjetiva u objetiva de la enfermedad u otros diagnósticos. Por ejemplo, dolor de pecho, dolor de cabeza, mareos, sarpullido, disnea, el abdomen está relajado, buen sonido abdominal, bien nutrido.

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity.png" alt-text="Ejemplo de una entidad de síntoma o de signo de enfermedad.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity-2.png" alt-text="Otro ejemplo de una entidad de síntoma o de signo de enfermedad.":::

### <a name="attributes"></a>Atributos

**CONDITION_QUALIFIER**: términos de calidad que se usan para describir una enfermedad. Todas las subcategorías siguientes se consideran calificadores:

1.  Expresiones relacionadas con el tiempo: son los términos que describen la dimensión de tiempo cualitativamente, como repentino, agudo, crónico y duradero. 
2.  Expresiones de calidad:  Son términos que describen la "naturaleza" de la enfermedad, como quemaduras o intenso.
3.  Expresiones de gravedad: grave, leve, moderado, incontrolado.
4.  Expresiones de extensión: local, focal, difuso.
5.  Expresiones de radiación: radianes, radiación.
6.  Escala de enfermedad: En algunos casos, una enfermedad se caracteriza por una escala, que es una lista ordenada y finita de valores. Por ejemplo, pacientes en fase III con cáncer de páncreas.
7.  Curso de la enfermedad: término relacionado con el curso o la progresión de una enfermedad, como la mejora, el empeoramiento, la curación y la remisión. 

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis.png" alt-text="Ejemplo de un atributo calificador de enfermedad y una entidad de diagnóstico.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-2.png" alt-text="Otro ejemplo de un atributo calificador de enfermedad y una entidad de diagnóstico.":::

:::image type="content" source="../../media/ta-for-health/conditional-qualifier-symptom-medication.png" alt-text="Ejemplo de un atributo calificador de enfermedad con entidades de síntomas y medicamentos.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-3.png" alt-text="Esta captura de pantalla muestra otro ejemplo de un atributo calificador de enfermedad con una entidad de diagnóstico.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-symptom.png" alt-text="Esta captura de pantalla muestra un ejemplo adicional de un atributo calificador de enfermedad con una entidad de diagnóstico.":::

**DIRECTION**: términos direccionales que caracterizan una enfermedad del cuerpo.

:::image type="content" source="../../media/ta-for-health/medical-condition-direction-attribute.png" alt-text="Ejemplo de un atributo de dirección con una entidad de enfermedad.":::

**FREQUENCY**: la frecuencia con que se ha producido, se produce o debe producirse una enfermedad.

:::image type="content" source="../../media/ta-for-health/medical-condition-frequency-attribute.png" alt-text="Ejemplo de un atributo de frecuencia con una entidad de enfermedad.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-frequency-attribute-2.png" alt-text="Otro ejemplo de un atributo de dirección con una entidad de síntoma o signo.":::

**MEASUREMENT_UNIT**: la unidad que caracteriza una enfermedad. Por ejemplo, en *tumor de 1,5x2x1 cm*, el término *cm* es la unidad de medida para el *tumor*. 

:::image type="content" source="../../media/ta-for-health/medical-condition-measure-unit-attribute.png" alt-text="Ejemplo de un atributo de unidad de medida con una entidad de enfermedad.":::

**MEASUREMENT_VALUE**: valor que caracteriza una enfermedad. Por ejemplo, en *tumor de 1,5x2x1 cm*, el término *1,5x2x1* es el valor de medida para el *tumor*. 

:::image type="content" source="../../media/ta-for-health/medical-condition-measure-value-attribute.png" alt-text="Captura de pantalla que muestra un ejemplo de un atributo de dirección con una entidad de síntoma o señal.":::

**RELATIONAL_OPERATOR**: frases que expresan la relación entre la información adicional sobre la enfermedad. Por ejemplo, el valor de tiempo o de medida. 

:::image type="content" source="../../media/ta-for-health/medical-condition-relational-operator.png" alt-text="Captura de pantalla que muestra otro ejemplo de un atributo de dirección con una entidad de síntoma o señal.":::

**TIME**: términos temporales relacionados con el inicio o la longitud (duración) de una enfermedad. Por ejemplo, cuando se inicia un síntoma (comienzo) o cuando se produjo una enfermedad.

:::image type="content" source="../../media/ta-for-health/medical-condition-time-attribute.png" alt-text="Captura de pantalla que muestra un ejemplo adicional de un atributo de dirección con una entidad de síntoma o señal.":::

### <a name="supported-relations"></a>Relaciones compatibles

+ **DIRECTION_OF_CONDITION**
+   **QUALIFIER_OF_CONDITION**
+   **TIME_OF_CONDITION**
+   **UNIT_OF_CONDITION**
+   **VALUE_OF_CONDITION**

## <a name="medication"></a>Medicación

### <a name="entities"></a>Entidades

**MEDICATION_CLASS**: conjunto de medicamentos que tienen un mecanismo de acción similar, un modo de acción relacionado, una estructura química similar o que se usan para tratar la misma enfermedad. Por ejemplo, el inhibidor de ACE, opioides, antibióticos y analgésicos.

:::image type="content" source="../../media/ta-for-health/medication-entities-class.png" alt-text="Ejemplo de una entidad de clase de medicación.":::

**MEDICATION_NAME**: menciones a la medicación, incluidos los nombres de marca con copyright y los nombres sin marca. Por ejemplo, Advil, Ibuprofeno.

:::image type="content" source="../../media/ta-for-health/medication-entities-name.png" alt-text="Ejemplo de una entidad de nombre de medicación.":::

### <a name="attributes"></a>Atributos

**DOSAGE**: cantidad de medicación solicitada. Por ejemplo, inocular *1000 mL* de una solución de cloruro sódico.

:::image type="content" source="../../media/ta-for-health/medication-dosage.png" alt-text="Ejemplo de un atributo de dosis de medicación.":::

**FREQUENCY**: la frecuencia con la que se debe tomar un medicamento.

:::image type="content" source="../../media/ta-for-health/medication-frequency.png" alt-text="Ejemplo de un atributo de frecuencia de medicación.":::

:::image type="content" source="../../media/ta-for-health/medication-frequency-2.png" alt-text="Otro ejemplo de un atributo de frecuencia de medicación.":::

**MEDICATION_FORM**: la forma del medicamento. Por ejemplo, solución, pastilla, cápsula, tableta, parche, gel, pasta, espuma, aerosol, gotas, crema y jarabe.

:::image type="content" source="../../media/ta-for-health/medication-form.png" alt-text="Ejemplo de un atributo de forma de medicación.":::

**MEDICATION_ROUTE**: el método de administración de la medicación. Por ejemplo, oral, vaginal, intravenosa, epidural, tópica, inhalada.

:::image type="content" source="../../media/ta-for-health/medication-route.png" alt-text="Ejemplo de un atributo de ruta de medicación.":::

**RELATIONAL_OPERATOR**: frases que expresan la relación entre un medicamento y la información adicional. Por ejemplo, el valor de medida necesario.

:::image type="content" source="../../media/ta-for-health/medication-relational-operator.png" alt-text="Captura de pantalla que muestra un ejemplo de un atributo de operador relacional con una entidad de medicación.":::

:::image type="content" source="../../media/ta-for-health/medication-time.png" alt-text="Captura de pantalla que muestra otro ejemplo de un atributo de operador relacional con una entidad de medicación.":::

### <a name="supported-relations"></a>Relaciones compatibles

+ **DOSAGE_OF_MEDICATION**
+   **FORM_OF_MEDICATION**
+   **FREQUENCY_OF_MEDICATION**
+   **ROUTE_OF_MEDICATION**
+   **TIME_OF_MEDICATION**
  
## <a name="treatment"></a>Tratamiento

### <a name="entities"></a>Entidades

**TREATMENT_NAME**: procedimientos terapéuticos. Por ejemplo, cirugía de sustitución de rodilla, trasplante de médula ósea, implante valvular aórtico transcatéter, dieta.

:::image type="content" source="../../media/ta-for-health/treatment-entities-name.png" alt-text="Ejemplo de una entidad de nombre de tratamiento.":::

### <a name="attributes"></a>Atributos

**DIRECTION**: términos direccionales que caracterizan un tratamiento.

:::image type="content" source="../../media/ta-for-health/treatment-direction.png" alt-text="Captura de pantalla que muestra un ejemplo de un atributo de dirección de tratamiento.":::

**FREQUENCY**: frecuencia con la que se lleva a cabo un tratamiento.

:::image type="content" source="../../media/ta-for-health/treatment-frequency.png" alt-text="Captura de pantalla que muestra otro ejemplo de un atributo de dirección de tratamiento.":::
 
**RELATIONAL_OPERATOR**: frases que expresan la relación entre un tratamiento y la información adicional.  Por ejemplo, cuánto tiempo pasó desde el procedimiento anterior.

:::image type="content" source="../../media/ta-for-health/treatment-relational-operator.png" alt-text="Ejemplo de un atributo de operador relacional de tratamiento.":::

**TIME**: términos temporales relacionados con el inicio o la longitud (duración) de un tratamiento. Por ejemplo, la fecha en la que se proporcionó el tratamiento.

:::image type="content" source="../../media/ta-for-health/treatment-time.png" alt-text="Captura de pantalla que muestra un ejemplo de un atributo de duración de tratamiento.":::


### <a name="supported-relations"></a>Relaciones compatibles

+ **DIRECTION_OF_TREATMENT**
+   **TIME_OF_TREATMENT**
+   **FREQUENCY_OF_TREATMENT**

## <a name="social"></a>Redes sociales

### <a name="entities"></a>Entidades

**FAMILY_RELATION**: menciones de familiares relativas al sujeto. Por ejemplo, padre, hija, hermanos, progenitores.

:::image type="content" source="../../media/ta-for-health/family-relation.png" alt-text="Captura de pantalla que muestra otro ejemplo de un atributo de duración de tratamiento.":::
