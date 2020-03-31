---
title: 'Protección contra amenazas avanzada de Microsoft Defender: Azure Security Center'
description: En este documento se presenta la integración entre Azure Security Center y la Protección contra amenazas avanzada de Microsoft Defender.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: memildin
ms.openlocfilehash: 13852acb39a420e2f0da84e18bef4df823c1fa78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206272"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Protección contra amenazas avanzada de Microsoft Defender con Azure Security Center

Azure Security Center va a ampliar su oferta de plataformas de protección de cargas de trabajo en la nube mediante la integración con la [Protección contra amenazas avanzada de Microsoft Defender](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp) (ATP).
Este cambio ofrece funcionalidades completas de detección y respuesta (EDR) de puntos de conexión. Gracias a la integración de ATP de Microsoft Defender, podrá detectar anomalías. También puede detectar y responder a ataques avanzados en los puntos de conexión de servidor que Azure Security Center supervisa.

## <a name="microsoft-defender-atp-features-in-security-center"></a>Características de ATP de Microsoft Defender en Security Center

Cuando usa ATP de Microsoft Defender, obtiene lo siguiente:

- **Sensores de detección de brechas de próxima avanzados**: los sensores de ATP de Microsoft Defender de los servidores Windows recopilan una amplia variedad de señales de comportamiento.

- **Detección de brechas basadas en la nube y el análisis**: ATP de Microsoft Defender se adapta rápidamente a las amenazas cambiantes. Utiliza análisis avanzados y macrodatos. ATP de Microsoft Defender se amplía gracias a la eficacia de Intelligent Security Graph con señales a través de Windows, Azure y Office para detectar amenazas desconocidas. Proporciona alertas que requieren intervención y le permite responder rápidamente.

- **Información sobre amenazas**: ATP de Microsoft Defender crea alertas cuando identifica las herramientas, las técnicas y los procedimientos de los atacantes. Usa datos generados por los equipos de seguridad y los buscadores de amenazas de Microsoft, con la ayuda de mecanismos de inteligencia que aportan los partners.

Las siguientes funcionalidades ya están disponibles en Azure Security Center:

- **Incorporación automatizada**: el sensor de ATP de Microsoft Defender se habilita automáticamente en los servidores Windows que se incorporan a Azure Security Center.

- **Panel único**: la consola de Azure Security Center muestra las alertas de ATP de Microsoft Defender.

- **Investigación detallada de las máquinas**: los clientes de Azure Security Center pueden acceder a la consola de ATP de Microsoft Defender para llevar a cabo una investigación detallada destinada a descubrir el ámbito de una brecha.

![Azure Security Center muestra una lista de alertas e información general sobre cada alerta](media/security-center-wdatp/image1.png)

Para obtener más información, use ATP de Microsoft Defender. Asimismo, ATP de Microsoft Defender proporciona información adicional como el árbol de procesos de alerta y el grafo de incidentes. También puede ver una escala de tiempo de la máquina detallada que muestra cada comportamiento durante un período histórico de hasta seis meses.

![Página de ATP de Microsoft Defender con información detallada sobre una alerta](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Compatibilidad con plataformas

ATP de Microsoft Defender en Security Center admite la detección en Windows Server 2016, 2012 R2 y 2008 R2 SP1, en el caso de las máquinas virtuales de Azure, necesita una suscripción de nivel estándar y, para las máquinas virtuales que no son de Azure, necesita el nivel estándar solo en el nivel de área de trabajo.

> [!NOTE]
> Al usar Azure Security Center para supervisar los servidores, se crea automáticamente un inquilino de ATP de Microsoft Defender y los datos de este se almacenan en Europa de forma predeterminada. Si necesita mover los datos a otra ubicación, deberá ponerse en contacto con el Soporte técnico de Microsoft para restablecer el inquilino. La supervisión del punto de conexión del servidor que usa esta integración se ha deshabilitado para los clientes de Office 365 GCC.

## <a name="onboarding-servers-to-security-center"></a>Incorporación de servidores a Security Center 

Para incorporar servidores a Security Center, haga clic en **Ir a Azure Security Center para incorporar los servidores** desde la incorporación del servidor de ATP de Microsoft Defender.

1. En el área de **incorporación**, seleccione o cree el área de trabajo en la que se almacenarán los datos. <br>
2. Si no puede ver todas las áreas de trabajo, puede deberse a una falta de permisos. Asegúrese de que el área de trabajo se establece en el nivel Estándar de Azure Security. Para obtener más información, consulte [Actualización al nivel Estándar de Security Center para mejorar la seguridad](security-center-pricing.md).
    
3. Seleccione **Agregar servidores** para ver instrucciones acerca de la instalación de Microsoft Monitoring Agent. 

4. Después de la incorporación, puede supervisar las máquinas en **Procesos y aplicaciones**.

   ![Incorporación de equipos](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>Habilitar la integración con ATP de Microsoft Defender

Para comprobar si la integración de ATP de Microsoft Defender está habilitada, seleccione **Security Center** > **Pricing & settings** (Precios y configuración) > y haga clic en su suscripción.
Aquí puede ver las integraciones habilitadas actualmente.

  ![Página de configuración de la detección de amenazas de Azure Security Center con la integración de ATP de Microsoft Defender habilitada](media/security-center-wdatp/enable-integrations.png)

- Si ya ha incorporado los servidores al nivel estándar de Azure Security Center, no tiene que realizar ninguna otra acción. Azure Security Center incorporará automáticamente los servidores de ATP de Microsoft Defender. Este proceso de incorporación puede tardar hasta 24 horas.

- Si nunca ha incorporado los servidores al nivel estándar de Azure Security Center, incorpórelos a Azure Security Center como de costumbre.

- Si ha incorporado los servidores mediante ATP de Microsoft Defender:
  - Consulte la documentación para obtener instrucciones sobre [cómo retirar las máquinas de servidor](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Incorpore estos servidores a Azure Security Center.

## <a name="access-to-the-microsoft-defender-atp-portal"></a>Acceso al portal de ATP de Microsoft Defender

Siga las instrucciones de [Asignar acceso de usuario al portal](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

## <a name="set-the-firewall-configuration"></a>Establecimiento de la configuración del firewall

Si tiene un servidor proxy o firewall que bloquea el tráfico anónimo, como un sensor de ATP de Microsoft Defender que se conecta desde el contexto del sistema, asegúrese de que el tráfico anónimo se permita. Siga las instrucciones para [habilitar el acceso a direcciones URL del servicio de ATP de Microsoft Defender en el servidor proxy](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Prueba de la característica

Para generar una alerta de prueba inofensiva de ATP de Microsoft Defender:

1. Cree una carpeta "C:\test-MDATP-test".

1. Use Escritorio remoto para acceder a una máquina virtual de Windows Server 2012 R2 o a una máquina virtual de Windows Server 2016. Abra una ventana de línea comandos.

1. En el símbolo del sistema, copie el siguiente comando y ejecútelo. La ventana del símbolo del sistema se cerrará automáticamente.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```

   ![Una ventana de símbolo del sistema con el comando anterior](media/security-center-wdatp/image4.jpeg)

3. Si el comando se ejecuta correctamente, verá una nueva alerta en el panel de Azure Security Center y el portal de ATP de Microsoft Defender. Esta alerta puede tardar unos minutos en aparecer.

4. Para revisar la alerta en Security Center, vaya a **Security Alerts (Alertas de seguridad)**  > **Suspicious Powershell CommandLine (Línea de comandos de PowerShell sospechosa)** .

5. En la ventana de la investigación, seleccione el vínculo para ir al portal de ATP de Microsoft Defender.

## <a name="next-steps"></a>Pasos siguientes

- [Características y plataformas compatibles con Azure Security Center](security-center-os-coverage.md)
- [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
- [Administración de recomendaciones de seguridad en el Centro de seguridad de Azure](security-center-recommendations.md): aprenda cómo las recomendaciones le ayudarán a proteger los recursos de Azure.
- [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
