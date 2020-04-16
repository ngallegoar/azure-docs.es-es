---
title: Entidades
description: Definición de entidades en el ámbito de la API Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: d7b9ecd048b080ae0ec9fd3fb7a4fb35009551b8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679434"
---
# <a name="entities"></a>Entidades

Una *entidad* representa un objeto movible en el espacio y es el bloque de creación fundamental del contenido representado de forma remota.

## <a name="entity-properties"></a>Propiedades de entidad

Las entidades tienen una transformación definida por una posición, rotación y escala. Por sí mismas, las entidades no tienen ninguna funcionalidad observable. En su lugar, el comportamiento se agrega a través de los componentes, que están asociados a las entidades. Por ejemplo, al adjuntar un elemento [CutPlaneComponent](../overview/features/cut-planes.md), se creará un plano de corte en la posición de la entidad.

El aspecto más importante de la propia entidad es la jerarquía y la transformación jerárquica resultante. Por ejemplo, cuando se asocian varias entidades como elementos secundarios a una entidad primaria compartida, todas estas entidades se pueden mover, girar y reducir horizontalmente al unísono cambiando la transformación de la entidad primaria.

Una entidad es propiedad exclusiva de su elemento primario, lo que significa que cuando el elemento primario se destruye con `Entity.Destroy()`, lo mismo ocurre con sus elementos secundarios y todos los [componentes](components.md) conectados. Por lo tanto, para quitar un modelo de la escena se realiza una llamada a `Destroy` en el nodo raíz de un modelo, devuelto por `AzureSession.Actions.LoadModelAsync()` o su variante `AzureSession.Actions.LoadModelFromSASAsync()` de SAS.

Las entidades se crean cuando el servidor carga el contenido o cuando el usuario desea agregar un objeto a la escena. Por ejemplo, si un usuario desea agregar un plano de corte para visualizar el interior de una malla, el usuario puede crear una entidad donde debe existir el plano y, a continuación, agregarle el componente de plano de corte.

## <a name="query-functions"></a>Funciones de consulta

Hay dos tipos de funciones de consulta en entidades: llamadas sincrónicas y asincrónicas. Las consultas sincrónicas solo se pueden usar para los datos que están presentes en el cliente y no implica mucho cálculo. Algunos ejemplos son las consultas de componentes, las transformaciones de objetos relativos o las relaciones de elementos primarios y secundarios. Las consultas asincrónicas se utilizan para los datos que solo residen en el servidor o implican cálculos adicionales cuya ejecución resultaría demasiado cara en el cliente. Algunos ejemplos son las consultas de límites espaciales o las consultas de metadatos.

### <a name="querying-components"></a>Consulta de componentes

Para buscar un componente de un tipo específico, use `FindComponentOfType`:

```cs
CutPlaneComponent cutplane = (CutPlaneComponent)entity.FindComponentOfType(ObjectType.CutPlaneComponent);

// or alternatively:
CutPlaneComponent cutplane = entity.FindComponentOfType<CutPlaneComponent>();
```

### <a name="querying-transforms"></a>Consulta de transformaciones

Las consultas de transformación son llamadas sincrónicas en el objeto. Es importante tener en cuenta que las transformaciones que se consultan a través de la API son transformaciones de espacio local, relativas al elemento primario del objeto. Las excepciones son objetos raíz, para los que el espacio local y el espacio global son idénticos.

> [!NOTE]
> No hay ninguna API dedicada para consultar la transformación de espacio global de objetos arbitrarios.

```cs
// local space transform of the entity
Double3 translation = entity.Position;
Quaternion rotation = entity.Rotation;
```

### <a name="querying-spatial-bounds"></a>Consulta de enlaces espaciales

Las consultas de límites son llamadas asincrónicas que operan en una jerarquía de objetos completa, utilizando una entidad como raíz. Vea el capítulo dedicado acerca de los [Límites de objetos](object-bounds.md).

### <a name="querying-metadata"></a>Consulta de metadatos

Los metadatos son datos adicionales almacenados en objetos que el servidor omite. Los metadatos de objeto son básicamente un conjunto de pares (nombre, valor), donde el _valor_ puede ser de tipo numérico, booleano o de cadena. Los metadatos se pueden exportar con el modelo.

Las consultas de metadatos son llamadas asincrónicas en una entidad específica. La consulta solo devuelve los metadatos de una sola entidad, no la información combinada de un subgrafo.

```cs
MetadataQueryAsync metaDataQuery = entity.QueryMetaDataAsync();
metaDataQuery.Completed += (MetadataQueryAsync query) =>
{
    if (query.IsRanToCompletion)
    {
        ObjectMetaData metaData = query.Result;
        ObjectMetaDataEntry entry = metaData.GetMetadataByName("MyInt64Value");
        System.Int64 intValue = entry.AsInt64;

        // ...
    }
};
```

La consulta se realizará correctamente incluso si el objeto no contiene metadatos.

## <a name="next-steps"></a>Pasos siguientes

* [Componentes](components.md)
* [Límites de objetos](object-bounds.md)
