---
title: Creación y combinación de solicitudes de firma de certificado en Azure Key Vault
description: Creación y combinación de solicitudes de firma de certificado en Azure Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.date: 06/17/2020
ms.author: sebansal
ms.openlocfilehash: c8f11f17c9e110509dcbcda291194f9b8d928c50
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658968"
---
# <a name="creating-and-merging-csr-in-key-vault"></a>Creación y combinación de solicitudes de firma de certificado en Key Vault

Azure Key Vault admite el almacenamiento de certificados digitales emitidos por la entidad de certificación de su elección en el almacén de claves. Admite la creación de solicitudes de firma de certificado con el par de claves pública y privada, que puede ser firmada por la entidad de certificación elegida. que tanto puede ser una entidad de certificación empresarial interna como una entidad de certificación pública externa. La solicitud de firma de certificado (también conocida como CSR o solicitud de certificación) es un mensaje enviado por el usuario a una entidad de certificación con el fin de solicitar la emisión de un certificado digital.

Para más información general acerca de los certificados, consulte [Certificados de Azure Key Vault](./about-certificates.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="adding-certificate-in-key-vault-issued-by-partnered-ca"></a>Incorporación de un certificado en Key Vault emitido por una entidad de certificación asociada
Key Vault se asocia con las dos entidades de certificación siguientes para simplificar la creación de certificados. 

|Proveedor|Tipo de certificado|Configuración  
|--------------|----------------------|------------------|  
|DigiCert|Key Vault ofrece certificados SSL OV o EV con DigiCert| [Guía de integración](./how-to-integrate-certificate-authority.md)
|GlobalSign|Key Vault ofrece certificados SSL OV o EV con GlobalSign| [Guía de integración](https://support.globalsign.com/digital-certificates/digital-certificate-installation/generating-and-importing-certificate-microsoft-azure-key-vault)

## <a name="adding-certificate-in-key-vault-issued-by-non-partnered-ca"></a>Incorporación de un certificado en Key Vault emitido por una entidad de certificación no asociada

Los pasos siguientes le ayudarán a crear un certificado procedente de entidades de certificación que no están asociadas con Key Vault (por ejemplo, GoDaddy no es una entidades de certificación de confianza en Key Vault). 


### <a name="azure-powershell"></a>Azure PowerShell



1.  En primer lugar, **cree la directiva de certificados**. Key Vault no inscribirá ni renovará el certificado del emisor en nombre del usuario, ya que la entidad de certificación elegida en este escenario no es compatible y, por lo tanto, el valor de IssuerName se establece en Unknown.

    ```azurepowershell
    $policy = New-AzKeyVaultCertificatePolicy -SubjectName "CN=www.contosoHRApp.com" -ValidityInMonths 1  -IssuerName Unknown
    ```


2. Cree una **solicitud de firma de certificado**.

   ```azurepowershell
   $csr = Add-AzKeyVaultCertificate -VaultName ContosoKV -Name ContosoManualCSRCertificate -CertificatePolicy $policy
   $csr.CertificateSigningRequest
   ```

3. Obtenga la **solicitud de CSR firmada por la entidad de certificación**. `$certificateOperation.CertificateSigningRequest` es la solicitud de firma de certificado codificada en base4. Puede tomar este blob y volcarlo en el sitio web de solicitud de certificados del emisor. Este paso varía entre las diferentes entidades de certificación, por lo que es preferible localizar las instrucciones de la entidad específica sobre cómo ejecutarlo. También puede usar herramientas como certreq u openssl para obtener la solicitud de certificado firmada y completar el proceso de generación de un certificado.


4. **Combine la solicitud firmada** en Key Vault. Después de que el emisor haya firmado la solicitud de certificado, puede recuperar el certificado firmado y combinarlo con el par de claves pública y privada inicial creado en Azure Key Vault.

    ```azurepowershell-interactive
    Import-AzKeyVaultCertificate -VaultName ContosoKV -Name ContosoManualCSRCertificate -FilePath C:\test\OutputCertificateFile.cer
    ```

    La solicitud de certificado se ha combinado correctamente.

### <a name="azure-portal"></a>Azure portal

1.  Para generar la CSR para la entidad de certificación de su elección, vaya al almacén de claves en el que desea agregar el certificado.
2.  En las páginas de propiedades de Key Vault, seleccione **Certificados**.
3.  Seleccione la pestaña **Generar o importar**.
4.  En la pantalla **Creación de certificado**, elija los siguientes valores:
    - **Método de creación de certificados**: Generar.
    - **Nombre del certificado**: ContosoManualCSRCertificate.
    - **Tipo de entidad de certificación (CA)** : Certificado emitido por una entidad de certificación no integrada.
    - **Asunto**: `"CN=www.contosoHRApp.com"`.
    - Seleccione los demás valores como desee. Haga clic en **Crear**.

    ![Propiedades del certificado](../media/certificates/create-csr-merge-csr/create-certificate.png)
6.  Verá que ahora se ha agregado el certificado en la lista Certificados. Seleccione este nuevo certificado que acaba de crear. El estado actual del certificado será "deshabilitado", ya que aún no ha sido emitido por la entidad de certificación.
7. Haga clic en la pestaña **Operación de certificados** y seleccione **Descargar CSR**.
 ![Captura de pantalla que resalta el botón Descargar CSR.](../media/certificates/create-csr-merge-csr/download-csr.png)

8.  Envíe el archivo .crs a la entidad de certificación para que se firme la solicitud.
9.  Una vez que la entidad de certificación firme la solicitud, devuelva el archivo de certificado para **combinar la solicitud firmada** en la misma pantalla Operación de certificados.

La solicitud de certificado se ha combinado correctamente.

## <a name="adding-more-information-to-csr"></a>Incorporación de más información a CSR

Si desea agregar más información al crear CSR, por ejemplo: 
    - País:
    - Ciudad o situación:
    - Estado o provincia:
    - Organización:
    - Unidad organizativa: Puede agregar toda esta información al crear un CSR si la define en subjectName.

Ejemplo
    ```SubjectName="CN = docs.microsoft.com, OU = Microsoft Corporation, O = Microsoft Corporation, L = Redmond, S = WA, C = US"
    ```

>[!Note]
>Si solicita un certificado de validación de dominio (DV) con todos esos detalles en el CSR, puede que la CA rechace la solicitud porque no pueda validar toda la información contenida en la misma. Si solicita un certificado de validación de organización (OV), sería más adecuado agregar toda esa información en el CSR.


## <a name="troubleshoot"></a>Solución de problemas

- **Error de tipo "La clave pública del certificado de entidad final en el contenido del certificado X.509 especificado no coincide con la parte pública de la clave privada especificada. Compruebe si el certificado es válido"** Este error se puede producir si no se combina el CSR con la misma solicitud de CSR iniciada. Cada vez que se crea un CSR, se crea una clave privada que debe coincidir al combinar la solicitud firmada.
    
- Cuando se combine CSR, ¿se combinará la cadena completa?
    Sí, se combinará toda la cadena, siempre que el usuario haya recuperado el archivo p7b para combinar.

- Si el certificado emitido se encuentra en el estado "deshabilitado" en Azure Portal, consulte la pantalla **Operación de certificados** y revise el mensaje de error sobre ese certificado.

Para más información, consulte las [operaciones con certificados en la referencia de la API REST de Key Vault](/rest/api/keyvault). Para obtener información sobre cómo establecer permisos, vea [Almacenes: crear o actualizar](/rest/api/keyvault/vaults/createorupdate) y [Almacenes: actualizar directiva de acceso](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Pasos siguientes

- [Autenticación, solicitudes y respuestas](../general/authentication-requests-and-responses.md)
- [Guía del desarrollador de Key Vault](../general/developers-guide.md)
