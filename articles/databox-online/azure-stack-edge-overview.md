---
title: Introducción a Microsoft Azure Stack Edge | Microsoft Docs
description: Describe Azure Stack Edge, una solución de almacenamiento que usa un dispositivo físico para las transferencias basadas en red a Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 11/04/2019
ms.author: alkohli
ms.openlocfilehash: c44d15e36135b8cf8c97484718d4600f8ba03521
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82564241"
---
# <a name="what-is-azure-stack-edge"></a>¿Qué es Azure Stack Edge?

[!INCLUDE [data-box-edge-gateway-rename-note](../../includes/data-box-edge-gateway-rename-note.md)]

Azure Stack Edge es un dispositivo informático perimetral habilitado para la inteligencia artificial que cuenta con funcionalidades de transferencia de datos de red. En este artículo se proporciona información general acerca de la solución de Azure Stack Edge, sus ventajas, sus funcionalidades clave y los escenarios donde puede implementar este dispositivo.

Azure Stack Edge es una solución de hardware como servicio. Microsoft le envía un dispositivo administrado en la nube con una matriz de puertas programables (FPGA) que permite la inferencia acelerada de IA y tiene todas las funcionalidades de una puerta de enlace de almacenamiento de red. 

## <a name="use-cases"></a>Casos de uso

Estos son los distintos escenarios en los que se puede usar Azure Stack Edge para la inferencia rápida de Machine Learning (ML) perimetral y el preprocesamiento de datos antes de enviarlos a Azure.

- **Inferencia con Azure Machine Learning**: con Azure Stack Edge, puede ejecutar modelos de ML para obtener resultados rápidos sobre los que se puede actuar antes de que los datos se envíen a la nube. El conjunto de datos completo se puede transferir opcionalmente para continuar entrenando y mejorando los modelos de Machine Learning. Para más información sobre cómo usar los modelos acelerados por hardware de Azure ML en el dispositivo Azure Stack Edge, consulte [Implementación de modelos acelerados por hardware de Azure ML en Azure Stack Edge](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Preprocesamiento de datos**: transforme datos antes de enviarlos a Azure para crear un conjunto de datos más accionable. El preprocesamiento se puede usar para: 

    - Agregar datos.
    - Modificar datos, por ejemplo, para quitar los datos personales.
    - Subconjuntos de datos para optimizar el almacenamiento y el ancho de banda, o para un análisis más exhaustivo.
    - Analizar y reaccionar a los eventos de IoT. 

- **Transferir datos a través de la red a Azure**: use Azure Stack Edge para transferir datos a Azure de forma rápida y sencilla para poder realizar más procesos y análisis o para fines de archivado. 

## <a name="key-capabilities"></a>Principales capacidades

Azure Stack Edge tiene las siguientes funcionalidades:

|Capacidad |Descripción  |
|---------|---------|
|Inferencia de IA acelerada| Habilitado por la FPGA integrada.|
|Procesamiento       |Permite el análisis, procesamiento y filtrado de datos.|
|Alto rendimiento | Transferencias de datos y proceso de alto rendimiento.|
|Acceso a datos     | Ofrece acceso directo a los datos de Azure Storage Blob y Azure Files mediante las API en la nube para un procesamiento de datos adicional en la nube. La memoria caché local del dispositivo se usa para agilizar el acceso a los archivos usados más recientemente.|
|Administración en la nube     |El dispositivo y el servicio se administran con Azure Portal.  |
|Carga sin conexión     | El modo sin conexión es compatible con escenarios de carga sin conexión.|
|Protocolos admitidos     | Compatibilidad con los protocolos estándar SMB y NFS para la ingesta de datos. <br> Para más información sobre las versiones compatibles, consulte [Requisitos del sistema de Azure Stack Edge](azure-stack-edge-system-requirements.md).|
|Actualización de datos     | Capacidad para actualizar los archivos locales con la versión más reciente de la nube.|
|Cifrado    | Compatibilidad con BitLocker para, localmente, cifrar los datos y proteger las transferencias de datos hacia la nube a través de *https*.|
|Limitación de ancho de banda| Limite el uso de ancho de banda durante las horas punta.|
|ExpressRoute | Mayor seguridad a través de ExpressRoute. Use la configuración de emparejamiento en la que el tráfico de los dispositivos locales a los puntos de conexión de almacenamiento en la nube viaja a través de ExpressRoute. Para más información, consulte [Información general sobre ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Componentes

La solución Azure Stack Edge consta de un recurso Azure Stack Edge, un dispositivo físico Azure Stack Edge y una interfaz de usuario web local.

* **Dispositivo físico Azure Stack Edge**: un servidor de montaje en bastidor 1U proporcionado por Microsoft que se puede configurar para enviar datos a Azure.
    
* **Recurso Azure Stack Edge**: es un recurso de Azure Portal que le permite administrar un dispositivo Azure Stack Edge desde una interfaz web a la cual puede acceder desde diferentes ubicaciones geográficas. Use el recurso Azure Stack Edge para crear y administrar recursos, ver y administrar dispositivos y alertas y administrar recursos compartidos.  

    <!--![The Azure Stack Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Para más información, vaya a [Creación de un pedido para el dispositivo Azure Stack Edge](azure-stack-edge-deploy-prep.md#create-a-new-resource).

* **Interfaz de usuario web local de Azure Stack Edge**: use la interfaz de usuario web local para ejecutar diagnósticos, apagar y reiniciar el dispositivo Azure Stack Edge, ver registros de copia y ponerse en contacto con el Soporte técnico de Microsoft para realizar una solicitud de servicio.

    <!--![The Azure Stack Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Para más información acerca de cómo usar la interfaz de usuario basada en web, consulte [Uso de la interfaz de usuario web para administrar Azure Stack Edge](azure-stack-edge-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Disponibilidad en regiones

El dispositivo físico Azure Stack Edge, el recurso de Azure y la cuenta de almacenamiento de destino a la que transferirá los datos no tienen que estar en la misma región.

- **Disponibilidad de recursos**: para ver una lista de todas las regiones en las que está disponible el recurso de Azure Stack Edge, consulte [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Azure Stack Edge también se puede implementar en la nube de Azure Government. Para más información, consulte [¿Qué es Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).
    
- **Cuentas de almacenamiento de destino**: las cuentas de almacenamiento que almacenan los datos están disponibles en todas las regiones de Azure. Las regiones en las que las cuentas de almacenamiento almacenan los datos de Azure Stack Edge deben estar ubicadas cerca de la ubicación del dispositivo para un rendimiento óptimo. Una cuenta de almacenamiento que se encuentra lejos del dispositivo causa latencias largas y un rendimiento más lento.

## <a name="next-steps"></a>Pasos siguientes

- Revise [los requisitos del sistema de Azure Stack Edge](azure-stack-edge-system-requirements.md).
- Consulte los [límites de Azure Stack Edge](azure-stack-edge-limits.md).
- Implementar [Azure Stack Edge](azure-stack-edge-deploy-prep.md) en Azure Portal.
