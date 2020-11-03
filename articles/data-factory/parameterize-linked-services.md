---
title: Parametrización de servicios vinculados en Azure Data Factory
description: Aprenda a parametrizar los servicios vinculados en Azure Data Factory y a pasar valores dinámicos en tiempo de ejecución.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/22/2020
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: e6c00a0d2b6ff8bbb4ba9e51110e995e93d6b558
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92426973"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Parametrización de servicios vinculados en Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Ahora puede parametrizar un servicio vinculado y pasar valores dinámicos en tiempo de ejecución. Por ejemplo, si quiere conectarse a bases de datos diferentes en el mismo servidor lógico de SQL, ahora puede parametrizar el nombre de la base de datos en la definición del servicio vinculado. Esto evita tener que crear un servicio vinculado para cada base de datos del servidor lógico de SQL. Por ejemplo, puede parametrizar otras propiedades en la definición de servicio vinculado también; por ejemplo, *Nombre de usuario.*

Puede usar la interfaz de usuario de Data Factory en Azure Portal o una interfaz de programación para parametrizar los servicios vinculados.

> [!TIP]
> Se recomienda no parametrizar las contraseñas ni los secretos. Almacene en cambio todas las cadenas de conexión en Azure Key Vault y parametrice el *nombre del secreto*.

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-linked-service-types"></a>Tipos de servicios vinculados admitidos

Puede parametrizar cualquier tipo de servicio vinculado.
Al crear un servicio vinculado en la interfaz de usuario, Data Factory proporciona una experiencia de parametrización integrada para los siguientes tipos de servicios vinculados. En la hoja de creación y edición del servicio vinculado, puede buscar las opciones de los nuevos parámetros y agregar contenido dinámico.

- Amazon Redshift
- Amazon S3
- Azure Cosmos DB (API de SQL)
- Azure Database for MySQL
- Azure Databricks
- Azure SQL Database
- Instancia administrada de Azure SQL
- Azure Synapse Analytics (anteriormente SQL DW)
- MySQL
- Oracle
- SQL Server
- HTTP genérico
- REST genérico

Para otros tipos de servicios vinculados que no están en la lista anterior, puede parametrizar el servicio vinculado mediante la edición del código JSON en la interfaz de usuario:

- En la hoja de creación y edición del servicio vinculado -> expanda "Advanced" (Avanzado) -> active la casilla "Specify dynamic contents in JSON format" (Especificar contenido dinámico en formato JSON) -> especifique la carga JSON del servicio vinculado. 
- O bien, después de crear un servicio vinculado sin parametrización, en [Management hub](author-visually.md#management-hub) (Centro de administración) -> Linked services (Servicios vinculados) -> busque el servicio vinculado específico -> haga clic en "código" (botón "{}") para editar el código JSON. 

Consulte el [ejemplo de JSON](#json) para agregar la sección ` parameters` para definir los parámetros y hacer referencia al parámetro mediante ` @{linkedService().paraName} `.

## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory

![Incorporación de contenido dinámico a la definición de servicio vinculado](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Crear un nuevo parámetro](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
