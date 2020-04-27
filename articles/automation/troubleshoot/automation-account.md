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
ms.openlocfilehash: 7b5e7171ac679384966e9dce79425cd1fa881c53
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679377"
---
# <a name="troubleshoot-the-automation-account"></a>Solucionar problemas de la cuenta de Automation

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

5. Si el proveedor no aparece en la lista, regístrelo como se describe en [Resolución de errores del registro del proveedor de recursos](/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece arriba o no puede resolverlo, intente obtener ayuda adicional a través de uno de los siguientes canales:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.