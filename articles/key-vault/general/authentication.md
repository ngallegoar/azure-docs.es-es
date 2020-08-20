---
title: Autenticación en Azure Key Vault
description: Aprenda a autenticarse en Azure Key Vault
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 06/08/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: 7645600a476a1c2294ddd4a24fe01e2ffe51d5ac
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589990"
---
# <a name="authenticate-to-azure-key-vault"></a>Autenticación en Azure Key Vault

## <a name="overview"></a>Información general

Azure Key Vault es una solución de administración de secretos que le permite centralizar el almacenamiento de los secretos de aplicación y controlar su distribución. Azure Key Vault elimina la necesidad de almacenar credenciales en las aplicaciones. La aplicación puede autenticarse en el almacén de claves para recuperar las credenciales necesarias. En este documento se tratan los conceptos básicos de la autenticación en el almacén de claves.

Este documento le ayudará a comprender cómo funciona la autenticación del almacén de claves. En este documento se explica el flujo de autenticación y se muestra cómo se concede el acceso al almacén de claves, además se incluye un tutorial para recuperar un secreto de una aplicación de Python de ejemplo almacenado en el almacén de claves.

Este documento trata:

* Conceptos clave
* Registro de entidad de seguridad
* Información sobre el flujo de autenticación de Key Vault
* Concesión de acceso a Key Vault a una entidad de servicio
* Tutorial (Python)

## <a name="key-concepts"></a>Conceptos clave

### <a name="azure-active-directory-concepts"></a>Conceptos de Azure Active Directory

* Azure Active Directory (AAD): Azure Active Directory (Azure AD) es un servicio de administración de identidades y acceso basado en la nube de Microsoft que ayuda al personal de una empresa o entidad a iniciar sesión y acceder a recursos.

* Definición de roles: una definición de roles es una colección de permisos.  AAD tiene roles estándar (propietario, colaborador o lector) que contienen niveles de permisos que permiten realizar operaciones como lectura, escritura y eliminación en un recurso de Azure. Los roles también pueden ser definiciones personalizadas creadas por usuarios con permisos pormenorizados específicos.

* Registro de aplicación: al registrar una aplicación de Azure AD, se crean dos objetos en el inquilino de Azure AD: un objeto de aplicación y un objeto de entidad de servicio. Considere el objeto de aplicación como una representación global de la aplicación para su uso en todos los inquilinos y la entidad de servicio como una representación local para su uso en un inquilino específico.

### <a name="security-principal-concepts"></a>Conceptos de entidad de seguridad

* Entidad de seguridad: una entidad de seguridad es un objeto que representa a un usuario, un grupo, una entidad de servicio o una identidad administrada que solicita acceso a recursos de Azure.

* Usuario: individuo que tiene un perfil en Azure Active Directory.

* Grupo: conjunto de usuarios creado en Azure Active Directory. Cuando se asigna un rol a un grupo, todos los usuarios dentro de ese grupo tienen ese rol.

* Entidad de servicio: identidad de seguridad que las aplicaciones o los servicios usan para acceder a recursos específicos de Azure. Puede considerarla como una identidad de usuario (nombre de usuario y contraseña o certificado) para una aplicación.

* Identidad administrada: una identidad de Azure Active Directory que Azure administra de forma automática.

* Id. de objeto (Id. de cliente): un identificador único que genera Azure AD y que está asociado a una entidad de servicio durante su aprovisionamiento inicial.

## <a name="security-principal-registration"></a>Registro de entidad de seguridad

1. El administrador registra a un usuario o una aplicación (entidad de servicio) en Azure Active Directory.

2. El administrador crea una instancia de Azure Key Vault y configura las directivas de acceso (ACL).

3. (Opcional) El administrador configura el firewall de Azure Key Vault.

![IMAGEN](../media/authentication-1.png)

## <a name="understand-the-key-vault-authentication-flow"></a>Información sobre el flujo de autenticación de Key Vault

1. Una entidad de servicio realiza una llamada para autenticarse en AAD. Esto se puede producir de varias maneras:
    * Un usuario puede iniciar sesión en Azure Portal mediante un nombre de usuario y una contraseña.
    * Una aplicación usa un Id. de cliente y presenta un secreto de cliente o un certificado de cliente a AAD.
    * Un recurso de Azure, como una máquina virtual, tiene asignado un MSI y se pone en contacto con el punto de conexión de IMDS REST para obtener un token de acceso.

2. Si la autenticación en AAD se realiza correctamente, se le concederá un token de OAuth a la entidad de servicio.
3. La entidad de servicio realiza una llamada a Key Vault.
4. El firewall de Azure Key Vault determina si se permite la llamada.
    * Escenario 1: El firewall de Key Vault está deshabilitado, el punto de conexión público (URI) del almacén de claves es accesible desde la red pública de Internet. Se permite la llamada.
    * Escenario 2: El autor de la llamada es un servicio de confianza de Azure Key Vault. Algunos servicios de Azure pueden omitir el firewall del almacén de claves si se selecciona la opción. [Lista de servicios de confianza de Key Vault](https://docs.microsoft.com/azure/key-vault/general/overview-vnet-service-endpoints#trusted-services)
    * Escenario 3: El autor de la llamada aparece listado en el firewall de Azure Key Vault por su dirección IP, red virtual o punto de conexión de servicio.
    * Escenario 4: El autor de la llamada puede llegar a Azure Key Vault mediante una conexión de vínculo privado configurada.
    * Escenario 5: El autor de la llamada no está autorizado y se devuelve una respuesta de prohibición.
5. Key Vault realiza una llamada a AAD para validar el token de acceso de la entidad de servicio.
6. Key Vault comprueba si la entidad de servicio tiene permisos de directiva de acceso suficientes para realizar la operación solicitada; en este ejemplo, la operación es obtener secreto.
7. Key Vault proporciona el secreto a la entidad de servicio.

![IMAGEN](../media/authentication-2.png)

## <a name="grant-a-service-principal-access-to-key-vault"></a>Concesión de acceso a Key Vault a una entidad de servicio

1. Cree una entidad de servicio, si aún no tiene ninguna. [Creación de una entidad de servicio](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)
2. Agregue una asignación de roles a la entidad de servicio en los valores de IAM de Azure Key Vault. Puede agregar roles previamente asignados de propietario, colaborador o lector. También puede crear roles personalizados para la entidad de servicio. Debe seguir el principio de privilegios mínimos y proporcionar solo el acceso mínimo necesario para la entidad de servicio. 
3.  Configure el firewall del almacén de claves. Puede mantener el firewall del almacén de claves deshabilitado y permitir el acceso desde la red pública de Internet (menos seguro, más fácil de configurar). También puede restringir el acceso a intervalos de direcciones IP, puntos de conexión de servicio, redes virtuales o puntos de conexión privados específicos (más seguro).
4.  Agregue una directiva de acceso para la entidad de servicio, se trata de una lista de operaciones que la entidad de servicio puede realizar en el almacén de claves. Debe utilizar el principio de privilegios mínimos y limitar las operaciones que la entidad de servicio puede realizar. De cualquier forma, si no proporciona suficientes permisos, se denegará el acceso a la entidad de servicio.

## <a name="tutorial"></a>Tutorial

En este tutorial, aprenderá a configurar una entidad de servicio para autenticarse en el almacén de claves y recuperar un secreto. 

### <a name="part-1--create-a-service-principal-in-the-azure-portal"></a>Parte 1:  Creación de una entidad de servicio en Azure Portal

1. Iniciar sesión en Azure Portal
1. Busque Azure Active Directory.
1. Haga clic en la pestaña "Registros de aplicaciones".
1. Haga clic en "+Nuevo registro".
1. Cree un nombre para la entidad de servicio.
1. Seleccione Registrar.

En este momento ya tiene una entidad de servicio registrada. Para verla, seleccione "Registros de aplicaciones". A la entidad de servicio se le asignará ahora un GUID para el Id. de cliente, considérelo como un "nombre de usuario" para la entidad de servicio. Ahora tenemos que crear una "contraseña" para la entidad de servicio, puede usar un secreto de cliente o un certificado de cliente. Tenga en cuenta que el uso de un secreto de cliente para la autenticación no es seguro y solo debe usarse con fines de prueba. En este tutorial se muestra cómo usar un certificado de cliente.

### <a name="part-2-create-a-client-certificate-for-your-service-principal"></a>Parte 2: Creación de un certificado de cliente para la entidad de servicio

1. Crear un certificado

    * Opción 1: Cree un certificado con [OpenSSL](https://www.openssl.org/) (solo con fines de prueba, no use certificados autofirmados en un entorno de producción).

    ```console
    openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
    ```

    * Opción 2: Cree un certificado mediante el almacén de claves. [Creación de un certificado en Azure Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/certificate-scenarios#creating-your-first-key-vault-certificate)

1. Descargue el certificado en el formato PEM.
1. Inicie sesión en Azure Portal y vaya a Azure Active Directory.
1. Haga clic en "Registros de aplicaciones".
1. Seleccione la entidad de servicio que creó en Parte 1.
1. Haga clic en la pestaña "Certificates and Secrets" (Certificados y secretos) de la entidad de servicio.
1. Cargue el certificado mediante el botón "Cargar certificado".

### <a name="part-3-configure-an-azure-key-vault"></a>Parte 3: Configuración de una instancia de Azure Key Vault

1. Cree un [vínculo](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault) de Azure Key Vault.

2. Configure los permisos IAM de Key Vault.
    1. Vaya a almacén de claves.
    1. Seleccione la pestaña "Access Control (IAM)".
    1. Haga clic en Agregar asignación de rol.
    1. Seleccione el rol "Colaborador" en la lista desplegable.
    1. Escriba el nombre o el identificador de cliente de la entidad de servicio que creó.
    1. Haga clic en "Ver asignaciones de roles" para confirmar que la entidad de servicio aparece en la lista.

3. Configure los permisos de la directiva de acceso de Key Vault.
    1. Vaya a almacén de claves.
    1. Seleccione la pestaña "Directivas de acceso" en "Configuración".
    1. Seleccione el vínculo "+ Agregar directiva de acceso".
    1. En el menú desplegable "Permisos de los secretos", compruebe los permisos "Get" y "List".
    1. Seleccione la entidad de servicio por nombre o ID. de cliente.
    1. Seleccione "Agregar".
    1. Seleccione "Guardar".

4. Cree un secreto en el almacén de claves.
    1. Vaya a almacén de claves.
    1. Haga clic en la pestaña "Secretos" en Configuración.
    1. Haga clic en "+ Generate/Import" (+ Generar/Importar).
    1. Cree un nombre para el secreto, en este ejemplo llamaremos "Test" al secreto.
    1. Cree un valor para el secreto, en este ejemplo, estableceremos un valor "password123".

Ahora, cuando ejecute código desde la máquina local, puede autenticarse en el almacén de claves con la obtención de un token de acceso mediante la presentación del Id. de cliente y una ruta de acceso al certificado.

### <a name="part-4-retrieve-the-secret-from-your-azure-key-vault-in-an-application-python"></a>Parte 4: Recuperación del secreto de Azure Key Vault en una aplicación (Python)

Use el siguiente ejemplo de código para comprobar si la aplicación puede recuperar un secreto del almacén de claves mediante la entidad de servicio configurada. 

```python
from azure.keyvault.secrets import SecretClient
from azure.identity import CertificateCredential


tenant_id = ""                                             ##ENTER AZURE TENANT ID
vault_url = "https://{VAULT NAME}.vault.azure.net/"        ##ENTER THE URL OF YOUR KEY VAULT
client_id = ""                                             ##ENTER CLIENT ID OF SERVICE PRINCIPAL
cert_path = r"C:\Users\{USERNAME}\{PATH}\{CERT_NAME}.pem"  ##ENTER PATH TO CERTIFICATE

def main():

    #AUTHENTICATION TO AAD USING CLIENT ID AND CLIENT CERTIFICATE
    token = CertificateCredential(tenant_id= tenant_id, client_id=client_id, certificate_path=cert_path)

    #AUTHENTICATION TO KEY VAULT PRESENTING AAD TOKEN
    client = SecretClient(vault_url=vault_url, credential=token)

    #CALL TO KEY VAULT TO GET SECRET
    secret = client.get_secret('{SECRET_NAME}')            ##ENTER NAME OF SECRET IN KEY VAULT

    #GET PLAINTEXT OF SECRET
    print(secret.value)

#CALL MAIN()
if __name__ == "__main__":
    main()
```

![IMAGEN](../media/authentication-3.png)


## <a name="next-steps"></a>Pasos siguientes

1. Aprenda a solucionar errores de autenticación de un almacén de claves. [Guía de solución de problemas de Key Vault](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes)
