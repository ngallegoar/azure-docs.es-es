---
title: Ejecución de la exportación mediante la invocación del comando $export en Azure API for FHIR
description: En este artículo se describe cómo exportar datos de FHIR mediante $export
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: 74fe09895f49cc9f7c3cdf6b6c97c1624c3e9c0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91839833"
---
# <a name="how-to-export-fhir-data"></a>Exportación de datos de FHIR


La característica de exportación masiva permite exportar datos desde el servidor de FHIR según la [especificación FHIR](https://hl7.org/fhir/uv/bulkdata/export/index.html). 

Antes de usar el comando $export, puede asegurarse de que Azure API for FHIR esté configurado para usarlo. Para configurar las opciones de exportación y crear una cuenta de almacenamiento de Azure, consulte la [página de configuración de los datos de exportación](configure-export-data.md).

## <a name="using-export-command"></a>Uso del comando $export

Después de configurar Azure API for FHIR para la exportación, puede usar el comando $export para exportar los datos fuera del servicio. Los datos se almacenarán en la cuenta de almacenamiento que especificó al configurar la exportación. Para más información sobre cómo invocar el comando $export en el servidor de FHIR, lea la documentación sobre la [especificación de dicho comando](https://hl7.org/Fhir/uv/bulkdata/export/index.html). 

El comando $export de Azure API for FHIR toma un parámetro _\_container_ adicional que especifica el contenedor de la cuenta de almacenamiento configurada a la que se deben exportar los datos. Si se especifica un contenedor, los datos se exportarán a este en una nueva carpeta con el nombre. Si no se especifica el contenedor, se exportará a un nuevo contenedor con un nombre generado de forma aleatoria. 

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

## <a name="supported-scenarios"></a>Escenarios admitidos

Azure API for FHIR admite el comando $export en el nivel de sistema, paciente y grupo. Para la exportación de grupo, se exportan todos los recursos relacionados, pero no las características del grupo.

> [!Note] 
> El comando $export exportará los recursos duplicados si el recurso está en un compartimiento de más de un recurso o se encuentra en varios grupos.

Además, se admite la comprobación del estado de la exportación a través de la dirección URL devuelta por el encabezado de ubicación durante la puesta en cola, junto con la cancelación del trabajo de exportación real.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a exportar recursos de FHIR mediante el comando $export. A continuación, puede revisar las características compatibles:
 
>[!div class="nextstepaction"]
>[Características compatibles](fhir-features-supported.md)
