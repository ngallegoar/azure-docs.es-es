---
title: Información sobre claves, secretos y certificados de Azure Key Vault
description: Información general de los detalles para desarrolladores y la interfaz de REST de Azure Key Vault para claves, secretos y certificados.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: dd8be482009e067bf9016cc8e351fc42a2db39c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233396"
---
# <a name="about-keys-secrets-and-certificates"></a>Información acerca de claves, secretos y certificados

Azure Key Vault permite a las aplicaciones y los usuarios de Microsoft Azure almacenar y usar varios tipos de datos de secretos y claves:

- Claves de cifrado: admite varios tipos de claves y algoritmos y habilita el uso de módulos de seguridad de hardware (HSM) para claves de alto valor. 
- Secretos: proporciona un almacenamiento seguro de secretos, como contraseñas y cadenas de conexión de base de datos.
- Certificates: admite certificados, que se basan en claves y secretos, y agrega una característica de renovación automática.
- Azure Storage: puede administrar automáticamente las claves de una cuenta de Azure Storage. Internamente, Key Vault puede enumerar (sincronizar) las claves con una cuenta de almacenamiento de Azure y volver a generar (rotar) las claves periódicamente. 

Para más información sobre Key Vault, vea [¿Qué es Azure Key Vault?](/azure/key-vault/key-vault-overview)

## <a name="azure-key-vault"></a>Azure Key Vault

Las secciones siguientes ofrecen información general aplicable a la implementación del servicio Key Vault.

### <a name="supporting-standards"></a>Compatibilidad con estándares

Las especificaciones de notación de objetos JavaScript (JSON) y de firma y cifrado de objetos JavaScript (JOSE) son fuentes importantes de información general.  

-   [Clave web JSON (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41)  
-   [Cifrado web JSON (JWE)](https://tools.ietf.org/html/draft-ietf-jose-json-web-encryption-40)  
-   [Algoritmos web JSON (JWA)](https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40)  
-   [Firma web JSON (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature-41)  

### <a name="data-types"></a>Tipos de datos

Consulte las especificaciones JOSE para tipos de datos relevantes para claves, cifrado y firma.  

-   **algorithm**: un algoritmo admitido para una operación de clave, por ejemplo, RSA1_5  
-   **ciphertext-value**: octetos de texto cifrado codificados con Base64URL  
-   **digest-value**: la salida de un algoritmo de hash codificada con Base64URL  
-   **key-type**: uno de los tipos de clave admitidos; por ejemplo, RSA (Rivest-Shamir-Adleman).  
-   **plaintext-value**: octetos de texto no cifrado codificados con Base64URL  
-   **signature-value**: la salida de un algoritmo de firma codificada con Base64URL  
-   **base64URL**: un valor binario codificado con Base64URL [RFC4648]  
-   **boolean**: true o false  
-   **Identity**: una identidad de Azure Active Directory (AAD).  
-   **IntDate**: un valor decimal JSON que representa el número de segundos desde 1970-01-01T0:0:0Z UTC hasta la fecha y hora UTC especificada. Consulte RFC3339 para más información acerca de la fecha y hora en general, y la hora UTC en particular.  

### <a name="objects-identifiers-and-versioning"></a>Objetos, identificadores y control de versiones

Se aplica el control de versiones de los objetos almacenados en Key Vault cuando se crea una instancia de un objeto. Cada versión tiene asignados un identificador único y una dirección URL. Cuando se crea un objeto por primera vez, se le asigna un identificador de versión único y se marca como la versión actual del objeto. La creación de una instancia con el mismo nombre de objeto proporciona al nuevo objeto un identificador de versión único, que hace que se convierta en la versión actual.  

Los objetos de Key Vault se pueden tratar con el identificador actual o con un identificador específico de la versión. Por ejemplo, dada una clave con el nombre `MasterKey`, realizar operaciones con el identificador actual hace que el sistema use la última versión disponible. Realizar operaciones con el identificador específico de la versión hace que el sistema use esa versión específica del objeto.  

Los objetos se identifican de forma única en Key Vault mediante una dirección URL. No hay dos objetos en el sistema que tengan la misma dirección URL, independientemente de la ubicación geográfica. La dirección URL completa a un objeto se denomina identificador de objeto. La dirección URL consta de un prefijo que identifica la instancia de Key Vault, el tipo de objeto, el nombre de objeto proporcionado por el usuario y la versión de un objeto. El nombre del objeto es inmutable y no distingue entre mayúsculas y minúsculas. Los identificadores que no incluyen la versión del objeto se conocen como identificadores base.  

Para más información, consulte [Autenticación, solicitudes y respuestas](authentication-requests-and-responses.md)

Un identificador de objeto tiene el formato general siguiente:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Donde:  

|||  
|-|-|  
|`keyvault-name`|Nombre de un almacén de claves en el servicio Microsoft Azure Key Vault.<br /><br /> Los nombres de los almacenes de claves los selecciona el usuario y son globalmente únicos.<br /><br /> El nombre de una instancia de Key Vault debe ser una cadena con una longitud de 3 a 24 caracteres que solo contenga 0-9, a-z, A-Z y -.|  
|`object-type`|Tipo de objeto, ya sea "claves" o "secretos".|  
|`object-name`|`object-name` es un nombre proporcionado por el usuario y debe ser único dentro de un almacén de claves. El nombre debe ser una cadena con una longitud de 1 a 127 caracteres que solo contenga 0-9, a-z, A-Z y -.|  
|`object-version`|`object-version` es un identificador de cadena de 32 caracteres generada por el sistema que, opcionalmente, se utiliza para referirse a una versión única de un objeto.|  

## <a name="key-vault-keys"></a>Claves en Key Vault

### <a name="keys-and-key-types"></a>Claves y tipos de clave

Las claves criptográficas en Key Vault se representan como objetos de clave web JSON [JWK]. Las especificaciones de JWK/JWA base también se han ampliado para habilitar los tipos de clave únicos para la implementación de Key Vault. Por ejemplo, la importación de claves mediante empaquetado específico del proveedor de HSM permite el transporte seguro de claves que solo pueden usarse en los HSM de Key Vault.  

- **Claves "débiles"** : una clave que Key Vault procesa en software, pero que se cifra en reposo con una clave del sistema que se encuentra en un HSM. Los clientes pueden importar una clave RSA o EC existente (curva elíptica) o solicitar que Key Vault genere una.
- **Claves "fuertes"** : una clave que se procesa en un HSM (módulo de seguridad de hardware). Estas claves se protegen en uno de los espacios de seguridad de HSM de Key Vault (hay un espacio de seguridad en cada región geográfica para mantener el aislamiento). Los clientes pueden importar una clave RSA o EC, de forma temporal o exportándola desde un dispositivo HSM compatible. Los clientes también pueden solicitar que Key Vault genere una clave. Este tipo de clave agrega el atributo key_hsm al objeto JWK para transportar el material de la clave de HSM.

     Para más información acerca de los límites geográficos, consulte [Centro de confianza de Microsoft Azure](https://azure.microsoft.com/support/trust-center/privacy/)  

Key Vault admite solo claves RSA y de curva elíptica. 

-   **EC**: clave de curva elíptica "suave".
-   **EC-HSM**: clave de curva elíptica "fuerte".
-   **RSA**: clave RSA "suave".
-   **RSA-HSM**: clave RSA "fuerte".

Key Vault admite claves RSA con tamaños de 2048, 3072 y 4096. Key Vault admite los tipos de claves de curva elíptica p-256, p-384, p-521 y P-256K (SECP256K1).

### <a name="cryptographic-protection"></a>Protección criptográfica

Los módulos criptográficos que utiliza Key Vault, sean HSM o software, tienen validación FIPS (Estándar federal de procesamiento de información). No es necesario hacer nada especial para que se ejecute en el modo FIPS. Las claves **creadas** o **importadas** con protección HSM se procesan dentro de un HSM y se validan con FIPS 140-2 Nivel 2. Las claves **creadas** o **importadas** con protección de software se procesan dentro de módulos criptográficos y se validan con FIPS 140-2 Nivel 1. Para más información, consulte [Claves y tipos de clave](#keys-and-key-types).

###  <a name="ec-algorithms"></a>Algoritmos de EC
 Los identificadores de algoritmo siguientes son compatibles con las claves EC y EC-HSM en Key Vault. 

#### <a name="curve-types"></a>Tipos de curvas

-   **P-256**: curva NIST P-256, definida en [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-256K**: curva SEC SECP256K1, definida en [SEC 2: parámetros de dominio de curva elíptica recomendados](https://www.secg.org/sec2-v2.pdf).
-   **P-384**: curva NIST P-384, definida en [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-521**: curva NIST P-521, definida en [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).

#### <a name="signverify"></a>SIGN/VERIFY

-   **ES256**: se crean claves y códigos hash de ECDSA para SHA-256 creados con la curva P-256. Este algoritmo se describe en [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **ES256K**: se crean claves y códigos hash de ECDSA para SHA-256 creados con la curva P-256K. Este algoritmo está pendiente de normalización.
-   **ES384**: se crean claves y códigos hash de ECDSA para SHA-384 creados con la curva P-384. Este algoritmo se describe en [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **ES512**: se crean claves y códigos hash de ECDSA para SHA-512 creados con la curva P-521. Este algoritmo se describe en [RFC7518](https://tools.ietf.org/html/rfc7518).


###  <a name="rsa-algorithms"></a>Algoritmos RSA  
 Los siguientes identificadores de algoritmo son compatibles con las claves RSA y RSA-HSM en Key Vault.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ENCRYPT/DECRYPT

-   **RSA1_5**: cifrado de clave RSAES-PKCS1-V1_5 [RFC3447]  
-   **RSA-OAEP**: RSAES con relleno de cifrado asimétrico óptimo (OAEP) [RFC3447], con los parámetros predeterminados especificados en RFC 3447 en la sección A.2.1. Estos parámetros predeterminados utilizan una función hash de SHA-1 y una función de generación de máscara de MGF1 con SHA-1.  

#### <a name="signverify"></a>SIGN/VERIFY

-   **PS256** - RSASSA-PSS con SHA-256 y MGF1 con SHA-256, como se indica en [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **PS384** - RSASSA-PSS con SHA-384 y MGF1 con SHA-384, como se indica en [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **PS512** - RSASSA-PSS con SHA-512 y MGF1 con SHA-512, como se describe en [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **RS256**: RSASSA-PKCS-v1_5 con SHA-256. El valor de hash proporcionado por la aplicación debe calcularse mediante SHA-256 y debe tener 32 bytes de longitud.  
-   **RS384**: RSASSA-PKCS-v1_5 con SHA-384. El valor de hash proporcionado por la aplicación debe calcularse mediante SHA-384 y debe tener 48 bytes de longitud.  
-   **RS512**: RSASSA-PKCS-v1_5 con SHA-512. El valor de hash proporcionado por la aplicación debe calcularse mediante SHA-512 y debe tener 64 bytes de longitud.  
-   **RSNULL**: consulte [RFC2437], un caso de uso especializado para habilitar ciertos escenarios de TLS.  

###  <a name="key-operations"></a>Operaciones con claves

Key Vault admite las siguientes operaciones en objetos de clave:  

-   **Crear**: permite a un cliente crear una clave en Key Vault. El valor de la clave lo genera y almacena Key Vault y no se entrega al cliente. Las claves asimétricas pueden crearse en Key Vault.  
-   **Import**: permite a un cliente importar una clave existente en Key Vault. Se pueden importar claves asimétricas en Key Vault mediante una serie de métodos de empaquetado diferentes dentro de una construcción JWK. 
-   **Actualizar**: permite a un cliente con los permisos suficientes modificar los metadatos (atributos de la clave) asociados con una clave almacenada previamente en Key Vault.  
-   **Eliminar**: permite a un cliente con los permisos suficientes eliminar una clave de Key Vault.  
-   **Enumerar**: permite a un cliente enumerar todas las claves de un determinado almacén de Key Vault.  
-   **Enumerar versiones**: permite a un cliente enumerar todas las versiones de una clave determinada en un determinado almacén de Key Vault.  
-   **Obtener**: permite a un cliente recuperar las partes públicas de una clave determinada en un almacén de Key Vault.  
-   **Backup (Copia de seguridad)** : exporta una clave en un formato protegido.  
-   **Restore (Restaurar)** : importa una clave desde una copia de seguridad previa.  

Para más información, consulte las [operaciones clave en la referencia de la API REST de Key Vault](/rest/api/keyvault).  

Una vez creada una clave en Key Vault, se pueden realizar las siguientes operaciones criptográficas con la clave:  

-   **Firmar y verificar**: estrictamente, esta operación es "firmar un hash" o "verificar un hash", ya que Key Vault no admite la creación de un hash del contenido como parte de la creación de la firma. Las aplicaciones deben crear el hash de los datos que se van a firmar de modo local y, a continuación, solicitar que Key Vault firme el hash. La verificación de valores hash firmados se admite como una operación conveniente para aplicaciones que no pueden acceder a material de clave [público]. Para obtener el mejor rendimiento de la aplicación, verifique que las operaciones se realizan localmente.  
-   **Cifrado/Encapsulado de clave**: una clave almacenada en Key Vault se puede utilizar para proteger otra clave, normalmente una clave de cifrado de contenido (CEK) simétrica. Cuando la clave en Key Vault es asimétrica, se usa el cifrado de claves. Por ejemplo, RSA-OAEP y las operaciones WRAPKEY/UNWRAPKEY son equivalentes a ENCRYPT/DECRYPT. Cuando la clave en Key Vault es simétrica, se usa el encapsulado de clave. Por ejemplo, AES-KW. La operación WRAPKEY se admite como una operación conveniente para aplicaciones que no pueden acceder a material de clave [público]. Para obtener el mejor rendimiento de la aplicación, las operaciones WRAPKEY deben realizarse localmente.  
-   **Cifrar y descifrar**: una clave almacenada en Key Vault puede utilizarse para cifrar o descifrar un único bloque de datos. El tamaño del bloque se determina en función del tipo de clave y del algoritmo de cifrado seleccionado. La operación de cifrado se proporciona por comodidad, para las aplicaciones que no pueden acceder a material de clave [público]. Para obtener el mejor rendimiento de la aplicación, las operaciones de cifrado se deben realizar localmente.  

Mientras que WRAPKEY/UNWRAPKEY con claves asimétricas puede parecer superfluo (porque la operación es equivalente a ENCRYPT/DECRYPT), es importante el uso de operaciones distintas. La distinción proporciona separación semántica y de autorización de estas operaciones, así como coherencia cuando otros tipos de claves son compatibles con el servicio.  

Azure Key Vault no admite operaciones de EXPORTACIÓN. Cuando se aprovisiona una clave en el sistema, no se puede extraer ni modificar el material de la clave. Sin embargo, los usuarios de Key Vault pueden requerir su clave para otros casos de uso, por ejemplo, después de que se haya eliminado. En este caso, pueden usar las operaciones de COPIA DE SEGURIDAD y RESTAURACIÓN para importar o exportar la clave en un formato protegido. Las claves creadas por la operación de COPIA DE SEGURIDAD no se pueden utilizar fuera de Key Vault. Como alternativa, la operación de IMPORTACIÓN se puede utilizar en varias instancias de Key Vault.  

Los usuarios pueden restringir cualquiera de las operaciones criptográficas que admite Key Vault en una base por cada clave mediante la propiedad key_ops del objeto JWK.  

Para más información acerca de los objetos JWK, consulte [Clave web JSON (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41).  

###  <a name="key-attributes"></a>Atributos de clave

Además del material de la clave, se pueden especificar los siguientes atributos. En una solicitud JSON, la palabra clave attributes y las llaves, "{' '}", son obligatorias aunque no haya ningún atributo especificado.  

- *enabled*: booleano, opcional, el valor predeterminado es **true**. Especifica si la clave está habilitada y se puede utilizar para operaciones criptográficas. El atributo *enabled* se utiliza junto con *nbf* y *exp*. Cuando se produce una operación entre *nbf* y *exp*, solo se permitirá si *enabled* está establecido en **true**. Las operaciones fuera de la franja *nbf* / *exp* se establecen automáticamente como no permitidas, salvo en determinados tipos de operación en [condiciones particulares](#date-time-controlled-operations).
- *nbf*: tipo IntDate, opcional, el valor predeterminado es now (ahora). El atributo *nbf* (no antes de) identifica la hora antes de la cual la clave NO DEBE utilizarse para operaciones criptográficas, excepto para ciertos tipos de operación en [condiciones particulares](#date-time-controlled-operations). El procesamiento del atributo *nbf* requiere que la fecha y hora actual sea posterior o igual que la fecha y hora enumerada en el atributo *nbf*. Key Vault PUEDE proporcionar un pequeño margen, normalmente no más de unos minutos, para considerar el sesgo del reloj. Su valor debe ser un número que contenga un valor IntDate.  
- *exp*: tipo IntDate, opcional, el valor predeterminado es "forever" (indefinidamente). El atributo *exp* (hora de expiración) identifica la hora después de la cual la clave no debe utilizarse para operaciones criptográficas, excepto para ciertos tipos de operación en las [condiciones particulares](#date-time-controlled-operations). El procesamiento del atributo *exp* requiere que la fecha y hora actual sea anterior a la fecha y hora enumerada en el atributo *exp*. Key Vault PUEDE proporcionar un pequeño margen, normalmente no más de unos minutos, para considerar el sesgo del reloj. Su valor debe ser un número que contenga un valor IntDate.  

Existen atributos de solo lectura adicionales que se incluyen en cualquier respuesta que incluya atributos de clave:  

- *created*: IntDate, opcional. El atributo *created* indica cuándo se creó esta versión de la clave. El valor es NULL para las claves creadas antes de agregar este atributo. Su valor debe ser un número que contenga un valor IntDate.  
- *updated*: IntDate, opcional. El atributo *updated* indica cuándo se modificó esta versión de la clave. El valor es NULL para las claves modificadas por última vez antes de agregar este atributo. Su valor debe ser un número que contenga un valor IntDate.  

Para más información acerca de IntDate y otros tipos de datos, consulte [Tipos de datos](#data-types)  

#### <a name="date-time-controlled-operations"></a>Operaciones controladas de fecha y hora

Las claves todavía no válidas y las expiradas, fuera de la franja *nbf* / *exp*, funcionarán para las operaciones **decrypt**, **unwrap** y **verify** (no devolverán 403, prohibido). La razón de usar el estado "todavía no válido" es permitir que una clave se pruebe antes de su uso en producción. La razón de usar el estado "expirado" es permitir las operaciones de recuperación de datos que se crearon cuando la clave era válida. Además, puede deshabilitar el acceso a una clave mediante directivas de Key Vault o actualizando el atributo de clave *enabled* como **false**.

Para más información sobre los tipos de datos, vea [Tipos de datos](#data-types).

Para más información sobre otros posibles atributos, vea [Clave web JSON (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41).

### <a name="key-tags"></a>Etiquetas de la clave

Puede especificar metadatos específicos de la aplicación adicionales en forma de etiquetas. Key Vault admite hasta 15 etiquetas, cada una de las cuales puede tener un nombre de 256 caracteres y un valor de 256 caracteres.  

>[!Note]
>Un llamador puede leer las etiquetas si tiene el permiso *list* o *get* para ese tipo de objeto (claves, secretos o certificados).

###  <a name="key-access-control"></a>Control de acceso a claves

El control de acceso para las claves administradas por Key Vault se proporciona en el nivel de un almacén de claves que actúa como contenedor de claves. La directiva de control de acceso para las claves es distinta de la directiva de control de acceso para los secretos en la misma instancia de Key Vault. Los usuarios pueden crear uno o varios almacenes para almacenar las claves y están obligados a mantener la segmentación adecuada del escenario y la administración de claves. El control de acceso para claves es independiente del control de acceso para secretos.  

Se pueden conceder los permisos siguientes, por usuario o por entidad de servicio, en la entrada de control de acceso a las claves de un almacén. Estos permisos reflejan fielmente las operaciones permitidas en un objeto de clave.  La operación para conceder acceso a una entidad de servicio del almacén de claves solo se realiza una vez, por lo que será igual en todas las suscripciones de Azure. Puede usarlo para implementar tantos certificados como desee. 

- Permisos para las operaciones de administración de claves
  - *get*: leer la parte pública de una clave, además de sus atributos
  - *list*: enumerar las claves o las versiones de una clave almacenada en un almacén de claves
  - *update*: actualizar los atributos de una clave
  - *create*: crear nuevas claves
  - *import*: importar una clave a un almacén de claves
  - *delete*: eliminar el objeto de clave
  - *recover*: recuperar una clave eliminada
  - *backup*: copia de seguridad de una clave de un almacén de claves
  - *restore*: restaurar una copia de seguridad de una clave a un almacén de claves

- Permisos para las operaciones criptográficas
  - *decrypt*: usar la clave para desproteger una secuencia de bytes
  - *encrypt*: usar la clave para proteger una secuencia arbitraria de bytes
  - *unwrapKey*: usar la clave para desproteger claves simétricas encapsuladas
  - *wrapKey*: usar la clave para proteger una clave simétrica
  - *verify*: usar la clave para verificar hashes  
  - *sign*: usar la clave para firmar hashes
    
- Permisos para las operaciones con privilegios
  - *purge*: purgar (eliminar permanentemente) una clave eliminada

Para más información sobre cómo trabajar con claves, consulte las [operaciones con claves en la referencia de la API de REST de Key Vault](/rest/api/keyvault). Para obtener información sobre cómo establecer permisos, vea [Almacenes: crear o actualizar](/rest/api/keyvault/vaults/createorupdate) y [Almacenes: actualizar directiva de acceso](/rest/api/keyvault/vaults/updateaccesspolicy). 

## <a name="key-vault-secrets"></a>Secretos en Key Vault 

### <a name="working-with-secrets"></a>Trabajo con secretos

Desde la perspectiva del desarrollador, las API de Key Vault aceptan y devuelven los valores de secreto como cadenas. Internamente, Key Vault almacena y administra secretos como secuencias de octetos (bytes de 8 bits), con un tamaño máximo de 25 000 bytes. El servicio Key Vault no proporciona semántica para los secretos. Solo acepta los datos, los cifra, los almacena y devuelve un identificador secreto ("id"). El identificador puede usarse para recuperar el secreto en un momento posterior.  

Si se trata de información muy confidencial, los clientes deben considerar las capas adicionales de protección de datos. Un ejemplo es el cifrado de datos mediante una clave de protección independiente antes de su almacenamiento en Key Vault.  

Key Vault también admite un campo contentType para secretos. Los clientes pueden especificar el tipo de contenido de un secreto para ayudar a interpretar los datos secretos cuando se recuperen. La longitud máxima de este campo es de 255 caracteres. No hay ningún valor predefinido. El uso sugerido es como sugerencia para interpretar los datos secretos. Por ejemplo, una implementación puede almacenar contraseñas y certificados como secretos y, a continuación, utilizar este campo para diferenciarlos. No hay ningún valor predefinido.  

### <a name="secret-attributes"></a>Atributos del secreto

Además los datos secretos, se pueden especificar los siguientes atributos:  

- *exp*: tipo IntDate, opcional, el valor predeterminado es **forever** (indefinidamente). El atributo *exp* (hora de expiración) identifica la hora de expiración después de la cual NO SE DEBEN recuperar los datos secretos, excepto en las [situaciones particulares](#date-time-controlled-operations). Este campo tiene fines **informativo** únicamente, que informa a los usuarios del servicio del almacén de claves que no se puede usar un determinado secreto. Su valor debe ser un número que contenga un valor IntDate.   
- *nbf*: tipo IntDate, opcional, el valor predeterminado es **now** (ahora). El atributo *nbf* (no antes de) identifica la hora antes de la cual NO SE DEBEN recuperar los datos secretos, excepto en las [situaciones particulares](#date-time-controlled-operations). Este campo tiene fines **informativos** únicamente. Su valor debe ser un número que contenga un valor IntDate. 
- *enabled*: booleano, opcional, el valor predeterminado es **true**. Este atributo especifica si se pueden recuperar los datos secretos. El atributo enabled se usa junto con *nbf* y *exp* y cuando se produce una operación entre *nbf* y *exp*, solo se permitirá si enabled se establece en **true**. Las operaciones fuera de la franja entre *nbf* y *exp* se deniegan automáticamente, excepto en [situaciones particulares](#date-time-controlled-operations).  

Existen atributos de solo lectura adicionales que se incluyen en cualquier respuesta que incluya atributos de secreto:  

- *created*: IntDate, opcional. El atributo created indica cuándo se creó esta versión del secreto. Este valor es NULL para los secretos creados antes de agregar este atributo. Su valor debe ser un número que contenga un valor IntDate.  
- *updated*: IntDate, opcional. El atributo updated indica cuándo se modificó esta versión del secreto. Este valor es NULL para los secretos modificados por última vez antes de agregar este atributo. Su valor debe ser un número que contenga un valor IntDate.

#### <a name="date-time-controlled-operations"></a>Operaciones controladas de fecha y hora

Una operación **get** de un secreto funcionará para los secretos todavía no válidos y los expirados, fuera de la franja *nbf* / *exp*. La llamada a la operación **get** de un secreto, para un secreto todavía no válido, se puede usar con fines de prueba. La obtención (**get**) de un secreto caducado se puede utilizar para operaciones de recuperación.

Para más información sobre los tipos de datos, vea [Tipos de datos](#data-types).  

### <a name="secret-access-control"></a>Control de acceso al secreto

El control de acceso para los secretos administrados por Key Vault se proporciona en el nivel de la instancia de Key Vault que contiene esos secretos. La directiva de control de acceso para los secretos es distinta de la directiva de control de acceso para las claves en la misma instancia de Key Vault. Los usuarios pueden crear uno o varios almacenes para almacenar los secretos y están obligados a mantener la segmentación adecuada del escenario y la administración de los secretos.   

Los siguientes permisos se pueden utilizar, en una base por entidad, en la entrada de control de acceso de secretos en un almacén y reflejan fielmente las operaciones permitidas en un objeto de secreto:  

- Permisos para las operaciones de administración de secretos
  - *get*: leer un secreto  
  - *list*: enumerar los secretos o las versiones de un secreto almacenado en un almacén de claves  
  - *set*: Crear un secreto  
  - *delete*: eliminar un secreto  
  - *recover*: recuperar un servidor eliminado
  - *backup*: copia de seguridad de un secreto de un almacén de claves
  - *restore*: restaurar una copia de seguridad de un secreto a un almacén de claves

- Permisos para las operaciones con privilegios
  - *purge*: purgar (eliminar permanentemente) un secreto eliminado

Para más información sobre cómo trabajar con secretos, consulte las [operaciones con secretos en la referencia de la API REST de Key Vault](/rest/api/keyvault). Para obtener información sobre cómo establecer permisos, vea [Almacenes: crear o actualizar](/rest/api/keyvault/vaults/createorupdate) y [Almacenes: actualizar directiva de acceso](/rest/api/keyvault/vaults/updateaccesspolicy). 

### <a name="secret-tags"></a>Etiquetas del secreto  
Puede especificar metadatos específicos de la aplicación adicionales en forma de etiquetas. Key Vault admite hasta 15 etiquetas, cada una de las cuales puede tener un nombre de 256 caracteres y un valor de 256 caracteres.  

>[!Note]
>Un llamador puede leer las etiquetas si tiene el permiso *list* o *get* para ese tipo de objeto (claves, secretos o certificados).

## <a name="key-vault-certificates"></a>Certificados en Key Vault

La compatibilidad con certificados de Key Vault proporciona la administración de sus certificados x509 y los comportamientos siguientes:  

-   Permite que el propietario de un certificado cree un certificado a través de un proceso de creación de Key Vault o a través de la importación de un certificado existente. Incluye tanto certificados autofirmados como certificados generados por una entidad de certificación.
-   Permite que el propietario de un certificado de Key Vault implemente un almacenamiento seguro y la administración de certificados X509 sin interacción con material de clave privada.  
-   Permite que el propietario de un certificado cree una directiva que indique a Key Vault cómo administrar el ciclo de vida de un certificado.  
-   Permite que los propietarios de certificados proporcionen información de contacto para la notificación de eventos del ciclo de vida de caducidad y renovación de los certificados.  
-   Admite la renovación automática con emisores seleccionados: proveedores de certificados X509 y entidades de certificación asociados con Key Vault.

>[!Note]
>Los proveedores y entidades no asociados también pueden, pero en este caso no se admitirá la característica de renovación automática.

### <a name="composition-of-a-certificate"></a>Composición de un certificado

Cuando se crea un certificado de Key Vault, se crean también una clave direccionable y un secreto con el mismo nombre. La clave de Key Vault permite las operaciones de clave y el secreto de Key Vault permite la recuperación del valor del certificado como un secreto. Un certificado de Key Vault también contiene metadatos del certificado X.509 público.  

El identificador y la versión de los certificados es similar al de las claves y los secretos. Una versión específica de una clave direccionable y el secreto creados con la versión del certificado de Key Vault está disponible en la respuesta del certificado de Key Vault.
 
![Los certificados son objetos complejos](media/azure-key-vault.png)

### <a name="exportable-or-non-exportable-key"></a>Clave exportable o no exportable

Cuando se crea un certificado de Key Vault, este se puede recuperar desde el secreto direccionable con la clave privada en formato PFX o PEM. La directiva utilizada para crear el certificado debe indicar que la clave es exportable. Si la directiva indica que no es exportable, la clave privada no forma parte del valor cuando se recupera como un secreto.  

La clave direccionable cobra más relevancia con los certificados de Key Vault no exportables. Las operaciones de la clave de Key Vault direccionable se asignan desde el campo *keyusage* de la directiva del certificado de Key Vault utilizada para crear el certificado de Key Vault.  

Se admiten dos tipos de clave con los certificados: *RSA* o *RSA HSM*. Los exportables solo se permiten con RSA, no se admiten en RSA HSM.  

### <a name="certificate-attributes-and-tags"></a>Etiquetas y atributos de certificado

Además de los metadatos del certificado, una clave direccionable y un secreto direccionable, un certificado de Key Vault también contiene atributos y etiquetas.  

#### <a name="attributes"></a>Atributos

Los atributos del certificado se reflejan en los atributos de la clave y el secreto direccionables que se crean cuando se crea el certificado de Key Vault.  

Un certificado de Key Vault tiene los siguientes atributos:  

-   *enabled*: booleano, opcional, el valor predeterminado es **true**. Se puede especificar para indicar si se pueden recuperar los datos del certificado como secreto o es operativo como una clave. Se usa también junto con *nbf* y *exp* cuando se produce una operación entre *nbf* y *exp*, solo se permitirá si enabled se establece en true. Las operaciones fuera de la franja entre *nbf* y *exp* se deniegan automáticamente.  

Existen atributos de solo lectura adicionales que se incluyen en la respuesta:

-   *created*: tipo IntDate, indica cuándo se creó esta versión del certificado.  
-   *updated*: tipo IntDate, indica cuándo se modificó esta versión del certificado.  
-   *exp*: tipo IntDate, contiene el valor de la fecha de expiración del certificado X.509.  
-   *nbf*: tipo IntDate, contiene el valor de la fecha del certificado X.509.  

> [!Note] 
> Si expira un certificado de Key Vault, la clave y el secreto direccionables dejan de funcionar.  

#### <a name="tags"></a>Etiquetas

 Diccionario especificado por el cliente de pares de clave y valor, similar a las etiquetas de las claves y los secretos.  

 > [!Note]
> Un llamador puede leer las etiquetas si tiene el permiso *list* o *get* para ese tipo de objeto (claves, secretos o certificados).

### <a name="certificate-policy"></a>Directiva de certificados

Una directiva de certificados contiene información sobre cómo crear y administrar el ciclo de vida de un certificado de Key Vault. Cuando se importa un certificado con clave privada en Key Vault, se crea una directiva predeterminada mediante la lectura del certificado X.509.  

Cuando se crea un certificado de Key Vault desde el principio, debe proporcionarse una directiva. La directiva especifica cómo crear esta versión de certificado de Key Vault o la próxima versión de certificado de Key Vault. Una vez que se ha establecido una directiva, no se requiere en sucesivas operaciones create para futuras versiones. Solo hay una instancia de una directiva para todas las versiones de un certificado de Key Vault.  

En un nivel general, una directiva de certificado contiene la siguiente información:  

-   Propiedades del certificado X509: contiene el nombre de asunto, nombres alternativos de asunto y otras propiedades que se usan para crear una solicitud de certificado X509.  
-   Propiedades de clave: contiene el tipo de clave, la longitud y los campos de clave exportable y de reutilización. Estos campos indican a Key Vault cómo generar una clave.  
-   Propiedades del secreto: contiene las propiedades del secreto, como el tipo de contenido del secreto direccionable para generar el valor secreto, para recuperar el certificado como un secreto.  
-   Acciones de vigencia: contiene las acciones de vigencia del certificado de Key Vault. Cada acción de vigencia contiene:  

     - Desencadenador: se especifica como días antes de la expiración o como un porcentaje del intervalo de vigencia  

     - Acción: se especifica el tipo de acción: *emailContacts* o *autoRenew*  

-   Emisor: parámetros sobre el emisor del certificado que se utiliza para emitir certificados X.509.  
-   Atributos de directiva: contiene atributos asociados a la directiva  

#### <a name="x509-to-key-vault-usage-mapping"></a>Correspondencia entre el uso de X.509 y Key Vault

La tabla siguiente representa la correspondencia entre la directiva de uso de claves X.509 y las operaciones de clave efectivas de una clave creada como parte de la creación de certificados de Key Vault.

|**Marcas de uso de claves X.509**|**Operaciones de clave de Key Vault**|**Comportamiento predeterminado**|
|----------|--------|--------|
|DataEncipherment|cifrar, descifrar| N/D |
|DecipherOnly|Descifrado| N/D  |
|DigitalSignature|firmar, verificar| Valor predeterminado de Key Vault sin una especificación de uso en el momento de creación del certificado | 
|EncipherOnly|encrypt| N/D |
|KeyCertSign|firmar, verificar|N/D|
|KeyEncipherment|encapsular clave, desencapsular clave| Valor predeterminado de Key Vault sin una especificación de uso en el momento de creación del certificado | 
|NonRepudiation|firmar, verificar| N/D |
|crlsign|firmar, verificar| N/D |

### <a name="certificate-issuer"></a>Emisor de certificados

Un objeto de certificado de Key Vault contiene una configuración que se usa para comunicarse con un proveedor de emisor de certificados seleccionado para solicitar certificados X.509.  

-   Key Vault está asociado con los proveedores de emisión de certificado siguientes para certificados TLS/SSL

|**Nombre del proveedor**|**Ubicaciones**|
|----------|--------|
|DigiCert|Compatible con todas las ubicaciones del servicio de Key Vault en la nube pública y Azure Government|
|GlobalSign|Compatible con todas las ubicaciones del servicio de Key Vault en la nube pública y Azure Government|

Para poder crear un emisor de certificados en un almacén de claves, deben realizarse correctamente los pasos de requisitos previos 1 y 2 siguientes.  

1. Incorporación de proveedores de entidad emisora (CA) de certificados  

    -   Un administrador de la organización debe incorporar su empresa (por ejemplo, Contoso) con al menos un proveedor de CA.  

2. El administrador crea las credenciales del solicitante para que Key Vault inscriba (y renueve) certificados TLS/SSL  

    -   Proporciona la configuración que se usará para crear un objeto de emisor del proveedor en el almacén de claves  

Para más información acerca de cómo crear objetos de emisor desde el portal de certificados, consulte el [blog de certificados de Key Vault](https://aka.ms/kvcertsblog)  

Key Vault permite la creación de varios objetos de emisor con diferente configuración del proveedor de emisor. Una vez que se crea un objeto de emisor, se puede hacer referencia a su nombre en una o varias directivas de certificado. Al hacer referencia al objeto de emisor se indica a Key Vault que utilice la configuración de acuerdo con lo especificado en el objeto de emisor al solicitar el certificado X.509 al proveedor de la entidad emisora de certificados durante la creación y la renovación de certificados.  

Los objetos de emisor se crean en el almacén y solo se pueden usar con certificados de Key Vault del mismo almacén.  

### <a name="certificate-contacts"></a>Contactos de certificados

Los contactos de certificados contienen información de contacto para enviar notificaciones desencadenadas por los eventos de vigencia del certificado. La información de los contactos es compartida por todos los certificados del almacén de claves. Se envía una notificación a todos los contactos especificados para un evento de cualquier certificado del almacén de claves.  

Si se establece la directiva de un certificado para la renovación automática, se envía una notificación en los siguientes eventos.  

- Antes de la renovación del certificado
- Tras la renovación del certificado, indicando si el certificado se renovó correctamente o si se produjo un error que exige la renovación manual del certificado.  

  Si se establece la directiva de certificado en renovación manual (solo correo electrónico), se envía una notificación cuando llega el momento de renovar el certificado.  

### <a name="certificate-access-control"></a>Control de acceso al certificado

 El control de acceso para los certificados lo administra Key Vault y lo proporciona la instancia de Key Vault que contiene dichos certificados. La directiva de control de acceso para los certificados es distinta de la directiva de control de acceso para las claves y los secretos en la misma instancia de Key Vault. Los usuarios pueden crear uno o varios almacenes para almacenar los certificados, para mantener la segmentación adecuada del escenario y la administración de los certificados.  

 Los siguientes permisos se pueden utilizar, en una base por entidad, en la entrada de control de acceso de secretos en un almacén de claves y reflejan fielmente las operaciones permitidas en un objeto de secreto:  

- Permisos para operaciones de administración de certificados
  - *get*: obtener la versión actual del certificado o cualquier versión de un certificado 
  - *list*: enumerar los certificados actuales o las versiones de un certificado  
  - *update*: crear un certificado
  - *create*: crear un certificado de Key Vault
  - *import*: importar material del certificado en un certificado de Key Vault
  - *delete*: eliminar un certificado, su directiva y todas sus versiones  
  - *recover*: recuperar un certificado eliminado
  - *backup*: copia de seguridad de un certificado de un almacén de claves
  - *restore*: restaurar una copia de seguridad de un certificado a un almacén de claves
  - *managecontacts*: administrar los contactos del certificado de Key Vault  
  - *manageissuers*: administrar emisores y entidades de certificados de Key Vault
  - *getissuers*: obtener emisores y entidades de un certificado
  - *listissuers*: enumerar emisores y entidades de un certificado  
  - *setissuers*: crear o actualizar emisores y entidades de un certificado de Key Vault  
  - *deleteissuers*: eliminar emisores y entidades de un certificado de Key Vault  
 
- Permisos para las operaciones con privilegios
  - *purge*: purgar (eliminar permanentemente) un certificado eliminado

Para más información, consulte las [operaciones con certificados en la referencia de la API REST de Key Vault](/rest/api/keyvault). Para obtener información sobre cómo establecer permisos, vea [Almacenes: crear o actualizar](/rest/api/keyvault/vaults/createorupdate) y [Almacenes: actualizar directiva de acceso](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="azure-storage-account-key-management"></a>Administración de claves de la cuenta de almacenamiento de Azure

Key Vault puede administrar las claves de la cuenta de almacenamiento de Azure:

- Internamente, Key Vault puede enumerar (sincronizar) las claves con una cuenta de almacenamiento de Azure. 
- Key Vault vuelve a generar (rotar) las claves periódicamente.
- Los valores de clave nunca se devuelven como respuesta al autor de la llamada.
- Key Vault administra las claves de las cuentas de almacenamiento y de las cuentas de almacenamiento clásicas.

Para más información, vea [Claves de cuenta de almacenamiento de Azure Key Vault](key-vault-ovw-storage-keys.md).

### <a name="storage-account-access-control"></a>Control de acceso a la cuenta de almacenamiento

Los siguientes permisos pueden usarse al autorizar a una entidad de seguridad de aplicación o usuario para realizar operaciones en una cuenta de almacenamiento administrada:  

- Permisos para la cuenta de almacenamiento administrada y operaciones de definiciones de SAS
  - *get*: obtener información sobre una cuenta de almacenamiento 
  - *list*: enumerar las cuentas de almacenamiento administradas por Key Vault
  - *update*: crear una cuenta de almacenamiento
  - *delete*: Eliminar una cuenta de almacenamiento  
  - *recover*: recuperar una cuenta de almacenamiento eliminada
  - *backup*: copia de seguridad de una cuenta de almacenamiento
  - *restore*: restaurar una copia de seguridad de una cuenta de almacenamiento en un almacén de claves
  - *set*: crear o actualizar una cuenta de almacenamiento
  - *regeneratekey*: volver a generar un valor de clave específico para una cuenta de almacenamiento
  - *getsas*: obtener información sobre una definición de SAS para una cuenta de almacenamiento
  - *listsas*: enumerar las definiciones de SAS de almacenamiento para una cuenta de almacenamiento
  - *deletesas*: eliminar una definición de SAS de una cuenta de almacenamiento
  - *setsas*: crear o actualizar definiciones o atributos de SAS nuevos para una cuenta de almacenamiento

- Permisos para las operaciones con privilegios
  - *purge*: purgar (eliminar permanentemente) una cuenta de almacenamiento administrada

Para más información, vea las [operaciones para cuentas de almacenamiento en la referencia de la API REST de Key Vault](/rest/api/keyvault). Para obtener información sobre cómo establecer permisos, vea [Almacenes: crear o actualizar](/rest/api/keyvault/vaults/createorupdate) y [Almacenes: actualizar directiva de acceso](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="see-also"></a>Consulte también

- [Autenticación, solicitudes y respuestas](authentication-requests-and-responses.md)
- [Guía del desarrollador de Key Vault](/azure/key-vault/key-vault-developers-guide)
