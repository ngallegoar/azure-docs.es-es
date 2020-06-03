---
title: Solución de problemas de cuentas de Azure Automation
description: En este artículo se describe cómo solucionar problemas con una cuenta de Azure.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: bebc89514da7f28015da5cb145ae50bf3fccdb6a
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680086"
---
# <a name="troubleshoot-azure-automation-account-issues"></a>Solución de problemas de cuentas de Azure Automation

En este artículo se describen soluciones para los problemas que puedan surgir al usar una cuenta de Azure Automation. Para obtener información general sobre las cuentas de Automation, vea [Información general sobre la autenticación de cuentas de Azure Automation](../automation-security-overview.md).

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Escenario: No se puede registrar el proveedor de recursos de Automation para las suscripciones

### <a name="issue"></a>Problema

Al trabajar con características de administración, por ejemplo Update Management, en la cuenta de Automation, se encuentra con el error siguiente:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Causa

El proveedor de recursos de Automation no está registrado en la suscripción.

### <a name="resolution"></a>Solución

Para registrar el proveedor de recursos de Automation, realice los pasos siguientes en Azure Portal:

1. En el explorador, vaya a [Azure Portal](https://portal.azure.com).

2. Vaya a **Suscripciones** y seleccione la suscripción.   

3. En **Configuración**, seleccione **Proveedores de recursos**.

4. En la lista de proveedores de recursos, compruebe que el proveedor de recursos de **Microsoft.Automation** esté registrado.

5. Si el proveedor no aparece en la lista, regístrelo como se describe en [Resolución de errores del registro del proveedor de recursos](/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="next-steps"></a>Pasos siguientes

Si en este artículo no se resuelve la incidencia, pruebe uno de los canales siguientes para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Conectar con [@AzureSupport](https://twitter.com/azuresupport). Se trata de la cuenta oficial de Microsoft Azure para conectar a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de Soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte técnico**.