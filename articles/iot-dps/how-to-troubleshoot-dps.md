---
title: Diagnóstico y solución de problemas de desconexiones con Azure IoT Hub DPS
description: Información sobre cómo diagnosticar y solucionar errores comunes de conectividad de dispositivos para Azure IoT Hub Device Provisioning Service (DPS)
author: xujing-ms
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: xujing
ms.openlocfilehash: 2d68314aab636180ff17e330c49c9859b8851b06
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94950693"
---
# <a name="troubleshooting-with-azure-iot-hub-device-provisioning-service"></a>Solución de problemas con Azure IoT Hub Device Provisioning Service

Los problemas de conectividad de los dispositivos IoT pueden ser difíciles de solucionar porque existen varios puntos de error posibles, por ejemplo, errores de registro o atestación, entre otros. En este artículo se proporcionan instrucciones sobre cómo detectar y solucionar problemas de conectividad a través de [Azure Monitor](../azure-monitor/overview.md).

## <a name="using-azure-monitor-to-view-metrics-and-set-up-alerts"></a>Uso de Azure Monitor para ver métricas y configurar alertas

En el procedimiento siguiente se describe cómo ver y configurar alertas en las métricas de IoT Hub Device Provisioning Service. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Vaya a IoT Hub Device Provisioning Service.

3. Seleccione **Métricas**.

4. Seleccione la métrica que desee. 
   <br />En estos momentos hay tres métricas para DPS:

    | Nombre de la métrica | Descripción |
    |-------|------------|
    | Intentos de atestación | Número de dispositivos que intentaron autenticarse con Device Provisioning Service|
    | Intentos de registro | Número de dispositivos que intentaron registrarse en IoT Hub después de una autenticación correcta|
    | Dispositivo asignado | Número de dispositivos que se asignaron correctamente a IoT Hub|

5. Seleccione el método de agregación deseado para crear una vista de la métrica. 

6. Para configurar la alerta de una métrica, seleccione **Nuevas reglas de alerta** en la parte superior derecha de la hoja de métricas; de igual modo, puede ir a la hoja **Alerta** y seleccionar **Nuevas reglas de alerta**.

7. Seleccione **Agregar condición** y, después, siga las indicaciones para seleccionar la métrica y el umbral deseados.

Para más información, consulte [¿Qué son las alertas clásicas en Microsoft Azure?](../azure-monitor/platform/alerts-overview.md)

## <a name="using-log-analytic-to-view-and-resolve-errors"></a>Uso de Log Analytics para ver y resolver errores

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Vaya a su instancia de IoT Hub.

3. Seleccione **Configuración de diagnóstico**.

4. Seleccione **Activar diagnósticos**.

5. Permite que se recopilen los registros deseados.

    | Nombre de registro | Descripción |
    |-------|------------|
    | DeviceOperations | Registros relacionados con eventos de conexión de dispositivo |
    | ServiceOperations | Registros de eventos relacionados con el uso del SDK de servicio (por ejemplo, crear o actualizar grupos de inscripción)|

6. Active **Enviar a Log Analytics** ([consulte los precios](https://azure.microsoft.com/pricing/details/log-analytics/)). 

7. Vaya a la pestaña **Registros** en Azure Portal, en el recurso Device Provisioning Service.

8. Haga clic en **Ejecutar** para ver los eventos recientes.

9. Si hay resultados, busque `OperationName`, `ResultType`, `ResultSignature` y `ResultDescription` (mensaje de error) para más detalles sobre el error.


## <a name="common-error-codes"></a>Códigos comunes de error
Use esta tabla para comprender y resolver errores comunes.

| Código de error| Descripción | Código de estado HTTP |
|-------|------------|------------|
| 400 | El cuerpo de la solicitud no es válido; por ejemplo, no se puede analizar o no se puede validar el objeto.| 400 Formato erróneo |
| 401 | No se puede validar el token de autorización; por ejemplo, ha expirado o no se aplica al URI de la solicitud. Este código de error también se devuelve a los dispositivos como parte del flujo de atestación de TPM. | 401 No autorizado|
| 404 | La instancia de Device Provisioning Service o un recurso (por ejemplo, una inscripción) no existe. |404 No encontrado |
| 412 | La ETag de la solicitud no coincide con la ETag del recurso existente, según las especificaciones de RFC7232. | 412 Error en la condición previa |
| 429 | El servicio está limitando las operaciones. Para conocer los límites de servicio específicos, consulte [Límites de IoT Hub Device Provisioning Service](../azure-resource-manager/management/azure-subscription-service-limits.md#iot-hub-device-provisioning-service-limits). | 429 Demasiadas solicitudes |
| 500 | Error interno. | Error de servidor interno 500|