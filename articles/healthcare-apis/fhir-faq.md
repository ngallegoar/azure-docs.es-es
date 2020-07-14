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
ms.openlocfilehash: cdec5ade4fc72fba6fcfba131b69ca9eb373874c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84870985"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>Preguntas más frecuentes sobre Azure API for FHIR

## <a name="what-is-fhir"></a>¿Qué es FHIR?
Recursos Rápidos de Interoperabilidad en Salud (FHIR, pronunciado como "fire" en inglés) es un estándar de interoperabilidad diseñado para permitir el intercambio de datos del sector sanitario entre diferentes sistemas sanitarios. Este estándar fue desarrollado por la organización HL7 y lo adoptan organizaciones del sector sanitario de todo el mundo. La versión más reciente de FHIR disponible es la R4 (versión 4). Azure API for FHIR admite la versión R4 y también es compatible con la versión anterior STU3 (Standard for Trial Use 3). Para más información sobre FHIR, visite [HL7.org](http://hl7.org/fhir/summary.html).

## <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>¿Están los datos detrás de las API de FHIR almacenados en Azure?

Sí, los datos se almacenan en bases de datos administradas en Azure. Azure API for FHIR no proporciona el acceso directo al almacén de datos subyacente.

## <a name="what-identity-provider-do-you-support"></a>¿Qué proveedor de identidades admite?

Actualmente se admite Microsoft Azure Active Directory como proveedor de identidades.

## <a name="what-fhir-version-do-you-support"></a>¿Qué versión de FHIR admite?

Se admiten las versiones 4.0.0 y 3.0.1 tanto en Azure API for FHIR (PaaS) como en FHIR Server for Azure (código abierto).

Para obtener detalles, consulte las [características admitidas](fhir-features-supported.md). Lea la información sobre lo que ha cambiado de una versión a otra en el [historial de versiones de HL7 FHIR](https://hl7.org/fhir/R4/history.html).

## <a name="whats-the-difference-between-the-open-source-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>¿Cuál es la diferencia entre Microsoft FHIR Server for Azure de código abierto y Azure API for FHIR?

Azure API for FHIR es una versión hospedada y administrada de Microsoft FHIR Server for Azure de código abierto. En el servicio administrado, Microsoft proporciona todo el mantenimiento y las actualizaciones. 

Al ejecutar FHIR Server for Azure, tiene acceso directo a los servicios subyacentes. Pero usted también es el responsable de mantener y actualizar el servidor y el trabajo de cumplimiento necesario si está almacenando los datos de PHI.

Desde el punto de vista del desarrollo, cada característica se implementa primero en Microsoft FHIR Server for Azure de código abierto. Una vez que se haya validado en el código abierto, se lanzará en la solución Azure API for FHIR de PaaS. El tiempo entre el lanzamiento en el código abierto y PaaS depende de la complejidad de la característica y de otras prioridades del mapa de ruta. 

## <a name="what-is-smart-on-fhir"></a>¿Qué es SMART on FHIR?

SMART (Substitutable Medical Applications and Reusable Technology) on FHIR es un conjunto de especificaciones de código abierto para integrar aplicaciones de asociados con servidores de FHIR y otros sistemas de TI sanitarios, como los registros sanitarios electrónicos y los intercambios de información sanitaria. Al crear una aplicación SMART on FHIR, puede garantizar que una gran cantidad de sistemas diferentes pueden acceder a la aplicación y aprovecharla.
Autenticación y Azure API for FHIR. Para obtener más información acerca de SMART, visite [SMART Health IT](https://smarthealthit.org/).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha leído algunas de las preguntas más frecuentes sobre Azure API for FHIR. Lea información acerca de las características admitidas en FHIR Server for Azure:
 
>[!div class="nextstepaction"]
>[Características de FHIR admitidas](fhir-features-supported.md)