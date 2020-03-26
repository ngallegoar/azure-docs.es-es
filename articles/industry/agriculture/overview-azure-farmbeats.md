---
title: ¿Qué es Azure FarmBeats?
description: Proporciona información general sobre Azure FarmBeats.
author: uhabiba04
ms.topic: overview
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: b067b18985905b226287f9dd10ad4b937fab6df1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76767962"
---
# <a name="overview-of-azure-farmbeats-preview"></a>Introducción a Azure FarmBeats (versión preliminar)

Azure FarmBeats es una oferta de negocio a negocio disponible en Azure Marketplace. Permite la agregación de conjuntos de datos de agricultura entre proveedores. Azure FarmBeats permite crear modelos de inteligencia artificial (AI) o de aprendizaje automático (ML) basados en conjuntos de datos combinados. Mediante Azure FarmBeats, las empresas agrarias pueden centrarse en la agregación de valores principales, en lugar de la pesada sobrecarga no diferenciada de la ingeniería de datos.

> [!NOTE]
> Azure FarmBeats actualmente está en versión preliminar pública. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Azure FarmBeats se proporciona sin un acuerdo de nivel de servicio. Use el [foro de Azure FarmBeats](https://aka.ms/FarmBeatsMSDN ) para soporte técnico.

![Proyecto de FarmBeats](./media/architecture-for-farmbeats/farmbeats-architecture-1.png)

Con la versión preliminar de Azure FarmBeats puede:

- Evalúe el estado de la granja con el índice de vegetación y el índice de agua basado en imágenes satelitales.
- Obtener recomendaciones sobre el número de sensores de humedad del suelo que se deben usar y dónde colocarlos.
- Realizar un seguimiento de las condiciones en que se encuentra la granja mediante la visualización de los datos acerca del suelo recopilados por sensores de varios proveedores.
- Obtenga un mapa de la humedad del suelo, en función de la fusión de datos de satélites y sensores.
- Obtenga información útil al crear modelos de AI/ML sobre conjuntos de datos agregados.
- Cree o aumente su solución de agricultura digital proporcionando avisos de estado de la granja.

## <a name="datahub"></a>Centro de datos

Azure FarmBeats Datahub es una capa de API, lo que permite la agregación, normalización y contextualización de varios conjuntos de datos agrícolas entre proveedores. Azure FarmBeats se puede usar para obtener:
- Los **datos de los sensores** de dos proveedores de sensores: [Davis Instruments](https://www.davisinstruments.com/product/enviromonitor-gateway/), [Teralytic](https://teralytic.com/), [Pessl Instruments](https://metos.at/)
- **Imágenes de satélite** de la misión del satélite [Sentinel-2](https://sentinel.esa.int/web/sentinel/home) de la Agencia Espacial Europea
- **Imágenes de dron** de tres proveedores de imágenes de dron: [senseFly](https://www.sensefly.com/), [SlantRange](https://slantrange.com/) y [DJI](https://dji.com/)

Datahub está diseñado como una plataforma de API ampliable. Estamos trabajando con muchos más proveedores para que se integren en Azure FarmBeats, con el fin de que tenga más opciones al crear su solución.

## <a name="accelerator"></a>Acelerador

Azure FarmBeats Accelerator es una aplicación web de ejemplo que se crea sobre Datahub. Accelerator impulsa su interfaz de usuario y el desarrollo del modelo. Esta aplicación usa las API de Azure FarmBeats. Visualiza los datos de los sensores ingeridos como gráficos y modela las salidas como mapas. Por ejemplo, se puede usar para crear rápidamente una granja y obtener fácilmente un mapa de índice de vegetación o un mapa de ubicación de sensor para esa granja.

## <a name="resources"></a>Recursos

Azure FarmBeats se ofrece sin cargo adicional y solo se pagan los recursos de Azure que se usen. Puede usar los siguientes recursos para obtener más información sobre la oferta:

- Manténgase informado sobre las últimas noticias de Azure FarmBeats en nuestro [blog de Azure FarmBeats](https://aka.ms/farmbeatsblog).
- Para buscar ayuda, publique una pregunta en el [Foro de soporte técnico de Azure FarmBeats](https://aka.ms/farmbeatssupport).
- Para enviar sus comentarios, publíquelos, o bien vote una idea de una característica en nuestro [Foro de comentarios de Azure FarmBeats](https://aka.ms/farmbeatsfeedback).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Instalación de Azure FarmBeats](install-azure-farmbeats.md)
