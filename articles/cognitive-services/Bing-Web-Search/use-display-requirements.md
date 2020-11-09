---
title: Requisitos de uso y visualización de Bing Search APIs
titleSuffix: Azure Cognitive Services
description: Requisitos para mostrar resultados de la búsqueda de Bing Search APIs en aplicaciones.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 93be72f2afcda90dde1b74c5ee317a7ad3350be1
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93075694"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Requisitos de uso y visualización de Bing Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Estos requisitos de uso y visualización se aplican a cualquier implementación de contenido y la información asociada de las siguientes Bing Search APIs, incluidas las relaciones, los metadatos y otras señales.

- Bing Custom Search
- Bing Entity Search
- Bing Image Search
- Bing News Search
- Bing Video Search
- Bing Visual Search
- Bing Web Search
- Bing Spell Check
- Bing Autosuggest

## <a name="definitions"></a>Definiciones


|Término  |Descripción  |
|---------|---------|
|Respuesta     | Una categoría de resultados devueltos en una respuesta. Por ejemplo, una respuesta de Bing Web Search API puede incluir respuestas en las categorías de resultados de página web, imágenes, vídeos, objetos visuales y noticias. |
|Response     | Cualesquiera y todas las respuestas y los datos asociados recibidos en respuesta a una sola llamada a una API de búsqueda. |
|Resultado    | Un elemento de información en una respuesta. Por ejemplo, el conjunto de datos conectados con un único artículo de noticias es un resultado en una respuesta de noticias. |
|API de búsqueda    | De manera colectiva: las API Bing Custom Search, Entity Search, Image Search, News Search, Video Search, Visual Search, Local Business Search y Web Search. |

## <a name="bing-spell-check-and-bing-autosuggest-api-restrictions"></a>Restricciones de Bing Spell Check API y Bing Autosuggest API

No:

- Copiar, almacenar ni almacenar en caché los datos que reciba de las API Bing Spell Check ni Bing Autosuggest.
- Use los datos que reciba de Bing Spell Check API o de Bing Autosuggest API como parte de cualquier actividad de aprendizaje automático u otra actividad algorítmica similar. No utilice estos datos para entrenar, evaluar o mejorar los servicios nuevos o existentes que pueda ofrecer usted u otros terceros.

## <a name="bing-search-apis"></a>Bing Search APIs

> [!NOTE]
> Los requisitos de esta sección se aplican solo a las API de búsqueda, que no incluyen Bing Spell Check ni Bing Autosuggest. 

### <a name="internet-search-experience-requirements"></a>Requisitos para la experiencia de búsqueda en Internet

Todos los datos devueltos en las respuestas solo pueden usarse en experiencias de búsqueda en Internet. Una experiencia de búsqueda en Internet significa que el contenido presentado: 

- Es pertinente y responde a la consulta directa del usuario final u otra indicación del interés y propósito de búsqueda (por ejemplo, una consulta de búsqueda indicada por el usuario). 

- Ayuda a los usuarios a buscar y navegar a los orígenes de datos de la respuesta. Por ejemplo, al proporcionar vínculos seleccionables desde los hipervínculos en la respuesta.

- Incluye varios resultados para que el usuario seleccione. 

- Se encuentra en una posición que permite buscar a los usuarios.

- Incluye una indicación visible de que el contenido es un resultado de búsqueda de Internet. Por ejemplo, una declaración de que el contenido es "de la web".

- Incluye cualquier otra medida apropiada para garantizar que los datos de Bing Search API no incumplen ninguna ley aplicable, ni vulneran los derechos de terceros. Consulte con sus asesores legales para determinar las medidas que pueden ser adecuadas.

La única excepción a estos requisitos de experiencia de búsqueda de Internet es para la detección de direcciones URL, tal como se describe más adelante en este artículo. 

### <a name="restrictions"></a>Restricciones

No:

- Copiar, almacenar ni almacenar en caché ningún dato de las respuestas (excepto la retención en la medida permitida por la sección [Continuidad del servicio](#continuity-of-service)). 

- Utilizar los datos recibidos de las API de búsqueda como parte de cualquier actividad de aprendizaje automático u otra actividad algorítmica similar. No utilice estos datos para entrenar, evaluar o mejorar los servicios nuevos o existentes que pueda ofrecer usted u otros terceros.

- Modificar el contenido de los resultados (en otra forma que no sea el volver a darles un formato que no infrinja cualquier otro requisito) a menos que así lo exija la ley o un acuerdo con Microsoft. 

- Omitir la información de atribución y las direcciones URL asociadas al contenido de resultado.

- Volver a ordenar (también por omisión) los resultados que se presentan en una respuesta cuando se proporciona un orden o una clasificación, a menos que así lo exija la ley o un acuerdo con Microsoft. 

    > [!NOTE]
    > Este requisito no se aplica cuando la reordenación se implementa a través del portal de Bing Custom Search API.

- Presentar otro contenido dentro de cualquier parte de una respuesta de forma que pueda dar lugar a que un usuario crea que el otro contenido es parte de la respuesta. 

- Presentar publicidad que no proporcione Microsoft en cualquier página que muestre cualquier parte de una respuesta. 

- Mostrar ninguna publicidad en las páginas con las respuestas:
    - Desde las API Bing Image, News Search, Video Search ni Visual Search
    - Filtradas o limitadas principalmente (o exclusivamente) a los resultados de la búsqueda de imágenes, noticias o de vídeo o visual.

### <a name="notices-and-branding"></a>Avisos y personalización de marca 
Sí:

- Debe incluir en un lugar destacado un hipervínculo funcional a la [declaración de privacidad de Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839), cerca de cada punto en la experiencia del usuario que ofrezca a este la posibilidad de introducir una consulta de búsqueda. Etiquete el vínculo como **declaración de privacidad de Microsoft**.

- Debe incluir en un lugar destacado la marca Bing, de forma coherente con lo indicado en [Bing Trademark Usage Guidelines](https://go.microsoft.com/fwlink/?linkid=833278) (Directrices de uso de la marca comercial Bing), cerca de cada punto en la experiencia del usuario que ofrezca a este la posibilidad de introducir una consulta de búsqueda. Esta marca debe declarar claramente al usuario que es Microsoft quien ofrece la experiencia de búsqueda en Internet.

- Puede atribuir cada respuesta (o parte de una respuesta) de las que se reciben de las API Bing Web Search, Image Search, News Search, Video Search y Visual Search a Microsoft, a menos que Microsoft especifique lo contrario por escrito para su uso. Esto se describe en [Bing Trademark Usage Guidelines](https://go.microsoft.com/fwlink/?linkid=833278) (Directrices de uso de la marca comercial Bing). 

No:

- Atribuya respuestas (o partes de respuestas) de las que se reciben de Bing Custom Search API a Microsoft, a menos que Microsoft especifique lo contrario por escrito para su uso particular.

### <a name="transferring-responses"></a>Transferencia de las respuestas

Si permite a un usuario transferir una respuesta de una API de búsqueda a otro usuario, por ejemplo, a través de una aplicación de mensajería o de su publicación en redes sociales, se aplica lo siguiente: 

- Las respuestas transferidas tienen que:
  - Constar de contenido que no se haya modificado en relación al contenido de las respuestas que se presentaron al usuario que realiza la transferencia. Los cambios de formato están permitidos.
  - No incluir ningún dato en formato de metadatos.
  - Para las respuestas que se reciben de las API Bing Web, Image, News, Video y Visual, presente texto que indique que la respuesta se obtuvo a través de una experiencia de búsqueda de Internet usando la tecnología de Bing. Por ejemplo, puede presentar un texto como "Con tecnología de Bing" u "Obtener más información sobre esta imagen en Bing", o puede utilizar el logotipo de Bing.
  - Para las respuestas que se reciben de Bing Custom Search API, presente un texto que indique que la respuesta se obtuvo a través de una experiencia de búsqueda en Internet. Por ejemplo, presente un texto como "Obtener más información sobre este resultado de búsqueda”.
  - Presentar de forma destacada la consulta completa utilizada para generar la respuesta.
  - Incluir un vínculo destacado o atribución similar con el origen subyacente de la respuesta, ya sea directamente o a través del motor de búsqueda (bing.com, m.bing.com o su propio servicio de búsqueda personalizado, según sea aplicable).
- No puede automatizar la transferencia de las respuestas. Una transferencia tiene que iniciarse a partir de una acción del usuario que evidencie claramente un intento de transferir una respuesta.
- Solo puede permitir que un usuario transfiera las respuestas que haya obtenido como resultado de la consulta del usuario para realizar la transferencia.

### <a name="continuity-of-service"></a>Continuidad del servicio 

No copie, almacene o almacene en caché ningún dato de las respuestas de las API de búsqueda. Sin embargo, para habilitar la continuidad del acceso al servicio y la representación de datos, puede conservar los resultados únicamente con las siguientes condiciones:

#### <a name="device"></a>Dispositivo

Se puede permitir a los usuarios conservar los resultados en un dispositivo durante (i) 24 horas desde el momento de la consulta o (ii) hasta que un usuario envía otra consulta para obtener resultados actualizados, lo que ocurra primero, y siempre que los resultados retenidos se utilicen solamente:

- Para permitir que el usuario acceda a los resultados devueltos anteriormente a ese usuario en ese dispositivo (por ejemplo, en caso de interrupción del servicio).
- Para almacenar los resultados devueltos para la consulta proactiva personalizada en previsión de las necesidades del usuario basadas en las señales del usuario (por ejemplo, en caso de interrupción prevista del servicio).

#### <a name="server"></a>Servidor

Puede conservar los resultados específicos de un solo usuario de forma segura en un servidor bajo su control y presentar los resultados retenidos solo:

- Para permitir que el usuario final acceda a un informe histórico de resultados devueltos anteriormente para ese usuario en la solución. Los resultados (i) no se pueden conservar más de 21 días desde el momento de la consulta inicial del usuario final y (ii) no se deben presentar en respuesta a una consulta nueva o repetida de un usuario.
- Para almacenar los resultados devueltos para la consulta proactiva personalizada en previsión de las necesidades del usuario basadas en las señales del usuario. Puede almacenar estos resultados durante (i) 24 horas desde el momento de la consulta o (ii) hasta que un usuario envíe otra consulta para obtener resultados actualizados, el período que resulte menor.

Cuando se conserven, los resultados para un usuario específico no se pueden entremezclar con los resultados de otro usuario. Es decir, los resultados de cada usuario tienen que conservarse y entregarse por separado.

### <a name="general"></a>General 

Para cualquier presentación de los resultados retenidos, tiene que:

- Incluir un aviso claro y visible de la hora en la que se envió la consulta.
- Presentar al usuario un botón o medio similar para volver a consultar y obtener resultados actualizados. 
- Conservar la personalización de marca de Bing en la presentación de los resultados.
- Eliminar (y actualizar con una consulta nueva si es necesario) los resultados almacenados dentro de los períodos de tiempo especificados.

### <a name="non-display-url-discovery"></a>Detección de direcciones URL fuera de presentación 

Solo puede usar las respuestas de búsqueda en una experiencia de búsqueda sin conexión a Internet con el único fin de detectar direcciones URL de los orígenes de la información, en respuesta a una consulta desde el usuario o el cliente. Puede copiar estas direcciones URL en un informe o en una respuesta similar que usted proporcione:

- Solo a ese usuario o cliente, en respuesta a esa consulta.
- Solo si incluye un contenido significativo adicional que tenga valor y sea relevante para la consulta.

Los requisitos de uso y presentación de las secciones anteriores de las API de búsqueda no se aplican a este uso fuera de presentación, excepto en los siguientes casos: 

- No debe almacenar en caché, copiar o almacenar ningún dato o contenido de, o derivado de, la respuesta de búsqueda, que no sea la copia limitada de dirección URL que se ha descrito anteriormente.
- Tiene que garantizar que el uso de los datos (incluidas las direcciones URL) recibidos de las API de búsqueda no incumple ninguna ley aplicable ni vulnera los derechos de terceros.
- No debe usar los datos (incluidas las direcciones URL) recibidos desde las API de búsqueda como parte de cualquier índice de búsqueda o aprendizaje automático o actividad algorítmica similar. No utilice estos datos para entrenar, evaluar o mejorar los servicios que pueda ofrecer usted u otros terceros.

## <a name="gdpr-compliance"></a>Cumplimiento del GDPR  

Con respecto a los datos personales sujetos al Reglamento general de protección de datos (RGPD) y que se procesan en relación con las llamadas a las API Search, Bing Spell Check o Bing Autosuggest, comprende que usted y Microsoft son controladores independientes de los datos sujetos al RGPD. Usted es responsable de manera independiente del cumplimiento del Reglamento general de protección de datos.  

