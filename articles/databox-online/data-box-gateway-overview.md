---
title: Introducción a Microsoft Azure Data Box Gateway | Microsoft Docs
description: Describe Azure Data Box Gateway, una solución de almacenamiento de aplicaciones virtuales que permite transferir datos en Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: overview
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: aa68b746ef5c22c97fcfdcca889bf71577c19479
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "85100758"
---
# <a name="what-is-azure-data-box-gateway"></a>¿Qué es Azure Data Box Gateway?

Azure Data Box Gateway es una solución de almacenamiento que permite enviar fácilmente datos a Azure. En este artículo se proporciona información general acerca de la solución de Azure Data Box Gateway, sus ventajas, funcionalidades clave y los escenarios donde puede implementar este dispositivo.

Data Box Gateway es un dispositivo virtual basado en una máquina virtual aprovisionada en un entorno virtualizado o hipervisor. El dispositivo virtual reside en su entorno local y se pueden escribir datos en él mediante los protocolos SMB y NFS. El dispositivo, a continuación, transfiere los datos a blob en bloques de Azure, blob en páginas o Azure Files.

## <a name="use-cases"></a>Casos de uso

Data Box Gateway puede usarse para transferir datos a la nube, como archivos en la nube, para la recuperación ante desastres o si es necesario procesar los datos en la nube. Estos son los distintos escenarios donde se puede usar Data Box Gateway para la transferencia de datos.

- **Archivos en la nube**: copie cientos de TB de datos en Azure Storage mediante Data Box Gateway de manera segura y eficaz. Los datos pueden ingerirse de una vez o de forma continuada en los escenarios de archivo.

- **Ingesta continua de datos**: los datos se ingieren continuamente en el dispositivo y se copian en la nube, sin importar su tamaño. Como los datos se escriben en el dispositivo de puerta de enlace, el dispositivo carga los datos en Azure Storage.  

- **Transferencia masiva inicial seguida de transferencia incremental**: use Data Box para la transferencia masiva en modo sin conexión (inicialización) y Data Box Gateway para las transferencias incrementales (fuente continua) a través de la red.

Para más información, vaya a [Casos de uso de Azure Data Box Gateway](data-box-gateway-use-cases.md).

## <a name="benefits"></a>Ventajas

Data Box Gateway tiene las siguientes ventajas:

- **Transferencia de datos sencilla**: facilita el traslado de datos hacia y desde Azure Storage tan fácilmente como si estuviera trabajando con un recurso compartido de red local.  
- **Alto rendimiento**: evita los inconvenientes de tener que transportar los datos de red con transferencias de alto rendimiento a y desde Azure.
- **Acceso rápido y altas tasas de ingesta de datos durante el horario laboral**: Data Box Gateway tiene una caché local que se define como el tamaño de la capacidad local cuando se aprovisiona el dispositivo virtual. El tamaño del disco de datos debe especificarse según los [requisitos mínimos del dispositivo virtual](data-box-gateway-system-requirements.md#specifications-for-the-virtual-device). La caché local proporciona las siguientes ventajas:
    - La memoria caché local permite la ingesta de datos a alta velocidad. Cuando se ingiere una gran cantidad de datos durante las horas punta, la caché puede retener los datos y cargarlos en la nube.
    - La caché local permite un acceso rápido a la lectura hasta un determinado umbral. Hasta que el dispositivo esté lleno al 50-60 %, se accede a todas las lecturas del dispositivo desde la caché, lo que las hace más rápidas. Una vez que el espacio utilizado en el dispositivo supera este umbral, el dispositivo comienza a quitar los archivos locales.
 
- **Uso de ancho de banda limitado**: se pueden escribir datos en Azure incluso cuando la red limita el uso durante las horas punta comerciales.  

## <a name="key-capabilities"></a>Principales capacidades

Data Box Gateway tiene las siguientes funcionalidades:

|Capacidad |Descripción  |
|---------|---------|
|Velocidad     | Transferencia de datos y ancho de banda totalmente automatizados y altamente optimizados.|
|Protocolos admitidos     | Compatibilidad con los protocolos estándar SMB y NFS para la ingesta de datos. <br> Para obtener más información sobre las versiones compatibles, vaya a [Requisitos del sistema de Data Box Gateway](data-box-gateway-system-requirements.md).|
|Acceso a datos     | Cuando los datos enviados por el dispositivo están en la nube, se pueden modificar aún más mediante el acceso directo a las API de nube.|
|Acceso rápido     | Memoria caché local en el dispositivo para agilizar el acceso a los archivos usados más recientemente.|
|Carga sin conexión     | El modo sin conexión es compatible con escenarios de carga sin conexión.|
|Actualización de datos     | Capacidad para actualizar los archivos locales con la versión más reciente de la nube.|
|Resistencia     | Resistencia de red integrada        |


## <a name="specifications"></a>Especificaciones

El dispositivo virtual Data Box Gateway tiene las siguientes especificaciones:

| Especificaciones                                          | Descripción              |
|---------------------------------------------------------|--------------------------|
| Procesadores virtuales (núcleos)   | Cuatro como mínimo |
| Memoria  |8 GB como mínimo|
| Disponibilidad|Nodo único|
| Discos|Disco del sistema operativo: 250 GB <br> Disco de datos: 2 TB como mínimo, con aprovisionamiento fino y debe estar basado en SSD|
| Interfaces de red |Una o varias interfaces de red virtual|
| Protocolos de uso compartido de archivos nativos|SMB y NFS  |
| Seguridad|Autenticación para desbloquear el acceso a datos y dispositivos <br> Cifrado de datos en proceso mediante el cifrado AES de 256 bits|
| Administración|Interfaz de usuario web local: programa de instalación inicial, diagnósticos y administración de energía de dispositivos <br> Azure Portal: administración diaria de dispositivos Data Box Gateway       |

## <a name="components"></a>Componentes

La solución Data Box Gateway consta de un recurso Data Box Gateway, un dispositivo físico Data Box Gateway y una interfaz de usuario web local.

- **Dispositivo virtual Data Box Gateway**: es un dispositivo virtual basado en una máquina virtual aprovisionada en un entorno virtualizado o hipervisor que le permite enviar datos a Azure.
    
- **Recurso Data Box Gateway**: es un recurso de Azure Portal que le permite administrar un dispositivo Data Box Gateway desde una interfaz web a la cual puede obtener acceso desde diferentes ubicaciones geográficas. Use el recurso de Data Box Gateway para ver y administrar dispositivos, recursos compartidos, usuarios y alertas. Para más información, consulte [Administración mediante Azure Portal](data-box-gateway-manage-shares.md).

- **Interfaz de usuario web local de Data Box**: use la interfaz de usuario web local para ejecutar diagnósticos, apagar y reiniciar el dispositivo, generar un paquete de soporte técnico o ponerse en contacto con el servicio de soporte técnico de Microsoft para presentar una solicitud de servicio. Para más información, consulte [Administración con la interfaz de usuario web local](data-box-gateway-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Disponibilidad en regiones

El dispositivo físico de Data Box Edge, el recurso de Azure y la cuenta de almacenamiento de destino a la que transferirá los datos no tienen que estar en la misma región.

- **Disponibilidad de recursos**: para obtener una lista de todas las regiones en que está disponible el recurso de Azure Data Box Gateway, vaya a [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=databox). Data Box Gateway también se puede implementar en la nube de Azure Government. Para más información, consulte [¿Qué es Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).

- **Cuentas de almacenamiento de destino**: las cuentas de almacenamiento que almacenan los datos están disponibles en todas las regiones de Azure.

    Las regiones en las que las cuentas de almacenamiento almacenan los datos de Data Box deben estar ubicadas cerca de la ubicación del dispositivo para un rendimiento óptimo. Una cuenta de almacenamiento que se encuentra lejos del dispositivo causa latencias largas y un rendimiento más lento.


## <a name="next-steps"></a>Pasos siguientes

- Revise [los requisitos del sistema Data Box Gateway](data-box-gateway-system-requirements.md).
- Información acerca de los [límites de Data Box Gateway](data-box-gateway-limits.md).
- Implemente [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) en Azure Portal.

