---
title: 'Tutorial: Asignación de un dominio personalizado existente a Azure Spring Cloud'
description: Asignación a Azure Spring Cloud de un nombre existente personalizado del servicio de nombres distribuido (DNS)
author: MikeDodaro
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: brendm
ms.openlocfilehash: 5b57a2463815d5db1c83d3bc4e8cd56314fb204d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82176840"
---
# <a name="map-an-existing-custom-domain-to-azure-spring-cloud"></a>Asignación de un dominio personalizado existente a Azure Spring Cloud
El servicio de nombres distribuido (DNS) es una técnica para almacenar nombres de nodos de red en toda una red. En este tutorial se asigna un dominio, como www.contoso.com, mediante un registro CNAME. El dominio personalizado se protege con un certificado y se muestra cómo aplicar Seguridad de la capa de transporte (TLS), también conocida como Capa de sockets seguros (SSL). 

El tráfico web se cifra mediante certificados. Estos certificados TLS/SSL se pueden almacenar en Azure Key Vault. 

## <a name="prerequisites"></a>Prerrequisitos
* Una aplicación implementada en Azure Spring Cloud (consulte [Inicio rápido: Inicio de una aplicación de Azure Spring Cloud existente desde Azure Portal](spring-cloud-quickstart-launch-app-portal.md)) o puede usar una aplicación existente.
* Un nombre de dominio con acceso al registro DNS para el proveedor de dominios, como GoDaddy.
* Un certificado privado de un proveedor de terceros. El certificado debe coincidir con el dominio.
* Una instancia implementada de [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

## <a name="import-certificate"></a>Importación de certificado 
Para importar un certificado, es necesario que el archivo codificado PEM o PFX esté en el disco y se debe tener la clave privada. 

Para cargar el certificado en el almacén de claves:
1. Vaya a la instancia de Key Vault.
1. En el panel de navegación izquierdo, haga clic en **Certificados**.
1. En el menú superior, haga clic en **Generar o importar**.
1. En el cuadro de diálogo **Crear un certificado** en **Método de creación de certificados**, seleccione `Import`.
1. En **Cargar archivo de certificado**, vaya a la ubicación del certificado y selecciónela.
1. En **Contraseña**, escriba la clave privada del certificado.
1. Haga clic en **Crear**.

![Importación del certificado 1](./media/custom-dns-tutorial/import-certificate-a.png)

Para importar el certificado en Azure Spring Cloud:
1. Vaya a la instancia del servicio. 
1. En el panel de navegación izquierdo de la aplicación, seleccione **Configuración de TLS/SSL**.
1. Luego, haga clic en **Import Key Vault Certificate** (Importar certificado de Key Vault).

![Importación de certificado](./media/custom-dns-tutorial/import-certificate.png)

Cuando haya importado correctamente el certificado, lo verá en la lista de **Private Key Certificates** (Certificados de clave privada).

![Certificado de clave privada](./media/custom-dns-tutorial/key-certificates.png)

>[!IMPORTANT] 
> Para proteger un dominio personalizado con este certificado, todavía debe enlazar el certificado a un dominio determinado. Siga los pasos de este documento que se indican en **Agregar enlace SSL**.

## <a name="add-custom-domain"></a>Adición de un dominio personalizado
Puede usar un registro CNAME para asignar un nombre DNS personalizado a Azure Spring Cloud. 

> [!NOTE] 
> No se admite el registro D. 

### <a name="create-the-cname-record"></a>Crear un registro CNAME
Vaya a su proveedor de DNS y agregue un registro CNAME para asignar el dominio a < service_name>.azuremicroservices.io. Aquí <service_name> es el nombre de la instancia de Azure Spring Cloud. Se admite el dominio y el subdominio con comodín. Después de agregar el registro CNAME, la página de registros DNS se parecerá al ejemplo siguiente: 

![Página de registros DNS](./media/custom-dns-tutorial/dns-records.png)

## <a name="map-your-custom-domain-to-azure-spring-cloud-app"></a>Asignación del dominio personalizado a la aplicación de Azure Spring Cloud
Si no tiene una aplicación en Azure Spring Cloud, siga las instrucciones que se indican en [Inicio rápido: Inicio de una aplicación de Azure Spring Cloud existente desde Azure Portal](https://review.docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal?branch=master).

Vaya a la página de la aplicación.

1. Seleccione **Dominio personalizado**.
2. Luego, elija **Add Custom Domain** (Agregar dominio personalizado). 

![Dominio personalizado](./media/custom-dns-tutorial/custom-domain.png)

3. Escriba el nombre de dominio completo para el que ha agregado un registro CNAME, como www.contoso.com. Asegúrese de que el tipo de registro de nombre de host esté establecido en CNAME (<service_name>.azuremicroservices.io)
4. Haga clic en **Validar** para habilitar el botón **Agregar**.
5. Haga clic en **Agregar**.

![Adición de un dominio personalizado](./media/custom-dns-tutorial/add-custom-domain.png)

Una aplicación puede tener varios dominios, pero un dominio solo puede asignarse a una aplicación. Cuando haya asignado correctamente el dominio personalizado a la aplicación, lo verá en la tabla de dominios personalizados.

![Tabla de dominios personalizados](./media/custom-dns-tutorial/custom-domain-table.png)

>[!NOTE]
> Una etiqueta **Not Secure** (No seguro) para el dominio personalizado significa que aún no está enlazado a un certificado SSL. Todas las solicitudes HTTPS a este dominio personalizado desde un explorador recibirán un error o una advertencia.

## <a name="add-ssl-binding"></a>Agregar enlace SSL
En la tabla de dominios personalizados, seleccione **Agregar enlace SSL** como se muestra en la ilustración anterior.  
1. Seleccione el **certificado** o impórtelo.
1. Haga clic en **Save**(Guardar).

![Agregar enlace SSL](./media/custom-dns-tutorial/add-ssl-binding.png)

Después de agregar correctamente el enlace SSL, el estado del dominio será seguro: **Correcto**. 

![Agregar enlace SSL](./media/custom-dns-tutorial/secured-domain-state.png)

## <a name="enforce-https"></a>Aplicación de HTTPS
De forma predeterminada, los usuarios pueden seguir accediendo a la aplicación mediante HTTP, pero puede redirigir todas las solicitudes HTTP al puerto HTTPS.

En la página de la aplicación, en el panel de navegación izquierdo, seleccione **Dominio personalizado**. Luego, establezca **Solo HTTPS** en *True*.

![Agregar enlace SSL](./media/custom-dns-tutorial/enforce-http.png)

Una vez finalizada la operación, vaya a cualquiera de las direcciones URL HTTP que apuntan a la aplicación. Observe que las direcciones URL HTTP no funcionan.

## <a name="see-also"></a>Consulte también
* [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Importación de un certificado](https://docs.microsoft.com/azure/key-vault/certificate-scenarios#import-a-certificate)
* [Inicio de la aplicación Spring Cloud desde la CLI de Azure](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli)

