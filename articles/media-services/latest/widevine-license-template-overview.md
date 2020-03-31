---
title: Introducción a Azure Media Services v3 con plantilla de licencia de Widevine
description: En este tema se proporciona información general sobre una plantilla de licencia de Widevine que se usó para configurar las licencias de Widevine.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 94ce5e45a9a43e81020096ddc0a67429b286d9b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705639"
---
# <a name="media-services-v3-with-widevine-license-template-overview"></a>Introducción a Media Services v3 con plantilla de licencia de Widevine

Azure Media Services le permite cifrar el contenido HLS con **Widevine de Google**. Asimismo, Media Services también proporciona un servicio para entregar licencias de Widevine. Puede usar las API de Azure Media Services para configurar y solicitar licencias de Widevine. Cuando el reproductor intenta reproducir contenido protegido de Widevine, se envía una solicitud al servicio de entrega de licencias para obtener una licencia. Si el servicio de licencias aprueba la solicitud, el servicio emite la licencia. A continuación, se envía al cliente y se usa para descifrar y reproducir el contenido especificado.

Una solicitud de licencia de Widevine tiene el formato de un mensaje JSON.  

>[!NOTE]
> Puede crear un mensaje vacío y sin valores usando simplemente "{}". A continuación, se crea una plantilla de licencia con los valores predeterminados. La configuración predeterminada funciona para la mayoría de los casos. Los escenarios de entrega de licencia basados en Microsoft deben utilizar siempre los valores predeterminados. Si tiene que establecer los valores de "provider" y "content_id", el proveedor debe coincidir con las credenciales de Widevine.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage":<renew with usage>
       }
    }

## <a name="json-message"></a>Mensaje JSON

| Nombre | Value | Descripción |
| --- | --- | --- |
| payload |Cadena codificada en Base64 |La solicitud de licencia enviada por un cliente. |
| content_id |Cadena codificada en Base64 |Identificador utilizado para derivar el id. de la clave y la clave de contenido de cada elemento content_key_specs.track_type. |
| provider |string |Utilizado para buscar directivas y claves de contenido. En el caso de que se use la entrega de claves de Microsoft para entregar licencias de Widevine, este parámetro se omite. |
| policy_name |string |Nombre de una directiva previamente registrada. Opcional. |
| allowed_track_types |enum |SD_ONLY o SD_HD. Controla qué claves de contenido deben incluirse en una licencia. |
| content_key_specs |Matriz de estructuras JSON; consulte "Especificaciones de clave de contenido".  |Un control más preciso sobre qué claves de contenido se devolverán. Para obtener más información, consulte la sección "Especificaciones de clave de contenido". Solo se puede especificar uno de los valores de allowed_track_types y content_key_specs. |
| use_policy_overrides_exclusively |Valor booleano (true o false) |Utilice los atributos de directiva que especifica policy_overrides y omita todas las directivas almacenadas previamente. |
| policy_overrides |Estructura JSON; consulte "Invalidaciones de directivas". |Configuración de directiva para esta licencia.  En caso de que este activo tenga una directiva definida previamente, se usarán estos valores especificados. |
| session_init |Estructura JSON; consulte "Inicialización de la sesión". |Los datos opcionales se pasan a la licencia. |
| parse_only |Valor booleano (true o false) |Se analiza la solicitud de licencia, pero no se emite ninguna licencia. Sin embargo, los valores de la solicitud de licencia se devuelven en la respuesta. |

## <a name="content-key-specs"></a>Especificaciones de clave de contenido
Si existe una directiva anterior, no es necesario especificar ninguno de los valores en las especificaciones de clave de contenido. La directiva anterior que estaba asociada con este contenido se usa para determinar la protección de salida como, por ejemplo, Protección de contenido digital de ancho de banda alto (HDCP) y Copy General Management System (CGMS). Si no hay una directiva anterior registrada con el servidor de licencias de Widevine, el proveedor de contenido puede insertar los valores en la solicitud de licencia.   

Cada valor de content_key_specs debe especificarse en todas las pistas, independientemente de la opción use_policy_overrides_exclusively. 

| Nombre | Value | Descripción |
| --- | --- | --- |
| content_key_specs. track_type |string |Un nombre de tipo de pista. Si se especifica content_key_specs en la solicitud de licencia, asegúrese de especificar todos los tipos de pista explícitamente. Si no lo hace, se producirán errores en la reproducción transcurridos 10 segundos. |
| content_key_specs  <br/> security_level |uint32 |Define los requisitos de solidez del cliente para la reproducción. <br/> - Se requiere criptografía de caja blanca basada en software. <br/> - Se requiere criptografía de software y un descodificador de ofuscación. <br/> - Las operaciones de criptografía y material clave deben realizarse en un entorno de ejecución de confianza con respaldo del hardware. <br/> - La criptografía y la descodificación del contenido deben realizarse dentro de un entorno de ejecución de confianza con respaldo del hardware.  <br/> - La criptografía, la descodificación y todo el tratamiento de los medios (comprimidos y descomprimidos) deben administrarse dentro de un entorno de ejecución de confianza con respaldo del hardware. |
| content_key_specs <br/> required_output_protection.hdc |cadena, una de HDCP_NONE, HDCP_V1, HDCP_V2 |Indica si se requiere HDCP. |
| content_key_specs <br/>key |Base64-<br/>cadena codificada |Clave de contenido que se utilizará para esta pista. Si se especifica, se requiere track_type o key_id. Esta opción permite que el proveedor de contenido inserte la clave de contenido para esta pista en lugar de permitir que el servidor de licencias de Widevine genere o busque una clave. |
| content_key_specs.key_id |Binario de cadena codificada en Base64, 16 bytes |Identificador único para la clave. |

## <a name="policy-overrides"></a>Invalidaciones de directivas
| Nombre | Value | Descripción |
| --- | --- | --- |
| policy_overrides&#46;can_play |Valor booleano (true o false) |Indica que la reproducción del contenido está permitida. El valor predeterminado es False. |
| policy_overrides&#46;can_persist |Valor booleano (true o false) |Indica que la licencia puede conservarse en el almacenamiento no volátil para su uso sin conexión. El valor predeterminado es False. |
| policy_overrides&#46;can_renew |Valor booleano (true o false) |Indica que se permite la renovación de la presente licencia. Si es true, se puede ampliar la duración de la licencia mediante latido. El valor predeterminado es False. |
| policy_overrides&#46;license_duration_seconds |int64 |Indica el período de tiempo para esta licencia específica. Un valor de 0 indica que no hay ningún límite para la duración. El valor predeterminado es 0. |
| policy_overrides&#46;rental_duration_seconds |int64 |Indica el período de tiempo en el que se permite la reproducción. Un valor de 0 indica que no hay ningún límite para la duración. El valor predeterminado es 0. |
| policy_overrides&#46;playback_duration_seconds |int64 |El período de tiempo de visualización una vez que la reproducción comienza en el plazo de duración de la licencia. Un valor de 0 indica que no hay ningún límite para la duración. El valor predeterminado es 0. |
| policy_overrides&#46;renewal_server_url |string |Todas las solicitudes de latido (renovación) de esta licencia se dirigirán a la dirección URL especificada. Este campo solo se usa si can_renew es "true". |
| policy_overrides&#46;renewal_delay_seconds |int64 |El número de segundos después de license_start_time, antes de intentar la renovación por primera vez. Este campo solo se usa si can_renew es "true". El valor predeterminado es 0. |
| policy_overrides&#46;renewal_retry_interval_seconds |int64 |Especifica el plazo en segundos entre las posteriores solicitudes de renovación de licencia, en caso de error. Este campo solo se usa si can_renew es "true". |
| policy_overrides&#46;renewal_recovery_duration_seconds |int64 |El período de tiempo en el que la reproducción puede continuar mientras se intenta la renovación, aunque no se realice correctamente debido a problemas de back-end con el servidor de licencias. Un valor de 0 indica que no hay ningún límite para la duración. Este campo solo se usa si can_renew es "true". |
| policy_overrides&#46;renew_with_usage |Valor booleano (true o false) |Indica que la licencia se enviará para renovación cuando se inicie el uso. Este campo solo se usa si can_renew es "true". |

## <a name="session-initialization"></a>Inicialización de la sesión
| Nombre | Value | Descripción |
| --- | --- | --- |
| provider_session_token |Cadena codificada en Base64 |Este token de sesión se pasa de nuevo en la licencia y existirá en renovaciones posteriores. El token de sesión no se conservará una vez agotadas las sesiones. |
| provider_client_token |Cadena codificada en Base64 |Token de cliente para devolver en la respuesta de licencia. Si la solicitud de licencia contiene un token de cliente, este valor se omite. El token del cliente se conservará una vez agotadas las sesiones de licencia. |
| override_provider_client_token |Valor booleano (true o false) |Si es false y la solicitud de licencia contiene un token de cliente, use el token de la solicitud incluso si se especificó un token de cliente en esta estructura. Si es true, utilice siempre el token especificado en esta estructura. |

## <a name="configure-your-widevine-license-with-net"></a>Configuración de la licencia de Widevine con .NET 

Media Services proporciona una clase que le permite configurar una licencia de Widevine. Para construir la licencia, pase el JSON a [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Para configurar la plantilla puede:

### <a name="directly-construct-a-json-string"></a>Crear directamente una cadena JSON.

Este método puede ser propenso a errores. Se recomienda utilizar otro método, tal como se describe en la sección [Definir las clases necesarias y serializarlas a JSON](#classes).

```csharp
ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
{
    WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
};
```

### <a name="define-needed-classes-and-serialize-to-json"></a><a id="classes"></a> Definir las clases necesarias y serializarlas a JSON

#### <a name="define-classes"></a>Definir las clases

En el ejemplo siguiente se muestra un ejemplo de definiciones de clases que se asignan al esquema JSON de Widevine. Puede crear instancias de las clases antes de serializarlas a la cadena JSON.  

```csharp
public class PolicyOverrides
{
    public bool CanPlay { get; set; }
    public bool CanPersist { get; set; }
    public bool CanRenew { get; set; }
    public int RentalDurationSeconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
    public int PlaybackDurationSeconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
    public int LicenseDurationSeconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
}

public class ContentKeySpec
{
    public string TrackType { get; set; }
    public int SecurityLevel { get; set; }
    public OutputProtection RequiredOutputProtection { get; set; }
}

public class OutputProtection
{
    public string HDCP { get; set; }
}

public class WidevineTemplate
{
    public string AllowedTrackTypes { get; set; }
    public ContentKeySpec[] ContentKeySpecs { get; set; }
    public PolicyOverrides PolicyOverrides { get; set; }
}
```

#### <a name="configure-the-license"></a>Configuración de la licencia

Utilice las clases definidas en la sección anterior para crear el archivo JSON que se usa para configurar [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate):

```csharp
private static ContentKeyPolicyWidevineConfiguration ConfigureWidevineLicenseTempate()
{
    WidevineTemplate template = new WidevineTemplate()
    {
        AllowedTrackTypes = "SD_HD",
        ContentKeySpecs = new ContentKeySpec[]
        {
            new ContentKeySpec()
            {
                TrackType = "SD",
                SecurityLevel = 1,
                RequiredOutputProtection = new OutputProtection()
                {
                    HDCP = "HDCP_V2"
                }
            }
        },
        PolicyOverrides = new PolicyOverrides()
        {
            CanPlay = true,
            CanPersist = true,
            CanRenew = false,
            RentalDurationSeconds = 2592000,
            PlaybackDurationSeconds = 10800,
            LicenseDurationSeconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(template)
    };
    return objContentKeyPolicyWidevineConfiguration;
}
```

## <a name="additional-notes"></a>Notas adicionales

* Widevine es un servicio que ofrece Google Inc. y que está sujeto a los términos del servicio y la directiva de privacidad de Google, Inc.

## <a name="next-steps"></a>Pasos siguientes

Consulte cómo [proteger con DRM](protect-with-drm.md).
