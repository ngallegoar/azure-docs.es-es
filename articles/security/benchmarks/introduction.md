---
title: Introducción a las pruebas comparativas de seguridad de Azure
description: Introducción a las pruebas comparativas de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-baselines
ms.openlocfilehash: 8f65eb008d5df6ea3f2f85778d538023e6ce98eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945332"
---
# <a name="azure-security-benchmark-introduction"></a>Introducción a las pruebas comparativas de seguridad de Azure

Puede que tenga varios años o incluso décadas de experiencia con la informática local. Sabe cómo proteger dichas implementaciones, pero las implementaciones en la nube son diferentes. ¿Cómo sabe si las implementaciones en la nube son seguras? ¿Cuáles son las diferencias entre los procedimientos de seguridad para los sistemas locales y para las implementaciones en la nube?

Hay una gran cantidad de notas del producto, procedimientos recomendados, arquitecturas de referencia, guías web, herramientas de código abierto, soluciones comerciales, fuentes de inteligencia, etc., que se pueden usar para proteger la nube. ¿Qué opción debería usar? ¿Qué puede hacer para obtener un nivel aceptable de seguridad en la nube? 

Una de las mejores formas de proteger las implementaciones en la nube es centrarse en las recomendaciones de las pruebas comparativas de seguridad en la nube. Las recomendaciones de las pruebas comparativas para proteger cualquier servicio empiezan con una comprensión fundamental del riesgo de ciberseguridad y cómo administrarlo. Después, puede usar este conocimiento adoptando las recomendaciones de seguridad comparativa de su proveedor de servicios en la nube para ayudarle a seleccionar opciones de configuración de seguridad específicas en su entorno. 

La prueba comparativa de seguridad de Azure incluye una recopilación de recomendaciones de seguridad de gran impacto que puede usar para ayudar a proteger la mayoría de los servicios que usa en Azure. Puede considerar estas recomendaciones como "generales" u "organizativas", ya que son aplicables a la mayoría de los servicios de Azure. Las recomendaciones de las pruebas comparativas de seguridad de Azure se personalizan después para cada servicio de Azure y esta guía personalizada se incluye en los artículos de recomendaciones de servicio. 

La documentación de las pruebas comparativas de seguridad de Azure especifica controles de seguridad y recomendaciones de servicio.

- **Controles de seguridad**: Las recomendaciones de las pruebas comparativas de seguridad de Azure se clasifican por controles de seguridad. Los controles de seguridad representan requisitos de seguridad independientes del proveedor de alto nivel, como la seguridad de red y la protección de datos. Cada control de seguridad tiene un conjunto de recomendaciones de seguridad e instrucciones que le ayudan a implementar esas recomendaciones. 
- **Recomendaciones de servicio**: Cuando estén disponibles, las recomendaciones de las pruebas comparativas de los servicios de Azure incluirán recomendaciones de pruebas comparativas de seguridad exclusivas para el servicio en concreto. 

Los términos "control", "prueba comparativa" y "base de referencia" se usan con frecuencia en la documentación de Azure Security Benchmark y es importante comprender cómo usa Azure esos términos. 

| Término | Descripción | Ejemplo |
|--|--|--|
| Control | Un **control** es una descripción de alto nivel de una característica o actividad que debe abordarse y no es específico de una tecnología o implementación. | La protección de datos es uno de los controles de seguridad. Este control contiene acciones específicas que deben abordarse para ayudar a garantizar la protección de datos. |
| Prueba comparativa | Una **prueba comparativa** contiene recomendaciones de seguridad para una tecnología específica, como Azure. Las recomendaciones se clasifican por el control al que pertenecen. | La prueba comparativa de seguridad de Azure incluye las recomendaciones de seguridad específicas de la plataforma Azure.  |
| Línea base | Una **base de referencia** son los requisitos de seguridad para una organización. Los requisitos de seguridad se basan en las recomendaciones de las pruebas comparativas. Cada organización decide qué recomendaciones de las pruebas comparativas incluye en su base de referencia. | La empresa Contoso crea su base de referencia de seguridad eligiendo requerir recomendaciones específicas en la prueba comparativa de seguridad de Azure. |

Le agradeceremos sus comentarios sobre la prueba comparativa de seguridad de Azure. Le animamos a que proporcione comentarios en el área de comentarios que aparece a continuación. Si prefiere compartir su opinión de forma más privada con el equipo de pruebas comparativas de seguridad de Azure, debe rellenar el formulario en https://aka.ms/AzSecBenchmark. 
