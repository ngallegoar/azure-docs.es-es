---
title: Detección avanzada de ataques de varias fases en Azure Sentinel
description: Use la tecnología de fusión de Azure Sentinel para reducir el exceso de alertas y crear incidentes útiles basados en la detección avanzada de ataques de varias fases.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 87ca322cbdfdd8a53a3ecefcb120a961ea1bb936
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "77587930"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Detección avanzada de ataques de varias fases en Azure Sentinel


> [!IMPORTANT]
> Algunas características de Fusion de Azure Sentinel están actualmente en versión preliminar pública.
> Estas características se ofrecen sin Acuerdo de Nivel de Servicio y no es aconsejable usarlas en cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Mediante el uso de la tecnología de fusión basada en el aprendizaje automático, Azure Sentinel puede detectar automáticamente ataques de varias fases mediante la combinación de los comportamientos anómalos y las actividades sospechosas que se observan en varias fases de la cadena de eliminación. De esta forma, Azure Sentinel genera incidentes que, de otro modo, serían muy difíciles de detectar. Estos incidentes incluyen dos o más alertas o actividades. Por diseño, estos incidentes tienen poco volumen, alta fidelidad y alta gravedad.

Cuando se personaliza para adaptarla a su entorno, esta detección no solo reduce las tasas de falsos positivos, sino que también puede detectar ataques con información limitada o que falta.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Configuración de la detección avanzada de ataques de varias fases

Esta detección está habilitada de forma predeterminada en Azure Sentinel. Para comprobar el estado, o para deshabilitar la detección quizás porque está usando una solución alternativa para crear incidentes basados en varias alertas, siga estas instrucciones:

1. Si aún no lo ha hecho, inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Vaya a **Azure Sentinel** > **Configuración** > **Análisis**.

3. Seleccione **Reglas activas** y busque **Detección avanzada de ataques de varias fases** en la columna **NOMBRE**. Compruebe la columna **ESTADO** para confirmar si esta detección está habilitada o deshabilitada.

4. Para cambiar el estado, seleccione esta entrada y, en la hoja **Detección avanzada de ataques de varias fases**, seleccione **Editar**.

5. En la hoja **Asistente para creación de reglas**, se selecciona automáticamente el cambio de estado, así que seleccione **Siguiente: revisar** y, a continuación, **Guardar**. 

Las plantillas de reglas no son aplicables para la detección avanzada de ataques de varias fases.

> [!NOTE]
> Actualmente, Azure Sentinel usa 30 días de datos del historial para entrenar los sistemas de aprendizaje automático. Estos datos siempre se cifran mediante las claves de Microsoft cuando pasan por la canalización de aprendizaje automático. Sin embargo, los datos de entrenamiento no se cifran mediante [claves administradas por el cliente (CMK)](customer-managed-keys.md) si CMK se ha habilitado en el área de trabajo de Azure Sentinel. Para optar por no recibir Fusion, vaya a **Azure Sentinel** \> **Configuration** \> **Analytics \>Active rules \> Advanced Multistage Attack Detection** (Azure Sentinel > Configuración > Analytics > Reglas activas > Detección avanzada de ataques en varias fases) y, en la columna **Status** (Estado), seleccione **Disable** (Deshabilitar).

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Fusión mediante Palo Alto Networks y ATP de Microsoft Defender

Estos escenarios combinan dos de los registros fundamentales que utilizan los analistas de seguridad: registros de firewall de Palo Alto Networks y registros de detección de punto de conexión de Microsoft Defender ATP. En todos los escenarios que se enumeran a continuación, se detecta una actividad sospechosa en el punto de conexión que implica una dirección IP externa y, a continuación, un tráfico anómalo desde la dirección IP externa al firewall. En los registros de Palo Alto, Azure Sentinel se centra en [registros de amenazas](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs) y el tráfico se considera sospechoso cuando se permiten amenazas (datos sospechosos, archivos, inundaciones, paquetes, exámenes, spyware, direcciones URL, virus, puntos vulnerables, virus Wildfire y malware Wildfire).

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Solicitud de red a un servicio de anonimización de TOR, seguida por un tráfico anómalo marcado por el firewall de Palo Alto Networks.

En este escenario, Azure Sentinel primero detecta la alerta de que Protección contra amenazas avanzada de Microsoft Defender ha detectado una solicitud de red para un servicio de anonimización de TOR que provoca una actividad anómala. Esta se ha iniciado en la cuenta {account name} con el identificador de SID {SID} a las {time}. La dirección IP saliente para la conexión era {IndividualIp}.
Después, el firewall de Palo Alto Networks detectó una actividad inicial a las {TimeGenerated}. Esto indica que ha entrado tráfico malintencionado entró en la red. La dirección IP de destino del tráfico de red es {DestinationIP}.

Este escenario está actualmente en versión preliminar pública.


### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell estableció una conexión de red sospechosa, seguida por tráfico anómalo marcado por el firewall de Palo Alto Networks.

En este escenario, Azure Sentinel primero detecta la alerta de que Protección contra amenazas avanzada de Microsoft Defender ha detectado que PowerShell estableció una conexión de red sospechosa que provocaba una actividad anómala detectada por un firewall de Palo Alto Network. Esta la ha iniciado la cuenta {account name} con el identificador de SID {SID} a las {time}. La dirección IP saliente para la conexión era {IndividualIp}. Después, el firewall de Palo Alto Networks detectó una actividad inicial a las {TimeGenerated}. Esto indica que ha entrado tráfico malintencionado en la red. La dirección IP de destino del tráfico de red es {DestinationIP}.

Este escenario está actualmente en versión preliminar pública.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Una conexión saliente a IP con un historial de intentos de acceso no autorizados, seguida por tráfico anómalo marcado por el firewall de Palo Alto Networks

En este escenario, Azure Sentinel detecta la alerta de que Protección contra amenazas avanzada de Microsoft Defender ha detectado una conexión saliente a una dirección IP con un historial de intentos de acceso no autorizado que conducen que el firewall de Palo Alto Networks detecte una actividad anómala. Esta la ha iniciado la cuenta {account name} con el identificador de SID {SID} a las {time}. La dirección IP saliente para la conexión era {IndividualIp}. Después de esto, el firewall de Palo Alto Networks detectó una actividad inicial a las {TimeGenerated}. Esto indica que ha entrado tráfico malintencionado en la red. La dirección IP de destino del tráfico de red es {DestinationIP}.

Este escenario está actualmente en versión preliminar pública.



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Fusión mediante Identity Protection y Microsoft Cloud App Security

En la detección avanzada de ataques de varias fases, Azure Sentinel admite los siguientes escenarios que combinan eventos anómalos de Azure Active Directory Identity Protection y Microsoft Cloud App Security:

- [Viaje imposible a una ubicación inusual seguido de una actividad anómala de Office 365](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión para una ubicación desconocida seguida de una actividad anómala de Office 365](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión desde un dispositivo infectado seguida de una actividad anómala de Office 365](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión desde una dirección IP anónima seguida de una actividad anómala de Office 365](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión de un usuario con credenciales filtradas seguida de una actividad anómala de Office 365](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

Debe tener el [conector de datos de Azure AD Identity Protection](connect-azure-ad-identity-protection.md) y los conectores de [Cloud App Security](connect-cloud-app-security.md) configurados.

En las descripciones siguientes, Azure Sentinel mostrará el valor real de los datos que se representan en esta página como variables entre corchetes. Por ejemplo, el nombre para mostrar real de una cuenta en lugar de \<*account name*> y el número real en lugar de \<*number*>.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Viaje imposible a una ubicación inusual seguido de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de viaje imposible a ubicaciones inusuales de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Viaje imposible a ubicaciones inusuales que conducen a la filtración de los buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual, seguido de una regla de reenvío de bandeja de entrada sospechosa que se ha establecido en la bandeja de entrada de un usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*account name*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía todos los correos electrónicos entrantes a la dirección externa \<*email address*>.

- **Viaje imposible a ubicaciones inusuales que conducen a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual.
    
    Luego, la cuenta \<*account name*> ha realizado más de \<*number*> actividades de administración en una única sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> en \<*location*>, una ubicación inusual. 
    
    Luego, la cuenta \<*account name*> ha eliminado \<*number of*> archivos únicos en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    Luego, la cuenta \<*account name*> ha descargado más de \<*number of*> archivos únicos en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a la suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    Luego, la cuenta \<*account name*> ha realizado una cantidad inusual (\<*number of activities*>) de actividades de suplantación en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    Luego, la cuenta \<*account name*> ha compartido más de \<*number of*> archivos únicos en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    Luego, la cuenta \<*account name*> ha cargado \<*number of*> archivos y ha eliminado un total de \<*number of*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión para una ubicación desconocida seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión para ubicaciones desconocidas de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a la filtración de buzones de Exchange Online**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida, seguido de una regla de reenvío de bandeja de entrada sospechosa que se ha establecido en la bandeja de entrada de un usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*account name*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía todos los correos electrónicos entrantes a la dirección externa \<*email address*>. 

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida. 
    
    Luego, la cuenta \<*account name*> ha realizado más de \<*number of*> actividades de administración en una única sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida. 
    
    Luego, la cuenta \<*account name*> ha eliminado \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida. 
    
    Luego, la cuenta \<*account name*> ha descargado más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida.
    
    Luego, la cuenta \<*account name*> ha suplantado a más de \<*number of*> cuentas diferentes en una única sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida. 
    
    Luego, la cuenta \<*account name*> ha compartido más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conducen a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde \<*location*>, una ubicación desconocida. 
    
    Luego, la cuenta \<*account name*> ha cargado \<*number of*> archivos y ha eliminado un total de \<*number of*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión desde un dispositivo infectado seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión desde dispositivos infectados de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a la filtración de buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware, seguido de una regla de reenvío de bandeja de entrada sospechosa que se ha establecido en la bandeja de entrada de un usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*account name*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía todos los correos electrónicos entrantes a la dirección externa \<*email address*>. 

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware.
    
    Luego, la cuenta \<*account name*> ha realizado más de \<*number of*> actividades de administración en una única sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware. 
    
    Luego, la cuenta \<*account name*> ha eliminado \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware. 
    
    Luego, la cuenta \<*account name*> ha descargado más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware. 
    
    Luego, la cuenta \<*account name*> ha suplantado a más de \<*number of*> cuentas diferentes en una única sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware. 
    
    Luego, la cuenta \<*account name*> ha compartido más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware. 
    
    Luego, la cuenta \<*account name*> ha cargado \<*number of*> archivos y ha eliminado un total de \<*number of*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión desde una dirección IP anónima seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión desde una dirección IP anónima de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a la filtración de buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>, seguido de una regla de reenvío de bandeja de entrada sospechosa que se ha establecido en la bandeja de entrada de un usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*account name*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía todos los correos electrónicos entrantes a la dirección externa \<*email address*>. 

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>. 
    
    Luego, la cuenta \<*account name*> ha realizado más de \<*number of*> actividades de administración en una única sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>. 
    
    Luego, la cuenta \<*account name*> ha eliminado \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>. 
    
    Luego, la cuenta \<*account name*> ha descargado más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>. 
    
    Luego, la cuenta \<*account name*> ha suplantado a más de \<*number of*> cuentas diferentes en una única sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>. 
    
    Luego, la cuenta \<*account name*> ha compartido más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde una dirección IP de proxy anónima \<*IP address*>. 
    
    Luego, la cuenta \<*account name*> ha cargado \<*number of*> archivos y ha eliminado un total de \<*number of*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión de un usuario con credenciales filtradas seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión de un usuario con credenciales filtradas de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a la filtración de buzones de Office 365**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas, seguido de una regla de reenvío de bandeja de entrada sospechosa que se ha establecido en la bandeja de entrada de un usuario. 
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*account name*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía todos los correos electrónicos entrantes a la dirección externa \<*email address*>. 

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas.
    
    Luego, la cuenta \<*account name*> ha realizado más de \<*number of*> actividades de administración en una única sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas.
    
    Luego, la cuenta \<*account name*> ha eliminado \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas.
    
    Luego, la cuenta \<*account name*> ha descargado más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a la suplantación de Office 365**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas. 
    
    Luego, la cuenta \<*account name*> ha suplantado a más de \<*number of*> cuentas diferentes en una única sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas.
    
    Luego, la cuenta \<*account name*> ha compartido más de \<*number of*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*account name*> ha usado credenciales filtradas. 
    
    Luego, la cuenta \<*account name*> ha cargado \<*number of*> archivos y ha eliminado un total de \<*number of*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce más detalles sobre la detección avanzada de ataques de varias fases, puede que le interese el siguiente inicio rápido para aprender a obtener visibilidad sobre sus datos y a detectar posibles amenazas: [Introducción a Azure Sentinel](quickstart-get-visibility.md).

Si está preparado para investigar los incidentes que se han creado, consulte el siguiente tutorial: [Investigación de incidentes con Azure Sentinel](tutorial-investigate-cases.md).

