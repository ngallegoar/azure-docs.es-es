---
title: Uso de las claves de creación y tiempo de ejecución con LUIS
titleSuffix: Azure Cognitive Services
description: 'LUIS usa dos claves: la clave de creación para crear el modelo y la clave de tiempo de ejecución para consultar el punto de conexión de predicción con expresiones del usuario.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 9d213c8fa03ad2ca5e5fd7e620e52aa502749be2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219158"
---
# <a name="authoring-and-runtime-keys"></a>Creación y claves en tiempo de ejecución

Language Understanding (LUIS) tiene dos servicios y conjuntos de API: 

* Creación (conocido anteriormente como de _programación_)
* Tiempo de ejecución de predicción

Hay varios tipos clave, según el servicio con el que desea trabajar y cómo desea trabajar con ellos.

## <a name="non-azure-resources-for-luis"></a>Recursos que no son de Azure para LUIS

### <a name="starter-key"></a>Clave de inicio

Cuando empiece a usar LUIS por primera vez, se creará una **clave de inicio**. Este recurso proporciona lo siguiente:

* solicitudes de servicio de creación gratuitas mediante el portal o las API de LUIS (incluidos los SDK)
* 1000 solicitudes de punto de conexión de predicción gratuitas al mes mediante un explorador, una API o SDK

## <a name="azure-resources-for-luis"></a>Recursos de Azure para LUIS

<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

LUIS permite tres tipos de recursos de Azure: 
 
|Clave|Propósito|Cognitive Service `kind`|Cognitive Service `type`|
|--|--|--|--|
|[Clave de creación](#programmatic-key)|Acceda y administre los datos de la aplicación con la creación, el entrenamiento, la publicación y las pruebas. Cree una clave de creación de LUIS si tiene previsto crear aplicaciones de LUIS mediante programación.<br><br>El propósito de la clave de `LUIS.Authoring` es permitirle:<br>* administrar mediante programación las aplicaciones y los modelos de Language Understanding, incluido el entrenamiento y la publicación<br> * controlar los permisos para el recurso de creación mediante la asignación de usuarios al [rol de colaborador](#contributions-from-other-authors).|`LUIS.Authoring`|`Cognitive Services`|
|[Clave de predicción](#prediction-endpoint-runtime-key)| Consulte las solicitudes del punto de conexión de predicción. Cree una clave de predicción de LUIS antes de que la aplicación cliente solicite predicciones más allá de las 1000 solicitudes proporcionadas por el recurso de inicio. |`LUIS`|`Cognitive Services`|
|[Clave de recursos de varios servicios de Cognitive Services](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|Consulte las solicitudes de punto de conexión de predicción compartidas con LUIS y otros servicios de Cognitive Services admitidos.|`CognitiveServices`|`Cognitive Services`|

Cuando finalice el proceso de creación de recursos, [asigne la clave](luis-how-to-azure-subscription.md) a la aplicación del portal de LUIS.

Es importante crear las aplicaciones de LUIS en [regiones](luis-reference-regions.md#publishing-regions) en las que también quiera publicar y consultar contenido.

> [!CAUTION]
> Para mayor comodidad, en muchos de los ejemplos se usa la [clave de inicio](#starter-key), ya que ofrece algunas llamadas de punto de conexión de predicción en su [cuota](luis-boundaries.md#key-limits).  


### <a name="query-prediction-resources"></a>Consulta de recursos de predicción

* La clave de tiempo de ejecución se puede usar en todas las aplicaciones de LUIS o en aplicaciones de LUIS específicas. 
* No use la clave de tiempo de ejecución para crear aplicaciones de LUIS. 

El punto de conexión de tiempo de ejecución de LUIS acepta dos estilos de consulta y ambos usan la clave de tiempo de ejecución del punto de conexión, pero en distintos lugares.

El punto de conexión que se usa para obtener acceso al tiempo de ejecución, usa un subdominio que es único en la región del recurso, que se indica con `{region}` en la tabla siguiente. 

## <a name="assignment-of-the-key"></a>Asignación de la clave

Puede [asignar](luis-how-to-azure-subscription.md) la clave de tiempo de ejecución en el [portal de LUIS](https://www.luis.ai) o a través de las API correspondientes. 

## <a name="key-limits"></a>Límites de la clave

Puede crear hasta 10 claves de creación por región y por suscripción. 

Consulte [Límites de la clave](luis-boundaries.md#key-limits) y [Regiones de Azure](luis-reference-regions.md). 

Las regiones de publicación son diferentes de las regiones de creación. Asegúrese de que crea una aplicación en la región de creación correspondiente a la región de publicación donde quiera ubicar su aplicación cliente.

## <a name="key-limit-errors"></a>Errores del límite de la clave
Si supera su cuota de transacciones por segundo (TPS), recibirá un error HTTP 429. Si supera su cuota de transacciones por mes (TPS), recibirá un error HTTP 403. 

## <a name="contributions-from-other-authors"></a>Contribuciones de otros autores

**Para la [creación de aplicaciones migradas](luis-migration-authoring.md) de recursos**: los _colaboradores_ se administran en Azure Portal para el recurso de creación, mediante la página del **control de acceso (IAM)** . Obtenga información acerca de [cómo agregar un usuario](luis-how-to-collaborate.md) mediante la dirección de correo electrónico del colaborador y el rol del _colaborador_. 

**En el caso de las aplicaciones que no se han migrado todavía**: todos los _colaboradores_ se administran en el portal de LUIS desde la página **Manage -> Collaborators** (Administrar -> Colaboradores).

## <a name="move-transfer-or-change-ownership"></a>Movimiento, transferencia o cambio de propiedad

Una aplicación se define mediante sus recursos de Azure, que se determinan en función de la suscripción del propietario. 

Puede mover la aplicación de LUIS. Use los siguientes recursos de documentación en Azure Portal o la CLI de Azure:

* [Mover la aplicación entre los recursos de creación de LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Mover los recursos a un nuevo grupo de recursos o a una nueva suscripción](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Mueve el recurso dentro de la misma suscripción o entre suscripciones](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)

Para transferir la [propiedad](../../cost-management-billing/manage/billing-subscription-transfer.md) de la suscripción: 

**Para los usuarios que han migrado: [creación de aplicaciones de recursos migradas](luis-migration-authoring.md)** : Como propietario del recurso, puede agregar un `contributor`.

**Para los usuarios que no han migrado todavía**: Exporte la aplicación como un archivo JSON. Otro usuario de LUIS puede importar la aplicación y, por tanto, se convierte en el propietario de la aplicación. La nueva aplicación tendrá un identificador de aplicación diferente.  

## <a name="access-for-private-and-public-apps"></a>Acceso para aplicaciones públicas y privadas

Para una aplicación **privada**, el acceso al tiempo de ejecución está disponible para los propietarios y los colaboradores. Para una aplicación **pública**, el acceso al tiempo de ejecución está disponible para todos los usuarios que tengan su propio recurso de tiempo de ejecución de Azure [Cognitive Services](../cognitive-services-apis-create-account.md) o de [LUIS](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) y el identificador de la aplicación pública. 

Actualmente no hay un catálogo de aplicaciones públicas.

### <a name="authoring-access"></a>Acceso de creación
El acceso a la aplicación desde el portal de [LUIS](luis-reference-regions.md#luis-website) o las [API de creación](https://go.microsoft.com/fwlink/?linkid=2092087) lo controla el recurso de creación de Azure. 

El propietario y todos los colaboradores tienen acceso a la creación de la aplicación. 

|El acceso de creación incluye|Notas|
|--|--|
|Agregar o quitar claves del punto de conexión||
|Exportar la versión||
|Exportar los registros de punto de conexión||
|Importar la versión||
|Hacer pública la aplicación|Cuando una aplicación es pública, cualquiera que tenga una clave de creación o del punto de conexión puede consultar la aplicación.|
|Modificar el modelo|
|Publicar|
|Revisar las expresiones de punto de conexión para un [aprendizaje activo](luis-how-to-review-endpoint-utterances.md)|
|Train|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Acceso de tiempo de ejecución del punto de conexión de predicción

El acceso para consultar el punto de conexión de predicción se controla mediante una opción de configuración de la página **Application Information** (Información de la aplicación) en la sección **Manage** (Administrar). 

|[Punto de conexión privado](#runtime-security-for-private-apps)|[Punto de conexión público](#runtime-security-for-public-apps)|
|:--|:--|
|Disponible para el propietario y los colaboradores.|Disponible para el propietario, los colaboradores y cualquier otra persona que conozca el identificador de la aplicación.|

Puede controlar quién puede ver su clave de tiempo de ejecución de LUIS llamándola en un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de servidor (por ejemplo, una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API del lado servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Aunque esta estrategia no impide los ataques de tipo "Man in the middle", ofusca la clave y la dirección URL del punto de conexión de los usuarios, le permite realizar un seguimiento del acceso y agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).

#### <a name="runtime-security-for-private-apps"></a>Seguridad del tiempo de ejecución para aplicaciones privadas

El tiempo de ejecución de una aplicación privada solo está disponible para lo siguiente:

|Clave y usuario|Explicación|
|--|--|
|Clave de creación del propietario| Hasta 1000 visitas del punto de conexión|
|Claves de creación de colaborador| Hasta 1000 visitas del punto de conexión|
|Cualquier clave que un autor o colaborador haya asignado a LUIS|Según el nivel de uso de las claves|

#### <a name="runtime-security-for-public-apps"></a>Seguridad del tiempo de ejecución para aplicaciones públicas

Una vez que una aplicación se ha configurado como pública, _cualquier_ clave de creación de LUIS válida o clave del punto de conexión de LUIS puede consultar la aplicación, siempre y cuando la clave no haya alcanzado el límite de cuota del punto de conexión.

Un usuario que no sea propietario ni colaborador solo podrá obtener acceso al tiempo de ejecución de una aplicación pública si se le proporciona el identificador de la aplicación. LUIS no tiene un _mercado_ público u otro medio para buscar una aplicación pública.  

Una aplicación pública se pone a disposición de los usuarios en todas las regiones para que los que tengan una clave de recurso de LUIS basada en regiones puedan acceder a la aplicación en cualquier región que esté asociada a la clave de recurso.

## <a name="transfer-of-ownership"></a>Transferencia de propiedad

LUIS no tiene el concepto de transferir la propiedad de un recurso. 

## <a name="securing-the-endpoint"></a>Proteger el punto de conexión 

Puede controlar quién puede ver su clave del punto de conexión de tiempo de ejecución de predicción de LUIS llamándola a un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de servidor (por ejemplo, una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API de servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Aunque esta estrategia no impide los ataques de tipo "Man in the middle", ofusca el punto de conexión de los usuarios, le permite realizar un seguimiento de acceso y agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="next-steps"></a>Pasos siguientes

* Descripción de los conceptos de [control de versiones](luis-concept-version.md). 
* Obtenga más información sobre [cómo crear claves](luis-how-to-azure-subscription.md).
