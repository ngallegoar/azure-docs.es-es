---
title: Comprensión del uso de las aplicaciones móviles y del comportamiento de los usuarios con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de App Center que le ayudan a comprender el uso que los usuarios hacen de sus aplicaciones móviles y así poder tomar decisiones empresariales inteligentes.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 4dc3cea4497c55dda0d8da2ca29201615dadff19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241058"
---
# <a name="analyze-and-understand-mobile-application-use"></a>Análisis y comprensión del uso de las aplicaciones móviles
¿Comprende bien cómo los usuarios usan las aplicaciones? ¿Cuántos usuarios activos tiene la aplicación y cómo cambia el uso con el tiempo? ¿Qué características usan y cuáles son las más utilizadas? ¿Dónde se encuentran estos usuarios? ¿Cuántos usuarios usan la versión más reciente de la aplicación? Todas estas preguntas son importantes para comprender el modo de convertir la aplicación en un éxito. Para responder a estas clases de preguntas de análisis de uso, debe recopilar datos de uso de las aplicaciones.

Cuanto más estudie los datos, más formas se podrán encontrar de mejorar la aplicación y mantener contentos a los usuarios. Es importante usar los datos para buscar información detallada sobre la que poder actuar y que los usuarios estén satisfechos.

## <a name="importance-of-analytics"></a>Importancia del análisis
- Comprenda a los usuarios, cómo interactúan con la aplicación y qué les hace volver para ajustar la aplicación y proporcionar excelentes experiencias que hagan crecer su negocio.
- Realice un seguimiento de las métricas de uso para tomar decisiones fundamentadas sobre cómo comercializar su aplicación y ofrecer un mejor servicio a su cliente.
- Mida el rendimiento de la aplicación.
- Aprenda qué partes de la aplicación generan valor y rendimiento.
- Obtenga conclusiones basadas en datos en relación con los problemas relacionados con el abandono y la retención de clientes.

Use los servicios siguientes para habilitar el análisis de las aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[Analytics de App Center](/appcenter/analytics/) le permite aumentar la audiencia al centrarse en lo que es importante. Ofrece informes detallados y conclusiones sobre las sesiones de usuario, los dispositivos principales, las versiones del sistema operativo y el análisis del comportamiento. Cree eventos personalizados con facilidad para realizar el seguimiento de todo con análisis de aplicaciones completos.

   **Características principales**
   - Realice un seguimiento de los patrones de uso, la adopción de usuarios y otras métricas de participación de forma gratuita.
   - Identifique tendencias, el comportamiento de los usuarios y la participación mediante eventos personalizados.
   - Métricas preparadas e información detallada sobre el uso de las aplicaciones (diario, semanal, mensual), sesiones, propiedades de los dispositivos y datos demográficos de los usuarios en un único panel.
   - Exportación continua de todos los datos de App Center Analytics a Azure para conservarlos de forma ilimitada. App Center Analytics admite la exportación a Azure Blob Storage y Azure Application Insights.
   - Integración con Azure Application Insights para obtener información aún más detallada, como la retención, el análisis de embudo y los cohortes.
   - Use la Integración rápida del SDK para empezar en cuestión de minutos.
   - Obtenga compatibilidad con las plataformas iOS, Android, macOS, tvOS, Xamarin, React Native, Unity y Cordova.

   **Referencias**
   - [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
   - [Introducción a App Center Analytics](/appcenter/analytics/)

## <a name="azure-monitor"></a>Azure Monitor
Azure Monitor incluye [Application Insights](/azure/azure-monitor/app/app-insights-overview), que proporciona herramientas para recopilar y analizar la telemetría con el fin de maximizar el rendimiento y supervisar la aplicación móvil. Puede aprovechar las ventajas de Application Insights con App Center Analytics para configurar la exportación a Application Insights. Application Insights permite consultar, segmentar, filtrar y analizar la telemetría de eventos personalizada desde las aplicaciones, de forma más exhaustiva que las herramientas de análisis que App Center ofrece.

**Características principales**
   - Consulta de la telemetría de eventos personalizados.
   - Filtrado de la telemetría de eventos con funcionalidades de segmentación eficaces.
   - Análisis de los patrones de conversión, retención y navegación de la aplicación. Puede usar:
     - Embudos para analizar y supervisar las tasas de conversión.
     - Retención para analizar el grado en que la aplicación retiene a los usuarios con el paso del tiempo.
     - Workbooks para combinar visualizaciones y texto en un informe que se pueda compartir.
     - Cohortes para nombrar y guardar grupos de usuarios o eventos específicos, para poder hacer referencia a ellos fácilmente desde otras herramientas de análisis.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [Análisis de la aplicación móvil con App Center y Application Insights](/azure/azure-monitor/learn/mobile-center-quickstart)
- [Uso de App Center Analytics con Application Insights](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab
[Azure PlayFab](https://playfab.com/) ofrece una plataforma de back-end completa con servicios de juegos, análisis en tiempo real y LiveOps que necesita para crear juegos conectados a la nube de nivel mundial. Estos servicios reducen las barreras de lanzamiento para los desarrolladores de juegos. Ofrecen soluciones de desarrollo rentables de estudio grandes y pequeñas que se escalan con sus juegos. Estos servicios pueden ayudar a los estudios a interactuar con los jugadores, a conservarlos y a rentabilizarlos. Con PlayFab, los desarrolladores pueden usar la nube inteligente para compilar y gestionar los juegos, analizar los datos de juego y mejorar las experiencias de juego en general.

**Características principales**
   - Supervisión de paneles en tiempo real.
   - Evaluación del rendimiento del juego a través de las principales métricas.
   - Revise los resúmenes del rendimiento diario y mensual del juego mediante informes generados automáticamente. Puede ver los informes en el administrador de juegos y hacer que se descarguen o entreguen a la bandeja de entrada diariamente.
   - Use pruebas A/B para ejecutar experimentos y determinar el valor óptimo para una variable determinada.
   - Use la segmentación para que los jugadores puedan definir agrupaciones automatizadas de jugadores.
    
**Referencias**
- [Portal de PlayFab](https://developer.playfab.com/en-US/sign-up)
- [Analytics](/gaming/playfab/#pivot=documentation&panel=analytics)
- [Guías de inicio rápido](/gaming/playfab/#pivot=documentation&panel=quickstarts) 
