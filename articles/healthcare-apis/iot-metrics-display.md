---
title: Visualización y configuración de las métricas de Azure IoT Connector for FHIR (versión preliminar)
description: En este artículo se muestra cómo mostrar y configurar las métricas de Azure IoT Connector for FHIR (versión preliminar).
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/29/2020
ms.author: jasteppe
ms.openlocfilehash: 76166fc5c525d36474a585179472e93b22dad647
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133446"
---
# <a name="view-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>Visualización y configuración de las métricas de Azure IoT Connector for FHIR (versión preliminar) 

En este artículo, aprenderá a ver y configurar las métricas de Azure IoT Connector for FHIR*. 

> [!TIP]
> Siga las instrucciones que aparecen en [Exportación de las métricas de Azure IoT Connector for FHIR (versión preliminar) mediante la configuración de diagnóstico](https://docs.microsoft.com/azure/healthcare-apis/iot-metrics-diagnostics-export) para aprender a configurar la exportación de los datos de métricas.

## <a name="view-metrics-for-azure-iot-connector-for-fhir-preview"></a>Visualización de las métricas de Azure IoT Connector for FHIR (versión preliminar)
1. Para ver las métricas de los conectores de IoT, seleccione el servicio Azure API for FHIR en Azure Portal. 

2. Vaya a **Métricas**. 

3. Seleccione la pestaña **Conector de IoT**.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-main.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-main.png"::: 

4. Seleccione un conector de IoT para ver sus métricas (por ejemplo, hay (4) conectores de IoT asociados con este servicio Azure API for FHIR).

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-connector.png" alt-text="IoT Connector2" lightbox="media/iot-metrics-display/iot-metrics-select-connector.png"::: 

> [!NOTE]
> La pestaña **Personalizado** permite crear combinaciones específicas de fecha y hora para ver las métricas de los conectores de IoT.

5. Seleccione el período de las métricas de conectores de IoT que se mostrará (por ejemplo, 1 hora, 24 horas, 7 días o Personalizado).

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-time.png" alt-text="IoT Connector3" lightbox="media/iot-metrics-display/iot-metrics-select-time.png"::: 
 
## <a name="metrics-types-for-azure-iot-connector-for-fhir-preview"></a>Tipos de métricas para Azure IoT Connector for FHIR (versión preliminar) 
Las métricas de conector de IoT que se muestran son las siguientes:

|Tipo de métrica|Propósitos de la métrica| 
|-----------|--------------|
|Número de mensajes entrantes|El número de mensajes entrantes recibidos sin procesar (por ejemplo, los eventos de dispositivo).|
|Número de mensajes normalizados|El número de mensajes normalizados.|
|Número de grupos de mensajes|El número de grupos que tienen mensajes agregados en la ventana de tiempo designada.|
|Latencia de la fase normalizada promedio|La latencia promedio de la fase normalizada. La fase normalizada consiste en realizar la normalización de los mensajes entrantes sin procesar.|
|Latencia de la fase de grupo promedio|La latencia promedio de la fase de grupo. La fase de grupo consiste en realizar el almacenamiento en búfer, la agregación y la agrupación en mensajes normalizados.| 
|Recuento total de errores|El número total de errores.| 

## <a name="focusing-and-configuring-azure-iot-connector-for-fhir-preview-metrics"></a>Enfoque y configuración de las métricas de Azure IoT Connector for FHIR (versión preliminar)
En este ejemplo, nos enfocaremos en la métrica **Número de mensajes entrantes**.

1. Seleccione un momento dado en el que quiera centrarse.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-focus.png" alt-text="IoT Connector4" lightbox="media/iot-metrics-display/iot-metrics-focus.png"::: 

2. En esta pantalla, puede **agregar métrica** , **agregar filtro** y **aplicar división** para realizar más personalizaciones. 

   :::image type="content" source="media/iot-metrics-display/iot-metrics-add-options.png" alt-text="IoT Connector5" lightbox="media/iot-metrics-display/iot-metrics-add-options.png"::: 

## <a name="conclusion"></a>Conclusión 
Tener acceso a las métricas del plano de datos es fundamental para las tareas de supervisión y solución de problemas.  Azure IoT Connector for FHIR lo ayuda a realizar estas acciones mediante las métricas. 

## <a name="next-steps"></a>Pasos siguientes

Consulte las preguntas más frecuentes sobre el conector de Azure IoT para FHIR

>[!div class="nextstepaction"]
>[P+ del conector de Azure IoT para FHIR](fhir-faq.md)

*En Azure Portal, el conector de Azure IoT para FHIR se conoce como conector de IoT (versión preliminar).

FHIR es la marca registrada de HL7 y se usa con su permiso.