---
title: 'Recursos de Azure: QnA Maker'
description: QnA Maker usa varios orígenes de Azure, cada uno con un propósito diferente. Entender cómo se usan individualmente le permite planear y seleccionar el plan de tarifa correcto o saber cuándo debe cambiar el plan de tarifa. Entender cómo se usan en combinación le permite encontrar y corregir los problemas cuando se producen.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 46b1cf2681ab5d804035c98d458600de5081c77d
ms.sourcegitcommit: 051908e18ce42b3b5d09822f8cfcac094e1f93c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94376803"
---
# <a name="azure-resources-for-qna-maker"></a>Recursos de Azure para QnA Maker

QnA Maker usa varios orígenes de Azure, cada uno con un propósito diferente. Entender cómo se usan individualmente le permite planear y seleccionar el plan de tarifa correcto o saber cuándo debe cambiar el plan de tarifa. Entender cómo se usan _en combinación_ le permite encontrar y corregir los problemas cuando se producen.

## <a name="resource-planning"></a>Planeamiento de recursos

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

Cuando se desarrolla por primera vez una base de conocimiento QnA Maker, en la fase de prototipo, es habitual tener un único recurso de QnA Maker para las pruebas y la producción.

Al pasar a la fase de desarrollo del proyecto, debe tener en cuenta lo siguiente:

* ¿Cuántos idiomas tendrá su sistema de knowledge base?
* ¿En cuántas regiones necesita que la knowledge base esté disponible?
* ¿Cuántos documentos de cada dominio contendrá el sistema?

Planee tener un único recurso de QnA Maker que contenga todas las bases de conocimiento que tengan el mismo idioma, la misma región y la misma combinación de dominio de sujeto.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

Cuando se desarrolla por primera vez una knowledge base administrada de QnA Maker, en la fase de prototipo, es habitual tener un único recurso administrado de QnA Maker para las pruebas y la producción.

Al pasar a la fase de desarrollo del proyecto, debe tener en cuenta lo siguiente:

* ¿Cuántos idiomas tendrá su sistema de knowledge base?
* ¿En cuántas regiones necesita que la knowledge base esté disponible?
* ¿Cuántos documentos de cada dominio contendrá el sistema?

---

## <a name="pricing-tier-considerations"></a>Consideraciones sobre planes de tarifa

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

Normalmente hay tres parámetros que necesita tener en cuenta:

* **Rendimiento que se precisa del servicio**:
    * seleccione el [plan de aplicación](https://azure.microsoft.com/pricing/details/app-service/plans/) apropiado para App Service en función de sus necesidades. Puede [escalar o reducir verticalmente](https://docs.microsoft.com/azure/app-service/manage-scale-up) la aplicación.
    * Esto debería influir también en la selección de la SKU de Azure **Cognitive Search**; consulte más detalles [aquí](https://docs.microsoft.com/azure/search/search-sku-tier). Además, es posible que necesite ajustar la [capacidad](../../../search/search-capacity-planning.md) de Cognitive Search con réplicas.

* **Tamaño y número de bases de conocimiento**: Elija la [SKU de Azure Search](https://azure.microsoft.com/pricing/details/search/) adecuada para su escenario. Normalmente, puede decidir el número de bases de conocimiento que necesita en función del número de dominios de sujeto distintos. Si el dominio de sujeto (para un único idioma) debe estar en una base de conocimiento.

    Puede publicar N-1 bases de conocimiento en un nivel particular, donde N se corresponde con los índices máximos permitidos en el nivel. Compruebe también el tamaño y el número máximos de documentos permitidos por cada nivel.

    Por ejemplo, si el nivel tiene 15 índices permitidos, puede publicar 14 bases de conocimiento (1 índice por base de conocimiento publicada). El decimoquinto índice se utiliza para crear y probar todas las bases de conocimiento.

* **Número de documentos como orígenes**: la SKU gratuita del servicio de administración de QnA Maker limita el número de documentos que puede administrar mediante el portal y las API a 3 (de 1 MB cada uno). La SKU estándar no tiene ningún límite en relación con el número de documentos que puede administrar. Puede consultar más información [aquí](https://aka.ms/qnamaker-pricing).

En la tabla siguiente se proporcionan algunas directrices de alto nivel.

|                            | Administración de QnA Maker | App Service | Azure Cognitive Search | Limitaciones                      |
| -------------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| **Experimentación**        | SKU gratuita             | Nivel Gratis   | Nivel Gratis    | Publicar hasta 2 KB; tamaño de 50 MB  |
| **Entorno de desarrollo/pruebas**   | SKU Estándar         | Compartido      | Básico        | Publicaciones de hasta 14 KB; tamaño de 2 GB    |
| **Entorno de producción** | SKU Estándar         | Básico       | Estándar     | Publicar hasta 49 KB; tamaño de 25 GB |

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

Normalmente hay tres parámetros que necesita tener en cuenta:

* **Rendimiento que se precisa del servicio**:
    * QnA Maker administrado (versión preliminar) es un servicio gratuito con un rendimiento limitado actualmente a 10 TPS para las API de administración y las de predicción.
    * Esto debería influir también en la selección de la SKU de Azure **Cognitive Search**; consulte más detalles [aquí](https://docs.microsoft.com/azure/search/search-sku-tier). Además, es posible que necesite ajustar la [capacidad](../../../search/search-capacity-planning.md) de Cognitive Search con réplicas.

* **Tamaño y número de bases de conocimiento**: Elija la [SKU de Azure Search](https://azure.microsoft.com/pricing/details/search/) adecuada para su escenario. Normalmente, puede decidir el número de bases de conocimiento que necesita en función del número de dominios de sujeto distintos. Si el dominio de sujeto (para un único idioma) debe estar en una base de conocimiento.

    Con QnA Maker administrado (versión preliminar), tiene la opción de configurar el servicio de QnA Maker para las knowledge bases de un solo idioma o de varios. Puede realizar esta selección cuando cree la primera knowledge base en el servicio QnA Maker administrado (versión preliminar).

    ![Selección de knowledge base multilingüe de QnA Maker administrado (versión preliminar)](../media/concept-plan-your-knowledge-base/qnamaker-v2-select-multilanguage-knowledge-base.png)

    Puede publicar N-1 knowledge bases de un solo idioma en un nivel particular o N/2 knowledge bases en distintos idiomas, donde N se corresponde con los índices máximos permitidos en el nivel. Compruebe también el tamaño y el número máximos de documentos permitidos por cada nivel.

    Por ejemplo, si el nivel tiene 15 índices permitidos, puede publicar 14 knowledge bases del mismo idioma (1 índice por knowledge base publicada). El decimoquinto índice se utiliza para crear y probar todas las bases de conocimiento. Si opta por tener knowledge bases en distintos idiomas, solo podrá publicar 7 knowledge bases.

* **Número de documentos como orígenes**: QnA Maker administrado (versión preliminar) es un servicio gratuito y no hay ningún límite en cuanto al número de documentos que se pueden agregar como orígenes. Puede consultar más información [aquí](https://aka.ms/qnamaker-pricing).

En la tabla siguiente se proporcionan algunas directrices de alto nivel.

|                            |Azure Cognitive Search | Limitaciones                      |
| -------------------------- |------------ | -------------------------------- |
| **Experimentación**        |Nivel Gratis    | Publicar hasta 2 KB; tamaño de 50 MB  |
| **Entorno de desarrollo/pruebas**   |Básico        | Publicaciones de hasta 14 KB; tamaño de 2 GB    |
| **Entorno de producción** |Estándar     | Publicar hasta 49 KB; tamaño de 25 GB |

---

## <a name="recommended-settings"></a>Configuración recomendada

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

|QPS de destino | App Service | Azure Cognitive Search |
| -------------------- | ----------- | ------------ |
| 3             | S1, 1 instancia   | S1, 1 instancia    |
| 50         | S3, 10 instancias       | S1, 12 instancias         |
| 80         | S3, 10 instancias      |  S3, 12 instancias  |
| 100         | P3V2, 10 instancias  | S3, 12 instancias, 3 particiones   |
| 200 a 250         | P3V2, 20 instancias | S3, 12 instancias, 3 particiones    |

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

QnA Maker administrado es un servicio gratuito con un rendimiento limitado actualmente a 10 transacciones por segundo para las API de administración y las de predicción. Con el fin de obtener un rendimiento de 10 transacciones por segundo para su servicio, se recomienda la SKU S1 (1 instancia) de Azure Cognitive Search.

---

## <a name="when-to-change-a-pricing-tier"></a>Cuándo cambiar un plan de tarifa

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

|Actualizar|Motivo|
|--|--|
|[Actualización](../How-to/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku) de la SKU de administración de QnA Maker|Desea tener más pares de QnA o orígenes de documento en la base de conocimiento.|
|[Actualización](../How-to/set-up-qnamaker-service-azure.md#upgrade-app-service) de la SKU de App Service y comprobación del nivel de Cognitive Search y [creación de réplicas de Cognitive Search](../../../search/search-capacity-planning.md)|Su base de conocimiento debe atender más solicitudes de la aplicación cliente, como un bot de chat.|
|[Actualización](../How-to/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service) del servicio Azure Cognitive Search|Planea tener muchas bases de conocimiento.|

Para obtener las últimas actualizaciones del entorno de ejecución, [actualice la instancia de App Service en Azure Portal](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates).

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

[Actualice](../How-to/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service) el servicio Azure Cognitive Search si tiene previsto disponer de muchas knowledge bases.

---

## <a name="resource-naming-considerations"></a>Consideraciones de nomenclatura de recursos

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

El nombre del recurso QnA Maker, como `qna-westus-f0-b`, también se usa para asignar un nombre a los demás recursos.

La ventana de creación de Azure Portal le permite crear un recurso de QnA Maker y seleccionar los planes de tarifa para los demás recursos.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de Azure Portal para la creación de recursos de QnA Maker](../media/concept-azure-resource/create-blade.png)

Una vez creados los recursos, tienen el mismo nombre, excepto el recurso opcional de Application Insights, que pospone caracteres al nombre.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla con las listas de recursos de Azure Portal](../media/concept-azure-resource/all-azure-resources-created-qnamaker.png)

> [!TIP]
> Cree un nuevo grupo de recursos al crear un recurso de QnA Maker. Esto le permite ver todos los recursos asociados al recurso de QnA Maker al buscar por grupo de recursos.

> [!TIP]
> Utilice una convención de nomenclatura para indicar los planes de tarifa en el nombre del recurso o del grupo de recursos. Si recibe errores al crear una nueva base de conocimiento o al agregar nuevos documentos, el límite del plan de tarifa de Cognitive Search es un problema común.

### <a name="resource-purposes"></a>Propósitos de los recursos

Cada recurso de Azure creado con QnA Maker tiene un propósito específico:

* Recurso QnA Maker
* Recurso de Cognitive Search
* App Service
* Plan de App Service
* Servicio Application Insights


### <a name="cognitive-search-resource"></a>Recurso de Cognitive Search

El recurso de [Cognitive Search](../../../search/index.yml) se usa para:

* Almacenar los pares de QnA
* Proporcionar la clasificación inicial (número 1 del clasificador) de los pares de QnA en el runtime

#### <a name="index-usage"></a>Uso de índices

El recurso mantiene un índice para que actúe como índice de prueba y los índices restantes se correlacionan con una base de conocimiento publicada.

Un recurso con un precio para contener 15 índices, mantendrá 14 bases de conocimiento publicadas y se utiliza un índice para probar todas las bases de conocimiento. La base de conocimiento divide este índice de prueba para que una consulta que usa el panel de prueba interactiva utilice el índice de prueba, pero solo devolverá los resultados de la partición específica asociada a la base de conocimiento concreta.

#### <a name="language-usage"></a>Uso de idiomas

La primera base de conocimiento creada en el recurso de QnA Maker se utiliza para determinar el _único_ conjunto de idiomas para el recurso de Cognitive Search y todos sus índices. Solo puede tener _un conjunto de idiomas_ para un servicio de QnA Maker.

### <a name="qna-maker-resource"></a>Recurso QnA Maker

El recurso QnA Maker proporciona acceso a las API de creación y publicación, así como al procesamiento de lenguaje natural (NLP) basado en la segunda capa de clasificación (número 2 del clasificador) de los pares de QnA en el runtime.

La segunda clasificación aplica filtros inteligentes que pueden incluir metadatos y mensajes de seguimiento.

#### <a name="qna-maker-resource-configuration-settings"></a>Opciones de configuración de recursos de QnA Maker

Al crear una nueva base de conocimiento en el [portal de QnA Maker](https://qnamaker.ai), el valor de **idioma** es el único valor que se aplica en el nivel de recursos. El idioma se selecciona al crear la primera base de conocimiento en el recurso.

### <a name="app-service-and-app-service-plan"></a>App Service y plan de App Service

La aplicación cliente usa [App Service](../../../app-service/index.yml) para tener acceso a las bases de conocimiento publicadas a través del punto de conexión del entorno de ejecución.

Para consultar la base de conocimiento publicada, todas las bases de conocimiento publicadas usan el mismo punto de conexión de dirección URL, pero especifican el **identificador de la base de conocimiento** dentro de la ruta.

`{RuntimeEndpoint}/qnamaker/knowledgebases/{kbId}/generateAnswer`

### <a name="application-insights"></a>Application Insights

[Application Insights](../../../azure-monitor/app/app-insights-overview.md) se usa para recopilar registros y telemetría de chat. Revise las [consultas de Kusto](../how-to/get-analytics-knowledge-base.md) comunes para obtener información sobre el servicio.

## <a name="share-services-with-qna-maker"></a>Uso compartido de los servicios con QnA Maker

QnA Maker crea varios recursos de Azure. Para simplificar la administración y beneficiarse del uso compartido de los costos, use la siguiente tabla para saber lo que puede y no puede compartir:

|Servicio|Compartir|Motivo|
|--|--|--|
|Cognitive Services|X|No es posible por diseño|
|Plan de App Service|✔|Espacio en disco fijo asignado para un plan de App Service. Si otras aplicaciones que comparten el mismo plan de App Service usan un espacio en disco significativo, la instancia de App Service en QnAMaker tendrá problemas.|
|App Service|X|No es posible por diseño|
|Application Insights|✔|Se puede compartir|
|Servicio de búsqueda|✔|1. `testkb` es un nombre reservado para el servicio QnAMaker; no lo pueden usar otros.<br>2. La asignación de sinónimo por el nombre `synonym-map` está reservada para el servicio QnAMaker.<br>3. El número de bases de conocimiento publicadas está limitado por el nivel del servicio Search. Si hay índices libres disponibles, otros servicios pueden utilizarlos.|

### <a name="using-a-single-cognitive-search-service"></a>Uso de un servicio único de Cognitive Search

Si crea un servicio QnA y sus dependencias (como, Search) en el portal, se creará el servicio Search de inmediato y se vinculará al servicio QnA Maker. Después de crear estos recursos puede actualizar el valor de App Service para usar un servicio Search existente y eliminar el que acaba de crear.

Aprenda [cómo configurar](../How-To/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) QnA Maker para usar un recurso de Cognitive Service diferente del que se creó como parte del proceso de creación de recursos de QnA Maker.

## <a name="management-service-region"></a>Región del servicio de administración

El servicio de administración de QnA Maker solo se usa para el portal de QnA Maker y para el procesamiento de datos inicial. Este servicio solo está disponible en la región **Oeste de EE. UU.** En este servicio de Oeste de EE. UU., no se almacena ningún dato de cliente.

## <a name="keys-in-qna-maker"></a>Claves de QnA Maker

El servicio de QnA Maker se ocupa de dos tipos de claves: **claves de creación** y **claves de punto de conexión de consulta** que se utilizan con el entorno de ejecución hospedado en la instancia de App Service.

Si está buscando la **clave de suscripción**, [la terminología ha cambiado](#subscription-keys).

Use estas claves al realizar solicitudes al servicio mediante las API.

![Administración de claves](../media/qnamaker-how-to-key-management/key-management.png)

|Nombre|Location|Propósito|
|--|--|--|
|Clave de creación|[Azure Portal](https://azure.microsoft.com/free/cognitive-services/)|estas claves se usan para acceder a las [API del servicio de administración de QnA Maker](https://go.microsoft.com/fwlink/?linkid=2092179). Estas API permiten editar las preguntas y respuestas de una base de conocimiento y publicar una base de conocimiento. Estas claves se crean al crear al mismo tiempo que los servicios QnA Maker.<br><br>Busque estas claves en el recurso **Cognitive Services** de la página **Claves**.|
|Clave de punto de conexión de consulta|[Portal de QnA Maker](https://www.qnamaker.ai)|Estas claves se usan para consultar el punto de conexión de la base de conocimiento publicado para obtener una respuesta para una pregunta de un usuario. Este punto de conexión de consulta normalmente se usa en un bot de chat o el código de la aplicación cliente que se conecta el servicio QnA Maker. Estas claves se crean al publicar una base de conocimiento de QnA Maker.<br><br>Busque estas claves en la página **Configuración del servicio**. Busque esta página en el menú del usuario de la parte superior derecha de la página, en el menú desplegable.|

### <a name="subscription-keys"></a>Claves de suscripción

Los términos de clave de creación y de clave de punto de conexión de consulta son términos corregidos. El término anterior era **clave de suscripción**. Si consulta otra documentación que haga referencia a las claves de suscripción, estas son equivalentes a las claves de creación y de punto de conexión de consulta (usadas en el entorno de ejecución).

Debe saber cuál es la clave de acceso, la administración de la base de conocimiento o la consulta a la base de conocimiento para saber qué clave necesita buscar.

### <a name="recommended-settings-for-network-isolation"></a>Configuración recomendada para el aislamiento de red

* Proteja el recurso de Cognitive Services del acceso público con la [configuración de la red virtual](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal).
* Proteja App Service (QnA Runtime) del acceso público:
    * Permita el tráfico solo desde direcciones IP de Cognitive Services. Ya están incluidas en la etiqueta de servicio "CognitiveServicesManagement". Esto es necesario para la creación de API (crear o actualizar KB) para invocar el servicio de aplicaciones y actualizar el servicio Azure Search en consecuencia.
    * Asegúrese de que también permite otros puntos de entrada como el servicio de bot, el portal de QnA Maker (puede ser su red corporativa), etc., para el acceso de la API "GenerateAnswer" de predicción.
    * Consulte [más información sobre las etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/service-tags-overview).

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

El nombre del recurso de QnA Maker administrado (versión preliminar) como, por ejemplo `qna-westus-f0-b`, también se usa para asignar un nombre a los demás recursos.

La ventana de creación de Azure Portal le permite crear un recurso de QnA Maker administrado (versión preliminar) y seleccionar los planes de tarifa para los demás recursos.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de Azure Portal para la creación de recursos de QnA Maker administrado (versión preliminar)](../media/qnamaker-how-to-setup-service/enter-qnamaker-v2-info.png) Una vez creados los recursos, estos tienen el mismo nombre.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla con las listas de recursos de Azure Portal en QnA Maker administrado (versión preliminar)](../media/qnamaker-how-to-setup-service/resources-created-v2.png)
> [!TIP]
> Cree un nuevo grupo de recursos al crear un recurso de QnA Maker. Esto le permite ver todos los recursos asociados al recurso de QnA Maker administrado (versión preliminar) al buscar por grupo de recursos.
> [!TIP]
> Utilice una convención de nomenclatura para indicar los planes de tarifa en el nombre del recurso o del grupo de recursos. Si recibe errores al crear una nueva base de conocimiento o al agregar nuevos documentos, el límite del plan de tarifa de Cognitive Search es un problema común.

### <a name="resource-purposes"></a>Propósitos de los recursos

Cada recurso de Azure creado con QnA Maker administrado (versión preliminar) tiene un propósito específico:

* Recurso QnA Maker
* Recurso de Cognitive Search

### <a name="azure-cognitive-search-resource"></a>Recurso de Azure Cognitive Search

El recurso de [Cognitive Search](../../../search/index.yml) se usa para:

* Almacenar los pares de QnA
* Proporcionar la clasificación inicial (número 1 del clasificador) de los pares de QnA en el runtime

#### <a name="index-usage"></a>Uso de índices

Puede publicar N-1 knowledge bases de un solo idioma o N/2 knowledge bases de distintos idiomas en un nivel particular, donde N se corresponde con los índices máximos permitidos en el nivel de Azure Cognitive Search. Compruebe también el tamaño y el número máximos de documentos permitidos por cada nivel.

Por ejemplo, si el nivel tiene 15 índices permitidos, puede publicar 14 knowledge bases del mismo idioma (1 índice por knowledge base publicada). El decimoquinto índice se utiliza para crear y probar todas las bases de conocimiento. Si opta por tener knowledge bases en distintos idiomas, solo podrá publicar 7 knowledge bases.

#### <a name="language-usage"></a>Uso de idiomas

Con QnA Maker administrado (versión preliminar), tiene la opción de configurar el servicio de QnA Maker para la knowledge bases de un solo idioma o de varios. Puede elegir esta opción durante la creación de la primera knowledge base del servicio QnA Maker. Consulte [aquí](#pricing-tier-considerations) cómo habilitar la configuración de idioma por knowledge base.

### <a name="qna-maker-resource"></a>Recurso QnA Maker

El recurso de QnA Maker administrado (versión preliminar) proporciona acceso a las API de creación y publicación, hospeda el entorno de ejecución de la clasificación y proporciona datos de telemetría.

## <a name="region-support"></a>Regiones admitidas

En QnA Maker administrado (versión preliminar) los servicios de administración y los de predicción están ubicados en la misma región. En la actualidad, QnA Maker administrado (versión preliminar) está disponible en **Centro-sur de EE. UU., Norte de Europa y Este de Australia**.

### <a name="keys-in-qna-maker-managed-preview"></a>Claves de QnA Maker administrado (versión preliminar)

El servicio QnA Maker administrado (versión preliminar) se ocupa de dos tipos de claves: **claves de creación** y **claves de Azure Cognitive Search** que se usan para acceder al servicio en la suscripción del cliente.

Si está buscando la **clave de suscripción**, [la terminología ha cambiado](#subscription-keys).

Use estas claves al realizar solicitudes al servicio mediante las API.

![Versión preliminar administrada de administración de claves](../media/qnamaker-how-to-key-management/qnamaker-v2-key-management.png)

|Nombre|Location|Propósito|
|--|--|--|
|Clave de creación|[Azure Portal](https://azure.microsoft.com/free/cognitive-services/)|estas claves se usan para acceder a las [API del servicio de administración de QnA Maker](https://go.microsoft.com/fwlink/?linkid=2092179). Estas API permiten editar las preguntas y respuestas de una base de conocimiento y publicar una base de conocimiento. Estas claves se crean al crear al mismo tiempo que los servicios QnA Maker.<br><br>Busque estas claves en el recurso **Cognitive Services** de la página **Claves**.|
|Clave de administración de Azure Cognitive Search|[Azure Portal](https://docs.microsoft.com/azure/search/search-security-api-keys)|Estas claves se utilizan para comunicarse con el servicio Azure Cognitive Search implementado en la suscripción de Azure del usuario. Cuando asocia una instancia de Azure Cognitive Search con el servicio QnA Maker administrado (versión preliminar), la clave de administración se pasa automáticamente al servicio QnA Maker. <br><br>Puede encontrar estas claves en el recurso **Azure Cognitive Search** de la página **Claves**.|

### <a name="subscription-keys"></a>Claves de suscripción

Los términos de clave de creación y de clave de punto de conexión de consulta son términos corregidos. El término anterior era **clave de suscripción**. Si consulta otra documentación que haga referencia a las claves de suscripción, estas son equivalentes a las claves de creación y de punto de conexión de consulta (usadas en el entorno de ejecución).

Debe saber cuál es la clave de acceso, la administración de la base de conocimiento o la consulta a la base de conocimiento para saber qué clave necesita buscar.

### <a name="recommended-settings-for-network-isolation"></a>Configuración recomendada para el aislamiento de red 

Proteja el recurso de Cognitive Services del acceso público con la [configuración de la red virtual](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal).

---

## <a name="next-steps"></a>Pasos siguientes

* Aprenda más sobre la [base de conocimiento](knowledge-base.md) de QnA Maker.
* Comprenda que es un [ciclo de vida de la base de conocimiento](development-lifecycle-knowledge-base.md).
* Revise los [límites](../limits.md) de servicio y de la base de conocimiento.

