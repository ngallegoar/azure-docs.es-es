---
title: 'Modelo de diseño de Azure Cosmos DB: Aplicaciones de redes sociales'
description: Obtenga información sobre un patrón de diseño para redes sociales con la flexibilidad de almacenamiento de Azure Cosmos DB y otros servicios de Azure.
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: maquaran
ms.openlocfilehash: 329c4b40f11b36de80581d4a1396813bc8de5c73
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097335"
---
# <a name="going-social-with-azure-cosmos-db"></a>Redes sociales y Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Vivir en una sociedad enormemente interconectada significa que, en algún momento de la vida, uno formará parte de una **red social**. Las redes sociales se usan para mantenerse en contacto con amigos, compañeros de trabajo y familiares y, a veces, para compartir intereses comunes con otras personas.

Como ingeniero o desarrollador, puede que se haya preguntado cómo es que estas redes almacenan e interconectan sus datos. O bien es posible que incluso le hayan encargado crear o diseñar una nueva red social para un segmento de mercado específico. Es ahí cuando surge la pregunta importante: ¿Cómo se almacenan todos estos datos?

Suponga que quiere crear una red social nueva en la que los usuarios puedan publicar artículos y materiales como imágenes, vídeos o incluso música. En esta red social, los usuarios podrán comentar y valorar las publicaciones con fines de clasificación. Además, en la página de aterrizaje del sitio web, los usuarios verán e interactuarán con una fuente de publicaciones. Este método no parece complejo en un principio, pero por motivos de simplicidad, nos detendremos allí. (Puede profundizar en las fuentes de usuario personalizadas que son afectadas por las relaciones, pero eso va más allá del objetivo de este artículo).

Entonces, ¿cómo y dónde almacena estos datos?

Debe tener experiencia en bases de datos SQL o al menos tener nociones de [modelado de datos relacional](https://en.wikipedia.org/wiki/Relational_model). Puede empezar a dibujar algo como lo siguiente:

:::image type="content" source="./media/social-media-apps/social-media-apps-sql.png" alt-text="Diagrama en el que se ilustra un modelo relacional relativo" border="false":::

Sin embargo, a pesar de ser una estructura de datos perfectamente normalizada, no se escala.

No me malinterprete, he trabajado con bases de datos SQL toda mi vida. Son muy importantes, pero al igual que todos los patrones, prácticas y plataformas de software, no son perfectas para todos los escenarios.

¿Por qué SQL no es la mejor opción en este escenario? Echemos un vistazo a la estructura de una sola publicación. Si quisiéramos esa publicación en un sitio web o una aplicación, tendríamos que realizar una consulta WITH... mediante la combinación de ocho tablas(!) para mostrar una sola publicación. Ahora, cree una secuencia de entradas que se cargue dinámicamente y aparezcan en la pantalla.

Podría usar una instancia de SQL enorme con capacidad suficiente para resolver miles de consultas con muchas combinaciones para servir el contenido. Pero, ¿por qué habría de hacerlo cuando existe una solución más sencilla?

## <a name="the-nosql-road"></a>La vía NoSQL

En este artículo, se le guiará por el proceso de modelar los datos de su plataforma social con la base de datos de NoSQL de Azure, [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), de forma rentable. También, se le indicará cómo usar otras características de Azure Cosmos DB, como la [Gremlin API](../cosmos-db/graph-introduction.md). Con un enfoque [NoSQL](https://en.wikipedia.org/wiki/NoSQL), almacenamiento de datos en formato JSON y la aplicación de [desnormalización](https://en.wikipedia.org/wiki/Denormalization), la publicación, que antes era complicada, ahora puede transformarse en un único [documento](https://en.wikipedia.org/wiki/Document-oriented_database):

```json
{
    "id":"ew12-res2-234e-544f",
    "title":"post title",
    "date":"2016-01-01",
    "body":"this is an awesome post stored on NoSQL",
    "createdBy":User,
    "images":["https://myfirstimage.png","https://mysecondimage.png"],
    "videos":[
        {"url":"https://myfirstvideo.mp4", "title":"The first video"},
        {"url":"https://mysecondvideo.mp4", "title":"The second video"}
    ],
    "audios":[
        {"url":"https://myfirstaudio.mp3", "title":"The first audio"},
        {"url":"https://mysecondaudio.mp3", "title":"The second audio"}
    ]
}
```

Además, puede obtenerse con una sola consulta y sin combinaciones. Esta consulta es mucho más sencilla, directa, económica y requiere menos recursos para conseguir un mejor resultado.

Azure Cosmos DB se asegura de que todas las propiedades se indexen con su indexación automática. Además, puede [personalizar](index-policy.md) la indexación automática. El enfoque sin esquema nos permite almacenar documentos con estructuras dinámicas y diferentes. ¿Tal vez mañana quiera que las publicaciones tengan una lista de categorías o hashtags asociados a ellas? Cosmos DB gestionará los documentos nuevos con los atributos agregados sin requerir esfuerzo adicional de nuestra parte.

Los comentarios en una publicación pueden tratarse del mismo modo que otras publicaciones con una propiedad primaria. (Esta práctica simplifica la asignación de objetos).

```json
{
    "id":"1234-asd3-54ts-199a",
    "title":"Awesome post!",
    "date":"2016-01-02",
    "createdBy":User2,
    "parent":"ew12-res2-234e-544f"
}

{
    "id":"asd2-fee4-23gc-jh67",
    "title":"Ditto!",
    "date":"2016-01-03",
    "createdBy":User3,
    "parent":"ew12-res2-234e-544f"
}
```

Por otro lado, todas las interacciones sociales pueden almacenarse en un objeto independiente como contadores:

```json
{
    "id":"dfe3-thf5-232s-dse4",
    "post":"ew12-res2-234e-544f",
    "comments":2,
    "likes":10,
    "points":200
}
```

Para la creación de fuentes solo es necesario crear documentos que puedan contener una lista de identificadores de publicación con un orden de importancia determinado:

```json
[
    {"relevance":9, "post":"ew12-res2-234e-544f"},
    {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
    {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
]
```

Podría tener una secuencia "más recientes" con publicaciones ordenadas por fecha de creación. O bien podría tener una secuencia "más populares" con las publicaciones que han recibido más "me gusta" en las últimas 24 horas. Incluso podría implementar una secuencia personalizada para cada usuario según alguna lógica, como los seguidores e intereses. Aún así, sería una lista de publicaciones. Sin importar cómo cree estas listas, el rendimiento de lectura no se ve afectado. Una vez que adquiere una de estas listas, emite una consulta única a Cosmos DB con la [palabra clave IN](sql-query-keywords.md#in) para obtener páginas de publicaciones cada vez.

Los flujos de fuente se pueden generar mediante procesos en segundo plano de [Azure App Services](https://azure.microsoft.com/services/app-service/): [Trabajos web](../app-service/webjobs-create.md). Una vez que se crea una publicación, el procesamiento en segundo plano puede activarse mediante el uso de [Azure Storage](https://azure.microsoft.com/services/storage/) [Queues](../storage/queues/storage-dotnet-how-to-use-queues.md) y Webjobs desencadenados mediante el [SDK Azure Webjobs](https://github.com/Azure/azure-webjobs-sdk/wiki), implementando la propagación de publicaciones dentro de los flujos en función de su propia lógica personalizada.

La puntuación y los "me gusta" de una publicación se pueden procesar de manera aplazada usando esta misma técnica para crear un entorno coherente.

Con los seguidores es más complicado. Cosmos DB tiene un límite de tamaño para los documentos, por lo que la lectura o escritura de documentos de gran tamaño puede afectar a la escalabilidad de la aplicación. Por esta razón, debería plantearse almacenar los seguidores como un documento con esta estructura:

```json
{
    "id":"234d-sd23-rrf2-552d",
    "followersOf": "dse4-qwe2-ert4-aad2",
    "followers":[
        "ewr5-232d-tyrg-iuo2",
        "qejh-2345-sdf1-ytg5",
        //...
        "uie0-4tyg-3456-rwjh"
    ]
}
```

Esta estructura podría funcionar con un usuario con unos miles de seguidores. Sin embargo, si se trata de una persona famosa, este enfoque produciría un tamaño de documento grande y terminaría superándose el límite de tamaño de documentos.

Para solucionar este problema, podemos adoptar un enfoque mixto. Como parte del documento Estadísticas de usuario, se puede almacenar el número de seguidores:

```json
{
    "id":"234d-sd23-rrf2-552d",
    "user": "dse4-qwe2-ert4-aad2",
    "followers":55230,
    "totalPosts":452,
    "totalPoints":11342
}
```

Y puede almacenar el gráfico real de los seguidores con la [Gremlin API](../cosmos-db/graph-introduction.md) de Azure Cosmos DB, para crear [vértices](http://mathworld.wolfram.com/GraphVertex.html) para cada usuario y [bordes](http://mathworld.wolfram.com/GraphEdge.html) que mantienen relaciones del tipo "A sigue a B". Con la Gremlin API, puede obtener los seguidores de un determinado usuario y crear consultas más complejas para sugerir personas en común. Si se agregan al gráfico las categorías de contenido que gustan o encantan a los individuos, podemos comenzar a componer escenarios que incluyen detección inteligente de contenido, sugerencias de contenido que gustan a las personas a las que seguimos o encontrar personas con quienes puede que se tenga mucho en común.

El documento de Estadísticas de usuario se sigue pudiendo usar para crear tarjetas en la interfaz de usuario o vistas previas rápidas de los perfiles.

## <a name="the-ladder-pattern-and-data-duplication"></a>El modelo "escalera" y la duplicación de datos

Como habrá observado en el documento JSON que hace referencia a una publicación, hay muchas coincidencias de un usuario. Y, como ya habrá imaginado, estos duplicados significan que la información que describe a un usuario, dada esta desnormalización, puede existir en más de un lugar.

Para permitir consultas más rápidas, se incurre en la duplicación de datos. El problema con este efecto secundario es que si, por alguna acción, cambian los datos de un usuario, habrá que buscar todas las actividades que ha hecho y actualizarlas. Lo cierto es que no parece muy práctico.

Se va a resolver mediante la identificación de los atributos clave de un usuario que se muestran en la aplicación para cada actividad. Si en nuestra aplicación se muestra una publicación tan solo con el nombre y la imagen del creador, ¿por qué almacenar todos los datos del usuario en el atributo "createdBy"? Si en cada comentario solo se muestra la imagen del usuario, no es necesario el resto de su información. Y aquí es donde entra en juego lo que yo llamo el modelo "escalera".

Tomemos como ejemplo información de usuario:

```json
{
    "id":"dse4-qwe2-ert4-aad2",
    "name":"John",
    "surname":"Doe",
    "address":"742 Evergreen Terrace",
    "birthday":"1983-05-07",
    "email":"john@doe.com",
    "twitterHandle":"\@john",
    "username":"johndoe",
    "password":"some_encrypted_phrase",
    "totalPoints":100,
    "totalPosts":24
}
```

Al examinar esta información, podemos detectar rápidamente cuál es información importante y cuál no, creando así una "escalera":

:::image type="content" source="./media/social-media-apps/social-media-apps-ladder.png" alt-text="Diagrama de un patrón de escalera" border="false":::

El paso más pequeño se denomina UserChunk: es el fragmento mínimo de información que identifica a un usuario y se usa para la duplicación de datos. Al reducir el tamaño de los datos duplicados a tan solo la información que "se muestra", se reduce también la posibilidad de actualizaciones masivas.

El paso intermedio se denomina usuario. Son todos los datos que se usarán en la mayoría de las consultas dependientes del rendimiento en Cosmos DB: los datos a los que más accede y los más críticos. Incluye la información representada por un UserChunk.

El mayor es el usuario extendido. Incluye la información crítica del usuario y otros datos que no es necesario leer rápidamente ni tienen un posible uso, como el proceso de inicio de sesión. Estos datos pueden almacenarse fuera de Cosmos DB, en Azure SQL Database o en tablas de Azure Storage.

¿Por qué se habría de dividir el usuario e incluso almacenar esta información en diferentes lugares? Porque desde el punto de vista del rendimiento, cuanto mayores sean los documentos, más costosas serán las consultas. Mantenga los documentos pequeños con la información necesaria para realizar todas las consultas dependientes del rendimiento de la red social. Almacene el resto de la información adicional para escenarios probables (como modificaciones del perfil completo, inicios de sesión y minería de datos) para los análisis de uso y las iniciativas de macrodatos. Realmente no importa si la recopilación de datos para minería de datos es más lenta, ya que se ejecuta en Azure SQL Database. Lo importante es que sus usuarios tengan una experiencia ligera y rápida. La apariencia de un usuario almacenado en Cosmos DB sería similar al código siguiente:

```json
{
    "id":"dse4-qwe2-ert4-aad2",
    "name":"John",
    "surname":"Doe",
    "username":"johndoe"
    "email":"john@doe.com",
    "twitterHandle":"\@john"
}
```

Asimismo, una solicitud Post tendría el aspecto siguiente:

```json
{
    "id":"1234-asd3-54ts-199a",
    "title":"Awesome post!",
    "date":"2016-01-02",
    "createdBy":{
        "id":"dse4-qwe2-ert4-aad2",
        "username":"johndoe"
    }
}
```

Cuando se produce una edición que afecta a un atributo de fragmento, puede encontrar fácilmente a los documentos afectados. Use consultas que apunten a los atributos indizados, como `SELECT * FROM posts p WHERE p.createdBy.id == "edited_user_id"` y, a continuación, actualice los fragmentos.

## <a name="the-search-box"></a>El cuadro de búsqueda

Los usuarios generarán, con suerte, gran cantidad de contenido. Debe proporcionar la capacidad de buscar y encontrar contenido que podría no estar directamente en los flujos de contenido de los usuarios, quizás porque no siguen a los creadores o quizás porque están buscando una publicación antigua de hace seis meses.

Gracias al empleo de Azure Cosmos DB, se puede implementar fácilmente un motor de búsqueda con [Azure Cognitive Search](https://azure.microsoft.com/services/search/) en un par de minutos y sin escribir una sola línea de código, aparte del proceso de búsqueda y la interfaz de usuario.

¿Por qué es tan fácil este proceso?

Azure Cognitive Search implementa lo que llaman "[indexadores](/rest/api/searchservice/Indexer-operations)"; es decir, procesos en segundo plano que se enlazan en los repositorios de datos y automáticamente agregan, actualizan o quitan objetos en los índices. Son compatibles con [indexadores de Azure SQL Database](/archive/blogs/kaevans/indexing-azure-sql-database-with-azure-search), [indexadores de Blobs de Azure](../search/search-howto-indexing-azure-blob-storage.md) y, afortunadamente, [indexadores de Cosmos DB](../search/search-howto-index-cosmosdb.md). La transición de la información de Cosmos DB a Azure Cognitive Search es sencilla. Ambas tecnologías almacenan información en formato JSON, por lo que basta con [crear el índice](../search/search-what-is-an-index.md) y asignar los atributos de los documentos que quiere indizar. ¡Ya está! Según el tamaño de los datos, todo el contenido estará disponible para realizar búsquedas en cuestión de minutos con la ayuda de la mejor solución de búsquedas como servicio en la infraestructura en la nube.

Para obtener más información sobre Azure Cognitive Search, puede consultar la [Guía de búsqueda de Hitchhiker](/archive/blogs/mvpawardprogram/a-hitchhikers-guide-to-search).

## <a name="the-underlying-knowledge"></a>La información subyacente

Después de almacenar todo este contenido que crece cada día más, es posible que se pregunte lo siguiente: ¿Qué puedo hacer con todo este flujo de información de mis usuarios?

La respuesta es sencilla: póngalo a trabajar y aprenda de él.

Pero, ¿qué se puede aprender? Por ejemplo, [análisis de sentimiento](https://en.wikipedia.org/wiki/Sentiment_analysis), recomendaciones de contenido según las preferencias de un usuario o, incluso, un moderador automatizado que se asegura de que el contenido que publique nuestra red social sea adecuado para todos los públicos.

Ahora que ya está interesado, probablemente pensará que necesita un doctorado en ciencias matemáticas para extraer estos patrones y la información de los archivos y las bases de datos, pero no es así.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), que forma parte de [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), es un servicio en la nube totalmente administrado que permite crear flujos de trabajo mediante algoritmos en una sencilla interfaz de arrastrar y colocar, programar sus propios algoritmos en [R](https://en.wikipedia.org/wiki/R_\(programming_language\)) o usar algunas de las API integradas y listas para usar, como: [Text Analytics](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), Content Moderator o [Recommendations](https://gallery.azure.ai/Solution/Recommendations-Solution).

Para posibilitar cualquiera de estos escenarios de Machine Learning, puede usar [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) para ingerir la información de distintas fuentes. También puede usar [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) para procesar la información y generar una salida que Azure Machine Learning pueda procesar.

Otra opción disponible es usar [Azure Cognitive Services](https://www.microsoft.com/cognitive-services) para analizar el contenido de los usuarios; no solo se puede comprender mejor (mediante el análisis de lo que escriben con [Text Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), sino que también se puede detectar contenido no deseado o contenido para adultos y actuar en consecuencia con [Computer Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Cognitive Services incluye una gran cantidad de soluciones listas para usar que no requieren ningún conocimiento de Machine Learning para usarlas.

## <a name="a-planet-scale-social-experience"></a>Una experiencia social a escala mundial

Hay un último, pero no por ello menos importante, artículo que abordaremos: la **escalabilidad**. Al diseñar una arquitectura, cada componente debe escalarse por sí mismo. En algún momento tendrá que procesar más datos o querrá ampliar su cobertura geográfica. Afortunadamente, llevar a cabo ambas tareas es una **experiencia inmediata** con Cosmos DB.

Cosmos DB admite particiones dinámicas de fábrica. Automáticamente crea particiones basadas en una **clave de partición** específica, que se define como atributo en sus documentos. Debe definir la clave de partición correcta durante el tiempo de diseño. Para más información, consulte [Creación de particiones en Azure Cosmos DB](partitioning-overview.md).

Para obtener una experiencia social, debe alinear la estrategia de creación de particiones con el modo de consulta y escritura. (Por ejemplo, es conveniente tener lecturas en la misma partición, así como evitar las "zonas activas" al propagar los procesos de escritura en varias particiones). Algunas opciones son: las particiones basadas en una clave temporal (día/mes/semana), por categoría de contenido, por región geográfica o por usuario. Todo depende de cómo va a consultar los datos y cómo va a mostrarlos en la experiencia social.

Cosmos DB ejecutará las consultas (incluidas las [agregaciones](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) en todas las particiones de forma transparente, por lo que no necesita agregar ninguna lógica a medida que crecen los datos.

Con el tiempo, el tráfico crecerá y su consumo de recursos (que se mide en [RU](request-units.md) o unidades de solicitud) aumentará. Leerá y escribirá con más frecuencia a medida que crece su base de usuarios. La base de usuarios comenzará a crear y a leer más contenido. Por lo que la capacidad de **escalar el rendimiento** es fundamental. Es fácil aumentar las RU. Se puede hacer con algunos clics en Azure Portal o a través de la [emisión de comandos mediante la API](/rest/api/cosmos-db/replace-an-offer).

:::image type="content" source="./media/social-media-apps/social-media-apps-scaling.png" alt-text="Escalado vertical y definición de una clave de partición":::

¿Qué ocurre si las cosas siguen mejorando? Supongamos que los usuarios de otro continente, país o región descubren su plataforma y empiecen a usarla. ¡Qué fantástica sorpresa!

Pero espere un momento. Pronto se dará cuenta de que su experiencia con la plataforma no es óptima. Están tan lejos de su región operativa que la latencia es enorme. Obviamente, no quiere que abandonen su red social. ¡Si tan solo hubiese una forma sencilla de **extender su alcance global** ! Y la hay.

Cosmos DB le permite [replicar los datos global](../cosmos-db/tutorial-global-distribution-sql-api.md) y transparentemente con un par de clics y seleccionar de forma automática entre las regiones disponibles del [código de cliente](../cosmos-db/tutorial-global-distribution-sql-api.md). Este proceso también significa que tiene [varias regiones de conmutación por error](high-availability.md).

Cuando replica globalmente los datos, debe asegurarse de que los clientes puedan aprovecharlos. Si usa un front-end web o tiene acceso a las API desde clientes para dispositivos móviles, puede implementar [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) y clonar la instancia de Azure App Service en todas las regiones deseadas mediante una configuración de rendimiento para admitir la cobertura global extendida. Cuando los clientes tienen acceso al front-end o a las API, se enrutan a la instancia de App Service más cercana que, a su vez, se conecta a la réplica local de Cosmos DB.

:::image type="content" source="./media/social-media-apps/social-media-apps-global-replicate.png" alt-text="Incorporación de la cobertura global en la plataforma social" border="false":::

## <a name="conclusion"></a>Conclusión

En este artículo se arroja luz sobre las alternativas para crear redes sociales completas en Azure con servicios de bajo costo. Se ofrecen resultados al fomentar el uso de una solución de almacenamiento y distribución de datos de varias capas denominada "escalera".

:::image type="content" source="./media/social-media-apps/social-media-apps-azure-solution.png" alt-text="Diagrama de interacción entre los servicios de Azure para redes sociales" border="false":::

La verdad es que no hay ninguna solución mágica para este tipo de escenarios. Es la sinergia creada mediante la combinación de excelentes servicios lo que nos permite crear grandes experiencias: la velocidad y la libertad de Azure Cosmos DB para proporcionar una gran aplicación social; la inteligencia de una solución de búsqueda de primera clase como Azure Cognitive Search; la flexibilidad de Azure App Services para hospedar aplicaciones independientes del lenguaje y eficaces procesos en segundo plano; los ampliables Azure Storage y Azure SQL Database para guardar cantidades ingentes de datos; y la potencia analítica de Azure Machine Learning para crear conocimiento e inteligencia que proporcionen información a nuestros procesos y nos ayuden a suministrar el contenido correcto a los usuarios adecuados.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los casos de uso de Cosmos DB, consulte [Casos de uso comunes de Cosmos DB](use-cases.md).