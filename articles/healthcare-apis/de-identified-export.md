---
title: Exportación de datos sin identificación (versión preliminar) para Azure API for FHIR
description: En este artículo se indica cómo configurar y usar la exportación sin identificación.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 9/28/2020
ms.author: matjazl
ms.openlocfilehash: bdbab0e032764d07119402686051d391376cb913
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843660"
---
# <a name="exporting-de-identified-data-preview"></a>Exportación de datos sin identificación (versión preliminar)

> [!Note] 
> Los resultados cuando se usa la exportación sin identificación variarán en función de factores como los datos introducidos y las funciones seleccionadas por el cliente. Microsoft no puede evaluar las salidas de exportación sin identificación ni determinar la aceptabilidad para los casos de uso del cliente y las necesidades de cumplimiento. No se garantiza que la exportación sin identificación cumpla los requisitos legales, normativos o de cumplimiento específicos.

También se puede usar el comando $export para exportar datos sin identificación desde el servidor de FHIR. Usa el motor de anonimización de [Herramientas de FHIR para la anonimización](https://github.com/microsoft/FHIR-Tools-for-Anonymization) y toma los detalles de configuración de anonimización en los parámetros de la consulta. Puede crear su propio archivo de configuración de anonimización o usar el [archivo de configuración de ejemplo](https://github.com/microsoft/FHIR-Tools-for-Anonymization#sample-configuration-file-for-hipaa-safe-harbor-method) para el método Safe Harbor de HIPAA como punto de partida. 

 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

|Parámetro de consulta            | Ejemplo |Opcionalidad| Descripción|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |DemoConfig.json|Obligatorio para la exportación sin identificación |Nombre del archivo de configuración. Consulte el formato del archivo de configuración [aquí](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format). Este archivo debe mantenerse dentro de un contenedor llamado **anonymization** dentro de la misma cuenta de almacenamiento de Azure que está configurada como ubicación de exportación. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Opcional para la exportación sin identificación|Este es el valor de Etag del archivo de configuración. Puede obtener el valor de Etag mediante Explorador de Azure Storage desde la propiedad del blob.|

> [!IMPORTANT]
> Tanto la exportación sin formato como la exportación sin identificación escriben en la misma cuenta de almacenamiento de Azure especificada como parte de la configuración de la exportación. Se recomienda que use distintos contenedores que se correspondan con las diferentes configuraciones sin identificación y que administren el acceso de usuarios en el nivel de contenedor.