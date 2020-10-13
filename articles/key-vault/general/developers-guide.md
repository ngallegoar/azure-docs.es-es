---
title: Guía del desarrollador de Azure Key Vault
description: Los desarrolladores pueden usar Azure Key Vault para administrar las claves criptográficas en el entorno de Microsoft Azure.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/05/2020
ms.author: mbaldwin
ms.openlocfilehash: a04435b1e2feb537231bb80d2777b9ea2599c241
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812410"
---
# <a name="azure-key-vault-developers-guide"></a>Guía del desarrollador de Azure Key Vault

Key Vault le permite acceder de forma segura a información confidencial desde sus aplicaciones:

- Las claves y los secretos y los certificados están protegidos sin tener que escribir el código manualmente, y puede usarlos fácilmente en sus aplicaciones.
- Puede hacer que los clientes posean y administren sus propias claves, secretos y certificados para que pueda concentrarse en proporcionar las características de software principales. De este modo, las aplicaciones no serán responsables de las claves, secretos y certificados del inquilino de sus clientes.
- La aplicación puede usar claves para la firma y el cifrado, pero también mantiene la opción de administrar de forma externa las claves de la aplicación. Para obtener más información sobre las claves, consulte [Acerca de las claves.](../keys/about-keys.md)
- Puede administrar credenciales, como contraseñas, claves de acceso y tokens de SAS almacenándolas en Key Vault como secretos; consulte [Acerca de los secretos](../secrets/about-secrets.md) para obtener más información.
- Administrar certificados. Para obtener más información, consulte [Acerca de los certificados](../certificates/about-certificates.md).

Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Key Vault?](overview.md).

## <a name="public-previews"></a>Versiones preliminares públicas

Periódicamente se publica una versión preliminar pública de una nueva característica de Key Vault. Pruebe las características de la versión preliminar públicas y díganos lo que piensa a través de nuestra dirección de correo electrónico para enviar comentarios, azurekeyvault@microsoft.com.

## <a name="creating-and-managing-key-vaults"></a>Creación y administración de almacenes de claves

La administración de Key Vault es similar a otros servicios de Azure y se realiza a través del servicio Azure Resource Manager. Azure Resource Manager es el servicio de implementación y administración para Azure. Proporciona una capa de administración que le permite crear, actualizar y eliminar recursos de la cuenta de Azure. Para obtener más información, consulte [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview).

El acceso a la capa de administración se controla mediante el [Control de acceso basado en roles de Azure](https://docs.microsoft.com/azure/role-based-access-control/overview). En Key Vault, la capa de administración, también conocida como administración o plano de control, le permite crear y administrar almacenes de claves y sus atributos, incluidas las directivas de acceso; no podrá administrar claves, secretos y certificados, puesto que se administran en el plano de datos. Puede usar el rol predefinido `Key Vault Contributor` para conceder acceso de administración a Key Vault.     

**API y SDK para la administración del almacén de claves:**

| Azure CLI | PowerShell | API DE REST | Resource Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Referencia](/cli/azure/keyvault)<br>[Guía de inicio rápido](quick-create-cli.md)|[Referencia](/powershell/module/az.keyvault)<br>[Guía de inicio rápido](quick-create-powershell.md)|[Referencia](/rest/api/keyvault/)|[Referencia](/azure/templates/microsoft.keyvault/vaults)|[Referencia](/dotnet/api/microsoft.azure.management.keyvault)|[Referencia](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault)|[Referencia](/java/api/com.microsoft.azure.management.keyvault)|[Referencia](/javascript/api/@azure/arm-keyvault)|

Consulte las [Bibliotecas de cliente](client-libraries.md) para ver los paquetes de instalación y el código fuente.

Para obtener más información acerca del plano de administración de Key Vault, consulte [Plano de administración de Key Vault](https://docs.microsoft.com/azure/key-vault/general/secure-your-key-vault#management-plane-and-azure-rbac).

## <a name="authenticate-to-key-vault-in-code"></a>Autenticación en Key Vault mediante el código

Como Key Vault usa la autenticación de Azure AD, requiere que la entidad de seguridad de Azure AD le conceda acceso. Una entidad de seguridad de Azure AD puede ser un usuario, una entidad de servicio de aplicación, una [identidad administrada para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md) o un grupo de cualquier tipo de entidad de seguridad.

### <a name="authentication-best-practices"></a>Procedimientos recomendados de autenticación
Se recomienda usar la identidad administrada para las aplicaciones implementadas en Azure. Si usa servicios de Azure, los cuales no admiten la identidad administrada o si las aplicaciones se implementan de forma local, la [entidad de servicio con un certificado](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) es una alternativa posible. En ese escenario, el certificado debe almacenarse en Key Vault y cambiarse a menudo. La entidad de servicio que cuenta con el secreto se puede usar en entornos de desarrollo y pruebas; si se usa de forma local o en Cloud Shell, es recomendable usar la entidad de seguridad de usuario.

Los escenarios de autenticación anteriores son compatibles con la biblioteca de cliente de Azure Identity y se integran con los SDK de Key Vault. La biblioteca de identidades de Azure se puede usar en diferentes entornos y plataformas sin tener que cambiar el código. La identidad de Azure también recuperará automáticamente el token de autenticación del usuario que inició sesión en Azure con la CLI de Azure, Visual Studio, Visual Studio Code y otros. 

Para obtener más información, vea: 

| .NET | Python | Java | JavaScript |
|--|--|--|--|
|[SDK .NET de Azure Identity](https://docs.microsoft.com/dotnet/api/overview/azure/identity-readme)|[SDK Python de Azure Identity](https://docs.microsoft.com/python/api/overview/azure/identity-readme)|[SDK Java de Azure Identity](https://docs.microsoft.com/java/api/overview/azure/identity-readme)|[SDK JavaScript de Azure Identity](https://docs.microsoft.com/javascript/api/overview/azure/identity-readme)|     

Autenticación en Key Vault mediante las aplicaciones:
- [Autenticación en Key Vault en una aplicación hospedada en la VM de .NET](https://docs.microsoft.com/azure/key-vault/general/tutorial-net-virtual-machine)
- [Autenticación en Key Vault en una aplicación hospedada en la VM de Python](https://docs.microsoft.com/azure/key-vault/general/tutorial-python-virtual-machine)
- [Autenticación en Key Vault con App Service](https://docs.microsoft.com/azure/key-vault/general/tutorial-net-create-vault-azure-web-app)

## <a name="manage-keys-certificates-and-secrets"></a>Administración de claves, certificados y secretos

El acceso a las claves, los secretos y los certificados se controla mediante el plano de datos. El control de acceso del plano de datos se puede realizar mediante las directivas de acceso de almacén local o RBAC (versión preliminar).

**API y SDK de claves**


| Azure CLI | PowerShell | API DE REST | Resource Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Referencia](/cli/azure/keyvault/key)<br>[Guía de inicio rápido](../keys/quick-create-cli.md)|[Referencia](/powershell/module/az.keyvault/)<br>[Guía de inicio rápido](../keys/quick-create-powershell.md)|[Referencia](/rest/api/keyvault/#key-operations)|N/D|[Referencia](/dotnet/api/azure.security.keyvault.keys)|[Referencia](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault)<br>[Guía de inicio rápido](../keys/quick-create-python.md)|[Referencia](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-keys/4.2.0/index.html)|[Referencia](/javascript/api/@azure/keyvault-keys/)|

**SDK y API de certificados**


| Azure CLI | PowerShell | API DE REST | Resource Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Referencia](/cli/azure/keyvault/certificate)<br>[Guía de inicio rápido](../certificates/quick-create-cli.md)|[Referencia](/powershell/module/az.keyvault)<br>[Guía de inicio rápido](../certificates/quick-create-powershell.md)|[Referencia](/rest/api/keyvault/#certificate-operations)|N/D|[Referencia](/dotnet/api/azure.security.keyvault.certificates)|[Referencia](/python/api/overview/azure/keyvault-certificates-readme)<br>[Guía de inicio rápido](../certificates/quick-create-python.md)|[Referencia](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-certificates/4.1.0/index.html)|[Referencia](/javascript/api/@azure/keyvault-certificates/)|

**SDK y API de secretos**


| Azure CLI | PowerShell | API DE REST | Resource Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Referencia](/cli/azure/keyvault/secret)<br>[Guía de inicio rápido](../secrets/quick-create-cli.md)|[Referencia](/powershell/module/az.keyvault/)<br>[Guía de inicio rápido](../secrets/quick-create-powershell.md)|[Referencia](/rest/api/keyvault/#secret-operations)|[Referencia](/azure/templates/microsoft.keyvault/vaults/secrets)<br>[Guía de inicio rápido](../secrets/quick-create-template.md)|[Referencia](/dotnet/api/azure.security.keyvault.secrets)<br>[Guía de inicio rápido](../secrets/quick-create-net.md)|[Referencia](/python/api/overview/azure/keyvault-secrets-readme)<br>[Guía de inicio rápido](../secrets/quick-create-python.md)|[Referencia](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-secrets/4.2.0/index.html)<br>[Guía de inicio rápido](../secrets/quick-create-java.md)|[Referencia](/javascript/api/@azure/keyvault-secrets/)<br>[Guía de inicio rápido](../secrets/quick-create-node.md)|

Consulte las [Bibliotecas de cliente](client-libraries.md) para ver los paquetes de instalación y el código fuente.

Para obtener más información sobre la seguridad del plano de datos de Key Vault, consulte las [Directivas de acceso y plano de datos de Key Vault](https://docs.microsoft.com/azure/key-vault/general/secure-your-key-vault#data-plane-and-access-policies) y el [Plano de datos y RBAC (versión preliminar) de Key Vault](https://docs.microsoft.com/azure/key-vault/general/secure-your-key-vault#data-plane-and-azure-rbac-preview).

### <a name="code-examples"></a>Ejemplos de código

Para obtener ejemplos completos de cómo usar Key Vault con las aplicaciones, vea:

- [Ejemplos de código de Azure Key Vault](https://azure.microsoft.com/resources/samples/?service=key-vault). Ejemplos de código para Azure Key Vault. 

## <a name="how-tos"></a>Procedimientos

Los artículos y escenarios siguientes proporcionan instrucciones específicas de tarea sobre cómo trabajar con Azure Key Vault:

- [Acceso a Key Vault detrás de un firewall](access-behind-firewall.md). Para acceder a un almacén de claves, es preciso que la aplicación cliente de Key Vault pueda acceder a varios puntos de conexión para diversas funcionalidades.
- Cómo implementar certificados en VM desde Key Vault ([Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-windows), [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-linux)): una aplicación de nube que se ejecute en una VM de Azure necesita un certificado. Sepa cómo añadirlo a la máquina virtual hoy mismo.
- [Implementar un certificado de Azure Web App mediante Key Vault](https://docs.microsoft.com/azure/app-service/configure-ssl-certificate#import-a-certificate-from-key-vault).
- Asignación de una directiva de acceso ([CLI](assign-access-policy-cli.md) | [PowerShell](assign-access-policy-powershell.md) | [Azure Portal](assign-access-policy-portal.md)). 
- [Uso de la eliminación temporal de Key Vault con la CLI](soft-delete-cli.md) le guiará por el uso y ciclo de vida de un almacén de claves y de varios objetos de almacén de claves con la eliminación temporal habilitada.
- [Paso de valores seguros (como contraseñas) durante la implementación](../../azure-resource-manager/templates/key-vault-parameter.md). Si necesita pasar un valor seguro (como una contraseña) como un parámetro durante la implementación, puede almacenar ese valor como un secreto en Azure Key Vault y hacer referencia al valor en otras plantillas de Resource Manager.

## <a name="integrated-with-key-vault"></a>Integración con Key Vault

En estos artículos se describen otros escenarios y servicios que usan Key Vault o se integran en este servicio.

- El [cifrado en reposo](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) permite la codificación (cifrado) de datos cuando se conserva. Las claves de cifrado de datos se cifran a menudo con la clave de cifrado de claves en Azure Key Vault para limitar aún más el acceso.
- [Azure Information Protection](/azure/information-protection/plan-implement-tenant-key) le permite administrar su propia clave de inquilino. Por ejemplo, en lugar de que Microsoft administre su clave de inquilino (la opción predeterminada), puede administrar su propia clave de inquilino para cumplir con las normas específicas que se aplican a su organización. La administración de su propia clave de inquilino también se conoce como "aportar su propia clave", o BYOK.
- El [servicio Azure Private Link](private-link-service.md) le permite acceder a los servicios de Azure (por ejemplo, Azure Key Vault, Azure Storage y Azure Cosmos DB) y a los servicios de asociados o clientes hospedados de Azure mediante un punto de conexión privado de la red virtual.
- La integración de Key Vault con [Event Grid](https://docs.microsoft.com/azure/event-grid/event-schema-key-vault) permite a los usuarios recibir una notificación cuando cambia el estado de un secreto almacenado en el almacén de claves. Puede distribuir la nueva versión de los secretos en las aplicaciones, o cambiar los secretos que estén a punto de expirar para evitar interrupciones.
- Puede proteger los secretos de [Azure DevOps](https://docs.microsoft.com/azure/devops/pipelines/release/azure-key-vault) de accesos no deseados en Key Vault.
- [Use el secreto almacenado en Key Vault en DataBricks para conectarse a Azure Storage](https://docs.microsoft.com/azure/key-vault/general/integrate-databricks-blob-storage)
- Configure y ejecute el proveedor de Azure Key Vault correspondiente al [controlador Secrets Store CSI](https://docs.microsoft.com/azure/key-vault/general/key-vault-integrate-kubernetes) en Kubernetes

## <a name="key-vault-overviews-and-concepts"></a>Conceptos y datos globales de Key Vault

- [Comportamiento de eliminación temporal de Key Vault](soft-delete-overview.md) describe una característica que permite la recuperación de objetos eliminados tanto si dicha eliminación ha sido accidental como intencionada.
- [Limitación del cliente de Key Vault](overview-throttling.md) proporciona orientación sobre los conceptos básicos de la limitación y ofrece un enfoque para la aplicación.
- [Espacios de seguridad de Key Vault](overview-security-worlds.md) describe las relaciones entre regiones y zonas de seguridad.

## <a name="social"></a>Redes sociales

- [Blog de Key Vault](https://aka.ms/kvblog)
- [Foro sobre Key Vault](https://aka.ms/kvforum)
