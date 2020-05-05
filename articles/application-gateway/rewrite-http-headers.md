---
title: Reescritura de encabezados HTTP con Azure Application Gateway | Microsoft Docs
description: En este artículo, se proporciona información general sobre la reescritura de encabezados HTTP en Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 04/27/2020
ms.author: absha
ms.openlocfilehash: 421c1f4d1abe9be5f5081235e78ebe77b1813e6e
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82562243"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Reescritura de encabezados HTTP con Azure Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Los encabezados HTTP permiten que el cliente y el servidor pasen información adicional con la solicitud o la respuesta. Al reescribir estos encabezados, puede realizar tareas importantes, como agregar campos de encabezado relacionados con la seguridad (como HSTS y X-XSS-Protection), quitar campos de encabezado de respuesta que pueden revelar información confidencial o eliminar información de puertos de los encabezados X-Forwarded-For.

Application Gateway permite agregar, quitar o actualizar los encabezados de respuesta y de solicitudes HTTP, mientras los paquetes de solicitudes y respuestas se mueven entre los grupos de back-end y de cliente. También permite agregar las condiciones necesarias para asegurarse de que los encabezados especificados se reescriben solo cuando se cumplen ciertas condiciones.

Application Gateway también admite varias [variables de servidor](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) que ayudan a almacenar información adicional acerca de las solicitudes y respuestas. Así se facilita la creación de reglas de reescritura eficaces.

> [!NOTE]
>
> La compatibilidad con la reescritura de encabezados HTTP solo está disponible para [Standard_V2 y WAF_v2 SKU](application-gateway-autoscaling-zone-redundant.md).

![Reescritura de encabezados](media/rewrite-http-headers/rewrite-headers.png)

## <a name="supported-headers"></a>Encabezados admitidos

Puede volver a escribir todos los encabezados de solicitudes y respuestas, excepto para los encabezados Host, Connection y Upgrade. También puede usar la puerta de enlace de aplicaciones para crear los encabezados personalizados y agregarlos a las solicitudes y respuestas que se enrutan por ella.

## <a name="rewrite-conditions"></a>Condiciones de reescritura

Puede utilizar condiciones de reescritura para evaluar el contenido de las solicitudes HTTP(S) y las respuestas y realizar una reescritura de encabezados cuando una o varias condiciones se cumplan. La puerta de enlace de aplicaciones utiliza estos tipos de variables para evaluar el contenido de las respuestas y las solicitudes HTTP(S):

- Encabezados HTTP en la solicitud.
- Encabezados HTTP en la respuesta.
- Variables de servidor de Application Gateway.

Puede usar una condición para evaluar si una variable especificada está presente o coincide con un valor específico o con un patrón específico. Se usa la [biblioteca de expresiones regulares compatibles de Perl (PCRE)](https://www.pcre.org/) para configurar la coincidencia de los patrones de expresión regular en las condiciones. Para obtener información sobre la sintaxis de las expresiones regulares, vea la [página principal de las expresiones regulares de Perl](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Acciones de reescritura

Las acciones de reescritura se usan para especificar los encabezados de solicitud y de respuesta que quiere reescribir y el nuevo valor para los encabezados. Puede crear un nuevo encabezado, modificar el valor de un encabezado existente o eliminar un encabezado existente. El valor de un nuevo encabezado o un encabezado existente se pueden establecer para estos tipos de valores:

- Texto.
- Encabezado de la solicitud. Para especificar un encabezado de solicitud, tiene que usar la sintaxis siguiente: {http_req_*headerName*}.
- Encabezado de respuesta. Para especificar un encabezado de respuesta, tiene que usar la sintaxis {http_req_*headerName*}.
- Variable de servidor. Para especificar una variable de servidor, tiene que usar la sintaxis {var_*serverVariable*}.
- Una combinación de texto, un encabezado de solicitud, un encabezado de respuesta y una variable de servidor.

## <a name="server-variables"></a>Variables de servidor

App Gateway usa variables de servidor para almacenar información útil sobre el servidor, la conexión con el cliente y la solicitud actual en la conexión. La dirección IP del cliente y el tipo de explorador web son ejemplos de la información almacenada. Las variables de servidor cambian dinámicamente, por ejemplo, cuando se carga una página nueva o cuando se publica un formulario. Puede usar estas variables para evaluar las condiciones de reescritura y volver a escribir los encabezados. Para usar el valor de las variables del servidor para volver a escribir los encabezados, deberá especificar estas variables en la sintaxis {var_*serverVariable*}.

Application Gateway admite estas variables de servidor:

| Nombre de la variable | Descripción                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | El campo de encabezado de solicitud de cliente X-Forwarded-For con la variable `client_ip` (consulte la explicación más adelante en esta tabla) anexada a él en el formato IP1, IP2, IP3 y así sucesivamente. Si el campo X-Forwarded-For no se encuentra en el encabezado de solicitud de cliente, la variable `add_x_forwarded_for_proxy` es igual que la variable `$client_ip`. Esta variable es especialmente útil cuando se desea reescribir el encabezado X-Forwarded-For establecido por Application Gateway, de forma que el encabezado contenga solo la dirección IP del cliente y no la información del puerto. |
| ciphers_supported          | Una lista de cifrados admitidos por el cliente.          |
| ciphers_used               | La cadena de cifrados que se usa para una conexión TLS establecida. |
| client_ip                  | La dirección IP del cliente desde la que la puerta de enlace de la aplicación recibió la solicitud. Si hay un proxy inverso antes de la puerta de enlace de aplicaciones y el cliente de origen, *client_ip* devolverá la dirección IP del proxy inverso. |
| client_port                | Puerto del cliente.                                                  |
| client_tcp_rtt             | Información sobre la conexión TCP de cliente. Está disponible en los sistemas que admiten la opción de socket TCP_INFO. |
| client_user                | Al usar la autenticación HTTP, el nombre de usuario proporcionado para la autenticación. |
| host                       | En este orden de prioridad: nombre de host de la línea de la solicitud, nombre de host del campo de encabezado de la solicitud del “Host”, o bien el nombre del servidor que coincida con una solicitud. Ejemplo: en la solicitud *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* , el valor del host será *contoso.com*. |
| cookie_*name*              | El cookie de *nombre*.                                            |
| http_method                | Método usado para realizar la solicitud de URL. Por ejemplo, GET, POST, etc. |
| http_status                | Estado de la sesión. Por ejemplo, 200, 400 o 403.                       |
| http_version               | Protocolo de solicitud. Normalmente, HTTP/1.0, HTTP/1.1 o HTTP/2.0. |
| query_string               | La lista de pares de variable-valor que aparecen después de “?” en la dirección URL solicitada. Ejemplo: en la solicitud *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* , el valor de query_string será *id=123&title=fabrikam*. |
| received_bytes             | La longitud de la solicitud (incluida la línea de la solicitud, el encabezado y el cuerpo de la solicitud). |
| request_query              | Los argumentos en la línea de la solicitud.                                |
| request_scheme             | El esquema de la solicitud: HTTP o HTTPS.                            |
| request_uri                | El URI original completo de la solicitud (con argumentos). Ejemplo: en la solicitud *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* , el valor de request_uri será */article.aspx?id=123&title=fabrikam*.   |
| sent_bytes                 | El número de bytes enviados a un cliente.                             |
| server_port                | El puerto del servidor que ha aceptado una solicitud.                 |
| ssl_connection_protocol    | El protocolo de una conexión TLS establecida.        |
| ssl_enabled                | "On" si la conexión funciona en modo TLS. No puede ser una cadena vacía. |
| uri_path                   | Identifica el recurso específico en el host al que el cliente web quiere acceder. Esta es la parte del URI de solicitud sin los argumentos. Ejemplo: en la solicitud *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* , el valor de uri_path será */article.aspx*.  |

## <a name="rewrite-configuration"></a>Configuración de la reescritura

Para configurar la reescritura de encabezados HTTP, es preciso completar estos pasos.

1. Cree los objetos necesarios para la reescritura de encabezados HTTP:

   - **Acción de reescritura**: Se usa para especificar los campos de solicitud y encabezado de solicitud que desee reescribir y el nuevo valor para los encabezados. Puede asociar una o varias condiciones de reescritura con una acción de reescritura.

   - **Condición de reescritura**: Una configuración opcional. Las condiciones de reescritura evalúan el contenido de las solicitudes y respuestas HTTP(S). La acción de reescritura tendrá lugar si la solicitud o respuesta HTTP(S) coinciden con la condición de reescritura.

     Si asocia más de una condición con una acción, la acción se produce solo cuando se cumplen todas las condiciones. En otras palabras, se trata de una operación AND lógica.

   - **Regla de reescritura**: Contiene varias combinaciones de acción de reescritura y condición de reescritura.

   - **Secuencia de reglas**: Ayuda a determinar el orden en el que se ejecutan las reglas de reescritura. Esta configuración es útil cuando hay varias reglas de reescritura en un conjunto de reescritura. Una regla de reescritura que tiene un valor de secuencia de reglas más bajo se ejecuta primero. Si asigna el mismo valor de secuencia de reglas a dos reglas de reescritura, el orden de ejecución es no determinista.

   - **Conjunto de reescritura**: Contiene varias reglas de reescritura que se asociarán con una regla de enrutamiento de solicitudes.

2. Conecte el conjunto de reescritura (*rewriteRuleSet*) con una regla de enrutamiento. La configuración de reescritura se conecta al agente de escucha de origen mediante la regla de enrutamiento. Cuando usa una regla de enrutamiento básica, la configuración de reescritura de encabezados se asocia a un agente de escucha de origen y es una reescritura de encabezados global. Cuando usa una regla de enrutamiento basada en rutas, la configuración de reescritura de encabezados se define en la asignación de la ruta de URL. En este caso, solo se aplica al área específica de la ruta de acceso de un sitio.
   > [!NOTE]
   > La reescritura de dirección URL modifica los encabezados; no cambia la dirección URL de la ruta de acceso.

Puede crear varios conjuntos de reescritura de encabezados HTTP y aplicar cada conjunto de reescritura a varios agentes de escucha. Pero solo puede aplicar un conjunto de reescritura a un agente de escucha específico.

## <a name="common-scenarios"></a>Escenarios frecuentes

Estos son algunos escenarios habituales para usar la reescritura de encabezados.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Eliminación de la información del puerto desde el encabezado X-Forwarded-For

Application Gateway inserta un encabezado X-Forwarded-For en todas las solicitudes antes de reenviar las solicitudes al back-end. Este encabezado es una lista de puertos IP separados por comas. Puede haber escenarios en que los servidores back-end necesiten solo los encabezados que van a contener las direcciones IP. Puede usar la reescritura de encabezados para quitar la información del puerto desde el encabezado X-Forwarded-For. Una manera de hacerlo es establecer el encabezado en la variable de servidor add_x_forwarded_for_proxy:

![Eliminación del puerto](media/rewrite-http-headers/remove-port.png)

### <a name="modify-a-redirection-url"></a>Modificación de una dirección URL de redirección

Cuando una aplicación back-end envía una respuesta de redireccionamiento, es posible que le interese redirigir el cliente a una dirección URL diferente de la especificada por la aplicación back-end. Por ejemplo, es posible que quiera hacerlo cuando un servicio de aplicaciones se hospeda detrás de una puerta de enlace de aplicaciones y requiere que el cliente realice un redireccionamiento a su ruta de acceso relativa. (Por ejemplo, un redireccionamiento de contoso.azurewebsites.net/path1 a contoso.azurewebsites.net/path2).

Dado que App Service es un servicio multiinquilino, usa el encabezado de host en la solicitud para enrutar la solicitud al punto de conexión correcto. Los servicios de aplicaciones tienen un nombre de dominio predeterminado, *. azurewebsites.net, (por ejemplo, contoso.azurewebsites.net) que es diferente del nombre de dominio de la puerta de enlace de aplicaciones (por ejemplo, contoso.com). Dado que la solicitud original desde el cliente tiene el nombre de dominio (contoso.com) de la puerta de enlace de aplicaciones como el nombre de host, la puerta de enlace de aplicaciones cambia el nombre de host a contoso.azurewebsites.net. Realiza este cambio para que el servicio de aplicaciones pueda enrutar la solicitud al punto de conexión correcto.

Cuando el servicio de aplicaciones envía una respuesta de redireccionamiento, usa el mismo nombre de host en el encabezado de ubicación de su respuesta que el que aparece en la solicitud que recibe de la puerta de enlace de aplicaciones. Así pues, el cliente hará la solicitud directamente a contoso.azurewebsites.net/path2, en lugar de pasar por la puerta de enlace de aplicaciones (contoso.com/path2). No es conveniente omitir la puerta de enlace de aplicaciones.

Para resolver este problema, puede establecer el nombre de host en el encabezado de ubicación en el nombre de dominio de la puerta de enlace de aplicaciones.

Estos son los pasos para reemplazar el nombre de host:

1. Cree una regla de reescritura con una condición que evalúe si el encabezado de ubicación en la respuesta contiene azurewebsites.net. Escriba el patrón `(https?):\/\/.*azurewebsites\.net(.*)$`.
1. Realice una acción para volver a escribir el encabezado de ubicación de modo que tenga el nombre de host de la puerta de enlace de aplicaciones. Para ello, escriba `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como valor del encabezado.

![Modificación del encabezado de ubicación](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Implementación de los encabezados HTTP de seguridad para evitar las vulnerabilidades

Puede corregir varias vulnerabilidades de seguridad si implementa los encabezados necesarios en la respuesta de la aplicación. Estos encabezados de seguridad incluyen X-XSS-Protection Strict-Transport-Security y Content-Security-Policy. Puede usar Application Gateway para establecer estos encabezados para todas las respuestas.

![Encabezado de seguridad](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>Eliminación de los encabezados no deseados

Es posible que desee quitar los encabezados que revelen información confidencial de una respuesta HTTP. Por ejemplo, es posible que desee quitar información como el nombre del servidor back-end, el sistema operativo o los detalles de la biblioteca. Puede usar la puerta de enlace de aplicaciones para quitar estos encabezados:

![Eliminación del encabezado](media/rewrite-http-headers/remove-headers.png)

### <a name="check-for-the-presence-of-a-header"></a>Comprobación de la presencia de un encabezado

Puede evaluar un encabezado de respuesta o de solicitud HTTP para comprobar la presencia de una variable de servidor o encabezado. Esta evaluación es útil cuando desea realizar una reescritura de encabezado solo si está presente un encabezado determinado.

![Comprobación de la presencia de un encabezado](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Limitaciones

- Si una respuesta tiene más de un encabezado con el mismo nombre, volver a escribir el valor de uno de esos encabezados dará como resultado la eliminación de los demás en la respuesta. Normalmente, esto puede suceder con el encabezado Set-Cookie, ya que puede tener más de uno en una respuesta. Uno de estos escenarios es cuando se usa un servicio de aplicaciones con una puerta de enlace de aplicaciones y ha configurado la afinidad de sesión basada en cookies en la puerta de enlace de aplicaciones. En este caso, la respuesta contendrá dos encabezados Set-Cookie: uno utilizado por el servicio de aplicaciones, por ejemplo, `Set-Cookie: ARRAffinity=ba127f1caf6ac822b2347cc18bba0364d699ca1ad44d20e0ec01ea80cda2a735;Path=/;HttpOnly;Domain=sitename.azurewebsites.net`, y otro para la afinidad de la puerta de enlace de aplicaciones, es decir, `Set-Cookie: ApplicationGatewayAffinity=c1a2bd51lfd396387f96bl9cc3d2c516; Path=/`. Si se vuelve a escribir uno de los encabezados Set-Cookie en este escenario, podría quitar el otro encabezado Set-Cookie de la respuesta.

- No se admiten las reescrituras cuando la puerta de enlace de aplicaciones está configurada para redirigir las solicitudes o para mostrar una página de error personalizada.

- Aún no se admite la reescritura de los encabezados de conexión, actualización y hospedaje.

- Los nombres de encabezado solo pueden contener caracteres alfanuméricos y símbolos específicos definidos en [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Actualmente no se admite el carácter especial de subrayado (\_) en los nombres de encabezado.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo volver a escribir los encabezados HTTP, consulte:

- [Reescribir los encabezados HTTP de solicitud y respuesta con Azure Application Gateway mediante Azure Portal](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
- [Vuelva a escribir los encabezados HTTP con Application Gateway](add-http-header-rewrite-rule-powershell.md)
