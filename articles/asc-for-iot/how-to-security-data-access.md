---
title: Acceso a datos de seguridad y recomendaciones
description: Aprenda a obtener acceso a los datos de recomendaciones y alertas de seguridad al usar Azure Security Center for IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2020
ms.author: mlottner
ms.openlocfilehash: 77982bb8e001670ec1db57405e746cd849024278
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2020
ms.locfileid: "89504900"
---
# <a name="access-your-security-data"></a>Acceso a los datos de seguridad

Azure Security Center para IoT almacena los datos de recomendaciones y alertas de seguridad y los datos de seguridad sin procesar (si decide guardarlos) en el área de trabajo de Log Analytics.

## <a name="log-analytics"></a>Log Analytics

Para establecer el área de trabajo de Log Analytics que va a usar:

1. Abra el IoT Hub.
1. Haga clic en la hoja **Configuración** en la sección **Seguridad**.
1. Haga clic en **Recopilación de datos** y cambie la configuración del área de trabajo de Log Analytics.

Para acceder a las alertas y recomendaciones en el área de trabajo de Log Analytics después de la configuración:

1. Elija una alerta o una recomendación en Azure Security Center para IoT.
1. Haga clic en **further investigation** (Investigar más) y, luego, en **To see which devices have this alert click here and view the DeviceId column** (Para ver los dispositivos que tienen esta alerta, haga clic aquí y vea la columna DeviceId).

Para obtener más información sobre cómo consultar datos de Log Analytics, vea [Introducción a las consultas en Log Analytics](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Alertas de seguridad

Las alertas de seguridad se almacenan en la tabla _AzureSecurityOfThings.SecurityAlert_ del área de trabajo de Log Analytics configurada para la solución Azure Security Center para IoT.

Hemos incluido una serie de consultas útiles que le ayudarán a empezar a explorar las alertas de seguridad.

### <a name="sample-records"></a>Registros de ejemplo

Seleccione unos pocos registros aleatorios.

```
// Select a few random records
//
SecurityAlert
| project
    TimeGenerated,
    IoTHubId=ResourceId,
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

| TimeGenerated           | IoTHubId                                                                                                       | deviceId      | AlertSeverity | DisplayName                           | Descripción                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Brute force attack succeeded           | Se ha producido un ataque por fuerza bruta en el dispositivo.        |    { "Full Source Address": "[\"10.165.12.18:\"]", "User Names": "[\"\"]", "DeviceId": "IoT-Device-Linux" }                                                                       |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Successful local login on device      | Se ha detectado un inicio de sesión local correcto en el dispositivo.     | { "Remote Address": "?", "Remote Port": "", "Local Port": "", "Login Shell": "/bin/su", "Login Process Id": "28207", "User Name": "attacker", "DeviceId": "IoT-Device-Linux" } |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Failed local login attempt on device  | Se ha detectado un intento de inicio de sesión local con errores en el dispositivo. |    { "Remote Address": "?", "Remote Port": "", "Local Port": "", "Login Shell": "/bin/su", "Login Process Id": "22644", "User Name": "attacker", "DeviceId": "IoT-Device-Linux" } |

### <a name="device-summary"></a>Resumen de dispositivo

Obtenga un número de alertas de seguridad variadas detectadas la semana pasada, agrupadas por IoT Hub, dispositivo, gravedad de la alerta y tipo de alerta.

```
// Get the number of distinct security alerts detected in the last week, grouped by
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId,
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | deviceId      | AlertSeverity | DisplayName                           | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Brute force attack succeeded           | 9   |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Media        | Failed local login attempt on device  | 242 |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Successful local login on device      | 31  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Media        | Crypto Coin Miner                     | 4   |

### <a name="iot-hub-summary"></a>Resumen de IoT hub

Seleccione un número de dispositivos variados en los que se detectaron alertas la semana por IoT Hub, gravedad de la alerta y tipo de alerta.

```
// Select number of distinct devices which had alerts in the last week, by
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId,
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Alto          | Brute force attack succeeded           | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Media        | Failed local login attempt on device  | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Alto          | Successful local login on device      | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Media        | Crypto Coin Miner                     | 1          |

## <a name="security-recommendations"></a>Recomendaciones de seguridad

Las recomendaciones de seguridad se almacenan en la tabla _AzureSecurityOfThings.SecurityRecommendation_ del área de trabajo de Log Analytics configurada para la solución Azure Security Center para IoT.

Hemos incluido una serie de consultas útiles que le ayudarán a empezar a explorar las recomendaciones de seguridad.

### <a name="sample-records"></a>Registros de ejemplo

Seleccione unos pocos registros aleatorios.

```
// Select a few random records
//
SecurityRecommendation
| project
    TimeGenerated,
    IoTHubId=AssessedResourceId,
    DeviceId,
    RecommendationSeverity,
    RecommendationState,
    RecommendationDisplayName,
    Description,
    RecommendationAdditionalData
| take 2
```

| TimeGenerated | IoTHubId | deviceId | RecommendationSeverity | RecommendationState | RecommendationDisplayName | Descripción | RecommendationAdditionalData |
|---------------|----------|----------|------------------------|---------------------|---------------------------|-------------|------------------------------|
| 2019-03-22T10:21:06.060 |    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Media | Active | Se encontró una regla de firewall permisiva en la cadena de entrada | Se ha encontrado una regla en el firewall que contiene un patrón de permisos para una amplia gama de puertos o direcciones IP. | {"Rules":"[{\"SourceAddress\":\"\",\"SourcePort\":\"\",\"DestinationAddress\":\"\",\"DestinationPort\":\"1337\"}]"} |
| 2019-03-22T10:50:27.237 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Media | Active | Se encontró una regla de firewall permisiva en la cadena de entrada | Se ha encontrado una regla en el firewall que contiene un patrón de permisos para una amplia gama de puertos o direcciones IP. | {"Rules":"[{\"SourceAddress\":\"\",\"SourcePort\":\"\",\"DestinationAddress\":\"\",\"DestinationPort\":\"1337\"}]"} |

### <a name="device-summary"></a>Resumen de dispositivo

Obtenga un número de recomendaciones de seguridad activas variadas, agrupadas por IoT Hub, dispositivo, gravedad de la recomendación y tipo.

```
// Get the number of distinct active security recommendations, grouped by by
//   IoT hub, device, recommendation severity and type
//
SecurityRecommendation
| extend IoTHubId=AssessedResourceId
| summarize CurrentState=arg_max(RecommendationState, DiscoveredTimeUTC) by IoTHubId, DeviceId, RecommendationSeverity, RecommendationDisplayName
| where CurrentState == "Active"
| summarize Cnt=count() by IoTHubId, DeviceId, RecommendationSeverity
```

| IoTHubId                                                                                                       | deviceId      | RecommendationSeverity | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | 2   |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Media        | 1 |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | 1  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Media        | 4   |

## <a name="next-steps"></a>Pasos siguientes

- Lea la [información general](overview.md) sobre Azure Security Center para IoT.
- Obtenga información sobre la [arquitectura](architecture.md) de Azure Security Center para IoT.
- Comprenda y explore las [alertas de Azure Security Center para IoT](concept-security-alerts.md).
- Comprenda y explore las [recomendaciones de Azure Security Center para IoT](concept-recommendations.md).
