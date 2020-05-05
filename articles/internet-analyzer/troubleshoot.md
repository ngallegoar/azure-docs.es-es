---
title: Solución de problemas de Azure Internet Analyzer | Microsoft Docs
description: La referencia de solución de problemas de Azure Internet Analyzer.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: a265278652c16b4682707470d183a02a55b9a0ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "77069224"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Solución de problemas de Azure Internet Analyzer

Este artículo contiene los pasos para la solución de problemas comunes de Internet Analyzer.

## <a name="things-to-keep-in-mind"></a>Aspectos que debe tener en cuenta
- El script de cliente se debe insertar en un sitio web **HTTPS**. No se recopilarán datos de medición si el script se ejecuta en un sitio web de texto no cifrado (**http://** ) o un sitio local (**file://** ).
- Los datos de medición solo se recopilarán si el script de cliente del perfil de Internet Analyzer se ha incrustado en una aplicación que recibe tráfico de un usuario real. En general, el tráfico sintético (por ejemplo, las pruebas de rendimiento de Azure WebApp) no ejecuta código JavaScript incrustado, por lo que ese tipo de tráfico no generará ninguna medida.

## <a name="azure-portal"></a>Portal de Azure
**"No se ha generado ningún cuadro de mandos para la combinación de filtros seleccionada" en la sección Cuadros de mandos**
- Los cuadros de mandos se generan a diario (al final de cada día, hora UTC).
- Los cuadros de mandos solo se generan si se han recopilado más de 100 medidas para la combinación de filtros seleccionada (prueba, período de tiempo, país, etc.).

**El valor de "Total Measurement Count" ("Recuento total de mediciones") es cero para uno o ambos puntos de conexión de una prueba**
- Las series temporales y los recuentos de mediciones se calculan cada hora, por lo que tendrá que esperar al menos esa cantidad de tiempo para que se muestren los nuevos datos de medición.
- Internet Analyzer solo cuenta las mediciones correctas (es decir, las respuestas HTTP 200) para su análisis. Si uno o ambos puntos de conexión de una prueba son inalcanzables o devuelven un código HTTP que no sea 200, se mostrarán con cero mediciones totales.

## <a name="next-steps"></a>Pasos siguientes
Consulte [Preguntas frecuentes sobre Internet Analyzer](internet-analyzer-faq.md)
