---
title: Solución de errores comunes
description: Aprenda a solucionar problemas con los diversos SDK mientras consulta recursos de Azure con Azure Resource Graph.
ms.date: 10/18/2019
ms.topic: troubleshooting
ms.openlocfilehash: f881db4f75bcee8c13221717596442ac29a4b1ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "74303903"
---
# <a name="troubleshoot-errors-using-azure-resource-graph"></a>Solución problemas relativos a errores con Azure Resource Graph

Puede que se produzcan errores al consultar los recursos de Azure con Azure Resource Graph. En este artículo se describen diversos errores que pueden producirse y cómo resolverlos.

## <a name="finding-error-details"></a>Búsqueda de detalles del error

La mayoría de los errores son el resultado de un problema al ejecutar una consulta con Azure Resource Graph. Cuando se produce un error en una consulta, el SDK proporciona detalles sobre la consulta errónea. Esta información indica el problema, de modo que se pueda corregir y que una consulta posterior se realice correctamente.

## <a name="general-errors"></a>Errores generales

### <a name="scenario-too-many-subscriptions"></a><a name="toomanysubscription"></a>Escenario: Demasiadas suscripciones

#### <a name="issue"></a>Problema

Los clientes con acceso a más de 1000 suscripciones, incluidas las suscripciones entre inquilinos con [Azure Lighthouse](../../../lighthouse/overview.md), no pueden capturar datos en todas las suscripciones en una única llamada a Azure Resource Graph.

#### <a name="cause"></a>Causa

La CLI de Azure y PowerShell reenvían solo las primeras 1000 suscripciones a Azure Resource Graph. La API REST para Azure Resource Graph acepta un número máximo de suscripciones en las que realizar la consulta.

#### <a name="resolution"></a>Solución

Solicitudes por lotes de la consulta con un subconjunto de suscripciones para permanecer debajo del límite de suscripción de 1000. La solución es usar el parámetro **Subscription** en PowerShell.

```azurepowershell-interactive
# Replace this query with your own
$query = 'Resources | project type'

# Fetch the full array of subscription IDs
$subscriptions = Get-AzSubscription
$subscriptionIds = $subscriptions.Id

# Create a counter, set the batch size, and prepare a variable for the results
$counter = [PSCustomObject] @{ Value = 0 }
$batchSize = 1000
$response = @()

# Group the subscriptions into batches
$subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

# Run the query for each batch
foreach ($batch in $subscriptionsBatch){ $response += Search-AzGraph -Query $query -Subscription $batch.Group }

# View the completed results of the query on all subscriptions
$response
```

### <a name="scenario-unsupported-content-type-rest-header"></a><a name="rest-contenttype"></a>Escenario: Encabezado Content-Type de REST no admitido

#### <a name="issue"></a>Problema

Los clientes que consultan la API de REST de Azure Resource Graph reciben una respuesta _500_ (error interno del servidor).

#### <a name="cause"></a>Causa

La API de REST de Azure Resource Graph solo admite un `Content-Type` de **application/json**. Algunas herramientas o agentes de REST se establecen de forma predeterminada en **texto/sin formato**, un valor que la API de REST no admite.

#### <a name="resolution"></a>Solución

Compruebe que la herramienta o el agente que está usando para realizar la consulta a Azure Resource Graph tiene el encabezado de la API de REST `Content-Type` configurado para **application/json**.

### <a name="scenario-no-read-permission-to-all-subscriptions-in-list"></a><a name="rest-403"></a>Escenario: no tiene permiso de lectura para todas las suscripciones de la lista

#### <a name="issue"></a>Problema

Los clientes que pasan explícitamente una lista de suscripciones con una consulta de Azure Resource Graph reciben una respuesta _403_ (Prohibido).

#### <a name="cause"></a>Causa

Si el cliente no tiene permiso de lectura para todas las suscripciones proporcionadas, se denegará la solicitud debido a la falta de los derechos de seguridad adecuados.

#### <a name="resolution"></a>Solución

Incluya al menos una suscripción de la lista de suscripciones a la que el cliente que ejecuta la consulta tiene acceso de lectura como mínimo. Para más información, consulte [Permisos en Azure Resource Graph](../overview.md#permissions-in-azure-resource-graph).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
- Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.