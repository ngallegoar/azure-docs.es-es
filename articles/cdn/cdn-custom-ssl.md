---
title: 'Tutorial: Configuración de HTTPS en un dominio personalizado de Azure CDN | Microsoft Docs'
description: En este tutorial, aprenderá a habilitar y deshabilitar HTTPS en su dominio personalizado de punto de conexión de Azure CDN.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/1/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 5cf1181c41af1edc752205f4477f18b78680f484
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81254026"
---
# <a name="tutorial-configure-https-on-an-azure-cdn-custom-domain"></a>Tutorial: Configuración de HTTPS en un dominio personalizado de Azure CDN

En este tutorial se muestra cómo habilitar el protocolo HTTPS en un dominio personalizado asociado a un punto de conexión de Azure CDN. Mediante el protocolo HTTPS en el dominio personalizado (por ejemplo, https:\//www.contoso.com), se garantiza que los datos confidenciales se entregan de manera segura a través del cifrado TLS/SSL cuando se envían por Internet. Cuando el explorador web se conecta a un sitio web a través de HTTPS, valida el certificado de seguridad del sitio web y comprueba que lo ha emitido una entidad de certificación legítima. Este proceso aporta seguridad y protege las aplicaciones web de posibles ataques.

Azure CDN admite HTTPS en un punto de conexión de CDN de manera predeterminada. Por ejemplo, si crea un punto de conexión de CDN (como https:\//contoso.azureedge.net), HTTPS se habilita de manera automática.  

Algunos de los atributos clave de la característica de HTTPS personalizado son:

- Sin costo adicional: la adquisición o renovación de certificados no tiene costos y el tráfico HTTPS no supone un costo adicional. Solo es preciso pagar por los GB de salida de la red CDN.

- Habilitación simple: el aprovisionamiento en un solo clic está disponible desde [Azure Portal](https://portal.azure.com). También puede utilizar la API de REST u otras herramientas de desarrollo para habilitar la característica.

- La administración completa de certificados está disponible: toda la adquisición y administración de certificados se controla de manera automática. Los certificados se aprovisionan y se renuevan automáticamente antes de la expiración, lo que elimina los riesgos de interrupción del servicio debido a la expiración de un certificado.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> - Habilitar el protocolo HTTPS en su dominio personalizado
> - Usar un certificado administrado por CDN 
> - Usar su propio certificado
> - Validar el dominio
> - Deshabilitar el protocolo HTTPS en su dominio personalizado

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)] 

Para poder completar los pasos de este tutorial, primero es preciso crear un perfil de CDN y al menos un punto de conexión de CDN. Para más información, consulte [Inicio rápido: Creación de un perfil y un punto de conexión de Azure CDN](cdn-create-new-endpoint.md).

Además, debe asociar un dominio personalizado de Azure CDN en el punto de conexión de CDN. Para más información, consulte el [Tutorial: Incorporación de un dominio personalizado a un punto de conexión de Azure CDN](cdn-map-content-to-custom-domain.md).

> [!IMPORTANT]
> Los certificados administrados por CDN no están disponibles para dominios raíz o de Apex. Si el dominio personalizado de Azure CDN es un dominio raíz o de Apex, debe usar la característica Traiga su propio certificado. 
>

---

## <a name="tlsssl-certificates"></a>Certificados TLS/SSL
Para habilitar el protocolo HTTPS para entregar contenido en un dominio personalizado de Azure CDN de forma segura, debe usar un certificado TLS/SSL. Puede usar un certificado que esté administrado por Azure CDN o usar su propio certificado.


# <a name="option-1-default-enable-https-with-a-cdn-managed-certificate"></a>[Opción 1 (valor predeterminado): habilitación de HTTPS con un certificado administrado por CDN](#tab/option-1-default-enable-https-with-a-cdn-managed-certificate)

Cuando usa un certificado administrado por CDN, la característica HTTPS se puede activar con unos pocos clics. Azure CDN administra completamente las tareas de administración de certificados, como adquisición y renovación. Después de habilitar la característica, el proceso se inicia inmediatamente. Si el dominio personalizado ya está asignado al punto de conexión de CDN, no es necesario realizar ninguna otra acción. Azure CDN procesa los pasos y completa su solicitud automáticamente. Sin embargo, si su dominio personalizado se asigna en otra parte, debe usar el correo electrónico para validar la propiedad del dominio.

Para habilitar HTTPS en un dominio personalizado, siga estos pasos:

1. Vaya a [Azure Portal](https://portal.azure.com) para buscar un certificado administrado por Azure CDN. Busque y seleccione **Perfiles de CDN**. 

2. Elija el perfil **Azure CDN estándar de Microsoft**, **Azure CDN estándar de Akamai**, **Azure CDN estándar de Verizon** o **Azure CDN prémium de Verizon**.

3. En la lista de puntos de conexión de CDN, haga clic en el que contiene el dominio personalizado.

    ![Lista de puntos de conexión](./media/cdn-custom-ssl/cdn-select-custom-domain-endpoint.png)

    Aparece la página **Punto de conexión**.

4. En la lista de dominios personalizados, seleccione el dominio personalizado para el que desea habilitar HTTPS.

    ![Lista de dominios personalizados](./media/cdn-custom-ssl/cdn-custom-domain.png)

    Aparece la página **Dominio personalizado**.

5. En el tipo de administración de certificados, seleccione **Certificate management type** (Administrado por CDN).

6. Seleccione **On** (Activado) para habilitar HTTPS.

    ![Estado de HTTPS de dominio personalizado](./media/cdn-custom-ssl/cdn-select-cdn-managed-certificate.png)

7. Continúe y [valide el dominio](#validate-the-domain).


# <a name="option-2-enable-https-with-your-own-certificate"></a>[Opción 2: habilitación de HTTPS con su propio certificado](#tab/option-2-enable-https-with-your-own-certificate)

> [!IMPORTANT]
> Esta opción solo está disponible con los perfiles **Azure CDN de Microsoft** y **Azure CDN de Verizon**. 
>
 
Puede usar su propio certificado para habilitar la característica HTTPS. Este proceso se realiza a través de una integración con Azure Key Vault, lo cual permite almacenar de forma segura los certificados. Azure CDN usa este mecanismo de seguridad para obtener el certificado y requiere algunos pasos adicionales. Cuando cree el certificado TLS/SSL, debe usar una entidad de certificación (CA) permitida. Si no lo hace, si usa una entidad de certificación no permitida, la solicitud se rechazará. Para ver una lista de las entidades de certificación permitidas, vea [Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure CDN](cdn-troubleshoot-allowed-ca.md). Para **Azure CDN de Verizon**, se aceptará cualquier entidad de certificación válida. 

### <a name="prepare-your-azure-key-vault-account-and-certificate"></a>Preparación de la cuenta y el certificado de Azure Key Vault
 
1. Azure Key Vault: tiene que tener una cuenta de Azure Key Vault ejecutándose en la misma suscripción que el perfil de Azure CDN y los puntos de conexión de CDN para los que quiera habilitar HTTPS personalizado. Cree una cuenta de Azure Key Vault si no tiene una.
 
2. Certificados de Azure Key Vault: si ya tiene un certificado, puede cargarlo directamente en la cuenta de Azure Key Vault o puede crear uno nuevo directamente mediante Azure Key Vault a partir de una de las entidades de certificación asociadas con la que se integra Azure Key Vault. 

### <a name="register-azure-cdn"></a>Registro de Azure CDN

Registre Azure CDN como una aplicación en Azure Active Directory mediante PowerShell.

1. Si es necesario, instale [Azure PowerShell](/powershell/azure/install-az-ps) en la máquina local.

2. En PowerShell, ejecute el siguiente comando:

     `New-AzADServicePrincipal -ApplicationId "205478c0-bd83-4e1b-a9d6-db63a3e1e1c8"`

    ![Registro de Azure CDN en PowerShell](./media/cdn-custom-ssl/cdn-register-powershell.png)
              

### <a name="grant-azure-cdn-access-to-your-key-vault"></a>Concesión de acceso de Azure CDN al almacén de claves
 
Conceda permisos de Azure CDN para acceder a los certificados (secretos) de su cuenta de Azure Key Vault.

1. En su cuenta de almacén de claves, en Settings (Configuración), seleccione **Access policies** (Directivas de acceso) y, luego, seleccione **Add new** (Agregar nueva) para crear una nueva directiva.

    ![Creación de una nueva directiva de acceso](./media/cdn-custom-ssl/cdn-new-access-policy.png)

2. En **Seleccionar la entidad de seguridad**, busque **205478c0-bd83-4e1b-a9d6-db63a3e1e1c8** y elija **Microsoft.Azure.Cdn**. Haga clic en **Seleccionar**.

    ![Configuración de directiva de grupo](./media/cdn-custom-ssl/cdn-access-policy-settings.png)

3. Seleccione **Certificate permissions** (Permisos de certificados) y, después, active las casillas de **Get** y **List** para que CDN pueda ejecutar estos permisos y así obtener y enumerar los certificados.

4. Seleccione **Secret permissions** (Permisos de secretos) y, después, active las casillas **Get** (Obtener) y **List** (Enumerar) para que CDN pueda ejecutar estos permisos y así obtener y enumerar los secretos.

5. Seleccione **Aceptar**. 

    Azure CDN puede acceder ahora a este almacén de claves y a los certificados (secretos) almacenados en él.
 
### <a name="select-the-certificate-for-azure-cdn-to-deploy"></a>Selección del certificado de Azure CDN que se va a implementar
 
1. Vuelva al portal de Azure CDN y seleccione el perfil y el punto de conexión de CDN para los que quiere habilitar HTTPS personalizado. 

2. En la lista de dominios personalizados, seleccione el dominio personalizado para el que desea habilitar HTTPS.

    Aparece la página **Dominio personalizado**.

3. En el tipo de administración de certificados, seleccione **Use my own certificate** (Usar mi propio certificado). 

    ![Configuración del certificado](./media/cdn-custom-ssl/cdn-configure-your-certificate.png)

4. Seleccione un almacén de claves, un certificado (secreto) y la versión del certificado.

    Azure CDN muestra la siguiente información: 
    - Las cuentas del almacén de claves de su identificador de suscripción. 
    - Los certificados (secretos) en el almacén de claves seleccionado. 
    - Las versiones de certificado disponibles. 
 
5. Seleccione **On** (Activado) para habilitar HTTPS.
  
6. Si usa su propio certificado, no se requiere la validación del dominio. Continúe con [Esperar a la propagación](#wait-for-propagation).

---

## <a name="validate-the-domain"></a>Validar el dominio

Si ya tiene un dominio personalizado en uso que está asignado a su punto de conexión personalizado con un registro CNAME o usa su propio certificado, continúe con  
[El dominio personalizado está asignado al punto de conexión de CDN](#custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record). En caso contrario, si la entrada de registro CNAME para el punto de conexión ya no existe o no contiene el subdominio cdnverify, continúe con [El dominio personalizado no está asignado al punto de conexión de CDN](#custom-domain-is-not-mapped-to-your-cdn-endpoint).

### <a name="custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record"></a>El dominio personalizado está asignado al punto de conexión de CDN mediante un registro CNAME

Cuando agregó un dominio personalizado a su punto de conexión, creó un registro CNAME en la tabla DNS de su registrador de dominios para asignarlo a su nombre de host del punto de conexión de CDN. Si el registro CNAME aún existe, pero no contiene el subdominio cdnverify, la CA DigiCert lo usa para validar automáticamente la titularidad de su dominio personalizado. 

Si usa su propio certificado, no se requiere la validación del dominio.

El registro CNAME debe tener el formato siguiente, donde *Nombre* es el nombre de dominio personalizado y *Valor* es el nombre de host del punto de conexión de red CDN:

| Nombre            | Tipo  | Value                 |
|-----------------|-------|-----------------------|
| <www.contoso.com> | CNAME | contoso.azureedge.net |

Para obtener más información sobre los registros CNAME, consulte [Creación de los registros DNS de CNAME](https://docs.microsoft.com/azure/cdn/cdn-map-content-to-custom-domain).

Si el registro CNAME tiene el formato correcto, DigiCert automáticamente comprueba su nombre de dominio personalizado y crea un certificado dedicado para el nombre de dominio. DigitCert no enviará un correo electrónico de comprobación, y usted no tendrá que aprobar la solicitud. El certificado tiene una validez de un año y se renovará automáticamente antes de expirar. Continúe con [Esperar a la propagación](#wait-for-propagation). 

La validación automática suele tardar unas horas. Si no ve su dominio validado en 24 horas, abra una incidencia de soporte técnico.

>[!NOTE]
>Si tiene un registro de autorización de entidad de certificación (CAA) con el proveedor de DNS, debe incluir DigiCert como entidad de certificación válida. Un registro CAA permite que los propietarios de dominios especifiquen con sus proveedores de DNS qué entidades de certificación están autorizadas para emitir certificados para su dominio. Si una entidad de certificación recibe un pedido de un certificado para un dominio que posee un registro CAA y dicha entidad no figura como emisor autorizado, no se le permite emitir el certificado para ese dominio o subdominio. Para obtener información acerca de cómo administrar registros de CAA, consulte [Manage CAA records](https://support.dnsimple.com/articles/manage-caa-record/) (Administrar registros de CAA). Para obtener una herramienta de registros de CAA, consulte [CAA Record Helper](https://sslmate.com/caa/) (Asistente de registros de CAA).

### <a name="custom-domain-is-not-mapped-to-your-cdn-endpoint"></a>El dominio personalizado no está asignado al punto de conexión de CDN

>[!NOTE]
>Si usa **Azure CDN de Akamai**, debe configurar la entrada CNAME siguiente para habilitar la validación automatizada de dominios. "_acme-challenge.&lt;host de dominio personalizado&gt; -> CNAME -> &lt;host de dominio personalizado&gt;.ak-acme-challenge.azureedge.net"

Si la entrada del registro CNAME contiene el subdominio cdnverify, siga el resto de las instrucciones que aparecen en este paso.

DigiCert envía un correo electrónico de comprobación a las siguientes direcciones de correo electrónico. Compruebe que puede realizar las aprobaciones directamente desde una de las siguientes direcciones:

admin@&lt;su-nombre-de-dominio.com&gt;  
administrator@&lt;su-nombre-de-dominio.com&gt;  
webmaster@&lt;su-nombre-de-dominio.com&gt;  
hostmaster@&lt;su-nombre-de-dominio.com&gt;  
postmaster@&lt;su-nombre-de-dominio.com&gt;  

Debería recibir un correo electrónico en unos minutos, de forma similar al ejemplo siguiente, solicitando que apruebe la solicitud. Si utiliza un filtro de correo no deseado, agregue verification@digicert.com a la lista de permitidos. Si no recibe un correo electrónico en 24 horas, póngase en contacto con el equipo de soporte técnico de Microsoft.
    
![Correo electrónico de validación del dominio](./media/cdn-custom-ssl/domain-validation-email.png)

Al hacer clic en el vínculo de aprobación, obtendrá acceso al siguiente formulario de aprobación en línea: 
    
![Formulario de validación del dominio](./media/cdn-custom-ssl/domain-validation-form.png)

Siga las instrucciones que aparecen en el formulario; tiene dos opciones de comprobación:

- Puede aprobar todos los pedidos futuros realizados a través de la misma cuenta del mismo dominio raíz, por ejemplo, contoso.com. Se recomienda este método si piensa agregar más dominios personalizados al mismo dominio raíz.

- Puede aprobar solo el nombre de host específico usado en esta solicitud. Para las solicitudes posteriores se requiere una aprobación adicional.

Tras la aprobación, DigiCert completa la creación del certificados para el nombre de dominio personalizado. El certificado tiene una validez de un año y se renovará automáticamente antes de expirar.

## <a name="wait-for-propagation"></a>Esperar a la propagación

Una vez que el nombre de dominio se valide, la característica HTTPS del dominio personalizado tarda entre 6 y 8 horas en estar activa. Una vez completado el proceso, el estado de HTTPS personalizado en Azure Portal se establece en **Habilitado** y los cuatro pasos de la operación en el cuadro de diálogo del dominio personalizado se marcan como completados. El dominio personalizado ahora está listo para usar HTTPS.

![Cuadro de diálogo Habilitar HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Progreso de la operación

En la tabla siguiente se muestra el progreso de la operación que se produce cuando se habilita HTTPS. Después de habilitar HTTPS, los cuatro pasos de la operación aparecen en el cuadro de diálogo del dominio personalizado. A medida que cada paso se activa, aparecen detalles de subpasos adicionales bajo el paso. No se producirán todos estos subpasos. Después de que un paso se completa correctamente, aparece una marca de verificación verde junto a él. 

| Paso de la operación | Detalles del subpaso de la operación | 
| --- | --- |
| 1 Envío de la solicitud | Envío de la solicitud |
| | Se está enviando la solicitud HTTPS. |
| | Se ha enviado correctamente la solicitud HTTPS. |
| 2 Validación del dominio | El dominio se valida automáticamente si está asignado por CNAME al punto de conexión de CDN. En caso contrario, se enviará una solicitud de verificación al correo electrónico que aparece en el registro de su dominio (registrante WHOIS). Compruebe el dominio lo antes posible. |
| | La propiedad del dominio se ha validado correctamente. |
| | La solicitud de validación de la propiedad del dominio ha expirado (el cliente probablemente no respondió dentro del plazo de 6 días). HTTPS no se habilitará en el dominio. * |
| | El cliente rechazó la solicitud de validación de la propiedad del dominio. HTTPS no se habilitará en el dominio. * |
| 3 Aprovisionamiento del certificado | La entidad de certificación está emitiendo actualmente el certificado necesario para habilitar HTTPS en el dominio. |
| | El certificado se ha emitido y actualmente se está implementando en la red CDN. Esto puede tardar hasta 6 horas. |
| | El certificado se ha implementado correctamente en la red CDN. |
| 4 Finalizado | Se habilitó correctamente HTTPS en el dominio. |

\* Este mensaje no aparecerá a menos que se haya producido un error. 

Si se produce un error antes de que se envíe la solicitud, verá el mensaje de error siguiente:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>



## <a name="clean-up-resources---disable-https"></a>Clean up resources - disable HTTPS (Limpiar recursos: deshabilitar HTTPS)

En los pasos anteriores, habilitó el protocolo HTTPS en su dominio personalizado. Si ya no desea usar su dominio personalizado con HTTPS, puede deshabilitar HTTPS mediante estos pasos:

### <a name="disable-the-https-feature"></a>Deshabilitación de la característica HTTPS 

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Perfiles de CDN**. 

2. Elija el perfil **Azure CDN estándar de MIcrosoft**, **Azure CDN estándar de Verizon** o **Azure CDN prémium de Verizon**.

3. En la lista de puntos de conexión, escoja el que contenga el dominio personalizado.

4. Elija el dominio personalizado en el que desea deshabilitar HTTPS.

    ![Lista de dominios personalizados](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

5. Elija **Desactivar** para deshabilitar HTTPS y haga clic en **Aplicar**.

    ![Cuadro de diálogo Personalizar HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="wait-for-propagation"></a>Esperar a la propagación

Cuando se deshabilita la característica HTTPS del dominio personalizado, puede tardar hasta 6 a 8 horas para que surta efecto. Una vez completado el proceso, el estado de HTTPS personalizado en Azure Portal se establece en **Deshabilitado** y los tres pasos de la operación en el cuadro de diálogo del dominio personalizado se marcan como completados. El dominio personalizado ya no usa HTTPS.

![Cuadro de diálogo Deshabilitar HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

#### <a name="operation-progress"></a>Progreso de la operación

En la tabla siguiente se muestra el progreso de la operación que se produce cuando se deshabilita HTTPS. Después de deshabilitar HTTPS, los tres pasos de la operación aparecen en el cuadro de diálogo del dominio personalizado. A medida que cada paso se activa, aparecen detalles adicionales bajo el paso. Después de que un paso se completa correctamente, aparece una marca de verificación verde junto a él. 

| Progreso de la operación | Detalles de la operación | 
| --- | --- |
| 1 Envío de la solicitud | Enviando la solicitud |
| 2 Desaprovisionamiento del certificado | Eliminando certificado |
| 3 Finalizado | Certificado eliminado |

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

1. *¿Quién es el proveedor de certificados y qué tipo de certificado se utiliza?*

    Con **Azure CDN de Verizon** y **Azure CDN de Microsoft**, se usa para el dominio personalizado un certificado único o dedicado que proporciona Digicert. 

2. *¿Usa TLS/SSL basado en IP o SNI?*

    Tanto **Azure CDN de Verizon** como **Azure CDN estándar de Microsoft** usan TLS/SSL de SNI.

3. *¿Qué debo hacer si no recibo el correo electrónico de comprobación de dominio de DigiCert?*

    Si tiene una entrada CNAME para el dominio personalizado, que apunte directamente a su nombre de host del punto de conexión (y no utiliza el nombre del subdominio cdnverify), no recibirá un correo electrónico de comprobación de dominio. La validación se produce automáticamente. En caso contrario, si no tiene una entrada CNAME y no ha recibido un correo electrónico en 24 horas, póngase en contacto con el servicio de soporte técnico de Microsoft.

4. *¿Son menos seguros los certificados SAN que los certificados dedicados?*
    
    Los certificados SAN siguen los mismos estándares de cifrado y seguridad que los certificados dedicados. Todos los certificados TLS/SSL emitidos usan SHA-256 para mejorar la seguridad del servidor.

5. *¿Necesito un registro de autorización de entidad de certificación con mi proveedor de DNS?*

    No, actualmente no se requiere un registro de autorización de entidad de certificación. Pero si tiene uno, debe incluir DigiCert como entidad de certificación válida.

6. *El 20 de junio de 2018, Azure CDN de Verizon comenzó a usar un certificado dedicado con TLS/SSL de SNI de forma predeterminada. ¿Qué sucede con los dominios personalizados existentes que usan certificados de nombre alternativo del firmante (SAN) TLS/SSL basado en IP?*

    En los próximos meses, los dominios existentes se migrarán gradualmente a un único certificado si Microsoft analiza que solo se realizan solicitudes de cliente SNI a la aplicación. Si Microsoft detecta que se realizan algunas solicitudes de cliente que no son SNI a la aplicación, los dominios se quedarán con el certificado SAN con TLS/SSL basado en IP. En cualquier caso, no habrá ninguna interrupción en el servicio o soporte técnico de las solicitudes de cliente independientemente de si esas solicitudes son SNI o no SNI.

7. *Funcionamiento de la renovación de certificados con Bring Your Own Certificate*

    Para tener la certeza de que se implementa un certificado más reciente en la infraestructura de PoP, no tiene más que cargar el nuevo certificado en Azure KeyVault y, después, en la configuración de TLS de Azure CDN, elegir la versión más reciente del certificado y pulsar el botón para guardar. Azure CDN propagará el nuevo certificado actualizado. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> - Habilitar el protocolo HTTPS en su dominio personalizado
> - Usar un certificado administrado por CDN 
> - Usar su propio certificado
> - Validar el dominio
> - Deshabilitar el protocolo HTTPS en su dominio personalizado

Avance al siguiente tutorial para aprender a configurar el almacenamiento en caché en el punto de conexión de CDN.

> [!div class="nextstepaction"]
> [Tutorial: Establecimiento de las reglas de almacenamiento en caché de Azure CDN](cdn-caching-rules-tutorial.md)

