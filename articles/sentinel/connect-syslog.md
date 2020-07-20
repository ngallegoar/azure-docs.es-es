---
title: Conexión de datos de Syslog a Azure Sentinel | Microsoft Docs
description: Conecte cualquier dispositivo local que admita Syslog en Azure Sentinel mediante un agente en una máquina Linux entre el dispositivo y Sentinel. 
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 65c4e5d9e0752379541063c8a80a4316196ad7c3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565379"
---
# <a name="connect-your-external-solution-using-syslog"></a>Conectar su solución externa mediante Syslog

Puede conectar a Azure Sentinel cualquier dispositivo local que admita Syslog. Esto se realiza a través de un agente basado en una máquina Linux entre el dispositivo y Azure Sentinel. Si la máquina Linux está en Azure, puede transmitir los registros desde el dispositivo o la aplicación a un área de trabajo dedicada que cree en Azure y conectarla. Si la máquina Linux no está en Azure, puede transmitir los registros desde el dispositivo a una máquina virtual local dedicada o a un equipo donde tenga instalado el Agente para Linux. 

> [!NOTE]
> Si el dispositivo admite CEF de Syslog, la conexión será más completa, así que conviene elegir esta opción y seguir las instrucciones en [Conectar su solución externa mediante Syslog](connect-common-event-format.md).

## <a name="how-it-works"></a>Funcionamiento

Syslog es un protocolo de registro de eventos que es común a Linux. Las aplicaciones envían mensajes que pueden almacenarse en la máquina local o entregarse a un recopilador de Syslog. Al instalar el agente de Log Analytics para Linux, este configura el demonio Syslog local para que reenvíe mensajes al agente. En ese momento, el agente envía el mensaje a Azure Monitor, donde se crea un registro correspondiente.

Para más información, consulte [Orígenes de datos de Syslog en Azure Monitor](../azure-monitor/platform/data-sources-syslog.md).

> [!NOTE]
> - El agente puede recopilar registros de varios orígenes, pero debe estar instalado en el equipo proxy dedicado.
> - Si quiere admitir conectores para CEF y Syslog en la misma máquina virtual, siga estos pasos para evitar la duplicación de datos:
>    1. Siga las instrucciones para [conectar su CEF](connect-common-event-format.md).
>    2. Para conectar los datos de Syslog, vaya a **Configuración** > **Configuración del área de trabajo** > **Configuración avanzada** > **Datos** > **Syslog** y establezca las instalaciones y sus prioridades para que no sean las mismas instalaciones y propiedades que se usaron en la configuración de CEF. <br></br>Si selecciona **Aplicar la configuración siguiente a mis máquinas**, se aplica esta configuración a todas las máquinas virtuales conectadas a esta área de trabajo.


## <a name="connect-your-syslog-appliance"></a>Conectar el dispositivo de Syslog

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, seleccione el conector de **Syslog**.

2. En la hoja **Syslog**, seleccione **Open connector page** (Abrir página del conector).

3. Instale el agente de Linux:
    
    - Si la máquina virtual Linux se encuentra en Azure, seleccione **Download and install agent on Azure Linux virtual machine** (Descargar e instalar el agente en una máquina virtual Linux de Azure). En la hoja **Máquinas virtuales**, seleccione las máquinas virtuales en las que desea instalar el agente y, a continuación, haga clic en **Conectar**.
    - Si la máquina Linux no está en Azure, seleccione **Download and install agent on Linux non-Azure machine** (Descargar e instalar el agente en una máquina Linux que no es de Azure). En la hoja **Agente Directo**, copie el comando de **DESCARGAR E INCORPORAR AGENT PARA LINUX** y ejecútelo en el equipo. 
    
   > [!NOTE]
   > Asegúrese de que configura los valores de seguridad para estos equipos de acuerdo con la directiva de seguridad de su organización. Por ejemplo, puede configurar los valores de red para que se alineen con la directiva de seguridad de red de la organización y cambiar los puertos y protocolos del demonio para que se adapten a sus requisitos de seguridad.

4. Seleccione **Open your workspace advanced settings configuration** (Abrir la configuración de las opciones avanzadas del área de trabajo).

5. En la hoja **Configuración avanzada**, seleccione **Datos** > **Syslog**. A continuación, agregue los recursos del conector que se van a recopilar.
    
    Agregue los recursos que su dispositivo de Syslog incluye en sus encabezados de registro. Puede ver esta configuración en el dispositivo de Syslog en **Syslog-d** en la carpeta `/etc/rsyslog.d/security-config-omsagent.conf`, y en **r-Syslog** de `/etc/syslog-ng/security-config-omsagent.conf`.
    
    Si desea usar la detección de inicio de sesión de SSH anómalo con los datos que recopila, agregue **auth** y **authpriv**. Consulte la [siguiente sección](#configure-the-syslog-connector-for-anomalous-ssh-login-detection) para conocer más detalles.

6. Cuando haya agregado todos los recursos que desea supervisar y ajustado las opciones de gravedad de cada uno, active la casilla **Aplicar la configuración siguiente a mis máquinas**.

7. Seleccione **Guardar**. 

8. En el dispositivo de Syslog, asegúrese de que está enviando los recursos que ha especificado.

9. Para usar el esquema correspondiente en Azure Monitor para encontrar registros de Syslog, busque **Syslog**.

10. Puede usar la función de Kusto que se describe en [Uso de funciones en consultas de registros de Azure Monitor](../azure-monitor/log-query/functions.md) para analizar los mensajes de Syslog. Después, puede guardarlos como una nueva función de Log Analytics para usar la función como un nuevo tipo de datos.

### <a name="configure-the-syslog-connector-for-anomalous-ssh-login-detection"></a>Configuración del conector de Syslog para la detección de inicios de sesión de SSH anómalos

> [!IMPORTANT]
> La detección de inicios de sesión de SSH anómalos se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel puede aplicar aprendizaje automático (ML) a los datos de Syslog para identificar una actividad de inicio de sesión de Secure Shell anómala (SSH). Los escenarios incluyen:

- Viaje imposible: cuando se producen dos eventos de inicio de sesión correctos desde dos ubicaciones que son imposibles de alcanzar en el período de tiempo de los dos eventos de inicio de sesión.
- Ubicación inesperada: la ubicación desde donde se produjo un evento de inicio de sesión correcto es sospechosa. Por ejemplo, la ubicación no se ha visualizado recientemente.
 
Esta detección requiere una configuración específica del conector de datos de Syslog: 

1. En el paso 5 del procedimiento anterior, asegúrese de que **auth** y **authpriv** estén seleccionados como recursos para supervisar. Mantenga la configuración predeterminada de las opciones de gravedad, de modo que todas estén seleccionadas. Por ejemplo:
    
    > [!div class="mx-imgBorder"]
    > ![Recursos necesarios para la detección de inicios de sesión SSH anómalos](./media/connect-syslog/facilities-ssh-detection.png)

2. Deje tiempo suficiente para que se recopile la información de Syslog. A continuación, vaya a **Azure Sentinel: Registros** y copie y pegue la siguiente consulta:
    
        Syslog |  where Facility in ("authpriv","auth")| extend c = extract( "Accepted\\s(publickey|password|keyboard-interactive/pam)\\sfor ([^\\s]+)",1,SyslogMessage)| where isnotempty(c) | count 
    
    Cambie el **intervalo de tiempo** si es necesario y seleccione **Ejecutar**.
    
    Si el recuento resultante es cero, confirme la configuración del conector y que los equipos supervisados tienen una actividad de inicio de sesión correcta durante el período de tiempo que especificó para la consulta.
    
    Si el recuento resultante es mayor que cero, los datos de Syslog son adecuados para la detección de inicios de sesión de SSH anómalos. Puede habilitar esta detección en **Análisis** >  **Rule templates (Plantillas de reglas)**  >  **(Versión preliminar) Anomalous SSH Login Detection** (Detección de inicios de sesión de SSH anómalos).

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos locales de Syslog a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.

