---
title: 'Azure IoT Hub Device Provisioning Service: atestación del certificado X.509'
description: Describe conceptos específicos del uso de la atestación del certificado X.509 con Device Provisioning Service (DPS) e IoT Hub
author: wesmc7777
ms.author: wesmc
ms.date: 09/14/2020
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 911f819343f675ebe0a2604d912e6e26aa646eb5
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90533019"
---
# <a name="x509-certificate-attestation"></a>Atestación de certificados de cliente X.509

En este artículo se proporciona información general sobre los conceptos implicados en el aprovisionamiento de dispositivos mediante la atestación del certificado X.509. Este artículo es apropiado para todas las personas implicadas en la preparación de un dispositivo para la implementación.

Los certificados X.509 se pueden almacenar en un HSM de módulo de seguridad de hardware.

> [!TIP]
> Se recomienda usar un HSM con los dispositivos para almacenar de forma segura los secretos, como el certificado X.509, en los dispositivos de producción.


## <a name="x509-certificates"></a>Certificados X.509

El uso de certificados X.509 como un mecanismo de atestación es una manera excelente para escalar la producción y simplificar el aprovisionamiento de dispositivos. Los certificados X.509 normalmente están organizados en una cadena de certificados de confianza en la que cada certificado de la cadena está firmado por la clave privada del certificado superior siguiente y así sucesivamente, terminando en un certificado raíz autofirmado. Esta opción establece una cadena de confianza delegada a partir del certificado raíz que generó una entidad de certificación (CA) raíz de confianza, a través de cada entidad de certificación intermedia y hasta el certificado "de hoja" de entidad final instalado en un dispositivo. Para más información consulte [Autenticación de dispositivos mediante certificados de entidades de certificación X.509](/azure/iot-hub/iot-hub-x509ca-overview). 

A menudo la cadena de certificados representa alguna jerarquía lógica o física asociada con los dispositivos. Por ejemplo, un fabricante puede:
- emitir un certificado raíz de entidad de certificación autofirmado
- usar el certificado raíz para generar un certificado de entidad de certificación intermedia único para cada fábrica
- usar el certificado de cada fábrica para generar un certificado de entidad de certificación intermedia único para cada línea de producción en la planta
- y, por último, utilizar el certificado de la línea de producción para generar un certificado de dispositivo único (entidades finales) para cada dispositivo que se fabrica en la línea. 

Para más información consulte [Explicación de los conceptos de certificados de entidad de certificación X.509 en el sector de IoT](/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Certificado raíz

Un certificado raíz es un certificado X.509 autofirmado que representa una entidad de certificación (CA). Es el término o anclaje de veracidad de la cadena de certificados. Una organización puede emitir sus propios certificados raíz o puede adquirirlos a través de una entidad de certificación raíz. Para más información, consulte [Obtención de certificados de entidad de certificación X.509](/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). También se puede hacer referencia al certificado raíz como un certificado de CA raíz.

### <a name="intermediate-certificate"></a>Certificado intermedio

Un certificado intermedio es un certificado X.509 que ha sido firmado por el certificado raíz (o por otro certificado que actúa como certificado intermedio en la cadena hasta el certificado raíz). El último certificado intermedio en una cadena se usa para firmar el certificado de hoja. También se puede hacer referencia a un certificado intermedio como un certificado de CA intermedio.

### <a name="end-entity-leaf-certificate"></a>Certificado de entidad final "de hoja"

El certificado de hoja, o certificado de entidad final, identifica al titular del certificado. Tiene el certificado raíz en su cadena de certificados, así como cero o más certificados intermedios. El certificado de hoja no se usa para firmar otros certificados. Identifica de forma exclusiva al dispositivo ante el servicio de aprovisionamiento, y a veces se conoce como certificado del dispositivo. Durante la autenticación, el dispositivo usa la clave privada asociada con este certificado para responder a una prueba de desafío de posesión del servicio.

Los certificados de hoja usados con una entrada de [inscripción Individual](./concepts-service.md#individual-enrollment) requieren que el **nombre del sujeto** se establezca en el identificador de registro de la entrada de la inscripción individual. Los certificados de hoja utilizados con una entrada de [grupo de inscripción](./concepts-service.md#enrollment-group) deben tener el **nombre del sujeto** configurado en el identificador del dispositivo deseado que se mostrará en **Entradas de registro** para el dispositivo autenticado en el grupo de inscripción.

Para más información consulte [Autenticación de dispositivos firmados con certificados de entidad de certificación X.509](/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>Control del acceso de dispositivo al servicio de aprovisionamiento con certificados X.509

El servicio de aprovisionamiento expone dos tipos de entrada de inscripción que puede usar para controlar el acceso de los dispositivos que usan el mecanismo de atestación de X.509:  

- Las entradas de [inscripción individual](./concepts-service.md#individual-enrollment) se configuran con el certificado de dispositivo asociado a un dispositivo específico. Estas entradas controlan las inscripciones para dispositivos específicos.
- Las entradas de [grupo de inscripción](./concepts-service.md#enrollment-group) están asociadas con un certificado de CA raíz o intermedio específico. Estas entradas controlan las inscripciones para todos los dispositivos que tienen ese certificado intermedio o raíz en su cadena de certificados. 

Cuando un dispositivo se conecta al servicio de aprovisionamiento, el servicio da prioridad a las entradas de inscripción más específicas sobre las entradas de inscripción menos específicas. Es decir, si existe una inscripción individual para el dispositivo, el servicio de aprovisionamiento aplica a esa entrada. Si no hay ninguna inscripción individual para el dispositivo y existe un grupo de inscripción para el primer certificado intermedio en la cadena de certificados del dispositivo, el servicio aplica a esa entrada, y así sucesivamente hacia arriba en la cadena hasta la raíz. El servicio aplica la primera entrada aplicable que encuentra de forma que:

- Si la primera entrada de inscripción que se encuentra está habilitada, el servicio aprovisiona el dispositivo.
- Si la primera entrada de inscripción que se encuentra está deshabilitada, el servicio no aprovisiona el dispositivo.  
- Si no se encuentra ninguna entrada de inscripción para cualquiera de los certificados de la cadena de certificados del dispositivo, el servicio no aprovisiona el dispositivo. 

Este mecanismo y la estructura jerárquica de las cadenas de certificados proporciona una flexibilidad eficaz en la forma de controlar el acceso de dispositivos individuales, así como el de grupos de dispositivos. Por ejemplo, imagine cinco dispositivos con las siguientes cadenas de certificado: 

- *Dispositivo 1*: certificado raíz -> certificado A -> certificado de dispositivo 1
- *Dispositivo 2*: certificado raíz -> certificado A -> certificado de dispositivo 2
- *Dispositivo 3*: certificado raíz -> certificado A -> certificado de dispositivo 3
- *Dispositivo 4*: certificado raíz -> certificado B -> certificado de dispositivo 4
- *Dispositivo 5*: certificado raíz -> certificado B -> certificado de dispositivo 5

En principio, puede crear una única entrada de inscripción de grupo habilitada para el certificado raíz para habilitar el acceso para los cinco dispositivos. Si más adelante el certificado B se pone en peligro, puede crear una entrada de grupo de inscripción deshabilitada para el certificado B para evitar que el *Dispositivo 4* y el *Dispositivo 5* se inscriban. Si después el *Dispositivo 3* se pone en peligro, puede crear una entrada de inscripción individual deshabilitada para su certificado. Esto revoca el acceso del *Dispositivo 3*, pero sigue permitiendo que el *Dispositivo 1* y el *Dispositivo 2* se inscriban.