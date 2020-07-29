---
title: 'Preguntas más frecuentes sobre los servicios de FHIR en Azure: Azure API for FHIR'
description: Obtenga respuestas a las preguntas más frecuentes sobre Azure API for FHIR, como la ubicación de almacenamiento de datos detrás de las API de FHIR y la compatibilidad con versiones.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: af891935fd474e6f1f83ff1c2ce56ef71cd065c6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536732"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>Preguntas más frecuentes sobre Azure API for FHIR

## <a name="azure-api-for-fhir"></a>API de Azure para FHIR

### <a name="what-is-fhir"></a>¿Qué es FHIR?
Recursos Rápidos de Interoperabilidad en Salud (FHIR, pronunciado como "fire" en inglés) es un estándar de interoperabilidad diseñado para permitir el intercambio de datos del sector sanitario entre diferentes sistemas sanitarios. Este estándar fue desarrollado por la organización HL7 y lo adoptan organizaciones del sector sanitario de todo el mundo. La versión más reciente de FHIR disponible es la R4 (versión 4). Azure API for FHIR admite la versión R4 y también es compatible con la versión anterior STU3 (Standard for Trial Use 3). Para más información sobre FHIR, visite [HL7.org](http://hl7.org/fhir/summary.html).

### <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>¿Están los datos detrás de las API de FHIR almacenados en Azure?

Sí, los datos se almacenan en bases de datos administradas en Azure. Azure API for FHIR no proporciona el acceso directo al almacén de datos subyacente.

### <a name="what-identity-provider-do-you-support"></a>¿Qué proveedor de identidades admite?

Actualmente se admite Microsoft Azure Active Directory como proveedor de identidades.

### <a name="what-fhir-version-do-you-support"></a>¿Qué versión de FHIR admite?

Se admiten las versiones 4.0.0 y 3.0.1 tanto en Azure API for FHIR (PaaS) como en FHIR Server for Azure (código abierto).

Para obtener detalles, consulte las [características admitidas](fhir-features-supported.md). Lea la información sobre lo que ha cambiado de una versión a otra en el [historial de versiones de HL7 FHIR](https://hl7.org/fhir/R4/history.html).

### <a name="whats-the-difference-between-the-open-source-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>¿Cuál es la diferencia entre Microsoft FHIR Server for Azure de código abierto y Azure API for FHIR?

Azure API for FHIR es una versión hospedada y administrada de Microsoft FHIR Server for Azure de código abierto. En el servicio administrado, Microsoft proporciona todo el mantenimiento y las actualizaciones. 

Al ejecutar FHIR Server for Azure, tiene acceso directo a los servicios subyacentes. Pero usted también es el responsable de mantener y actualizar el servidor y el trabajo de cumplimiento necesario si está almacenando los datos de PHI.

Desde el punto de vista del desarrollo, cada característica se implementa primero en Microsoft FHIR Server for Azure de código abierto. Una vez que se haya validado en el código abierto, se lanzará en la solución Azure API for FHIR de PaaS. El tiempo entre el lanzamiento en el código abierto y PaaS depende de la complejidad de la característica y de otras prioridades del mapa de ruta. 

### <a name="what-is-smart-on-fhir"></a>¿Qué es SMART on FHIR?

SMART (Substitutable Medical Applications and Reusable Technology) on FHIR es un conjunto de especificaciones de código abierto para integrar aplicaciones de asociados con servidores de FHIR y otros sistemas de TI sanitarios, como los registros sanitarios electrónicos y los intercambios de información sanitaria. Al crear una aplicación SMART on FHIR, puede garantizar que una gran cantidad de sistemas diferentes pueden acceder a la aplicación y aprovecharla.
Autenticación y Azure API for FHIR. Para obtener más información acerca de SMART, visite [SMART Health IT](https://smarthealthit.org/).


## <a name="iot-connector-preview"></a>Conector de IoT (versión preliminar)

### <a name="what-is-iomt"></a>¿Qué es IoMT?
IoMT significa Internet de las cosas médicas y es una categoría de dispositivos IoT que capturan e intercambian datos de salud y bienestar con otros sistemas informáticos de salud a través de una red. Algunos ejemplos de dispositivos IoMT son: dispositivos ponibles para uso deportivo y clínico, sensores de supervisión, seguimiento de la actividad, quioscos de punto de atención o incluso una píldora inteligente.

### <a name="how-many-iot-connectors-do-i-need"></a>¿Cuántos conectores de IoT necesito?
Se puede usar un único conector de IoT para ingerir datos de un gran número de tipos diferentes de dispositivos. Puede seguir usando distintos conectores por los siguientes motivos:
- **Escala**: En el caso de la versión preliminar pública, la capacidad de los recursos del conector de IoT es fija y se espera que proporcione un rendimiento de aproximadamente 200 mensajes por segundo. Puede agregar más conectores de IoT si es necesario un mayor rendimiento.
- **Tipo de dispositivo** Puede configurar un conector de IoT independiente para cada tipo de dispositivos IoMT que tenga por motivos de administración de dispositivos.

### <a name="is-there-a-limit-on-number-of-iot-connectors-during-public-preview"></a>¿Hay un límite en el número de conectores de IoT durante la versión preliminar pública?
Sí, solo puede crear dos conectores de IoT por suscripción mientras la característica esté en versión preliminar pública. Este límite existe para evitar gastos inesperados, ya que la característica está disponible de forma gratuita durante la versión preliminar. A petición, este límite podría elevarse hasta un máximo de cinco conectores de IoT.

### <a name="what-azure-regions-iot-connector-feature-is-available-during-public-preview"></a>¿En qué regiones está disponible la característica de conector de IoT durante la versión preliminar pública?
El conector de IoT está disponible en todas las regiones de Azure donde esté disponible Azure API for FHIR.

### <a name="can-i-configure-scaling-capacity-for-iot-connector"></a>¿Puedo configurar la capacidad de escalado para el conector de IoT?
Dado que el conector de IoT es gratuito durante la versión preliminar pública, su capacidad de escalado es fija y limitada. Se espera que la configuración del conector de IoT disponible en la versión preliminar pública proporcione un rendimiento de aproximadamente 200 mensajes por segundo. En la versión de disponibilidad general (GA) habrá una manera de configurar la capacidad de los recursos.

### <a name="what-fhir-version-does-iot-connector-support"></a>¿Qué versión de FHIR admite el conector de IoT?
El conector de IoT actualmente solo admite la versión R4 de FHIR. Por lo tanto, esta característica solo es visible en las instancias R4 de Azure API for FHIR y Microsoft no tiene previsto admitir la versión STU3 en este momento.

### <a name="why-cant-i-install-iot-connector-when-private-link-is-enabled-on-azure-api-for-fhir"></a>¿Por qué no se puede instalar el conector de IoT cuando está habilitado Private Link en Azure API for FHIR?
El conector de IoT no admite la funcionalidad Private Link en este momento. Por lo tanto, si tiene Private Link habilitado en Azure API for FHIR, no podrá instalar el conector de IoT y viceversa. Se espera que esta limitación desaparezca cuando el conector de IoT esté en versión de disponibilidad general (GA).

### <a name="whats-the-difference-between-the-open-source-iomt-fhir-connector-for-azure-and-iot-connector-feature-of-azure-api-for-fhir-service"></a>¿Cuál es la diferencia entre el conector FHIR de IoMT de código abierto para Azure y la característica del conector de IoT del servicio Azure API for FHIR?
El conector de IoT es una versión hospedada y administrada del conector FHIR de IoMT de código abierto para Azure. En el servicio administrado, Microsoft proporciona todo el mantenimiento y las actualizaciones.

Cuando ejecute el conector FHIR de IoMT para Azure, tendrá acceso directo a los recursos subyacentes. Pero usted también es el responsable de mantener y actualizar el servidor y el trabajo de cumplimiento necesario si está almacenando los datos de PHI.

Desde el punto de vista del desarrollo, cada característica se implementa primero en el conector FHIR de IoMT de código abierto para Azure. Una vez que se haya validado en el código abierto, se lanzará en la característica de conector de IoT de PaaS del servicio Azure API for FHIR. El tiempo entre el lanzamiento en el código abierto y PaaS depende de la complejidad de la característica y de otras prioridades de la hoja de ruta.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha leído algunas de las preguntas más frecuentes sobre Azure API for FHIR. Lea información acerca de las características admitidas en FHIR Server for Azure:
 
>[!div class="nextstepaction"]
>[Características de FHIR admitidas](fhir-features-supported.md)