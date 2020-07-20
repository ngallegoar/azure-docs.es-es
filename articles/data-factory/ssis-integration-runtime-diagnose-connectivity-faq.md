---
title: Solución de problemas de diagnóstico de conectividad en SSIS Integration Runtime
description: En este artículo se proporciona orientación para solucionar problemas de diagnóstico de conectividad en SSIS Integration Runtime
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: meiyl
author: meiyl
ms.reviewer: sawinark
manager: yidetu
ms.date: 06/07/2020
ms.openlocfilehash: 8e520048a6067f134e847953f4f4aa0598d9926e
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86172478"
---
# <a name="troubleshoot-diagnose-connectivity-in-the-ssis-integration-runtime"></a>Solución de problemas de diagnóstico de conectividad en SSIS Integration Runtime

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Si encuentra problemas de conectividad al ejecutar paquetes SSIS en SSIS Integration Runtime, especialmente si en su instancia de SSIS Integration Runtime se ha unido a Azure Virtual Network. Puede intentar realizar un diagnóstico automático de los problemas mediante el uso de esta característica de diagnóstico de conectividad en la página supervisión de SSIS Integration Runtime del portal de Azure Data Factory. 

 ![Página Supervisión: diagnosticar la conectividad](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-diagnose-connectivity.png) ![Página Supervisión: conexión de prueba](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-test-connection.png)
 
En este artículo se incluyen los errores más comunes que puede encontrar al probar la conexión en SSIS Integration Runtime. Se describen las posibles causas y acciones para resolver los errores. 

## <a name="common-errors-potential-causes-and-recommendation-solutions"></a>Errores comunes, posibles causas y soluciones de recomendación

### <a name="error-code-invalidinput"></a>Código de error: InvalidInput.
* **Mensaje de error**: compruebe que la entrada es correcta.
* **Causas posibles:** la entrada es incorrecta.
* **Recomendación:** consulte la entrada.

### <a name="error-code-firewallornetworkissue"></a>Código de error: FirewallOrNetworkIssue.
* **Mensaje de error**: compruebe que este puerto está abierto en el firewall, el servidor o el grupo de seguridad de red y que la red es estable.
* **Causas posibles:** 
  * El servidor no abre este puerto.
  * El grupo de seguridad de red tiene denegado el tráfico de salida en este puerto.
  * La solicitud NVA, el firewall de Azure o el firewall local no abren este puerto.
* **Recomendación:** 
  * Abra este puerto en el servidor.
  * Actualice el grupo de seguridad de red para permitir el tráfico de salida en este puerto.
  * Abra este puerto en la solicitud NVA, el firewall de Azure o el firewall local.

### <a name="error-code-misconfigureddnssettings"></a>Código de error: MisconfiguredDnsSettings.
* **Mensaje de error**: si usa su propio servidor DNS en la red virtual unida por Azure-SSIS IR, compruebe que puede resolver el nombre de host.
* **Causas posibles:** 
  *  El problema de DNS personalizado.
  *  No usa un nombre de dominio completo (FQDN) para el nombre de host privado.
* **Recomendación:** 
  *  Corrija el problema de DNS personalizado para asegurarse de que puede resolver el nombre de host.
  *  Use un nombre de dominio completo (FQDN), como <su_servidor_privado>.contoso.com en lugar de <su_servidor_privado>, ya que Azure-SSIS IR no anexará automáticamente su sufijo DNS.

### <a name="error-code-servernotallowremoteconenction"></a>Código de error: ServerNotAllowRemoteConnection.
* **Mensaje de error**: compruebe que el servidor permite las conexiones TCP remotas a través de este puerto.
* **Causas posibles:** 
  *  El firewall del servidor no permite conexiones TCP remotas.
  *  El servidor no está en línea.
* **Recomendación:** 
  *  Permita las conexiones TCP remotas en el firewall del servidor.
  *  Inicie el servidor.
   
### <a name="error-code-misconfigurednsgsettings"></a>Código de error: MisconfiguredNsgSettings.
* **Mensaje de error**: compruebe que el NSG de la red virtual permite el tráfico de salida a través de este puerto. Si usa Azure ExpressRoute o una UDR, compruebe que este puerto está abierto en el firewall o el servidor.
* **Causas posibles:** 
  *  El grupo de seguridad de red tiene denegado el tráfico de salida en este puerto.
  *  La solicitud NVA, el firewall de Azure o el firewall local no abren este puerto.
* **Recomendación:** 
  *  Actualice el grupo de seguridad de red para permitir el tráfico de salida en este puerto.
  *  Abra este puerto en la solicitud NVA, el firewall de Azure o el firewall local.

### <a name="error-code-genericissues"></a>Código de error: GenericIssues.
* **Mensaje de error**: error en la conexión de prueba debido a problemas genéricos.
* **Causas posibles:** la conexión de prueba encontró un problema temporal general.
* **Recomendación:** vuelva a intentar la conexión de prueba más tarde. Si el reintento no sirve, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.


### <a name="error-code-pspingexecutiontimeout"></a>Código de error: PSPingExecutionTimeout.
* **Mensaje de error**: se agotó el tiempo de espera de la conexión de prueba. Inténtelo de nuevo más tarde.
* **Causas posibles:** se agotó el tiempo de espera de la conectividad de prueba.
* **Recomendación:** vuelva a intentar la conexión de prueba más tarde. Si el reintento no sirve, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.

### <a name="error-code-networkinstable"></a>Código de error: NetworkInstable.
* **Mensaje de error**: la conexión de prueba se realizó correctamente, pero con irregularidades, debido a la inestabilidad de la red.
* **Causas posibles:** problema de red transitorio.
* **Recomendación:** compruebe si la red del servidor o el firewall es estable.

## <a name="next-steps"></a>Pasos siguientes

- Implemente los paquetes. Para más información, consulte [Implementar un proyecto de SSIS en Azure con SSMS](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms).
- Ejecute los paquetes. Para más información, consulte [Ejecutar paquetes SSIS en Azure con SSMS](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms).
- Programe los paquetes. Para más información, vea [Programar paquetes SSIS en Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15).

