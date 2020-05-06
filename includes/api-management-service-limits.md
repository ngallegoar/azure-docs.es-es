---
title: archivo de inclusión
description: archivo de inclusión
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 04/14/2020
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 072f13f5a0884cf95fe760e17ff0d770111f4da0
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204536"
---
| Resource | Límite |
| ---------------------------------------------------------------------- | -------------------------- |
| Número máximo de unidades de escalado | 10 por región<sup>1</sup> |
| Tamaño de memoria caché | 5 GiB por unidad<sup>2</sup> |
| Conexiones simultáneas de back-end<sup>3</sup> por autoridad HTTP | 2048 por unidad<sup>4</sup> |
| Tamaño máximo de respuestas en caché | 2 MiB |
| Tamaño máximo del documento de directiva | 256 KiB<sup>5</sup> |
| Dominios de puerta de enlace personalizados máximos por cada instancia de servicio<sup>6</sup> | 20 |
| Número máximo de certificados de entidad de certificación por instancia de servicio<sup>7</sup> | 10 |
| Número máximo de instancias de servicio por suscripción<sup>8</sup> | 20 |
| Número máximo de suscripciones por instancia de servicio<sup>8</sup> | 500 |
| Número máximo de certificados de cliente por instancia de servicio<sup>8</sup> | 50 |
| Número máximo de API por instancia de servicio<sup>8</sup> | 50 |
| Número máximo de operaciones de API por instancia de servicio<sup>8</sup> | 1,000 |
| Duración total máxima de la solicitud<sup>8</sup> | 30 segundos |
| Tamaño máximo de carga útil en búfer<sup>8</sup> | 2 MiB |
| Tamaño máximo de la dirección URL de la solicitud<sup>9</sup> | 4096 bytes |
| Número máximo de puertas de enlace autohospedadas<sup>10</sup> | 25 |

<sup>1</sup>Los límites de escalado dependen del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, consulte [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>El tamaño de la caché por unidad depende del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, vea [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>Las conexiones se agrupan y se vuelven a utilizar, a menos que el back-end las cierre explícitamente.<br/>
<sup>4</sup>Este límite es por unidad de los planes Básico, Estándar y Premium. El plan de desarrollador está limitado a 1024. Este límite no se aplica al plan de consumo.<br/>
<sup>5</sup>Este límite se aplica a los planes Básico, Estándar y Premium. En el plan de consumo, el tamaño del documento de directiva se limita a 4 KiB.<br/>
<sup>6</sup>Solo se admiten varios dominios personalizados en los planes Desarrollador y Premium.<br/>
<sup>7</sup>Los certificados de CA no se admiten en el plan de consumo.<br/>
<sup>8</sup>Este recurso solo se aplica al plan de consumo.<br/>
<sup>9</sup>Solo se aplica al plan de consumo. Incluye una cadena de consulta de hasta 2048 bytes de longitud.<br/>
<sup>10</sup>Las puertas de enlace autohospedadas solo se admiten en los planes Desarrollador y Premium. El límite se aplica al número de [recursos de puerta de enlace autohospedados](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/gateway). Para aumentar este límite, póngase en contacto con el departamento de [soporte técnico](https://azure.microsoft.com/support/options/). Tenga en cuenta que el número de nodos (o réplicas) asociados a un recurso de puerta de enlace autohospedado es ilimitado en el plan Premium y está limitado a un solo nodo en el plan Desarrollador.
