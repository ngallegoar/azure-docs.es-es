---
title: Acerca de Azure Kinect DK
description: Introducción a los servicios integrados y a las herramientas del kit de desarrollador (DK) de Azure Kinect.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.topic: overview
ms.date: 06/26/2019
keywords: azure, kinect, overview, dev kit, DK, device, depth, body tracking, speech, cognitive services, SDKs, SDK, firmware
ms.openlocfilehash: d188d37c0247aebb16f51b4404da81d469136468
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359660"
---
# <a name="about-azure-kinect-dk"></a>Acerca de Azure Kinect DK

 ![Azure Kinect DK](./media/index/device-image.jpg)

Azure Kinect DK es un kit de desarrollo con sensores de inteligencia artificial avanzados que proporcionan sofisticados modelos de voz y de visión artificial.  Kinect contiene un sensor de profundidad, una matriz de micrófonos espaciales con una cámara de vídeo y un sensor de orientación en un dispositivo pequeño todo en uno con varios modos, opciones y kits de desarrollo de software (SDK). Se puede comprar en [Microsoft Online Store](https://www.microsoft.com/p/azure-kinect-dk/8pp5vxmd9nhq).

El entorno de desarrollo de Azure Kinect DK consta de los siguientes SDK:

- SDK de Sensor para el acceso a dispositivos y sensores de bajo nivel.
- SDK de Body Tracking para realizar el seguimiento corporal en 3D.
- SDK de Speech Cognitive Services para habilitar el acceso mediante micrófono y los servicios de voz basados en la nube de Azure.

Además, Cognitive Vision Services se puede usar con la cámara RGB del dispositivo.

   ![Diagrama de los SDK de Azure Kinect](./media/quickstarts/sdk-diagram.jpg)

## <a name="azure-kinect-sensor-sdk"></a>SDK de Sensor de Azure Kinect

El SDK de Sensor de Azure Kinect proporciona acceso al sensor de bajo nivel a los sensores de hardware de Azure Kinect DK y a la configuración del dispositivo.

Para más información sobre el SDK de Sensor de Azure Kinect, consulte el artículo sobre el [SDK de Sensor](about-sensor-sdk.md).

### <a name="azure-kinect-sensor-sdk-features"></a>Características del SDK de Sensor de Azure Kinect

El SDK de Sensor tiene las siguientes características que funcionan una vez que se instalan y ejecutan en Azure Kinect DK:

- Control del modo y del acceso a la cámara de profundidad (un modo de IR pasivo y modos de profundidad de campo de vista ancho y estrecho) 
- Control y acceso a la cámara RGB (por ejemplo, exposición y balance de blancos) 
- Acceso al sensor de movimiento (giroscopio y acelerómetro) 
- Streaming de la cámara RGB de profundidad sincronizada con retraso configurable entre cámaras 
- Control de sincronización de dispositivos externos con compensación de retraso entre dispositivos configurable 
- Acceso a los metadatos de los fotogramas de cámara para la resolución de imagen, marca de tiempo, etc. 
- Acceso a datos de calibración del dispositivo 

### <a name="azure-kinect-sensor-sdk-tools"></a>Herramientas del SDK de Sensor de Azure Kinect

Las siguientes herramientas están disponibles en el SDK de Sensor:

- Un visor para supervisar los flujos de datos del dispositivo y configurar los diferentes modos.
- Una herramienta de grabación de los datos del sensor y una API de lector de reproducción que usa el formato de contenedor Matroska.
- Una herramienta de actualización del firmware de Azure Kinect DK.

## <a name="azure-kinect-body-tracking-sdk"></a>SDK de Body Tracking de Azure Kinect

El SDK de Body Tracking incluye una biblioteca de Windows y un runtime para realizar el seguimiento corporal en 3D cuando se usa con el hardware de Azure Kinect DK.

### <a name="azure-kinect-body-tracking-features"></a>Características de seguimiento corporal de Azure Kinect

Las siguientes características de seguimiento corporal están disponibles en el SDK que lo acompaña:

- Proporciona segmentación de cuerpos.
- Contiene un esqueleto anatómicamente correcto para cada cuerpo parcial o completo que se encuentre en el campo de visión.
- Proporciona una identidad única para cada cuerpo.
- Puede realizar un seguimiento corporal a lo largo del tiempo.

### <a name="azure-kinect-body-tracking-tools"></a>Herramientas de seguimiento corporal de Azure Kinect

- Body Tracker tiene un visor para hacer un seguimiento corporal en 3D.

## <a name="speech-cognitive-services-sdk"></a>SDK de Speech Cognitive Services

El SDK de voz habilita los servicios de voz conectados a Azure.

### <a name="speech-services"></a>Servicios de voz

- Voz a texto
- Traducción de voz
- Text-to-Speech

>[!NOTE]
>Azure Kinect DK no tiene altavoces.

Para más información, visite la [documentación del servicio de voz](../cognitive-services/speech-service/index.yml).

## <a name="vision-services"></a>Vision Services

Las siguientes instancias de [Azure Cognitive Vision Services](https://azure.microsoft.com/services/cognitive-services/directory/vision/) proporcionan servicios de Azure que pueden identificar y analizar contenido en imágenes y vídeos.

- [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/)
- [Face](https://azure.microsoft.com/services/cognitive-services/face/)
- [Video Indexer](https://azure.microsoft.com/services/media-services/video-indexer/)
- [Content Moderator](https://azure.microsoft.com/services/cognitive-services/content-moderator/)
- [Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)

Los servicios evolucionan y mejoran constantemente, así que no olvide comprobar regularmente si hay nuevas instancias de [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) que mejoren su aplicación. Si quiere ser el primero en conocer los nuevos servicios, consulte los [laboratorios de Cognitive Services](https://labs.cognitive.microsoft.com/).

## <a name="azure-kinect-hardware-requirements"></a>Requisitos de hardware de Azure Kinect

Azure Kinect DK integra la más reciente tecnología de sensores de Microsoft en un único accesorio conectado por USB. Para más información, consulte [Especificaciones de hardware de Azure Kinect DK](hardware-specification.md).

## <a name="next-steps"></a>Pasos siguientes

Ya tiene información general acerca de Azure Kinect DK. El siguiente paso consiste en empezar a configurarlo.

> [!div class="nextstepaction"]
>[Inicio rápido: Configuración de Azure Kinect DK](set-up-azure-kinect-dk.md)