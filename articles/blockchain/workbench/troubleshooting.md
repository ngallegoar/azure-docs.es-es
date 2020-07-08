---
title: Solución de problemas de Azure Blockchain Workbench
description: Procedimientos para la solución de problemas de una aplicación de Azure Blockchain Workbench, versión preliminar.
ms.date: 10/14/2019
ms.topic: troubleshooting
ms.reviewer: brendal
ms.openlocfilehash: 20c0f9bdd6f820a73b1ba6660de805268c0d8714
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212860"
---
# <a name="azure-blockchain-workbench-preview-troubleshooting"></a>Solución de problemas de Azure Blockchain Workbench, versión preliminar

Está disponible un script de PowerShell para ayudar con la depuración del desarrollo y soporte técnico. El script genera un resumen y recopila registros detallados para solucionar el problema. Los registros recopilados incluyen:

* Red de Blockchain, por ejemplo, Ethereum
* Microservicios de Blockchain Workbench
* Application Insights
* Supervisión de Azure (registros de Azure Monitor)

Puede usar la información para determinar los próximos pasos y determinar la causa raíz de los problemas.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="troubleshooting-script"></a>Script de solución de problemas

El script de solución de problemas de PowerShell está disponible en GitHub. [Descargue un archivo zip](https://github.com/Azure-Samples/blockchain/archive/master.zip) o clone el ejemplo de GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Ejecute el script.
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Ejecute el script `collectBlockchainWorkbenchTroubleshooting.ps1` para recopilar registros y crear un archivo ZIP que contiene una carpeta con información para la solución de problemas. Por ejemplo:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
El script acepta los siguientes parámetros:

| Parámetro  | Descripción | Obligatorio |
|---------|---------|----|
| SubscriptionID | Identificador de suscripción para crear o ubicar todos los recursos. | Sí |
| ResourceGroupName | Nombre del grupo de recursos de Azure donde se haya implementado Blockchain Workbench. | Sí |
| OutputDirectory | Ruta de acceso para crear el archivo .ZIP de salida. Si no se especifica, tiene como valor predeterminado el directorio actual. | No |
| LookbackHours | Número de horas que se usará al extraer la telemetría. El valor predeterminado es 24 horas. El valor máximo es 90 horas. | No |
| OmsSubscriptionId | El identificador de la suscripción donde se han implementado registros de Azure Monitor. Pase este parámetro únicamente si se implementan registros de Azure Monitor para la red de la cadena de bloques fuera del grupo de recursos de Blockchain Workbench.| No |
| OmsResourceGroup |Grupo de recursos donde se han implementado registros de Azure Monitor. Pase este parámetro únicamente si se implementan registros de Azure Monitor para la red de la cadena de bloques fuera del grupo de recursos de Blockchain Workbench.| No |
| OmsWorkspaceName | Nombre del área de trabajo de Log Analytics. Pase este parámetro únicamente si se implementan registros de Azure Monitor para la red de la cadena de bloques fuera del grupo de recursos de Blockchain Workbench. | No |

## <a name="what-is-collected"></a>¿Qué información se recopila?

El archivo ZIP de salida contiene la estructura de carpetas siguiente:

| Archivo o carpeta | Descripción  |
|---------|---------|
| \Summary.txt | Resumen del sistema |
| \Metrics\blockchain | Métricas relacionadas con la cadena de bloques |
| \Metrics\Workbench | Métricas relacionadas con Workbench |
| \Details\Blockchain | Registros detallados sobre la cadena de bloques |
| \Details\Workbench | Registros detallados sobre Workbench |

El archivo de resumen proporciona una instantánea del mantenimiento de la aplicación y el estado general de la aplicación. El resumen proporciona las acciones recomendadas, resalta los errores destacados y proporciona metadatos sobre la ejecución de servicios.

La carpeta **Metrics** (Métricas) contiene las métricas de los diversos componentes del sistema a lo largo del tiempo. Por ejemplo, el archivo de salida `\Details\Workbench\apiMetrics.txt` contiene un resumen de los diferentes códigos de respuesta, y tiempos de respuesta a lo largo del período de recopilación. La carpeta **Details** (Detalles) contiene registros detallados para solucionar problemas específicos con Workbench o la red de la cadena de bloques subyacente. Por ejemplo, `\Details\Workbench\Exceptions.csv` contiene una lista de las excepciones más recientes que se han producido en el sistema, lo cual resulta útil para solucionar problemas de errores con contratos inteligentes o las interacciones con la cadena de bloques. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Guía de solución de problemas de Azure Blockchain Workbench en Application Insights](https://aka.ms/workbenchtroubleshooting)
