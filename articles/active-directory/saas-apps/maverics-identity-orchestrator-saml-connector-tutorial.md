---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Maverics Identity Orchestrator SAML Connector | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Maverics Identity Orchestrator SAML Connector.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/12/2020
ms.author: jeedes
ms.openlocfilehash: ec5368427f50f548be965bb883683c859759bbf3
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2020
ms.locfileid: "88518953"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-maverics-identity-orchestrator-saml-connector"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Maverics Identity Orchestrator SAML Connector

## <a name="introduction"></a>Introducción

Strata proporciona una manera sencilla de integrar aplicaciones locales con Azure AD para la autenticación y el control de acceso.

En esta guía se describe cómo configurar Maverics Identity Orchestrator&trade; para:
* Migrar incrementalmente los usuarios de un sistema de identidad local a Azure AD durante el inicio de sesión en una aplicación local heredada.
* Enrutar las solicitudes de inicio de sesión de un producto de administración de acceso web heredado, como CA SiteMinder u Oracle Access Manager a Azure AD.
* Autenticar usuarios en aplicaciones locales que estén protegidas mediante encabezados HTTP o cookies de sesión propias después de autenticar el usuario con Azure AD.

Strata proporciona software que se implementa de forma local o en la nube para detectar, conectar y coordinar proveedores de identidades con el fin de crear una administración de identidades distribuida para empresas híbridas y de nube múltiple.

En este tutorial se muestra cómo migrar una aplicación web local protegida por un producto de administración de acceso web heredado (CA SiteMinder) para usar Azure AD para la autenticación y el control de acceso.
1. Instalación de Maverics Identity Orchestrator&trade;
2. Registre su aplicación empresarial con Azure AD y configúrela para usar Maverics Azure AD SAML Zero Code Connector&trade; para el inicio de sesión único basado en SAML.
3. Integre Maverics con SiteMinder y el almacén de usuarios LDAP.
4. Configure Azure Key Vault y configure Maverics para usarlo como proveedor de administración de secretos.
5. Demuestre la migración de usuarios y la abstracción de sesión mediante Maverics para proporcionar acceso a una aplicación web Java local.

Para obtener instrucciones de instalación y configuración adicionales, visite https://strata.io/docs.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
- Una suscripción con el inicio de sesión único de Maverics Identity Orchestrator SAML Connector habilitado. Para obtener el software de Maverics, póngase en contacto con sales@strata.io.

## <a name="install-maverics-identity-orchestratortrade"></a>Instalación de Maverics Identity Orchestrator&trade;

Para empezar a instalar Maverics Identity Orchestrator, consulte las instrucciones de instalación en https://strata.io/docs.

## <a name="system-requirements"></a>Requisitos del sistema
### <a name="supported-operating-systems"></a>Sistemas operativos compatibles
* RHEL 7+
* CentOS 7+

### <a name="dependencies"></a>Dependencias
* systemd

## <a name="installation"></a>Instalación

1. Obtenga el paquete RPM de Maverics más reciente. Copie el paquete en el sistema en el que desee instalar el software de Maverics.

2. Instale el paquete de Maverics, sustituyendo el nombre de archivo `maverics.rpm` por el suyo.

    `sudo rpm -Uvf maverics.rpm`

3. Después de instalar Maverics, se ejecutará como servicio en `systemd`. Utilice el siguiente comando para comprobar que el servicio se está ejecutando.

    `sudo systemctl status maverics`

De forma predeterminada, Maverics se instala en el directorio `/usr/local/bin`.

Después de instalar Maverics, se crea el archivo `maverics.yaml` predeterminado en el directorio `/etc/maverics`. Antes de editar la configuración para incluir `workflows` y `connectors`, el archivo de configuración tendrá el siguiente aspecto:

```yaml
# © Strata Identity Inc. 2020. All Rights Reserved. Patents Pending.

version: 0.1
listenAddress: ":7474"
```
## <a name="config-options"></a>Opciones de configuración
### <a name="version"></a>Versión
El campo `version` declara qué versión del archivo de configuración se está usando. Si no se especifica, se usará la versión de configuración más reciente.

```yaml
version: 0.1
```
### <a name="listen-address"></a>Listen Address
`listenAddress` declara en qué dirección escuchará Orchestrator. Si la sección de host de la dirección está en blanco, Orchestrator escuchará en todas las direcciones IP de unidifusión y multidifusión disponibles del sistema local. Si la sección de puerto de la dirección está en blanco, se elige automáticamente un número de puerto.

```yaml
listenAddress: ":453"
```
### <a name="tls"></a>TLS

El campo `tls` declara un mapa de objetos de seguridad de la capa de transporte. Los conectores y el servidor de Orchestrator pueden usar objetos TLS. Para ver todas las opciones de TLS disponibles, consulte la documentación del paquete de `transport`.

Microsoft Azure requiere comunicación a través de TLS cuando se usa SSO basado en SAML. Puede encontrar más información [aquí](https://letsencrypt.org/getting-started/) para generar los certificados.

La clave `maverics` está reservada para el servidor de Orchestrator. Todas las demás claves están disponibles y se pueden usar para insertar un objeto TLS en un conector determinado.

```yaml
tls:
  maverics:
    certFile: /etc/maverics/maverics.cert
    keyFile: /etc/maverics/maverics.key
```  
### <a name="include-files"></a>Archivos de inclusión

`connectors` y `workflows` pueden definirse en sus propios archivos de configuración independientes y se puede hacer referencia a ellos en `maverics.yaml` mediante `includeFiles` en el ejemplo siguiente.
```yaml
includeFiles:
  - workflow/sessionAbstraction.yaml
  - connector/AzureAD-saml.yaml
  - connector/siteminder.yaml
  ```

En este tutorial se usa un único archivo de configuración `maverics.yaml`.

## <a name="using-azure-key-vault-as-your-secrets-provider"></a>Uso de Azure Key Vault como proveedor de secretos

### <a name="secret-management"></a>Administración de secretos

Maverics es capaz de integrarse con varias soluciones de administración de secretos para cargar secretos. Las integraciones actuales incluyen un archivo, Hashicorp Vault y Azure Key Vault. Si no se especifica ninguna solución de administración de secretos, Maverics cargará secretos en texto sin formato de `maverics.yaml` de forma predeterminada.
Para declarar un valor como secreto en un archivo de configuración `maverics.yaml`, encapsule el secreto entre corchetes angulares:

  ```yaml
  connectors:
  - name: AzureAD
    type: AzureAD
    apiToken: <AzureADAPIToken>
    oauthClientID: <AzureADOAuthClientID>
    oauthClientSecret: <AzureADOAuthClientSecret>
  ```

### <a name="file"></a>Archivo

Para cargar los secretos de un archivo, agregue la variable de entorno `MAVERICS_SECRET_PROVIDER` en el archivo `/etc/maverics/maverics.env` con:

`MAVERICS_SECRET_PROVIDER=secretfile:///<PATH TO SECRETS FILE>`

Después, reinicie el servicio de Maverics: `sudo systemctl restart maverics`

El contenido del archivo `secrets.yaml` se puede rellenar con cualquier número de `secrets`.
```yaml
secrets:
  AzureADAPIToken: aReallyGoodToken
  AzureADOAuthClientID: aReallyUniqueID
  AzureADOAuthClientSecret: aReallyGoodSecret
```
### <a name="azure-key-vault"></a>Azure Key Vault

En los pasos siguientes se muestra cómo configurar una instancia de Azure Key Vault mediante [Azure Portal](https://portal.azure.com) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest):

1. [Inicie sesión](https://portal.azure.com) con Azure Portal o con el comando de la CLI:
    ```shell
    az login
    ```

2. [Cree un nuevo almacén](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault) o use el comando de la CLI:
    ```shell
    az keyvault create --name "[VAULT_NAME]" --resource-group "[RESOURCE_GROUP]" --location "[REGION]"
    ```

3. [Agregue los secretos a Key Vault](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault) o use un comando de la CLI:
    ```shell
    az keyvault secret set --vault-name "[VAULT_NAME]" --name "[SECRET_NAME]" --value "[SECRET_VALUE]"
    ```

4. [Registre una aplicación con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application) o use un comando de la CLI:
    ```shell
    az ad sp create-for-rbac -n "MavericsKeyVault" --skip-assignment > azure-credentials.json
    ```

5. [Autorice a una aplicación para que utilice un secreto](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault) o use un comando de la CLI:
    ```shell
    az keyvault set-policy --name "[VAULT_NAME]" --spn [APPID] --secret-permissions list get
    #APPID can be found in the azure-credentials.json
    generated in the previous step
    ```

Para cargar secretos de Azure Key Vault, establezca la variable de entorno `MAVERICS_SECRET_PROVIDER` en el archivo `/etc/maverics/maverics.env`, con las credenciales que se encuentran en el archivo azure-credentials.json con el siguiente patrón: `MAVERICS_SECRET_PROVIDER='azurekeyvault://<KEYVAULT NAME>.vault.azure.net?clientID=<APPID>&clientSecret=<PASSWORD>&tenantID=<TENANT>'`

Después, reinicie el servicio de Maverics: `sudo systemctl restart maverics`

## <a name="configure-your-application-in-azure-ad-for-saml-based-sso"></a>Configuración de la aplicación en Azure AD para SSO basado en SAML

1. En el inquilino de Azure Active Directory, vaya a `Enterprise applications`, busque `Maverics Identity Orchestrator SAML Connector` y selecciónelo.

2. En la página de propiedades de Maverics Identity Orchestrator SAML Connector, establezca `User assignment required?` en No para permitir que la aplicación funcione para los usuarios migrados recientemente.

3. En la página de información general de Maverics Identity Orchestrator SAML Connector, seleccione `Setup single sign-on` y, a continuación, seleccione `SAML`.

4. En el inicio de sesión basado en SAML de Maverics Identity Orchestrator SAML Connector, edite la configuración básica de SAML.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

5. Establezca `Entity ID`; para ello, escriba la dirección URL con el patrón siguiente: `https://<SUBDOMAIN>.maverics.org`. `Entity ID` debe ser único en las apps del inquilino. Guarde el valor especificado aquí para incluirlo en la configuración de Maverics.

6. Establezca la dirección URL de respuesta con el patrón siguiente: `https://<AZURECOMPANY.COM>/<MY_APP>/`. 

7. Establezca la dirección URL de inicio de sesión con el siguiente patrón: `https://<AZURE-COMPANY.COM>/<MY_APP>/<LOGIN PAGE>` y haga clic en Guardar.

8. En la sección Certificado de firma de SAML, haga clic en el botón de copia para copiar la Dirección URL de metadatos de federación de aplicación y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

## <a name="maverics-identity-orchestrator-azure-ad-saml-connector-configuration"></a>Configuración de Maverics Identity Orchestrator Azure AD SAML Connector

Maverics Identity Orchestrator Azure AD Connector admite: 
- OpenID Connect
- Conexión SAML 

1. Para habilitar el inicio de sesión único basado en SAML, establezca `authType: saml`.

1. Cree el valor de `samlMetadataURL`: `samlMetadataURL:https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>`

1. Ahora, defina la dirección URL a la que se redirigirá Azure en la aplicación después de haber iniciado sesión con sus credenciales de Azure.
`samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>`

1. Copie el valor de EntityID configurado anteriormente: `samlEntityID: https://<SUBDOMAIN>.maverics.org`.

1. Copie el valor de la dirección URL de respuesta que utilizará Azure AD para publicar la respuesta SAML.
`samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>`

1. Genere una clave de firma JWT, que se usa para proteger la información de sesión de Maverics Identity Orchestrator&trade;, mediante la [herramienta OpenSSL](https://www.openssl.org/source/):

    ```shell 
    openssl rand 64 | base64
    ```
1. Copie la respuesta en la propiedad de configuración `jwtSigningKey`: `jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==`.

## <a name="attributes-and-attribute-mapping"></a>Atributos y asignación de atributos
La asignación de atributos se usa para definir la asignación de atributos de usuario desde un directorio de usuarios local de origen en Azure AD cuando se aprovisionan usuarios.

Los atributos determinan los datos de usuario que pueden devolverse a una aplicación en una notificación, pasarse a las cookies de sesión o pasarse a la aplicación en variables de encabezado HTTP.

## <a name="configure-maverics-identity-orchestrator-azure-ad-saml-connector-yaml"></a>Configuración del archivo YAML de Maverics Identity Orchestrator Azure AD SAML Connector

La configuración de Maverics Identity Orchestrator Azure AD SAML Connector tendrá el siguiente aspecto:
```yaml
- name: AzureAD
  type: azure
  authType: saml
  samlMetadataURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>
  samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>
  samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>
  jwtSigningKey: <SIGNING KEY>
  samlEntityID: https://<SUBDOMAIN>.maverics.org
  attributeMapping:
    displayName: username
    mailNickname: givenName
    givenName: givenName
    surname: sn
    userPrincipalName: mail
    password: password
```

## <a name="migrate-users-to-azure-ad"></a>Migración de usuarios a Azure AD

Siga esta configuración para migrar incrementalmente usuarios de un producto de administración de acceso web como CA SiteMinder, Oracle Access Manager o IBM Tivoli; un directorio LDAP, o una instancia de SQL Database.

## <a name="configure-your-application-permissions-in-azure-ad-to-create-users"></a>Configuración de los permisos de la aplicación en Azure AD para crear usuarios

1. En el inquilino de Azure Active Directory, vaya a `App registrations` y seleccione la aplicación "Maverics Identity Orchestrator SAML Connector".

2. En la página de certificados y secretos de Maverics Identity Orchestrator SAML Connector, seleccione `New client secret` y, después, seleccione la opción de expiración. Haga clic en el botón de copia para copiar el secreto y guárdelo en su equipo.

3. En la página de permisos de API de Maverics Identity Orchestrator SAML Connector, seleccione `Add permission` y, a continuación, en Solicitud de permisos de API, seleccione `Microsoft Graph` y luego `Application permissions`. En la pantalla siguiente, seleccione `User.ReadWrite.All` y, después, `Add permissions`. Esto le devolverá a los permisos de API, donde deberá seleccionar `Grant admin consent`.


## <a name="configure-the-maverics-identity-orchestrator-saml-connector-yaml-for-user-migration"></a>Configuración del archivo YAML de Maverics Identity Orchestrator Azure AD SAML Connector para migración de usuarios

Para habilitar el flujo de trabajo de migración de usuarios, agregue estas propiedades adicionales al archivo de configuración:
1. Establezca la dirección URL de Azure Graph: `graphURL: https://graph.microsoft.com`.
1. Establezca la dirección URL del token de OAuth, con el patrón: `oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>`.
1. Establezca el secreto de cliente generado anteriormente: `oauthClientSecret: <CLIENT SECRET>`.


La configuración final de Maverics Identity Orchestrator Azure AD SAML Connector tendrá el siguiente aspecto:
```yaml
- name: AzureAD
  type: azure
  authType: saml
  samlMetadataURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>
  samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>
  samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>
  jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==
  samlEntityID: https://<SUBDOMAIN>.maverics.org
  graphURL: https://graph.microsoft.com
  oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/oauth2/v2.0/token
  oauthClientID: <APP ID>
  oauthClientSecret: <NEW CLIENT SECRET>
  attributeMapping:
    displayName: username
    mailNickname: givenName
    givenName: givenName
    surname: sn
    userPrincipalName: mail
    password: password
```

## <a name="configure-the-maverics-zero-code-connectortrade-for-siteminder"></a>Configuración de Maverics Zero Code Connector&trade; para SiteMinder

El conector de SiteMinder se usa para migrar usuarios a Azure AD e iniciar la sesión de los usuarios en aplicaciones locales heredadas protegidas por SiteMinder con las identidades y credenciales de Azure AD creadas recientemente.

En este tutorial, SiteMinder se ha configurado para proteger la aplicación heredada con autenticación basada en formularios y el uso de la cookie `SMSESSION`. Para la integración con una aplicación que consume autenticación y sesión mediante encabezados HTTP, debe agregar la configuración de emulación de encabezados al conector.

En este ejemplo se asigna el atributo `username` al encabezado HTTP `SM_USER`:
```yaml
  headers:
    SM_USER: username
```

Establezca `proxyPass` en la ubicación a la que se redirigen mediante proxy las solicitudes. Normalmente, se trata del host de la aplicación protegida.

`loginPage` debe coincidir con la dirección URL del formulario de inicio de sesión que usa actualmente SiteMinder al redirigir a los usuarios para la autenticación.

```yaml
connectors:
- name: siteminder-login-form
  type: siteminder
  loginType: form
  loginPage: /siteminderagent/forms/login.fcc
  proxyPass: http://host.company.com
```

## <a name="configure-the-maverics-zero-code-connectortrade-for-ldap"></a>Configuración de Maverics Zero Code Connector&trade; para LDAP

Cuando las aplicaciones están protegidas por un producto WAM como SiteMinder, las identidades y los atributos de usuario normalmente se almacenan en un directorio LDAP.

En esta configuración de conector se muestra cómo conectarse al directorio LDAP configurado como el almacén de usuario para SiteMinder, de modo que se pueda recopilar la información de perfil de usuario correcta durante el flujo de trabajo de migración y se pueda crear un usuario correspondiente en Azure AD.

* `baseDN` especifica la ubicación en el directorio en el que se realizará la búsqueda LDAP.

* `url` es la dirección y el puerto del servidor LDAP con el que se va a establecer la conexión.

* `serviceAccountUsername` es el nombre de usuario utilizado para conectarse al servidor LDAP, normalmente expresado como un DN de enlace, por ejemplo `CN=Directory Manager`.

* `serviceAccountPassword` es la contraseña usada para conectarse al servidor LDAP. Este valor se almacena en la instancia de Azure Key Vault configurada anteriormente.  

* `userAttributes` define la lista de atributos relacionados con el usuario que se van a consultar. Estos atributos se asignan posteriormente a los atributos de Azure AD correspondientes.

```yaml
- name: company-ldap
  type: ldap
  url: "ldap://ldap.company.com:389"
  baseDN: ou=People,o=company,c=US
  serviceAccountUsername: uid=admin,ou=Admins,o=company,c=US
  serviceAccountPassword: <vaulted-password>
  userAttributes:
    - uid
    - cn
    - givenName
    - sn
    - mail
    - mobile
```

## <a name="configure-the-migration-workflow"></a>Configuración del flujo de trabajo de migración

La configuración del flujo de trabajo de migración determina el modo en que Maverics migrará usuarios de SiteMinder o LDAP a Azure AD.

Este flujo de trabajo:
- Usa el conector de SiteMinder para redirigir mediante proxy el inicio de sesión de SiteMinder. Las credenciales de usuario se validan mediante la autenticación SiteMinder y, a continuación, se pasan a los pasos posteriores del flujo de trabajo.
- Recupera los atributos de perfil de usuario del almacén de usuarios de SiteMinder.
- Realiza una solicitud a Microsoft Graph API para crear el usuario en el inquilino de Azure AD.

Pasos:
1. Asigne un nombre al flujo de trabajo, por ejemplo, Migración de SiteMinder a Azure AD.
2. Especifique `endpoint`, que es una ruta de acceso HTTP en la que se expone el flujo de trabajo que desencadena `actions` de ese flujo de trabajo en respuesta a las solicitudes. Normalmente, `endpoint` corresponde a la aplicación que se redirige mediante proxy, por ejemplo, `/my_app`. El valor debe incluir las barras diagonales inicial y final.
3. Agregue el valor de `actions` adecuado al flujo de trabajo.
    - Defina el método `login` para el conector de SiteMinder. El valor del conector debe coincidir con el valor del nombre de la configuración del conector.
     - Defina el método `getprofile` para el conector LDAP.
     - Defina `createuser` para el conector de Azure AD.

    ```yaml
      workflows:
      - name: SiteMinder to Azure AD Migration
        endpoint: /my_app/
        actions:
        - connector: siteminder-login-form
          method: login
        - connector: company-ldap
          method: getprofile
        - connector: AzureAD
          method: createuser
    ```
### <a name="verify-the-migration-workflow"></a>Comprobación del flujo de trabajo de migración

1. Si el servicio Maverics aún no se está ejecutando, inícielo ejecutando el comando siguiente: `sudo systemctl start maverics`.

2. Vaya a la dirección URL de inicio de sesión de proxy: `http://host.company.com/my_app`.
3. Proporcione las credenciales de usuario usadas para iniciar sesión en la aplicación mientras está protegida por SiteMinder.
4. Vaya a Inicio > Usuarios | Todos los usuarios para comprobar que el usuario se ha creado en el inquilino de Azure AD.  

## <a name="configure-the-session-abstraction-workflow"></a>Configuración del flujo de trabajo de abstracción de sesión

El flujo de trabajo de abstracción de sesión traslada la autenticación y el control de acceso de la aplicación web local heredada a Azure AD.

El conector de Azure usa el método `login` para redirigir al usuario a la dirección URL de inicio de sesión, suponiendo que no exista ninguna sesión.

Una vez autenticado, el token de sesión creado como resultado se pasa a Maverics y el método `emulate` del conector de SiteMinder se usa para emular la sesión basada en cookies y/o la sesión basada en el encabezado y, a continuación, agregar a la solicitud los atributos adicionales requeridos por la aplicación.

1. Asigne un nombre al flujo de trabajo, por ejemplo, Abstracción de sesión de SiteMinder.
2. Especifique `endpoint`, que corresponde a la aplicación que se redirige mediante proxy. El valor debe incluir las barras diagonales inicial y final, por ejemplo, `/my_app/`.
3. Agregue el valor de `actions` adecuado al flujo de trabajo.
    - Defina el método `login` para el conector de Azure. El valor de `connector` debe coincidir con el valor de `name` de la configuración del conector.
    - Defina el método `emulate` para el conector de SiteMinder.

     ```yaml
      - name: SiteMinder Session Abstraction
        endpoint: /my_app/
        actions:
      - connector: azure
        method: login
      - connector: siteminder-login-form
        method: emulate
     ```
### <a name="verify-the-session-abstraction-workflow"></a>Comprobación del flujo de trabajo de abstracción de sesión

1. Vaya a la dirección URL de la aplicación de proxy: `https://<AZURECOMPANY.COM>/<MY_APP>`. El usuario se redirigirá a la página de inicio de sesión de proxy.
2. Escriba las credenciales de usuario de Azure AD.
3. El usuario debe redirigirse a la aplicación como si lo autenticara directamente SiteMinder.
