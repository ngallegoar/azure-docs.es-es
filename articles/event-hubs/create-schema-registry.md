---
title: Creación de un registro de esquemas de Azure Event Hubs
description: En este artículo se muestra cómo crear un registro de esquemas en un espacio de nombres de Azure Event Hubs.
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 90556e0843cda94dc79330321d027f8d28eb7d20
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91652200"
---
# <a name="create-an-azure-event-hubs-schema-registry-preview"></a>Creación de un registro de esquemas de Azure Event Hubs (versión preliminar)
En este artículo se muestra cómo crear un grupo de esquemas con esquemas en un registro de esquemas hospedado en Azure Event Hubs. Para obtener información general sobre la característica de Registro de esquemas de Azure Event Hubs, vea [Registro de esquemas de Azure en Event Hubs](schema-registry-overview.md).

> [!NOTE]
> - La característica de **registro de esquema** se encuentra actualmente en **versión preliminar** y no se recomienda para las cargas de trabajo de producción.
> - La característica solo está disponible en los niveles **estándar** y **dedicado**, no en el nivel **básico**.

## <a name="prerequisites"></a>Requisitos previos
[Cree un espacio de nombres de Event Hubs](event-hubs-create.md#create-an-event-hubs-namespace). También puede usar un espacio de nombres existente. 

## <a name="create-a-schema-group"></a>Creación de un grupo de esquemas
1. Desplácese a la página **Espacio de nombres de Event Hubs**. 
1. Seleccione **Schema Registry** (Registro de esquemas) en el menú de la izquierda. Para crear un grupo de esquemas, seleccione **+ Schema Group** (+ Grupo de esquemas) en la barra de herramientas. 

    :::image type="content" source="./media/create-schema-registry/namespace-page.png" alt-text="Página Registro de esquemas":::
1. En la página **Crear grupo de esquemas**, siga estos pasos:
    1. Escriba un **nombre** para el grupo de esquemas.
    1. En **Tipo de serialización**, elija un formato de serialización que se aplique a todos los esquemas del grupo de esquemas. Actualmente, **Avro** es el único tipo admitido, así que seleccione **Avro**. 
    1. Seleccione un **modo de compatibilidad** para todos los esquemas del grupo. En el caso de **Avro**, se admiten los modos de compatibilidad con versiones anteriores y posteriores. 
    1. Después, seleccione **Crear** para crear el grupo de esquemas. 
1. Seleccione el nombre del **grupo de esquemas** en la lista de grupos de esquemas.

    :::image type="content" source="./media/create-schema-registry/select-schema-group.png" alt-text="Página Registro de esquemas":::    
1. Verá la página de **Grupo de esquemas** para el grupo.

    :::image type="content" source="./media/create-schema-registry/schema-group-page.png" alt-text="Página Registro de esquemas":::
    

## <a name="add-a-schema-to-the-schema-group"></a>Incorporación de un esquema al grupo de esquemas
En esta sección, agregará un esquema al grupo de esquemas mediante Azure Portal. 

1. En la página **Grupo de esquemas**, seleccione **+ Schema** (+ Esquema) en la barra de herramientas. 
1. En la página **Crear esquema**, siga estos pasos:
    1. Escriba un **nombre** para el esquema.
    1. Escriba el siguiente **esquema** en el cuadro de texto. También puede seleccionar un archivo con el esquema.
    
        ```json
        {
            "type": "record",
            "name": "AvroUser",
            "namespace": "com.azure.schemaregistry.samples",
            "fields": [
                {
                    "name": "name",
                    "type": "string"
                },
                {
                    "name": "favoriteNumber",
                    "type": "int"
                }
            ]
        }
        ```
    1. Seleccione **Crear**. 
1. Seleccione el **esquema** de la lista de esquemas. 

    :::image type="content" source="./media/create-schema-registry/select-schema.png" alt-text="Página Registro de esquemas":::
1. Verá la página de **Información general del esquema** del esquema. 

    :::image type="content" source="./media/create-schema-registry/schema-overview-page.png" alt-text="Página Registro de esquemas":::    
1. Si hay varias versiones de un esquema, se verán en la lista desplegable **Versiones**. Seleccione una versión para cambiar a ese esquema de versiones. 

## <a name="create-a-new-version-of-schema"></a>Creación de una nueva versión del esquema

1. Actualice el esquema en el cuadro de texto y seleccione **Validar**. En el ejemplo siguiente, se ha agregado un nuevo campo `id` al esquema. 

    :::image type="content" source="./media/create-schema-registry/update-schema.png" alt-text="Página Registro de esquemas":::    
    
1. Revise el estado y los cambios de validación y seleccione **Guardar**. 

    :::image type="content" source="./media/create-schema-registry/compare-save-schema.png" alt-text="Página Registro de esquemas":::     
1. Verá que se ha seleccionado `2` para la **versión** en la página de **Información general del esquema**. 

    :::image type="content" source="./media/create-schema-registry/new-version.png" alt-text="Página Registro de esquemas":::    
1. Seleccione `1` para ver la versión 1 del esquema. 

    :::image type="content" source="./media/create-schema-registry/select-version.png" alt-text="Página Registro de esquemas":::    


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el registro de esquemas, vea [Registro de esquemas de Azure en Event Hubs](schema-registry-overview.md).

