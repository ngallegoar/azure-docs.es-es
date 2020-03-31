---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 11783f8fac147e6fb2cf371ee1a588cf318a5e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75379453"
---
Form Recognizer trabaja con documentos de entrada que cumplen estos requisitos:

* El formato debe ser JPG, PNG o PDF (texto o digitalizado) o TIFF. Los PDF insertados de texto son mejores porque no hay posibilidad de error en la extracción de caracteres y en la ubicación.
* Si los archivos PDF están bloqueados con contraseña, debe quitar el bloqueo antes de enviarlos.
* Los documentos PDF y TIFF deben tener 200 páginas como máximo y el tamaño total del conjunto de datos de entrenamiento debe ser inferior a 500 páginas.
* Para las imágenes, las dimensiones deben tener entre 600 x 100 píxeles y 4200 x 4200 píxeles.
* Si se digitalizan desde documentos en papel, los formularios deben ser digitalizaciones de alta calidad.
* El texto debe usar el alfabeto latino (caracteres ingleses).
* En el caso del aprendizaje sin supervisión (sin datos etiquetados), los datos deben contener claves y valores.
* En el caso del aprendizaje sin supervisión (sin datos etiquetados), las claves deben aparecer por encima o a la izquierda de los valores; no pueden aparecer por abajo ni a la derecha.

Form Recognizer no admite actualmente estos tipos de datos de entrada:

* Tablas complejas (tablas anidadas, encabezados combinados o celdas, etc.)
* Casillas o botones de radio
