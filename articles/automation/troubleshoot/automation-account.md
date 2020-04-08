---
title: Solución de problemas de cuentas de Automation
description: Obtenga información acerca de cómo solucionar problemas con una cuenta de Azure.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: c66b1728144b8517f6ac444059b3a8def956c6e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80299200"
---
# <a name="automation-account-troubleshooting"></a>Solución de problemas de cuentas de Automation

En este artículo se describen soluciones para los problemas que puedan surgir al usar una cuenta de Automation. En las secciones siguientes se destacan los mensajes de error específicos y posibles soluciones para cada uno. Para obtener información general acerca de las cuentas de Automation, consulte [Creación de una cuenta de Azure](../automation-quickstart-create-account.md).

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Escenario: No se puede registrar el proveedor de recursos de Automation para las suscripciones

### <a name="issue"></a>Problema

Al trabajar con soluciones de administración en la cuenta de Automation, se encuentra con el siguiente error:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Causa

El proveedor de recursos de Automation no está registrado en la suscripción.

### <a name="resolution"></a>Solución

Para registrar el proveedor de recursos de Automation, realice los pasos siguientes en Azure Portal:

1. En el explorador, vaya a [Azure Portal](https://portal.azure.com).

2. Vaya a **Suscripciones** y seleccione su suscripción en la página Suscripciones.   

3. En **Configuración**, seleccione **Proveedores de recursos**.

4. En la lista de proveedores de recursos, compruebe que el proveedor de recursos de **Microsoft.Automation** esté registrado.

5. Si no aparece, registre el proveedor de **Microsoft.Automation** siguiendo los pasos descritos en [Resolución de errores del registro del proveedor de recursos](/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o no puede resolverlo, intente uno de los siguientes canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.