---
title: 'Tutorial: Actualización de la frecuencia de rotación automática de certificados en Key Vault | Microsoft Docs'
description: Tutorial que muestra cómo actualizar la frecuencia de rotación automática de un certificado en Azure Key Vault mediante Azure Portal.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/16/2020
ms.author: sebansal
ms.openlocfilehash: 2e6c250a0bcb9d73e7c572dfe8138c31269993e8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82107562"
---
# <a name="tutorial-configuring-certificates-auto-rotation-in-key-vault"></a>Tutorial: Configuración de la rotación automática de certificados en Key Vault

Azure Key Vault le permite aprovisionar, administrar e implementar fácilmente certificados digitales. Pueden ser certificados SSL/TLS públicos y privados firmados por la entidad de certificación o un certificado autofirmado. Key Vault también puede solicitar y renovar certificados mediante asociaciones con entidades de certificación, lo que proporciona una solución sólida para la administración del ciclo de vida de los certificados. En este tutorial, actualizará los atributos del certificado: el período de validez, la frecuencia de rotación automática y la CA. Para más información sobre Key Vault, consulte esta [introducción](../general/overview.md).

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> * Administrar un certificado mediante Azure Portal
> * Agregar la cuenta del proveedor de entidades de certificación
> * Actualizar el período de validez del certificado
> * Actualizar la frecuencia de rotación automática del certificado
> * Actualizar los atributos del certificado mediante Azure PowerShell


Antes de empezar, lea los [conceptos básicos de Key Vault](../general/basic-concepts.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-vault"></a>Creación de un almacén

Cree o seleccione la instancia de Key Vault existente para realizar operaciones. [(Pasos para crear una instancia de Key Vault)](../quick-create-portal.md) En el ejemplo, el nombre del almacén es **Example-Vault**. 

![Salida tras completarse la creación de Key Vault](../media/certificates/tutorial-import-cert/vault-properties.png)

## <a name="create-a-certificate-in-key-vault"></a>Creación de un certificado en Key Vault

Cree o importe un certificado en el almacén. [(Pasos para crear un certificado en el almacén)](../quick-create-portal.md) En este caso, trabajamos en un certificado llamado **ExampleCertificate**.

> [!NOTE]
> En Azure Key Vault, los atributos del ciclo de vida de un certificado se pueden actualizar en el momento de la creación del certificado, así como después de haberse creado. 
## <a name="updating-certificates-life-cycle-attributes"></a>Actualización de los atributos del ciclo de vida del certificado

Un certificado creado en Key Vault puede ser: 
- un certificado autofirmado
- un certificado creado con una entidad de certificación (CA) asociada a Key Vault
- un certificado con una entidad de certificación que no esté asociada a Key Vault

Las siguientes entidades de certificación son actualmente proveedores asociados a Key Vault:
- DigiCert: Key Vault ofrece certificados TLS/SSL OV con DigiCert.
- GlobalSign: Key Vault ofrece certificados TLS/SSL OV con GlobalSign.

Azure Key Vault rota automáticamente los certificados mediante asociaciones con entidades de certificación. Mediante esa asociación establecida, Key Vault solicita y renueva automáticamente los certificados. Por lo tanto, la **funcionalidad de rotación automática no es aplicable a los certificados creados con entidades de certificación que no estén asociadas a Key Vault.** 

> [!NOTE]
> Un administrador de cuentas de un proveedor de entidades de certificación crea las credenciales que usará Key Vault para crear, renovar y usar certificados TLS/SSL mediante Key Vault.
![Entidad de certificación](../media/certificates/tutorial-rotate-cert/cert-authority-create.png)
> 


### <a name="updating-certificates-life-cycle-attributes-at-the-time-of-certificate-creation"></a>Actualización de los atributos del ciclo de vida del certificado en el momento de la creación del certificado

1. En las páginas de propiedades de Key Vault, seleccione **Certificados**.
2. Haga clic en **Generar o Importar**.
3. En la pantalla **Crear un certificado**, actualice los siguientes valores:
    

    - **Periodo de validez**: Escriba el valor (en meses). Una práctica de seguridad recomendada es la creación de certificados de corta duración. De forma predeterminada, el valor de validez de un certificado recién creado es de 12 meses.
    - **Tipo de acción de duración**: seleccione la acción de renovación automática y alerta del certificado. Según la selección, actualice el "porcentaje de duración" o el "número de días antes de que expire". De forma predeterminada, la renovación automática de un certificado se establece en el 80 % de su duración.<br> En el menú desplegable, seleccione la opción:

    |  Automatically renew at a given time (Renovar automáticamente en un momento dado)| Email all contacts at a given time (Enviar por correo electrónico todos los contactos en un momento dado) |
    |-----------|------|
    |Al seleccionar esta opción, se ACTIVA la rotación automática. | Al seleccionar esta opción, NO se realizará la rotación automática, solo se alertará a los contactos.|
        


4. Haga clic en **Crear**.

![Ciclo de vida del certificado](../media/certificates/tutorial-rotate-cert/create-cert-lifecycle.png)

### <a name="updating-life-cycle-attributes-of-stored-certificate"></a>Actualización de los atributos del ciclo de vida del certificado almacenado

1. Seleccione la instancia de Key Vault.
2. En las páginas de propiedades de Key Vault, seleccione **Certificados**.
3. Seleccione el certificado que quiere actualizar. En este caso, trabajamos en un certificado llamado **ExampleCertificate**.
4. Seleccione **Directiva de emisión** en la barra de menús superior.

![Propiedades del certificado](../media/certificates/tutorial-rotate-cert/cert-issuance-policy.png)
5. En la pantalla **Directiva de emisión**, actualice los valores siguientes:
- **Periodo de validez**: actualice el valor (en meses).
- **Tipo de acción de duración**: seleccione la acción de renovación automática y alerta del certificado. Según la selección, actualice el "porcentaje de duración" o el "número de días antes de que expire". 

![Propiedades del certificado](../media/certificates/tutorial-rotate-cert/cert-policy-change.png)
6. Haga clic en **Guardar**.

> [!IMPORTANT]
> Al cambiar el tipo de acción de duración de un certificado, se registrarán inmediatamente las modificaciones de los certificados existentes.


### <a name="updating-certificates-attributes-using-powershell"></a>Actualización de los atributos del certificado mediante PowerShell

```azurepowershell


Set-AzureKeyVaultCertificatePolicy -VaultName $vaultName 
                                   -Name $certificateName 
                                   -RenewAtNumberOfDaysBeforeExpiry [276 or appropriate calculated value]
```

> [!TIP]
> Para modificar la directiva de renovación de una lista de certificados, especifique el archivo File.csv que contiene VaultName,CertName <br/>
>  vault1,Cert1 <br/>
>  vault2,Cert2
>
>  ```azurepowershell
>  $file = Import-CSV C:\Users\myfolder\ReadCSVUsingPowershell\File.csv 
> foreach($line in $file)
> {
> Set-AzureKeyVaultCertificatePolicy -VaultName $vaultName -Name $certificateName -RenewAtNumberOfDaysBeforeExpiry [276 or appropriate calculated value]
> }
>  ```
> 
Más información sobre los parámetros [aquí](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-set-attributes)

## <a name="clean-up-resources"></a>Limpieza de recursos

Otras guías de inicio rápido y tutoriales de Key Vault se basan en esta. Si tiene pensado seguir trabajando en otras guías de inicio rápido y tutoriales, considere la posibilidad de dejar estos recursos activos.
Cuando ya no lo necesite, elimine el grupo de recursos; de este modo se eliminarán también Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en esta guía de inicio rápido en los resultados de búsqueda, selecciónelo.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** escriba el nombre del grupo de recursos y seleccione **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha actualizado el ciclo de vida de un certificado. Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, continúe con los artículos siguientes.

Más información sobre la [administración de la creación de certificados en Azure Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-scenarios).
- Revise [Introducción a Key Vault](../general/overview.md).