---
title: Autenticación basada en certificados X.509 en un clúster de Service Fabric
description: Obtenga información acerca de la autenticación basada en certificados en clústeres de Service Fabric y cómo detectar, mitigar y corregir problemas relacionados con los certificados.
ms.topic: conceptual
ms.date: 03/16/2020
ms.custom: sfrev
ms.openlocfilehash: 36717f526f88af753f3929d62e84ee65be4320e9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86259020"
---
# <a name="x509-certificate-based-authentication-in-service-fabric-clusters"></a>Autenticación basada en certificados X.509 en clústeres de Service Fabric

En este artículo se complementa la introducción a [Seguridad de los clústeres de Service Fabric](service-fabric-cluster-security.md) y se profundiza en los detalles de la autenticación basada en certificados en los clústeres de Service Fabric. Damos por supuesto que el lector está familiarizado con los conceptos fundamentales de seguridad y también con los controles que Service Fabric expone para controlar la seguridad de un clúster.  

Temas que se tratan en este título:

* Conceptos básicos de la autenticación basada en certificados
* Identidades y sus respectivos roles
* Reglas de configuración de certificados
* Solución de problemas y preguntas frecuentes

## <a name="certificate-based-authentication-basics"></a>Conceptos básicos de la autenticación basada en certificados
A modo de breve repaso, en seguridad, un certificado es un instrumento diseñado para enlazar información relacionada con una entidad (el firmante) con su posesión de un par de claves criptográficas asimétricas, por lo que constituye una construcción básica de criptografía de clave pública. Las claves representadas por un certificado se pueden usar para proteger los datos o para demostrar la identidad de los titulares de la clave. Cuando se usa junto con un sistema de infraestructura de clave pública (PKI), un certificado puede representar rasgos adicionales de su firmante, como la propiedad de un dominio de Internet o ciertos privilegios concedidos por el emisor del certificado (conocido como entidad de certificación o CA). Una aplicación común de certificados es compatible con el protocolo criptográfico de Seguridad de la capa de transporte (TLS), lo que permite comunicaciones seguras a través de una red de equipos. En concreto, el cliente y el servidor usan certificados para garantizar la privacidad y la integridad de la comunicación y también para realizar la autenticación mutua.

En Service Fabric, la capa fundamental de un clúster (Federación) también se basa en TLS (entre otros protocolos) para lograr una red confiable y segura de nodos participantes. Las conexiones al clúster a través de API de cliente de Service Fabric también usan TLS para proteger el tráfico, así como para establecer las identidades de las entidades. En concreto, cuando se utiliza para la autenticación en Service Fabric, un certificado se puede usar para demostrar las siguientes notificaciones: a) el presentador de la credencial de certificado tiene la clave privada del certificado; b) el hash SHA-1 del certificado ("huella digital") coincide con una declaración incluida en la definición de clúster; o c) el nombre común distintivo del firmante del certificado coincide con una declaración incluida en la definición de clúster y el emisor del certificado es conocido o de confianza.

En la lista anterior, "b" se conoce coloquialmente como "asignación de huella digital"; en este caso, la declaración hace referencia a un certificado específico y la seguridad del esquema de autenticación se basa en la premisa de que, de forma computacional, no es viable falsificar un certificado que produce el mismo valor hash que otro, aunque siga siendo un objeto válido y con formato correcto en todos los demás aspectos. El elemento "c" representa una forma alternativa de declarar un certificado, donde la seguridad del esquema se basa en la combinación del firmante del certificado y de la autoridad emisora. En este caso, la declaración hace referencia a una clase de certificados: dos certificados con las mismas características se consideran totalmente equivalentes. 

En las secciones siguientes se explica con detalle cómo el entorno en tiempo de ejecución de Service Fabric usa y valida los certificados para garantizar la seguridad del clúster.

## <a name="identities-and-their-respective-roles"></a>Identidades y sus respectivos roles
Antes de profundizar en los detalles de la autenticación o de proteger los canales de comunicación, es importante enumerar los actores participantes y los roles correspondientes que se reproducen en un clúster:
- entorno en tiempo de ejecución de Service Fabric, denominado "sistema": conjunto de servicios que proporcionan las abstracciones y la funcionalidad que representa el clúster. Al hacer referencia a la comunicación en el clúster entre las instancias del sistema, usaremos el término "identidad del clúster"; al hacer referencia al clúster como destinatario o destino del tráfico desde fuera del clúster, usaremos el término "identidad del servidor".
- aplicaciones hospedadas, denominadas "aplicaciones": código proporcionado por el propietario del clúster, que se organiza y ejecuta en el clúster.
- clientes: entidades a las que se les permite conectarse y ejecutar la funcionalidad en un clúster según la configuración del clúster. Distinguimos entre dos niveles de privilegios: "usuario" y "administrador", respectivamente. Un cliente "usuario" está restringido principalmente a las operaciones de solo lectura (pero no a toda la funcionalidad de solo lectura), mientras que un cliente "administrador" tiene acceso sin restricciones a la funcionalidad del clúster. (Para obtener más información, consulte [Roles de seguridad en un clúster de Service Fabric](service-fabric-cluster-security-roles.md)).
- (solo en Azure) servicios de Service Fabric que organizan y exponen controles para el funcionamiento y la administración de clústeres de Service Fabric, que se denominan simplemente "servicio". En función del entorno, el "servicio" puede hacer referencia al proveedor de recursos de Azure Service Fabric u otros proveedores de recursos propiedad del equipo de Service Fabric y ofrecidos por este.

En un clúster seguro, cada uno de estos roles se puede configurar con su propia identidad distinta, que se declara como el emparejamiento de un nombre de rol predefinido y su credencial correspondiente. Service Fabric admite la declaración de credenciales como certificados o una entidad de servicio basada en dominio. (Las identidades basadas en Windows o Kerberos también se admiten, pero quedan fuera del ámbito de este artículo. Consulte [Seguridad basada en Windows en clústeres de Service Fabric](service-fabric-windows-cluster-windows-security.md)). En los clústeres de Azure, los roles de cliente también pueden declararse como [identidades basadas en Azure Active Directory](service-fabric-cluster-creation-setup-aad.md).

Tal como se ha indicado anteriormente, el entorno en tiempo de ejecución de Service Fabric define dos niveles de privilegios en un clúster: "administrador" y "usuario". Un cliente administrador y un componente de "sistema" funcionarían con privilegios de "administrador", por lo que no se distinguen entre sí. Al establecer una conexión en o con el clúster, el entorno en tiempo de ejecución de Service Fabric concederá a un autor de llamada autenticado uno de los dos roles como base para la autorización subsiguiente. Examinaremos la autenticación en profundidad en las secciones siguientes.

## <a name="certificate-configuration-rules"></a>Reglas de configuración de certificados
### <a name="validation-rules"></a>Reglas de validación
La configuración de seguridad de un clúster de Service Fabric describe, en principio, los siguientes aspectos:
- el tipo de autenticación; se trata de una característica inmutable de hora de creación del clúster. Algunos ejemplos de estas opciones de configuración son "ClusterCredentialType", "ServerCredentialType" y los valores permitidos son "none", "x509" o "Windows". Este artículo se centra en la autenticación de tipo x509.
- las reglas de validación (autenticación); esta configuración la establece el propietario del clúster y describe qué credenciales se aceptan para un rol determinado. Los ejemplos se examinarán en profundidad a continuación.
- configuración usada para retocar o alterar sutilmente el resultado de la autenticación; estos ejemplos incluyen marcas que restringen o eliminan el carácter restringido del cumplimiento de listas de revocación de certificados etc.

> [!NOTE]
> Los ejemplos de configuración de clúster que se proporcionan a continuación son fragmentos del manifiesto de clúster en formato XML, como el formato más reducido que admite directamente la funcionalidad de Service Fabric que se describe en este artículo. La misma configuración se puede expresar directamente en las representaciones JSON de una definición de clúster, ya sea un manifiesto de clúster JSON independiente o una plantilla de Azure Resource Mangement.

Una regla de validación de certificado consta de los siguientes elementos:
- el rol correspondiente: cliente, cliente administrador (rol con privilegios).
- la credencial aceptada para el rol, declarada mediante huella digital o nombre común del firmante.

#### <a name="thumbprint-based-certificate-validation-declarations"></a>Declaraciones de validación de certificados basadas en huellas digitales
En el caso de las reglas de validación basadas en huellas digitales, las credenciales presentadas por un autor de llamada que solicita una conexión en o con el clúster se validarán de la siguiente manera:
  - la credencial es un certificado válido y con formato correcto: su cadena se puede compilar y las firmas coinciden.
  - el certificado es una hora válida (NotBefore <= ahora < NotAfter).
  - el hash SHA-1 del certificado coincide con la declaración, como una comparación de cadenas que no distingue entre mayúsculas y minúsculas y excluye todos los espacios en blanco.

Los errores de confianza detectados durante la compilación o validación de la cadena se suprimirán para las declaraciones basadas en huellas digitales, excepto para los certificados expirados (aunque también existen disposiciones para ese caso). En concreto, los errores relacionados con: estado de revocación desconocido o sin conexión, la raíz no es de confianza, el uso de la clave no es válido, cadena parcial se consideran errores no graves; la premisa, en este caso, es que el certificado es simplemente un sobre para un par de claves: la seguridad radica en el hecho de que el propietario del clúster ha establecido la medida en lugares para proteger la clave privada.

En el siguiente fragmento de un manifiesto de clúster se ejemplifica un conjunto de reglas de validación basadas en huellas digitales:

```xml
<Section Name="Security">
  <Parameter Name="ClusterCredentialType" Value="X509" />
  <Parameter Name="ServerAuthCredentialType" Value="X509" />
  <Parameter Name="AdminClientCertThumbprints" Value="d5ec...4264" />
  <Parameter Name="ClientCertThumbprints" Value="7c8f...01b0" />
  <Parameter Name="ClusterCertThumbprints" Value="abcd...1234,ef01...5678" />
  <Parameter Name="ServerCertThumbprints" Value="ef01...5678" />
</Section>
```

Cada una de las entradas anteriores hace referencia a una identidad específica como se describió anteriormente; cada entrada también permite especificar varios valores, como una lista separada por comas de cadenas. En este ejemplo, tras validar correctamente las credenciales entrantes, al presentador de un certificado con la huella digital SHA-1 "d5ec...4264" se le concede el rol de "administrador"; en cambio, a un autor de llamada que se autentica con el certificado "7c8f...01b0" se le concede un rol de "usuario", restringido a las operaciones de solo lectura principalmente. Un autor de llamada entrante que presenta un certificado cuya huella digital es "abcd...1234" o "ef01...5678" se acepta como nodo del mismo nivel en el clúster. Por último, un cliente que se conecta a un punto de conexión de administración del clúster espera que la huella digital del certificado de servidor sea "ef01...5678". 

Como se mencionó anteriormente, Service Fabric realiza disposiciones para aceptar los certificados expirados; la razón es que los certificados tienen una duración limitada y, cuando se declaran mediante la huella digital (que hace referencia a una instancia de certificado específica), si se permite que un certificado expire, se producirá un error en la conexión con el clúster o un colapso total del clúster. Es muy fácil olvidarse de girar un certificado asignado por huella digital y, desafortunadamente, la recuperación de esta situación es difícil.

Para ello, el propietario del clúster puede indicar explícitamente que los certificados autofirmados declarados por la huella digital se consideran válidos, como se indica a continuación:

```xml
  <Section Name="Security">
    <Parameter Name="AcceptExpiredPinnedClusterCertificate" Value="true" />
  </Section>
```
Este comportamiento no se extiende a los certificados emitidos por una entidad de certificación. Si este fuera el caso, un certificado expirado que se ha revocado y que se ha puesto en peligro podría convertirse en "válido" en cuanto ya no figure en la lista de revocación de certificados de la CA y, por tanto, presentaría un riesgo para la seguridad. Con los certificados autofirmados, el propietario del clúster se considera la única entidad responsable de proteger la clave privada del certificado. Este no es el caso de los certificados emitidos por la entidad de certificación, donde el propietario del clúster puede no ser consciente de cómo o cuándo se declaró que su certificado estaba en peligro.

#### <a name="common-name-based-certificate-validation-declarations"></a>Declaraciones de validación de certificados basadas en nombres comunes
Las declaraciones basadas en nombres comunes adoptan una de las formas siguientes:
- nombre común del firmante (solo)
- nombre común del firmante con la asignación del emisor

En primer lugar, veamos un extracto de un manifiesto de clúster que ejemplifica ambos estilos de declaración:
```xml
    <Section Name="Security/ServerX509Names">
      <Parameter Name="server.demo.system.servicefabric.azure-int" Value="" />
    </Section>
    <Section Name="Security/ClusterX509Names">
      <Parameter Name="cluster.demo.system.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
```
Las declaraciones hacen referencia a las identidades del servidor y del clúster, respectivamente. Tenga en cuenta que las declaraciones basadas en nombres comunes tienen sus propias secciones en el manifiesto de clúster, independientes de la "seguridad" estándar. En ambas declaraciones, "nombre" representa el nombre común del firmante distintivo del certificado y el campo "valor" representa el emisor esperado, como se indica a continuación:

- En el primer caso, la declaración indica que se espera que el elemento de nombre común del firmante distintivo del certificado del servidor coincida con la cadena "server.demo.system.servicefabric.azure-int". El campo "valor" vacío denota la expectativa de que la raíz de la cadena de certificados sea de confianza en el nodo o la máquina donde se valida el certificado de servidor; en Windows, esto significa que el certificado puede encadenarse a cualquiera de los certificados instalados en el almacén de "entidad de certificación raíz de confianza".
- En el segundo caso, la declaración indica que el presentador de un certificado se acepta como nodo del mismo nivel en el clúster si el nombre común del certificado coincide con la cadena "cluster.demo.system.servicefabric.azure-int" *y* la huella digital del emisor directo del certificado coincide con una de las entradas separadas por comas en el campo "valor". (Este tipo de regla se suele conocer como "nombre común con asignación del emisor").

En cualquier caso, la cadena del certificado se compila y se espera que no tenga errores; es decir, los errores de revocación, de cadena parcial o de confianza de tiempo no válido se consideran graves y se producirá un error en la validación del certificado. Al asignar los emisores, se considerará el estado "La raíz no es de confianza" como un error no grave; a pesar de las apariencias, se trata de una forma de validación más estricta, ya que permite que el propietario del clúster restrinja el conjunto de emisores autorizados o aceptados a su propia PKI.

Una vez compilada la cadena de certificados, se valida con respecto a una directiva TLS/SSL estándar con el firmante declarado como nombre remoto; un certificado se considerará una coincidencia si su nombre común del firmante o cualquiera de sus nombres alternativos del firmante coincide con la declaración de nombre común del manifiesto de clúster. En este caso, se admiten caracteres comodín y la coincidencia de cadenas no distingue entre mayúsculas y minúsculas.

(Debemos aclarar que la secuencia descrita anteriormente se podría ejecutar para cada tipo de uso de clave declarado por el certificado; si el certificado especifica el uso de la clave de autenticación del cliente, la cadena se compila y evalúa primero para un rol de cliente. En caso de éxito, la evaluación se completa y la validación se realiza correctamente. Si el certificado no tiene el uso de autenticación del cliente o se produce un error en la validación, el entorno en tiempo de ejecución de Service Fabric compilará y evaluará la cadena para la autenticación del servidor).

Para completar el ejemplo, el fragmento siguiente muestra la declaración de certificados de cliente por nombre común:
```xml
    <Section Name="Security/AdminClientX509Names">
      <Parameter Name="admin.demo.client.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
    <Section Name="Security/ClientX509Names">
      <Parameter Name="user.demo.client.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
```

Las declaraciones anteriores corresponden a las identidades de administrador y de usuario, respectivamente; la validación de certificados declarada de esta manera es exactamente como se describe en los ejemplos anteriores, de los certificados de clúster y de servidor.

> [!NOTE]
> Las declaraciones basadas en nombres comunes están diseñadas para simplificar la rotación y, en general, la administración de los certificados de clúster. Sin embargo, se recomienda cumplir las siguientes recomendaciones para garantizar la disponibilidad y la seguridad continuas del clúster:
  * Preferir la asignación del emisor para confiar en raíces de confianza.
  * Evitar la combinación de emisores de diferentes PKI.
  * Asegurarse de que todos los emisores esperados aparecen en la declaración de certificado; un emisor no coincidente producirá un error de validación.
  * Asegurarse de que los puntos de conexión de la directiva de certificados de la PKI sean reconocibles y accesibles y que estén disponibles, lo que significa que los puntos de conexión de AIA, CRL o OCSP se declaran en el certificado de hoja y que son accesibles para que se pueda completar la compilación de la cadena de certificados.

Al enlazar todo tras recibir una solicitud de conexión en un clúster protegido con certificados X.509, el entorno en tiempo de ejecución de Service Fabric usará la configuración de seguridad del clúster para validar las credenciales de la entidad remota como se describió anteriormente. Si se realiza correctamente, se considera que el autor de llamada o la entidad remota se ha autenticado. Si la credencial coincide con varias reglas de validación, el entorno en tiempo de ejecución concederá al autor de llamada el rol con más privilegios de cualquiera de las reglas coincidentes. 

### <a name="presentation-rules"></a>Reglas de presentación
En la sección anterior se describe cómo funciona la autenticación en un clúster protegido por certificados. En esta sección se explica cómo el entorno en tiempo de ejecución de Service Fabric detecta y carga los certificados que usa para la comunicación en el clúster, lo que se denomina reglas de "presentación".

Al igual que con las reglas de validación, las reglas de presentación especifican un rol y la declaración de credenciales asociada, que se expresan mediante huella digital o nombre común. A diferencia de las reglas de validación, las declaraciones basadas en nombres comunes no tienen disposiciones para la asignación del emisor. Esto permite una mayor flexibilidad, además de mejorar el rendimiento. Las reglas de presentación se declaran en las secciones "NodeType" del manifiesto de clúster para cada tipo de nodo distinto. La configuración se divide en las secciones de seguridad del clúster para permitir que cada tipo de nodo tenga su configuración completa en una sola sección. En los clústeres de Azure Service Fabric, las declaraciones de certificado de tipo de nodo tienen como valor predeterminado la configuración correspondiente en la sección de seguridad de la definición del clúster.

#### <a name="thumbprint-based-certificate-presentation-declarations"></a>Declaraciones de presentación de certificados basadas en huellas digitales
Como se ha descrito anteriormente, el entorno en tiempo de ejecución de Service Fabric distingue entre su rol como el elemento del mismo nivel de otros nodos del clúster y como el servidor para las operaciones de administración del clúster. En principio, estos valores de configuración se pueden configurar de forma diferente, pero en la práctica tienden a alinearse. En el resto de este artículo, supondremos que la configuración coincide para simplificar.

Consideremos que el ejemplo siguiente es un extracto de un manifiesto de clúster:
```xml
  <NodeTypes>
    <NodeType Name="nt1vm">
      <Certificates>
        <ClusterCertificate X509FindType="FindByThumbprint" X509FindValue="cc71...1984" X509FindValueSecondary="49e2...19d6" X509StoreName="my" Name="ClusterCertificate" />
        <ServerCertificate X509FindValue="cc71...1984" Name="ServerCertificate" />
        <ClientCertificate X509FindValue="cc71...1984" Name="ClientCertificate" />
      </Certificates>
    </NodeType>
  </NodeTypes>
```
El elemento "ClusterCertificate" muestra el esquema completo, incluidos los parámetros opcionales ("X509FindValueSecondary") o los valores predeterminados adecuados ("X509StoreName"); las demás declaraciones se muestran en una forma abreviada. La declaración de certificado de clúster anterior indica que la configuración de seguridad de los nodos de tipo "nt1vm" se ha inicializado con el certificado "cc71..1984" como principal y el certificado "49e2..19d6" como secundario. Se espera que ambos certificados se encuentren en el almacén de certificados LocalMachine\'My' (o la ruta de acceso equivalente de Linux, *var/lib/sfcerts*).

#### <a name="common-name-based-certificate-presentation-declarations"></a>Declaraciones de presentación de certificados basadas en nombres comunes
Los certificados de tipo de nodo también pueden declararse mediante el nombre común del firmante, como se muestra a continuación:

```xml
  <NodeTypes>
    <NodeType Name="nt1vm">
      <Certificates>
        <ClusterCertificate X509FindType="FindBySubjectName" X509FindValue="demo.cluster.azuredocpr.system.servicefabric.azure-int" Name="ClusterCertificate" />
      </Certificates>
    </NodeType>
  </NodeTypes>
```

Para cualquier tipo de declaración, un nodo de Service Fabric leerá la configuración en el inicio, buscará y cargará los certificados especificados y los ordenará en orden descendente de su atributo NotAfter. Los certificados expirados se omiten y se selecciona el primer elemento de la lista como credencial de cliente para cualquier conexión de Service Fabric que haya intentado este nodo. (En efecto, Service Fabric favorece el certificado que va a expirar más tarde).

Tenga en cuenta que, para las declaraciones de presentación basadas en nombres comunes, un certificado se considera una coincidencia si su nombre común de firmante es igual al campo X509FindValue (o X509FindValueSecondary) de la declaración como una comparación de cadenas exactas que distingue entre mayúsculas y minúsculas. Esto contrasta con las reglas de validación, que admiten la coincidencia de caracteres comodín, así como las comparaciones de cadenas que no distinguen mayúsculas de minúsculas.  

### <a name="miscellaneous-certificate-configuration-settings"></a>Opciones de configuración de certificados varios
Se mencionó anteriormente que la configuración de seguridad de un clúster de Service Fabric también permite cambiar sutilmente el comportamiento del código de autenticación. Aunque el artículo sobre [Configuración de un clúster de Service Fabric](service-fabric-cluster-fabric-settings.md) representa la lista completa y más actualizada de valores de configuración, expandiremos el significado de una selección de opciones de configuración de seguridad para explicar en detalle la autenticación basada en certificados. Para cada opción de configuración, explicaremos la intención, el valor predeterminado o el comportamiento, cómo afecta a la autenticación y qué valores son aceptables.

Como se mencionó, la validación de certificados siempre implica la compilación y evaluación de la cadena del certificado. En el caso de los certificados emitidos por una entidad de certificación, esta llamada API del sistema operativo aparentemente simple conlleva varias llamadas salientes a varios puntos de conexión de la PKI emisora, el almacenamiento en caché de las respuestas etc. Dada la prevalencia de las llamadas de validación de certificados en un clúster de Service Fabric, cualquier problema en los puntos de conexión de la PKI puede dar lugar a una menor disponibilidad del clúster o a un desglose inadecuado. Aunque no se pueden suprimir las llamadas salientes (para obtener más información sobre esto, consulte a continuación la sección de preguntas más frecuentes), se puede usar la siguiente configuración para enmascarar los errores de validación causados por errores de llamadas de CRL.

  * CrlCheckingFlag: en la sección "Seguridad", cadena convertida en UINT. El valor de esta configuración se usa en Service Fabric para enmascarar los errores de estado de la cadena de certificados cambiando el comportamiento de la creación de cadenas. Se pasa a la llamada de Win32 CryptoAPI [CertGetCertificateChain](/windows/win32/api/wincrypt/nf-wincrypt-certgetcertificatechain) como parámetro "dwFlags" y se puede establecer en cualquier combinación válida de marcas aceptadas por la función. Un valor de 0 obliga al entorno en tiempo de ejecución de Service Fabric a omitir cualquier error de estado de confianza. No se recomienda, ya que su uso constituiría una exposición de seguridad significativa. El valor predeterminado es 0x40000000 (CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT).

  Cuándo usarlo: para las pruebas locales, con certificados autofirmados o certificados de desarrollador que no tienen el formato completo o no tienen una infraestructura de clave pública (PKI) adecuada para admitir los certificados. También puede usarlo como mitigación en entornos con separación de aire durante la transición entre las PKI.

  Modo de uso: veremos un ejemplo en el que se fuerza la comprobación de la revocación para que solo tenga acceso a las direcciones URL almacenadas en caché. Suponiendo que:
  ```C++
  #define CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY         0x80000000
  ```
  la declaración del manifiesto de clúster se convierte en:
  ```xml
    <Section Name="Security">
      <Parameter Name="CrlCheckingFlag" Value="0x80000000" />
    </Section>
  ```

  * IgnoreCrlOfflineError: en la sección "Seguridad", valor booleano con un valor predeterminado de "false". Representa un acceso directo para suprimir un estado de error de compilación de cadena "revocación sin conexión" (o un estado de error de validación de directiva de cadena posterior).

  Cuándo usarlo: con pruebas locales o certificados de desarrollador no respaldados por una PKI adecuada. Úselo como mitigación en entornos con separación de aire o cuando se sepa que la PKI no es accesible.

  Modo de uso:
  ```xml
    <Section Name="Security">
      <Parameter Name="IgnoreCrlOfflineError" Value="true" />
    </Section>
  ```

  Otros valores de configuración importantes (todo en la sección "Seguridad"):
  * AcceptExpiredPinnedClusterCertificate: se describe en la sección dedicada a la validación de certificados basados en huellas digitales; permite aceptar certificados de clúster autofirmados expirados. 
  * CertificateExpirySafetyMargin: intervalo expresado en minutos antes de la marca de tiempo de NotAfter del certificado y durante el cual el certificado se considera en riesgo por motivos de expiración. Service Fabric supervisa los certificados de clúster y emite periódicamente informes de mantenimiento sobre su disponibilidad restante. Dentro del intervalo de "Seguridad", estos informes de mantenimiento se elevan al estado "advertencia". El valor predeterminado es de 30 días.
  * CertificateHealthReportingInterval: controla la frecuencia de los informes de mantenimiento relativos a la validez del tiempo restante de los certificados de clúster. Los informes solo se emitirán una vez para este intervalo. El valor se expresa en segundos, con un valor predeterminado de 8 horas.
  * EnforcePrevalidationOnSecurityChanges: valor booleano que controla el comportamiento de la actualización del clúster al detectar cambios de configuración de seguridad. Si se establece en "true", la actualización de clúster intentará asegurarse de que al menos uno de los certificados que coincide con alguna de las reglas de presentación puede superar una regla de validación correspondiente. La validación previa se ejecuta antes de que la nueva configuración se aplique a cualquier nodo, pero solo se ejecuta en el nodo que hospeda la réplica principal del servicio Administrador de clústeres en el momento de iniciar la actualización. En el momento de redactar este documento, la configuración tiene un valor predeterminado de "false" y se establecerá en "true" para los nuevos clústeres de Azure Service Fabric con una versión de entorno en tiempo de ejecución que empiece por 7,1.
 
### <a name="end-to-end-scenario-examples"></a>Escenario de un extremo a otro (ejemplos)
Hemos examinado las reglas de presentación, las reglas de validación y las marcas de ajuste, pero ¿cómo funciona todo esto de forma conjunta? En esta sección, vamos a trabajar con dos ejemplos de un extremo a otro que muestran cómo se puede aprovechar la configuración de seguridad para actualizaciones de clústeres seguras. Tenga en cuenta que esto no pretende ser una disertación completa sobre la adecuada administración de certificados en Service Fabric. Para obtener información sobre ese tema, busque un artículo complementario.

La separación de las reglas de presentación y validación plantea la pregunta obvia (o preocupación) de si pueden diferir y cuáles serían las consecuencias. En realidad, es posible que la selección de un nodo de un certificado de autenticación no pase las reglas de validación de otro nodo. De hecho, esta discrepancia es la causa principal de los incidentes relacionados con la autenticación. Al mismo tiempo, la independencia de estas reglas permite que un clúster continúe funcionando durante una actualización que cambia la configuración de seguridad del clúster. Tenga en cuenta que, al aumentar las reglas de validación como primer paso, todos los nodos del clúster convergerán en la nueva configuración al mismo tiempo que se siguen usando las credenciales actuales. 

Recuerde que, en un clúster de Service Fabric, una actualización progresa a través de (hasta 5) "dominios de actualización" o UD. Solo se actualizan o cambian los nodos del UD actual y la actualización pasará al siguiente UD solo si la disponibilidad del clúster lo permite. (Consulte [Actualización de un clúster de Service Fabric](service-fabric-cluster-upgrade.md) y otros artículos del mismo tema para obtener más detalles). Los cambios de certificado o seguridad son especialmente arriesgados, ya que pueden aislar los nodos del clúster o dejar el clúster al borde de la pérdida de cuórum.

Usaremos la siguiente notación para describir la configuración de seguridad de un nodo:

Nk: {P:{TP=A}, V:{TP=A}}, donde:
  - "NK" representa un nodo en el dominio de actualización *k*.
  - "P" representa las reglas de presentación actuales del nodo (suponiendo que solo hacemos referencia a los certificados de clúster). 
  - "V" representa las reglas de validación actuales del nodo (solo en el certificado de clúster).
  - "TP=A" representa una declaración basada en huellas digitales, donde "A" es una huella digital de certificado.
  - "CN=B" representa una declaración basada en nombres comunes, donde "B" es el nombre común del firmante del certificado. 

#### <a name="rotating-a-cluster-certificate-declared-by-thumbprint"></a>Rotación de un certificado de clúster declarado por huella digital
En la secuencia siguiente se describe cómo se puede usar una actualización en dos fases para presentar de forma segura un certificado de clúster secundario, declarado mediante huella digital. En la primera fase se presenta la nueva declaración de certificado en las reglas de validación y, en la segunda, se presenta en las reglas de presentación:
  - Estado inicial: N0 = {P:{TP=A}, V:{TP=A}}, ... Nk = {P:{TP=A}, V:{TP=A}}: el clúster está en reposo; todos los nodos comparten una configuración común.
  - Al completar el dominio de actualización 0: N0 = {P:{TP=A}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A}, V:{TP=A}}: los nodos de UD0 presentarán el certificado A y aceptarán los certificados A o B; todos los demás nodos están presentes y solo aceptan el certificado A.
  - Al completar el dominio de actualización más reciente: N0 = {P:{TP=A}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A}, V:{TP=A, TP=B}}: todos los nodos presentan el certificado A; todos los nodos aceptarían el certificado A o B.
      
En este momento, el clúster vuelve a estar en equilibrio y la segunda fase de la configuración de seguridad de actualización o cambio puede comenzar:
  - Al completar el dominio de actualización 0: N0 = {P:{TP=A, TP=B}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A}, V:{TP=A, TP=B}}: los nodos de UD0 comenzarán a presentar B, que se acepta en cualquier otro nodo del clúster.
  - Al completar el dominio de actualización más reciente: N0 = {P:{TP=A, TP=B}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A, TP=B}, V:{TP=A, TP=B}}: todos los nodos han cambiado para presentar el certificado B. El certificado A ya se puede retirar o quitar de la definición de clúster con un conjunto posterior de actualizaciones.

#### <a name="converting-a-cluster-from-thumbprint--to-common-name-based-certificate-declarations"></a>Conversión de un clúster de declaraciones de certificado basadas en huellas digitales a declaraciones de certificado basadas en nombres comunes
Del mismo modo, el cambio del tipo de declaración de certificado (de huella digital a nombre común) seguirá el mismo patrón que el anterior. Tenga en cuenta que las reglas de validación permiten declarar los certificados de un rol determinado mediante la huella digital y el nombre común en la misma definición de clúster. Por el contrario, las reglas de presentación solo permiten una forma de declaración. Casualmente, el enfoque seguro para convertir un certificado de clúster de huella digital a nombre común consiste en presentar primero el certificado de destino previsto mediante huella digital y, a continuación, cambiar esa declaración por una basada en nombres comunes. En el ejemplo siguiente, se asume que la huella digital "A" y el nombre común del firmante "B" hacen referencia al mismo certificado. 

  - Estado inicial: N0 = {P:{TP=A}, V:{TP=A}}, ... Nk = {P:{TP=A}, V:{TP=A}}: el clúster está en reposo; todos los nodos comparten una configuración común, que tiene A como la huella digital del certificado principal.
  - Al completar el dominio de actualización 0: N0 = {P:{TP=A}, V:{TP=A, CN=B}}, ... Nk = {P:{TP=A}, V:{TP=A}}: los nodos de UD0 presentarán el certificado A y aceptarán la huella digital A o el nombre común B; todos los demás nodos están presentes y solo aceptan el certificado A.
  - Al completar el dominio de actualización más reciente: N0 = {P:{TP=A}, V:{TP=A, CN=B}}, ... Nk = {P:{TP=A}, V:{TP=A, CN=B}}: todos los nodos presentan el certificado A; todos los nodos aceptarían el certificado A (huella digital) o B (nombre común).

En este momento, podemos continuar con el cambio de las reglas de presentación con una actualización posterior:
  - Al completar el dominio de actualización 0: N0 = {P:{CN=B}, V:{TP=A, CN=B}}, ... Nk = {P:{TP=A}, V:{TP=A, CN=B}}: los nodos de UD0 presentan el certificado B que encuentra el nombre común y aceptan certificados con la huella digital A o el nombre común B; todos los demás nodos están presentes y aceptan solo el certificado A, seleccionado mediante huella digital.
  - Al completar el dominio de actualización más reciente: N0 = {P:{CN=B}, V:{TP=A, CN=B}}, ... Nk = {P:{CN=B}, V:{TP=A, CN=B}}: todos los nodos presentan el certificado B que encuentra el nombre común; todos los nodos aceptarían el certificado A (huella digital) o B (nombre común).
    
La finalización de la fase 2 también marca la conversión del clúster a certificados basados en nombres comunes. Las declaraciones de validación basadas en huellas digitales se pueden quitar en una actualización posterior del clúster.

> [!NOTE]
> En los clústeres de Azure Service Fabric, el proveedor de recursos de Service Fabric organiza los flujos de trabajo presentados anteriormente. El propietario del clúster sigue siendo responsable del aprovisionamiento de certificados en el clúster de acuerdo con las reglas indicadas (presentación o validación) y se recomienda realizar cambios en varios pasos.

En un artículo independiente, abordaremos el tema de administración y aprovisionamiento de certificados en un clúster de Service Fabric.

## <a name="troubleshooting-and-frequently-asked-questions"></a>Solución de problemas y preguntas frecuentes
Aunque no es fácil depurar los problemas relacionados con la autenticación en los clústeres de Service Fabric, esperamos que las sugerencias siguientes sirvan de ayuda. La manera más sencilla de comenzar las investigaciones consiste en examinar los registros de eventos de Service Fabric en los nodos del clúster; no solo los que muestran síntomas, sino también los nodos que están en funcionamiento pero que no pueden conectarse a uno de sus vecinos. En Windows, los eventos de importancia se registran normalmente en los canales "Applications and Services Logs\Microsoft-ServiceFabric\Admin" u "Operational", respectivamente. A veces, puede resultar útil [habilitar el registro de CAPI2](/archive/blogs/benjaminperkins/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues) para capturar más detalles sobre la validación de certificados, la recuperación de CRL/CTL etc. (Recuerde deshabilitarlo después de completar la reproducción; puede ser bastante detallado).

Los síntomas típicos que se manifiestan en un clúster que experimenta problemas de autenticación son: 
  - Los nodos están inactivos o en ciclo. 
  - Los intentos de conexión se rechazan.
  - Los intentos de conexión agotan el tiempo de espera.

Cada uno de los síntomas puede deberse a problemas diferentes y la misma causa raíz puede mostrar diferentes manifestaciones; como tal, simplemente mostraremos una pequeña muestra de problemas típicos, con recomendaciones para corregirlos. 

* Los nodos pueden intercambiar mensajes, pero no se pueden conectar. Una posible causa de que se produzcan intentos de conexión es el error "el certificado no coincide": una de las partes de una conexión de Service Fabric a Service Fabric presenta un certificado que produce un error en las reglas de validación del destinatario. Puede ir acompañado de cualquiera de los siguientes errores: 
  ```C++
  0x80071c44    -2147017660 FABRIC_E_SERVER_AUTHENTICATION_FAILED
  ```
  Para diagnosticar o investigar más: en cada uno de los nodos que intentan establecer la conexión, determine qué certificado se está presentando. Examine el certificado y pruebe y emule las reglas de validación (compruebe si existe igualdad en el nombre común o la huella digital; compruebe si se han especificado las huellas digitales del emisor).

  Otro código de error común que lo acompaña puede ser el siguiente:
  ```C++
  0x800b0109    -2146762487 CERT_E_UNTRUSTEDROOT
  ```
  En este caso, el certificado se declara mediante el nombre común y se aplica cualquiera de las siguientes acciones:
    - los emisores no están anclados y el certificado raíz no es de confianza; o bien
    - los emisores están anclados, pero la declaración no incluye la huella digital del emisor directo de este certificado.

* Un nodo está activo, pero no puede conectarse a otros nodos; otros nodos no reciben el tráfico entrante desde el nodo con errores. En este caso, es posible que se produzca un error en la carga del certificado en el nodo local. Busque los errores siguientes:
  - No se encontró el certificado: asegúrese de que el contenido del almacén de certificados LocalMachine\My (o como se especificó) puede resolver los certificados que se declaran en las reglas de presentación. 
    Entre las posibles causas de un error se incluyen las siguientes: 
      - Caracteres no válidos en la declaración de huella digital.
      - El certificado no se ha instalado.
      - El certificado ha caducado.
      - La declaración de nombre común incluye el prefijo "CN=".
      - La declaración especifica un carácter comodín y no existe ninguna coincidencia exacta en el almacén de certificados (declaración: CN=*.mydomain.com; certificado real: CN=server.mydomain.com)

  - Credenciales desconocidas: indica que falta una clave privada correspondiente al certificado; normalmente va acompañado por el código de error: 
    ```C++ 
    0x8009030d  -2146893043 SEC_E_UNKNOWN_CREDENTIALS
    0x8009030e  -2146893042 SEC_E_NO_CREDENTIALS
    ```
    Para solucionarlo, compruebe la existencia de la clave privada. Compruebe que SFAdmins tiene concedido acceso de "lectura y ejecución" para la clave privada.

  - Tipo de proveedor incorrecto: indica un certificado Crypto New Generation (CNG) ("Proveedor de almacenamiento de claves de software de Microsoft"). En este momento, Service Fabric solo admite certificados CAPI1. Normalmente va acompañado por el código de error:
    ```C++
    0x80090014  -2146893804 NTE_BAD_PROV_TYPE
    ```
    Para solucionarlo, vuelva a crear el certificado de clúster con un proveedor de CAPI1 (por ejemplo, "Microsoft ha mejorado el proveedor de servicios criptográficos RSA y AES"). Para obtener más información sobre los proveedores de cifrado, consulte [Información sobre los proveedores de servicios criptográficos](/windows/win32/seccertenroll/understanding-cryptographic-providers).
