---
title: Terminación SSL con certificados de Azure Key Vault
description: Obtenga información sobre cómo puede integrar Azure Application Gateway con Key Vault para certificados de servidor que se adjuntan a los clientes de escucha con HTTPS habilitado.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 5633dd7b72f4de22cd34b7d093e8ec4d9cb411f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137701"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>Terminación SSL con certificados de Key Vault

[Azure Key Vault](../key-vault/key-vault-overview.md) es un almacén de secretos administrado por la plataforma que puede usar para proteger los secretos, las claves y los certificados SSL. Azure Application Gateway admite la integración con Key Vault para certificados de servidor adjuntos a clientes de escucha con HTTPS habilitado. Esta compatibilidad está limitada a la versión 2 de la SKU de Application Gateway.

La integración de Key Vault ofrece dos modelos para la terminación SSL:

- Puede proporcionar explícitamente los certificados SSL asociados al cliente de escucha. Este modelo es la forma tradicional para pasar los certificados SSL a Application Gateway para la terminación SSL.
- También puede proporcionar una referencia a un certificado de Key Vault existente o un secreto al crear un cliente de escucha habilitado para HTTPS.

La integración de Application Gateway con Key Vault ofrece muchas ventajas, incluidas las siguientes:

- Mayor seguridad, dado que el equipo de desarrollo de aplicaciones no controla directamente los certificados SSL. La integración permite que un equipo de seguridad independiente haga lo siguiente:
  * Configurar instancias de Application Gateway.
  * Controlar los ciclos de vida de las instancias de Application Gateway.
  * Conceder permisos a las instancias de Application Gateway seleccionadas para tener acceso a los certificados almacenados en el almacén de claves.
- Compatibilidad con la importación de certificados existentes en el almacén de claves. También puede usar las API de Key Vault para crear y administrar certificados nuevos con cualquiera de los asociados de Key Vault de confianza.
- Compatibilidad con la renovación automática de certificados almacenados en el almacén de claves.

Actualmente, Application Gateway solo admite certificados validados por software. Los certificados validados por el módulo de seguridad de hardware (HSM) no se admiten. Después de configurar Application Gateway para usar certificados de Key Vault, sus instancias recuperan el certificado de Key Vault y lo instalan localmente para la terminación SSL. Además, las instancias sondean Key Vault en intervalos de 24 horas para recuperar una versión renovada del certificado, si existe. Si se encuentra un certificado actualizado, el certificado SSL asociado actualmente al cliente de escucha HTTPS se gira automáticamente.

> [!NOTE]
> Azure Portal solo admite certificados de KeyVault, no secretos. Application Gateway sigue admitiendo hacer referencia a secretos desde KeyVault, pero solo a través de recursos que no son de portal, como PowerShell, la CLI, API, plantillas de ARM, etc. 

## <a name="how-integration-works"></a>Funcionamiento de la integración

La integración de Application Gateway con Key Vault requiere un proceso de configuración de tres pasos:

1. **Creación de una identidad administrada asignada por el usuario**

   Debe crear o volver a usar una identidad administrada asignada por el usuario existente, que Application Gateway usará para recuperar los certificados de Key Vault en su nombre. Para más información, consulte [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md) Con este paso se crea una nueva identidad en el inquilino de Azure Active Directory. La identidad es de confianza para la suscripción que se usa para crear la identidad.

1. **Configuración del almacén de claves**

   A continuación, importe un certificado existente o cree uno nuevo en el almacén de claves. Las aplicaciones que se ejecutan a través de Application Gateway usarán el certificado. En este paso, también puede usar un secreto de almacén de claves que se almacena como archivo PFX codificado en Base 64 sin contraseña. Se recomienda usar un tipo de certificado debido a la capacidad de renovación automática que está disponible con los objetos de tipo certificado en el almacén de claves. Después de crear un certificado o un secreto, debe definir directivas de acceso en el almacén de claves para permitir que la identidad que se debe conceder *obtenga* acceso al secreto.

1. **Configuración de la instancia de Application Gateway**

   Después de completar los dos pasos anteriores, puede configurar o modificar una puerta de enlace de aplicaciones existente para usar la identidad administrada asignada por el usuario. También puede configurar el certificado SSL del cliente de escucha HTTP para que apunte al URI completo del certificado de Key Vault o al id. de secreto.

   ![Certificados de Key Vault](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Pasos siguientes

[Configuración de la terminación SSL con certificados de Key Vault mediante Azure PowerShell](configure-keyvault-ps.md)
