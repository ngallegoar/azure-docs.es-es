---
title: Acerca de la renovación de certificados de Azure Key Vault
description: En este artículo se describe cómo renovar los certificados de Azure Key Vault.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: overview
ms.date: 07/20/2020
ms.author: sebansal
ms.openlocfilehash: 2477bab244b8864fa9c82b52d5577d42fa47a7e0
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92124158"
---
# <a name="renew-your-azure-key-vault-certificates"></a>Renovación de los certificados de Azure Key Vault

Con Azure Key Vault puede aprovisionar, administrar e implementar fácilmente certificados digitales para una red y habilitar comunicaciones seguras para las aplicaciones. Para más información acerca de los certificados, consulte [Acerca de los certificados de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates).

Mediante el uso de certificados de corta duración o el aumento de la frecuencia de rotación de estos, puede evitar el acceso a las aplicaciones por parte de usuarios no autorizados.

En este artículo se describe cómo renovar los certificados de Azure Key Vault.

## <a name="get-notified-about-certificate-expiration"></a>Obtención de notificaciones sobre expiraciones de certificados
Para recibir notificaciones sobre los eventos de vida de un certificado, deberá agregar el contacto del certificado. Los contactos de certificados contienen información de contacto para enviar notificaciones desencadenadas por los eventos de vigencia del certificado. La información de los contactos es compartida por todos los certificados del almacén de claves. Se envía una notificación a todos los contactos especificados para un evento de cualquier certificado del almacén de claves.

### <a name="steps-to-set-certificate-notifications"></a>Pasos para establecer notificaciones de certificado:
En primer lugar, agregue un contacto del certificado al almacén de claves. Para ello, puede usar Azure Portal o el cmdlet de PowerShell [`Add-AzureKeyVaultCertificateContact`](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificatecontact?view=azurermps-6.13.0).

En segundo lugar, configure cuándo desea recibir una notificación sobre la expiración del certificado. Para configurar los atributos del ciclo de vida del certificado, consulte [Configuración de la rotación automática de certificados en Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate).

Si se establece la directiva de un certificado para la renovación automática, se envía una notificación en los siguientes eventos.

- Antes de la renovación del certificado
- Tras la renovación del certificado, indicando si el certificado se renovó correctamente o si se produjo un error que exige la renovación manual del certificado.  

  Si se establece una directiva de certificado en renovación manual (solo correo electrónico), cuando llega el momento de renovar el certificado se envía una notificación.  

En Key Vault, hay tres categorías de certificados:
-   Certificados que se crean con una entidad de certificación integrada, como DigiCert o GlobalSign
-   Certificados que se crean con una entidad de certificación no integrada
-   Certificados autofirmados

## <a name="renew-an-integrated-ca-certificate"></a>Renovación del certificado de una entidad de certificación integrada 
Azure Key Vault se encarga del mantenimiento integral de los certificados que emiten las entidades de certificación de confianza de Microsoft; es decir, DigiCert y GlobalSign. Aprenda a [integrar una entidad de certificación de confianza con Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/how-to-integrate-certificate-authority).

## <a name="renew-a-nonintegrated-ca-certificate"></a>Renovación del certificado de una entidad de certificación no integrada 
Mediante Azure Key Vault, puede importar certificados de cualquier entidad de certificación, una ventaja que le permite la integración con varios recursos de Azure y facilita la implementación. Si le preocupa no saber exactamente cuándo expirará su certificado o, peor aún, ha descubierto que el certificado ya ha expirado, Key Vault puede ayudarle a mantenerse al día. En el caso de los certificados de una entidad de certificación no integrada, Key Vault le permite configurar notificaciones de correo electrónico que le avisan de una próxima expiración. Estas notificaciones también se pueden configurar para varios usuarios.

> [!IMPORTANT]
> Un certificado es un objeto con versiones. Si la versión actual expira, debe crear una nueva versión. En términos conceptuales, cada nueva versión es un certificado nuevo compuesto por una clave y un blob que une esa clave a una identidad. Cuando se usa una entidad de certificación no asociada, Key Vault genera un par clave-valor y devuelve una solicitud de firma de certificado (CSR).

Para renovar un certificado de una entidad de certificación no integrada, haga lo siguiente:

1. Inicie sesión en Azure Portal y, a continuación, abra el certificado que desea renovar.
1. En el panel Certificado, seleccione el botón **+ Nueva versión** .
1. Seleccione **Operación de certificados** .
1. Seleccione **Descargar CSR** para descargar un archivo CSR en la unidad local.
1. Envíe el archivo CSR a la entidad de certificación de su elección para firmar la solicitud.
1. Recupere la solicitud firmada y seleccione **Merge CSR** (Fusionar CSR) en el mismo panel Operación de certificados.

> [!NOTE]
> Es importante fusionar el CSR firmado con la misma solicitud de CSR que creó. De lo contrario, la clave no coincidirá.

Para más información sobre la creación de un CSR, consulte [Creación y Combinación de un CSR en Key Vault]( https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-signing-request#azure-portal).

## <a name="renew-a-self-signed-certificate"></a>Renovación de un certificado autofirmado

Azure Key Vault también administra la renovación automática de certificados autofirmados. Para más información sobre el cambio de la directiva de emisión y la actualización de los atributos del ciclo de vida de un certificado, consulte [Configuración de la rotación automática de certificados en Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate).

## <a name="troubleshoot"></a>Solución de problemas
Si el certificado emitido se encuentra en el estado *deshabilitado* en Azure Portal, vaya a **Operación de certificados** y revise el mensaje de error de ese certificado.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**¿Cómo se puede probar la característica de rotación automática del certificado?**

Cree un certificado con una validez de **1 mes** y, a continuación, establezca la acción de duración para la rotación en **1%** . Esta configuración rotará el certificado cada 7,2 horas.
  
**¿Se replicarán las etiquetas después de la renovación automática del certificado?**

Sí, las etiquetas se replican después de la renovación automática.

## <a name="next-steps"></a>Pasos siguientes
*   [Integración de Key Vault con la entidad de certificación DigiCert](how-to-integrate-certificate-authority.md)
*   [Tutorial: Configuración de la rotación automática de certificados en Key Vault](tutorial-rotate-certificates.md)
