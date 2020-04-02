---
title: Creación de conceptos y definición de un índice
titleSuffix: Azure Cognitive Search
description: Introducción a los términos y conceptos de índice en Azure Cognitive Search, incluidos los elementos de los componentes y la estructura física.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: d2b8b2fecbf85e6590294f1fbd7ff2a4453b9e87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79236796"
---
# <a name="create-a-basic-index-in-azure-cognitive-search"></a>Creación de un índice básico en Azure Cognitive Search

En Azure Cognitive Search, un *índice* es un almacén persistente de *documentos* y otras construcciones que se usa para las búsquedas de texto completo y filtrado en los servicios de Azure Search. Desde un punto de vista conceptual, un documento es una sola unidad de datos habilitada para búsquedas en el índice. Por ejemplo, un minorista de comercio electrónico podría tener un documento para cada objeto que vende, una organización de noticias podría tener un documento para cada artículo, etc. Estos conceptos pueden equipararse a equivalentes de base de datos más conocidos: un *índice* es conceptualmente similar a una *tabla* y los *documentos* son más o menos equivalentes a las *filas* de una tabla.

Al agregar o cargar un índice, Azure Cognitive Search crea las estructuras físicas basadas en el esquema que proporciona. Por ejemplo, si un campo del índice está marcado como habilitado para búsquedas, se crea un índice invertido para ese campo. Posteriormente, cuando agrega o carga documentos o cuando envía consultas de búsqueda a Azure Cognitive Search, está enviando las solicitudes a un índice específico del servicio de búsqueda. La carga de campos con valores de documento se denomina *indexación* o ingesta de datos.

Puede crear un índice en el portal, la [API REST](search-create-index-rest-api.md) o el [SDK de .NET](search-create-index-dotnet.md).

## <a name="recommended-workflow"></a>Flujo de trabajo recomendado

Para llegar al diseño adecuado del índice, se necesitan normalmente varias iteraciones. El uso de una combinación de API y herramientas puede ayudarle a que finalice rápidamente el diseño.

1. Determine si puede usar un [indexador](search-indexer-overview.md#supported-data-sources). Si los datos externos son uno de los orígenes de datos admitidos, puede crear un prototipo y cargar un índice mediante el [**Asistente para importar datos**](search-import-data-portal.md).

2. Si no puede usar dicho asistente, puede [crear un índice inicial en el portal](search-create-index-portal.md), y agregar campos y tipos de datos y asignar atributos mediante los controles de la página **Add Index** (Agregar índice). El portal muestra los atributos que están disponibles para diferentes tipos de datos. Si no está familiarizado con el diseño de índices, esta información resulta útil.

   ![Página Agregar índice que muestra los atributos según el tipo de datos](media/search-create-index-portal/field-attributes.png "Página Agregar índice que muestra los atributos según el tipo de datos")
  
   Al hacer clic en **Create** (Crear), se crean todas las estructuras físicas que respaldan el índice en el servicio de búsqueda.

3. Descargue el esquema de índice mediante [la API REST Get Index](https://docs.microsoft.com/rest/api/searchservice/get-index) y una herramienta de prueba web como [Postman](search-get-started-postman.md). Ahora tiene una representación JSON del índice que creó en el portal. 

   Llegados a este punto, va a cambiar a un enfoque basado en código. El portal no es adecuado para la iteración porque no puede editar un índice que ya se ha creado. Sin embargo, puede usar Postman y REST en el resto de las tareas.

4. [Cargue el índice con datos](search-what-is-data-import.md). Azure Cognitive Search acepta documentos JSON. Para cargar los datos mediante programación, puede usar Postman con documentos JSON en la carga de solicitudes. Si los datos no se expresan con facilidad como JSON, este paso será más laborioso.

5. Consulte el índice, examine los resultados y siga recorriendo en iteración el esquema de índice hasta que empiece a ver los resultados esperados. Puede usar el [**explorador de búsqueda**](search-explorer.md) o Postman para consultar el índice.

6. Continúe usando código para recorrer en iteración su diseño.  

Como se crean estructuras físicas en el servicio, cada vez que realiza cambios materiales en una definición de campo existente, es necesario [quitar y volver a crear los índices](search-howto-reindex.md). Esto significa que durante el desarrollo, debe estar preparado para recompilaciones frecuentes. Considere la posibilidad de trabajar con un subconjunto de los datos para asegurarse de que las recompilaciones van más rápido. 

Para un diseño iterativo, se recomienda código, en lugar del portal. Si confía en el portal para la definición del índice, tendrá que rellenar la definición del índice en cada recompilación. Como alternativa, herramientas como [Postman y la API REST](search-get-started-postman.md) son útiles con pruebas de concepto cuando los proyectos de desarrollo están aún en las fases iniciales. Puede realizar cambios incrementales en una definición de índice de un cuerpo de solicitud y, luego, enviar la solicitud al servicio para volver a crear un índice mediante un esquema actualizado.

## <a name="components-of-an-index"></a>Componentes de un índice

De forma esquemática, un índice de Azure Cognitive Search se compone de los siguientes elementos. 

La [*recopilación de campos*](#fields-collection) es normalmente el elemento más grande de un índice, en el que cada campo recibe un nombre, se escribe y se le asignan unos atributos con los comportamientos permitidos que determinan cómo se usa. Otros elementos incluyen [proveedores de sugerencias](#suggesters), [perfiles de puntuación](#scoring-profiles), [analizadores](#analyzers) con elementos de componentes que admitan personalización y opciones de [CORS](#cors) y [clave de cifrado](#encryption-key).

```json
{
  "name": (optional on PUT; required on POST) "name_of_index",
  "fields": [
    {
      "name": "name_of_field",
      "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
      "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
      "filterable": true (default) | false,
      "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
      "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
      "key": true | false (default, only Edm.String fields can be keys),
      "retrievable": true (default) | false,
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }
  ],
  "suggesters": [
    {
      "name": "name of suggester",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["field1", "field2", ...]
    }
  ],
  "scoringProfiles": [
    {
      "name": "name of scoring profile",
      "text": (optional, only applies to searchable fields) {
        "weights": {
          "searchable_field_name": relative_weight_value (positive #'s),
          ...
        }
      },
      "functions": (optional) [
        {
          "type": "magnitude | freshness | distance | tag",
          "boost": # (positive number used as multiplier for raw score != 1),
          "fieldName": "...",
          "interpolation": "constant | linear (default) | quadratic | logarithmic",
          "magnitude": {
            "boostingRangeStart": #,
            "boostingRangeEnd": #,
            "constantBoostBeyondRange": true | false (default)
          },
          "freshness": {
            "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
          },
          "distance": {
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
          },
          "tag": {
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)
          }
        }
      ],
      "functionAggregation": (optional, applies only when functions are specified) 
        "sum (default) | average | minimum | maximum | firstMatching"
    }
  ],
  "analyzers":(optional)[ ... ],
  "charFilters":(optional)[ ... ],
  "tokenizers":(optional)[ ... ],
  "tokenFilters":(optional)[ ... ],
  "defaultScoringProfile": (optional) "...",
  "corsOptions": (optional) {
    "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
    "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
  },
  "encryptionKey":(optional){
    "keyVaultUri": "azure_key_vault_uri",
    "keyVaultKeyName": "name_of_azure_key_vault_key",
    "keyVaultKeyVersion": "version_of_azure_key_vault_key",
    "accessCredentials":(optional){
      "applicationId": "azure_active_directory_application_id",
      "applicationSecret": "azure_active_directory_application_authentication_key"
    }
  }
}
```

<a name="fields-collection"></a>

## <a name="fields-collection-and-field-attributes"></a>Recopilación de campos y atributos de campo

Al definir el esquema, debe especificar el nombre, el tipo y los atributos de cada campo del índice. El tipo de campo permite clasificar los datos que se almacenan en ese campo. Los atributos se establecen en campos individuales para especificar cómo se usa el campo. En la tabla siguiente se enumeran los tipos y los atributos que puede especificar.

### <a name="data-types"></a>Tipos de datos
| Tipo | Descripción |
| --- | --- |
| *Edm.String* |Texto que opcionalmente se puede acortar para búsquedas de texto completo (separación de palabras, lematización, etc.). |
| *Collection(Edm.String)* |Una lista de cadenas que opcionalmente se pueden acortar para búsquedas de texto completo. En teoría, no hay ningún límite superior para el número de elementos de una colección, pero el límite de 16 MB en el tamaño de la carga se aplica a las colecciones. |
| *Edm.Boolean* |Contiene valores true/false. |
| *Edm.Int32* |Valores enteros de 32 bits. |
| *Edm.Int64* |Valores enteros de 64 bits. |
| *Edm.Double* |Datos numéricos de precisión doble. |
| *Edm.DateTimeOffset* |Los valores de fecha y hora se representan con el formato OData V4 (por ejemplo, `yyyy-MM-ddTHH:mm:ss.fffZ` o `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Un punto que representa una ubicación geográfica en todo el mundo. |

Puede encontrar información más detallada sobre los [tipos de datos de Azure Cognitive Search admitidos aquí](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="index-attributes"></a>Atributos de índice

Solamente un campo del índice tiene que designarse como campo **clave** que identifica de forma exclusiva cada documento.

Los demás atributos determinan cómo se utiliza un campo en una aplicación. Por ejemplo, el atributo **searchable** se asigna a todos los campos que deben incluirse en una búsqueda de texto completo. 

Las API que usa para crear un índice tienen distintos comportamientos predeterminados. En el caso de las [API REST](https://docs.microsoft.com/rest/api/searchservice/Create-Index), la mayoría de los atributos están habilitados de forma predeterminada (por ejemplo, **searchable** y **retrievable** son true para los campos de cadena) y a menudo solo deberá establecerlos si desea desactivarlos. Para el SDK de .NET, ocurre lo contrario. En cualquier propiedad que no establezca de forma explícita, el valor predeterminado es que el comportamiento de búsqueda correspondiente esté deshabilitado, a menos que lo habilite específicamente.

| Atributo | Descripción |
| --- | --- |
| `key` |Una cadena que proporciona el identificador único de cada documento, que se usa para buscar los documentos. Todos los índices deben tener una clave. Solo un campo puede ser la clave y se debe establecer su tipo en Edm.String. |
| `retrievable` |Establece si el campo se puede devolver en un resultado de búsqueda. |
| `filterable` |Permite que el campo se use en consultas de filtro. |
| `Sortable` |Permite que una consulta ordene los resultados de búsqueda mediante este campo. |
| `facetable` |Permite que un campo se use en una estructura de [navegación con facetas](search-faceted-navigation.md) para el filtrado autodirigido. Normalmente los campos que contienen valores repetitivos que se pueden usar para agrupar varios documentos (por ejemplo, varios documentos que forman parte de una única categoría de servicio o un único producto) funcionan mejor como facetas. |
| `searchable` |Marca el campo como campo de búsqueda de texto completo. |

## <a name="index-size"></a>Tamaño de índice

El tamaño de un índice viene determinado por el tamaño de los documentos que se cargan, además de la configuración del índice, por ejemplo, si incluye los proveedores de sugerencias y cómo establece los atributos en los campos individuales. La siguiente captura de pantalla ilustra los patrones de almacenamiento de índices resultantes de diversas combinaciones de atributos.

El índice se basa en el origen de datos [ejemplo de real estate integrado](search-get-started-portal.md), que se puede indexar y consultar en el portal. Aunque no se muestran los esquemas de índice, puede deducir los atributos según el nombre del índice. Por ejemplo, el índice *realestate-searchable* tiene seleccionado el atributo **searchable** y nada más, el índice *realestate-retrievable* tiene seleccionado el atributo **retrievable** y nada más y así sucesivamente.

![Tamaño del índice basado en la selección de atributos](./media/search-what-is-an-index/realestate-index-size.png "Tamaño del índice basado en la selección de atributos")

Aunque estas variantes de índice son artificiales, se puede hacer referencia a ellas en comparaciones más amplias de cómo los atributos afectan al almacenamiento. ¿El valor **retrievable** aumenta el tamaño del índice? No. ¿Agregar campos a un **proveedor de sugerencias** aumenta el tamaño del índice? Sí.

Los índices que admiten la ordenación y el filtrado son en proporción más grandes que aquellos que solo admiten la búsqueda de texto completo. Las operaciones de filtro y ordenación buscan coincidencias exactas, lo que requiere la presencia de documentos intactos. En cambio, los campos que permiten búsquedas que admiten búsqueda de texto completo y búsqueda parcial usan índices invertidos, que se rellenan con términos acortados que consumen menos espacio que los documentos completos. 

> [!Note]
> La arquitectura de almacenamiento se considera un detalle de implementación de Azure Cognitive Search y podría cambiar sin previo aviso. No hay ninguna garantía de que el comportamiento actual se conserve en el futuro.

## <a name="suggesters"></a>Proveedores de sugerencias
Un proveedor de sugerencias es una sección del esquema que define qué campos de un índice se utilizan para admitir consultas con la función Autocompletar o con escritura automática en las búsquedas. Normalmente, las cadenas de búsqueda parcial se envían a [Sugerencias (API REST)](https://docs.microsoft.com/rest/api/searchservice/suggestions) mientras el usuario escribe una consulta de búsqueda, y la API devuelve un conjunto de documentos o frases sugeridas. 

Los campos agregados a un proveedor de sugerencias se usan para compilar los términos de búsqueda de escritura anticipada. Todos los términos de búsqueda se crean durante la indexación y se almacenan por separado. Para más información sobre la creación de una estructura de proveedor de sugerencias, consulte [Incorporación de proveedores de sugerencias](index-add-suggesters.md).

## <a name="scoring-profiles"></a>Perfiles de puntuación

Un [perfil de puntuación](index-add-scoring-profiles.md) es una sección del esquema que define comportamientos de puntuación personalizados que permiten influir en los elementos que aparecen más arriba en los resultados de la búsqueda. Los perfiles de puntuación se componen de ponderaciones de campos y de funciones. Para poder utilizarlos, especifique un perfil por nombre en la cadena de consulta.

El perfil de puntuación predeterminada funciona en segundo plano para calcular un resultado de búsqueda para todos los elementos de un conjunto de resultados. Puede usar un perfil de puntuación interno y sin nombre. Como alternativa, configure **defaultScoringProfile** para que use un perfil personalizado como valor predeterminado, al que se invoca cuando no se especifica un perfil personalizado en la cadena de consulta.

## <a name="analyzers"></a>Analizadores

El elemento de analizadores establece el nombre del analizador de idioma que se utilizará para el campo. Para más información sobre el intervalo de analizadores disponibles, consulte [Adición de analizadores a un índice de Azure Cognitive Search](search-analyzers.md). Los analizadores solo pueden usarse con campos de que permiten búsquedas. Una vez que el analizador se asigna a un campo, no se puede cambiar a menos que se vuelva a generar el índice.

## <a name="cors"></a>CORS

JavaScript del lado cliente no puede llamar a ninguna API de forma predeterminada debido a que el explorador impedirá todas las solicitudes entre orígenes. Para permitir consultas de origen cruzado en el índice, habilite CORS (uso compartido de recursos entre orígenes) estableciendo el atributo **corsOptions**. Por motivos de seguridad, solamente las API de consulta admiten CORS. 

Se pueden establecer las opciones siguientes para CORS:

+ **allowedOrigins** (obligatorio): se trata de una lista de orígenes a los que se le concederá acceso a su índice. Esto significa que cualquier código JavaScript que se suministre desde esos orígenes podrá consultar el índice (suponiendo que proporcione la clave de API correcta). Cada origen tiene normalmente el formato `protocol://<fully-qualified-domain-name>:<port>` aunque `<port>` se omite a menudo. Para más información, consulte [Uso compartido de recursos entre orígenes (wikipedia)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

  Si desea permitir el acceso a todos los orígenes, incluya `*` como elemento único en la matriz **allowedOrigins**. *Esto no es recomendable para los servicios de búsqueda de producción* pero a menudo resulta útil para el desarrollo y la depuración.

+ **maxAgeInSeconds** (opcional): los exploradores usan este valor para determinar la duración (en segundos) para almacenar en la memoria caché las respuestas preparatorias de CORS. Esto debe ser un entero no negativo. Cuanto mayor sea este valor es, mejor será el rendimiento, pero más tiempo tardarán en surtir efecto los cambios en la directiva CORS. Si no se establece, se usará una duración predeterminada de 5 minutos.

## <a name="encryption-key"></a>Clave de cifrado

Mientras que todos los índices de Azure Cognitive Search se cifran mediante claves administradas por Microsoft de manera predeterminada, los índices se pueden configurar para cifrarse con **claves administradas por el cliente** en Key Vault. Para más información, consulte [Administración de claves de cifrado en Azure Cognitive Search](search-security-manage-encryption-keys.md).

## <a name="next-steps"></a>Pasos siguientes

Con una descripción de la composición del índice, puede continuar en el portal para crear el primer índice.

> [!div class="nextstepaction"]
> [Incorporación de un índice (portal)](search-create-index-portal.md)
