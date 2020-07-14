---
title: Configuración de una base de datos en Azure API for FHIR
description: En este artículo se describe cómo configurar las opciones de la base de datos en Azure API for FHIR
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 11/15/2019
ms.author: matjazl
ms.openlocfilehash: adc6fdf144927d10f811a00aa33f244cfdc25042
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84870965"
---
# <a name="configure-database-settings"></a>Configuración de las opciones de la base de datos 

Azure API for FHIR usa una base de datos para almacenar sus datos. El rendimiento de la base de datos subyacente depende del número de unidades de solicitud (RU) seleccionado durante el aprovisionamiento del servicio o de la configuración de la base de datos una vez aprovisionado el servicio.

Azure API for FHIR toma prestado el concepto de RU de Cosmos DB (consulte [Unidades de solicitud en Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/request-units)) al establecer el rendimiento de la base de datos subyacente. 

El rendimiento se debe aprovisionar de modo que se garantice que haya suficientes recursos del sistema disponibles para la base de datos en todo momento. El número de RU necesario para la aplicación depende de las operaciones que se realicen. Las operaciones pueden abarcar desde simples lecturas y escrituras hasta consultas más complejas. 

> [!NOTE]
> Dado que las distintas operaciones consumen un número diferente de RU, devolvemos el número real de RU consumidas en cada llamada API en el encabezado de respuesta. De este modo, puede generar perfiles del número de RU consumidas por la aplicación.

## <a name="update-throughput"></a>Actualización del rendimiento
Para cambiar esta configuración en Azure Portal, vaya a Azure API for FHIR y abra la hoja de la base de datos. A continuación, cambie el rendimiento aprovisionado al valor deseado en función de sus necesidades. Puede cambiar el valor hasta un máximo de 10 000 RU/s. Si necesita un valor mayor, póngase en contacto con el Soporte técnico de Azure.

> [!NOTE] 
> Un valor más grande significa un mayor rendimiento de Azure API for FHIR y un costo más elevado del servicio.

![Cosmos DB de configuración](media/database/database-settings.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a actualizar las RU para Azure API for FHIR. A continuación, implemente una instancia de Azure API for FHIR totalmente administrada:
 
>[!div class="nextstepaction"]
>[Implementación de Azure API for FHIR](fhir-paas-portal-quickstart.md)