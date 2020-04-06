---
title: Preguntas más frecuentes de Azure Media Services v3 | Microsoft Docs
description: En este artículo se ofrecen respuestas a las preguntas más frecuentes de Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/18/2020
ms.author: juliako
ms.openlocfilehash: 11123ee04dd02a60dff0b88e2e6e85fcd613a7d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80068008"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Preguntas más frecuentes sobre Media Services v3

En este artículo se ofrecen respuestas a las preguntas más frecuentes de Azure Media Services (AMS) v3.

## <a name="general"></a>General

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>¿Qué roles de Azure pueden realizar acciones en recursos de Azure Media Services? 

Vea [Control de acceso basado en roles (RBAC) para cuentas de Media Services](rbac-overview.md).

### <a name="how-do-you-stream-to-apple-ios-devices"></a>¿Cómo se hace streaming a dispositivos de Apple iOS?

Asegúrese de que tiene "(format=m3u8-aapl)" al final de la ruta de acceso (después de la parte "/manifest" de la dirección URL) para indicar al servidor de origen de streaming que devuelva el contenido HLS para su consumo en dispositivos Apple iOS nativos (para más información, consulte la sección de [entrega de contenido](dynamic-packaging-overview.md)).

### <a name="how-do-i-configure-media-reserved-units"></a>¿Cómo se pueden configurar las unidades reservadas de multimedia?

Para los trabajos de análisis de audio y vídeo desencadenados por Media Services v3 o Video Indexer, se recomienda encarecidamente aprovisionar la cuenta con 10 MRU S3. Si necesita más de 10 MRU S3, abra una incidencia de soporte técnico desde [Azure Portal](https://portal.azure.com/).

Para más información, consulte [Escalado de procesamiento de medios con la CLI](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>¿Cuál es el método recomendado para procesar vídeos?

Use [transformaciones](https://docs.microsoft.com/rest/api/media/transforms) para configurar tareas comunes para codificar o analizar vídeos. Cada **Transformación** describe una receta, o un flujo de trabajo simple de tareas para procesar los archivos de vídeo o audio. Un [trabajo](https://docs.microsoft.com/rest/api/media/jobs) es la solicitud real a Media Services de aplicar la **transformación** a un contenido de vídeo o audio de entrada determinado. Una vez creada la transformación, puede enviar trabajos mediante las API de Media Services o cualquiera de los SDK publicados. Para obtener más información, consulte [Transformaciones y trabajos](transforms-jobs-concept.md).

### <a name="i-uploaded-encoded-and-published-a-video-what-would-be-the-reason-the-video-does-not-play-when-i-try-to-stream-it"></a>He cargado, codificado y publicado un vídeo. ¿Cuál es el motivo por el que el vídeo no se reproduce cuando intento transmitirlo?

Uno de los motivos más habituales es que no tiene el punto de conexión de streaming desde el que está intentando reproducir en estado Ejecutando.

### <a name="how-does-pagination-work"></a>¿Cómo funciona la paginación?

Al usar la paginación, siempre debe usar el vínculo siguiente para enumerar la colección y no tener que depender de un tamaño de página determinado. Para obtener información detallada y ejemplos, consulte [Filtrado, ordenación y paginación](entities-overview.md).

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>¿Qué características todavía no están disponibles en Azure Media Services v3?

Para obtener detalles, vea [Carencias de características con respecto a las API v2](media-services-v2-vs-v3.md#feature-gaps-with-respect-to-v2-apis).

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>¿Cómo se traslada una cuenta de Media Services entre suscripciones?  

Para más información, consulte [Traslado de una cuenta de Media Services entre suscripciones](media-services-account-concept.md).

## <a name="live-streaming"></a>Streaming en directo 

### <a name="how-to-stop-the-live-stream-after-the-broadcast-is-done"></a>¿Cómo detener el streaming en vivo una vez que termine la difusión?

Puede abordarlo desde el cliente o desde el servidor.

#### <a name="client-side"></a>En el cliente

La aplicación web debería preguntarle al usuario si quiere finalizar la difusión si está cerrando el explorador. Se trata de un evento del explorador que la aplicación web puede controlar.

#### <a name="server-side"></a>En el servidor

Puede supervisar eventos en directo si se suscribe a eventos de Event Grid. Para más información, consulte el [esquema de eventos de Event Grid](media-services-event-schemas.md#live-event-types).

* Puede [suscribirse](reacting-to-media-services-events.md) al nivel de secuencia [Microsoft.Media.LiveEventEncoderDisconnected](media-services-event-schemas.md#liveeventencoderdisconnected) y supervisar que no se produzca ninguna reconexión durante un tiempo para detener y eliminar el evento en directo.
* O bien puede [suscribirse](reacting-to-media-services-events.md) a los eventos de [latido](media-services-event-schemas.md#liveeventingestheartbeat) de nivel de pista. Si todas las pistas tienen una velocidad de bits de entrada que cae a 0, o bien si la última marca de tiempo ya no aumenta, también puede apagar el evento en directo de manera segura. Los eventos de latido ocurren cada 20 segundos por cada pista, por lo que podría ser un poco detallado.

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>¿Cómo insertar pausas o vídeos y caretas de imagen durante una transmisión en directo?

La codificación en directo de Media Services v3 no admite aún la inserción de caretas de vídeo o de imagen durante una transmisión en directo. 

Puede usar un [codificador local en directo](recommended-on-premises-live-encoders.md) para cambiar el vídeo de origen. Muchas aplicaciones proporcionan la posibilidad de cambiar los orígenes como, por ejemplo, Telestream Wirecast, Switcher Studio (en iOS), OBS Studio (aplicación gratuita), etc.

## <a name="content-protection"></a>Protección de contenido

### <a name="should-i-use-an-aes-128-clear-key-encryption-or-a-drm-system"></a>¿Debo usar un cifrado de clave sin cifrado AES-128 o un sistema DRM?

Los clientes suelen preguntarse si deben usar el cifrado de AES o un sistema de DRM. La diferencia principal entre los dos sistemas es que, con el cifrado de AES, la clave de contenido se transmite al cliente sobre TLS para que la clave se cifre en tránsito pero sin un cifrado adicional ("sin cifrado"). Como resultado, la clave usada para descifrar el contenido está accesible en el reproductor del cliente y se puede ver en un seguimiento de la red en el cliente en texto sin formato. Una clave sin cifrado AES-128 es adecuada para los casos de uso en los que el destinatario es una entidad de confianza (p. ej., cifrado de vídeos corporativos distribuidos dentro de una empresa para su visualización por parte de los empleados).

Los sistemas DRM como PlayReady, Widevine y FairPlay proporcionan un nivel adicional de cifrado en la clave utilizada para descifrar el contenido en comparación con una clave sin cifrado AES-128. La clave de contenido se cifra en una clave protegida por el entorno de ejecución de DRM, además de cualquier cifrado a nivel de transporte proporcionado por TLS. Además, el descifrado se controla en un entorno seguro en el nivel de sistema operativo donde a un usuario malintencionado le resulta más difícil atacar. DRM se recomienda para los casos de uso en los que es posible que el destinatario no sea una entidad de confianza y usted requiere el nivel de seguridad más alto.

### <a name="how-to-show-a-video-only-to-users-who-have-a-specific-permission-without-using-azure-ad"></a>¿Cómo mostrar un vídeo solo a los usuarios que tienen un permiso específico, sin usar Azure AD?

No tiene que usar ningún proveedor de tokens específico (como Azure AD). Puede crear su propio proveedor [JWT](https://jwt.io/) (denominado STS, servicio de token seguro) mediante el cifrado de claves asimétricas. En el STS personalizado, puede agregar notificaciones basadas en la lógica de negocios.

Asegúrese de que el emisor, el público y las notificaciones coincidan exactamente entre lo que hay en JWT y el valor de ContentKeyPolicyRestriction que se usa en ContentKeyPolicy.

Para más información, consulte [Protección del contenido mediante el cifrado dinámico de Media Services](content-protection-overview.md).

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>¿Cómo y dónde se puede obtener el token JWT antes de usarlo para solicitar la licencia o la clave?

1. Para entornos de producción, deberá tener servicios de token seguro (STS) (servicio web) que emite el token JWT tras una solicitud HTTPS. Para la prueba, puede usar el código que se muestra en el método **GetTokenAsync** definido en [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Una vez que se autentique un usuario, el reproductor deberá solicitar al STS dicho tipo de token y asignarlo como el valor del token. Puede usar la [API de Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/).

* Para un ejemplo de cómo ejecutar STS, con claves simétricas y asimétricas, consulte [https://aka.ms/jwt](https://aka.ms/jwt). 
* Para un ejemplo de un reproductor basado en Azure Media Player usando dicho token JWT, consulte [https://aka.ms/amtest](https://aka.ms/amtest) (expanda el vínculo "player_settings" para ver la entrada de token).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>¿Cómo se autorizan las solicitudes para transmitir vídeos con cifrado AES?

El enfoque correcto consiste en aprovechar STS (servicio de token seguro):

Según el perfil de usuario, agregue en STS notificaciones distintas (por ejemplo, "Usuario premium", "Usuario básico", "Usuario de evaluación gratuita"). Con notificaciones distintas en un token JWT, el usuario puede ver diferentes contenidos. Por supuesto, para otro contenido o recurso, la restricción ContentKeyPolicyRestriction tendrá el elemento RequiredClaims correspondiente.

Use las API de Azure Media Services para configurar la entrega de claves o licencias y cifrar los recursos (como se muestra en [este ejemplo](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Para más información, consulte:

- [Introducción a la protección de contenido](content-protection-overview.md)
- [Diseño del sistema de protección de contenido con DRM múltiple con control de acceso](design-multi-drm-system-with-access-control.md)

### <a name="http-or-https"></a>¿HTTP o HTTPS?
La aplicación de reproductor de ASP.NET MVC debe ser compatible con lo siguiente:

* Autenticación de usuarios mediante Azure AD, que está en HTTPS.
* Intercambio de tokens JWT entre el cliente y Azure AD, que debe estar en HTTPS.
* La adquisición de licencias DRM por el cliente, que debe estar en HTTPS si Media Services proporciona la entrega de licencias. El conjunto de productos de PlayReady no impone HTTPS para la entrega de licencias. Si el servidor de licencias de PlayReady está fuera de Media Services, se podría usar HTTP o HTTPS.

La aplicación de reproductor de ASP. NET utiliza HTTPS como procedimiento recomendado, por lo que Media Player se encuentra en una página en HTTPS. Sin embargo, HTTP es preferible para el streaming, por lo que se debe tener en cuenta la cuestión del contenido mixto.

* El explorador no permite contenido mixto. Sin embargo, los complementos como Silverlight y OSMF para Smooth y DASH sí lo hacen. El contenido mixto es un problema de seguridad debido a la amenaza de la posibilidad de insertar JavaScript malintencionado, que puede poner en riesgo los datos del cliente. Los exploradores lo bloquean de forma predeterminada. La única manera de solucionar esto es en el servidor (origen) al permitir todos los dominios (con independencia de si son HTTPS o HTTP). Probablemente esto tampoco sea una buena idea.
* Evite el contenido mixto. Tanto la aplicación de reproducción y Media Player deben utilizar HTTP o HTTPS. Al reproducir contenido mixto, silverlightSS tech requiere que se borre una advertencia de contenido mixto. flashSS tech controla el contenido mixto sin advertencia de contenido mixto.
* Si su punto de conexión de streaming se ha creado antes de agosto de 2014, no admitirá HTTPS. En este caso, cree y utilice un nuevo punto de conexión de streaming para HTTPS.

### <a name="what-about-live-streaming"></a>¿Y qué pasa con el streaming en vivo?

Puede usar exactamente el mismo diseño y la misma implementación para proteger el streaming en vivo en Media Services, solo hay que tratar el recurso asociado a un programa como un recurso VoD. Para proporcionar una protección de varias DRM del contenido en directo, aplique la misma configuración o procesamiento al recurso como si fuera un recurso VoD antes de asociar el recurso con la salida en directo.

### <a name="what-about-license-servers-outside-media-services"></a>¿Y qué sucede con los servidores de licencias que están fuera de Media Services?

Con frecuencia, los clientes invierten en granjas de servidores de licencias en su propio centro de datos o en uno hospedado por proveedores de servicios DRM. Con la protección de contenido de Media Services, puede funcionar en modo híbrido. El contenido puede alojarse y protegerse dinámicamente en Media Services, mientras que las licencias DRM las entregan servidores externos a Media Services. En este caso, tenga en cuenta los siguientes cambios:

* El servicio de token de seguridad debe emitir tokens que sean aceptables y que se puedan comprobar en la granja de servidores de licencias. Por ejemplo, los servidores de licencias de Widevine proporcionados por Axinom requieren un token JWT específico que contenga un mensaje de derechos. Por lo tanto, debe tener un servicio de token de seguridad para emitir dicho token JWT. 
* Ya no necesitará configurar el servicio de entrega de licencias en Media Services. Debe proporcionar las direcciones URL de adquisición de licencias (para PlayReady, Widevine y FairPlay) cuando configura ContentKeyPolicies.

> [!NOTE]
> Widevine es un servicio que ofrece Google Inc. y que está sujeto a los términos del servicio y la directiva de privacidad de Google, Inc.

## <a name="media-services-v2-vs-v3"></a>Comparación entre las versiones v2 y v3 de Media Services 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>¿Puedo usar Azure Portal para administrar los recursos de v3?

Actualmente, se puede usar [Azure Portal](https://portal.azure.com/) para:

* administrar los [eventos en directo](live-events-outputs-concept.md) de la versión 3 de Media Services, 
* ver (no administrar) los [recursos](assets-concept.md) de la versión 3, 
* [obtener información sobre el acceso a las API](access-api-portal.md). 

Para las restantes tareas de administración (por ejemplo, [Transformaciones y trabajos](transforms-jobs-concept.md) y [Protección de contenido](content-protection-overview.md)), use la [API REST](https://docs.microsoft.com/rest/api/media/), la [CLI](https://aka.ms/ams-v3-cli-ref), o uno de los[SDK](media-services-apis-overview.md#sdks) compatibles.

### <a name="is-there-an-assetfile-concept-in-v3"></a>¿Existe el concepto de AssetFile en la versión v3?

Los AssetFiles se eliminaron de la API de AMS para separar la dependencia del SDK de Media Services de Storage. Ahora Storage y no Media Services mantiene la información que pertenece a Storage. 

Para más información, consulte [Migración a Media Services v3](media-services-v2-vs-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>¿Qué hay del cifrado del almacenamiento del lado del cliente?

Actualmente, es recomendable utilizar el cifrado de almacenamiento en el lado de servidor (que está activo de forma predeterminada). Para más información, consulte [Cifrado del servicio Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Media Services v3](media-services-overview.md)
