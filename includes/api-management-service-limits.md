---
title: archivo de inclusión
description: archivo de inclusión
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 01/10/2020
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 2d4c8c2c831bd6ef16f60c34a6353f4a742798f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76159631"
---
| Resource | Límite |
| ---------------------------------------------------------------------- | -------------------------- |
| Número máximo de unidades de escalado | 10 por región<sup>1</sup> |
| Tamaño de memoria caché | 5 GiB por unidad<sup>2</sup> |
| Conexiones simultáneas de back-end<sup>3</sup> por autoridad HTTP | 2048 por unidad<sup>4</sup> |
| Tamaño máximo de respuestas en caché | 2 MiB |
| Tamaño máximo del documento de directiva | 256 KiB<sup>5</sup> |
| Dominios de puerta de enlace personalizados máximos por cada instancia de servicio<sup>6</sup> | 20 |
| Número máximo de certificados de cliente por instancia de servicio | 10 |
| Número máximo de instancias de servicio por suscripción<sup>7</sup> | 20 |
| Número máximo de suscripciones por instancia de servicio<sup>7</sup> | 500 |
| Número máximo de certificados de cliente por instancia de servicio<sup>7</sup> | 50 |
| Número máximo de API por instancia de servicio<sup>7</sup> | 50 |
| Número máximo de operaciones de API por instancia de servicio<sup>7</sup> | 1,000 |
| Duración total máxima de la solicitud<sup>7</sup> | 30 segundos |
| Tamaño de carga útil máximo en búfer<sup>7</sup> | 2 MiB |
| Tamaño máximo de la dirección URL de la solicitud<sup>8</sup> | 4096 bytes |

<sup>1</sup>Los límites de escalado dependen del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, vea [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>El tamaño de la caché por unidad depende del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, vea [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>Las conexiones se agrupan y se vuelven a utilizar, a menos que el back-end las cierre explícitamente.<br/>
<sup>4</sup>Este límite es por unidad de los planes Básico, Estándar y Premium. El plan de desarrollador está limitado a 1024. Este límite no se aplica al plan de consumo.<br/>
<sup>5</sup>Este límite se aplica a los planes Básico, Estándar y Premium. En el plan de consumo, el tamaño del documento de directiva se limita a 4 KiB.<br/>
<sup>6</sup>Este recurso solo está disponible solo en el plan Premium.<br/>
<sup>7</sup>Este recurso es aplicable solo al plan de consumo.<br/>
<sup>8</sup>Solo aplicable al plan de consumo. Incluye una cadena de consulta de hasta 2048 bytes de longitud.<br/>
