---
title: Ventajas de usar Azure NetApp Files para la automatización de diseños electrónicos | Microsoft Docs
description: Explica la solución que Azure NetApp Files proporciona para satisfacer las necesidades del sector de diseños de chips y semiconductores. Presenta escenarios de prueba que ejecutan una prueba comparativa del sector estándar para la automatización de diseños electrónicos (EDA) mediante Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: b-juche
ms.openlocfilehash: fcede16619e8488796adc6f4c60af30643c1aadf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82160160"
---
# <a name="benefits-of-using-azure-netapp-files-for-electronic-design-automation"></a>Ventajas de usar Azure NetApp Files para la automatización de diseños electrónicos

El tiempo de comercialización (TTM) es una consideración fundamental para el sector de diseños de chips y semiconductores. El sector tiene necesidades de ancho de banda elevado y baja latencia para el almacenamiento. En este artículo se explica la solución que Azure NetApp Files proporciona para satisfacer las necesidades del sector. Presenta escenarios de prueba que ejecutan una prueba comparativa del sector estándar para la automatización de diseños electrónicos (EDA) mediante Azure NetApp Files. 

## <a name="test-scenario-configurations"></a>Configuraciones de los escenarios de prueba

Las pruebas implican tres escenarios con las configuraciones siguientes. 

|    Escenario    |    Volúmenes    |    Clientes<br> SLES15 D16s_v3  |
|----------------|---------------|--------------------------------|
|    Uno         |    1          |    1                           |
|    Dos         |    6          |    24                          |
|    tres       |    12         |    24                          |

En el primer escenario se aborda qué lejos se puede llevar un solo volumen.  

El segundo y tercer escenarios evalúan los límites de un punto de conexión de Azure NetApp Files único. Investigan las posibles ventajas de la latencia y los límites superiores de E/S.

## <a name="test-scenario-results"></a>Resultados de los escenarios de prueba

En la tabla siguiente se resumen los resultados de los escenarios de prueba.

|    Escenario       |    Velocidad de E/S<br>  a 2 ms     |    Velocidad de E/S<br>  en el perímetro     |    Throughput<br>  a 2 ms     |    Throughput<br>  en el perímetro     |
|-------------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|
|    1 volumen       |    39 601                 |    49 502                      |    692 MiB/s                 |    866 MiB/s                      |
|    6 volúmenes      |    255 613                |    317 000                     |    4577 MiB/s               |    5568 MiB/s                    |
|    12 volúmenes     |    256 612                |    319 196                     |    4577 MiB/s               |    5709 MiB/s                    |

El escenario con un volumen único representa la configuración básica de la aplicación. Es el escenario de base de referencia de los escenarios de prueba de seguimiento.  

El escenario con seis volúmenes muestra un aumento lineal (600 %) con respecto a la carga de trabajo con un volumen único.  Se accede a todos los volúmenes de una sola red virtual a través de una dirección IP única.  

El escenario con 12 volúmenes muestra una disminución general en la latencia en comparación con el escenario con seis volúmenes, pero no tiene un aumento correspondiente en el rendimiento factible.   

En el gráfico siguiente se muestra la latencia y la tasa de operaciones de la carga de trabajo de EDA en Azure NetApp Files.  

![Latencia y tasa de operaciones de la carga de trabajo de EDA en Azure NetApp Files](../media/azure-netapp-files/solutions-electronic-design-automation-workload-latency-operation-rate.png)   

En el gráfico siguiente se muestra la latencia y el rendimiento de la carga de trabajo de EDA en Azure NetApp Files.  

![Latencia y rendimiento de la carga de trabajo de EDA en Azure NetApp Files](../media/azure-netapp-files/solutions-electronic-design-automation-workload-latency-throughput.png) 

## <a name="layout-of-test-scenarios"></a>Diseño de los escenarios de prueba 

En la tabla siguiente se resume el diseño de los escenarios de prueba.

|    Escenario de prueba     |    Número total de directorios     |    Número total de archivos     |
|----------------------|------------------------------------|------------------------------|
|    1 volumen          |    88 000                          |    880 000                   |
|    6 volúmenes         |    568 000                         |    5 680 000                 |
|    12 volúmenes        |    568 000                         |    5 680 000                 |

La carga de trabajo completa es una combinación de fases funcionales y físicas que se ejecutan simultáneamente. Representa un flujo típico de un conjunto de herramientas de EDA a otro.   

La fase funcional consta de especificaciones iniciales y un diseño lógico. La fase física se produce cuando el diseño lógico se convierte en un chip físico. Durante las fases de aprobación y entrega al cliente, se completa las revisiones finales y el diseño se entrega a una fundición para su fabricación.  

La fase funcional incluye una combinación de E/S de lectura y escritura en secuencia y aleatoria. La fase funcional implica muchos metadatos, como estadísticas de archivos y llamadas de acceso. Aunque las operaciones de metadatos son efectivamente sin tamaño, las operaciones de lectura y escritura oscilan entre menos de 1 K y 16 K. La mayoría de las lecturas están entre 4 K y 16 K. La mayoría de las escrituras, 4 K o menos. La fase física se compone completamente de operaciones de lectura y escritura en secuencia, con una combinación de tamaños de operación de 32 K y 64 K.  

En los gráficos anteriores, la mayor parte del rendimiento procede de la fase física en secuencia de la carga de trabajo. La E/S procede de la fase funcional pequeña aleatoria y con un uso intensivo de metadatos. Ambas fases se producen en paralelo. 

En conclusión, puede emparejar Azure Compute con Azure NetApp Files para el diseño de EDA para obtener un ancho de banda escalable. 

## <a name="next-steps"></a>Pasos siguientes

- [Arquitecturas de las soluciones con Azure NetApp Files](azure-netapp-files-solution-architectures.md)
