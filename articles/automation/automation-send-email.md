---
title: Envío de un correo electrónico desde un runbook de Azure Automation
description: Aprenda a usar SendGrid para enviar un correo electrónico desde un runbook.
services: automation
ms.subservice: process-automation
ms.date: 07/15/2019
ms.topic: tutorial
ms.openlocfilehash: d4b35458c76da82b33dfcb530cfdc71ee3da3bb6
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604783"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Tutorial: Envío de un correo electrónico desde un runbook de Azure Automation

Puede enviar un correo electrónico desde un runbook con [SendGrid](https://sendgrid.com/solutions) mediante PowerShell. En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Cree un almacén de Azure Key Vault.
> * Almacene la clave de API `SendGrid` en el almacén de claves.
> * Cree un runbook reutilizable que recupere la clave de API y envíe un correo electrónico con una clave de API almacenada en [Azure Key Vault](/azure/key-vault/).

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="prerequisites"></a>Prerrequisitos

Los siguientes requisitos son necesarios para completar este tutorial:

* Suscripción de Azure: Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Crear una cuenta de SendGrid](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account).
* Una [cuenta de Automation](automation-offering-get-started.md) con módulos **Az** y una [conexión de ejecución](automation-create-runas-account.md) para almacenar y ejecutar el runbook.

## <a name="create-an-azure-key-vault"></a>Crear una instancia de Azure Key Vault

Puede crear una instancia de Azure Key Vault mediante el siguiente script de PowerShell. Reemplace los valores variables por valores específicos de su entorno. Use Azure Cloud Shell integrado a través del botón **Pruébelo**, situado en la esquina superior derecha del bloque de código. También puede copiar y ejecutar el código localmente si tiene el [módulo Azure PowerShell](/powershell/azure/install-az-ps) instalado en la máquina local.

> [!NOTE]
> Para recuperar la clave de API, siga los pasos para [encontrar la clave de API de SendGrid](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key).

```azurepowershell-interactive
$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
# If you omit the SubscriptionId parameter, the default subscription is selected.
Connect-AzAccount -SubscriptionId $SubscriptionId

# Use Get-AzLocation to see your available locations.
$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "<Enter a universally unique vault name>"
$SendGridAPIKey = "<SendGrid API key>"
$AutomationAccountName = "testaa"

# Create new Resource Group, or omit this step if you already have a resource group.
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region
$resourceId = $newKeyVault.ResourceId

# Convert the SendGrid API key into a SecureString
$Secret = ConvertTo-SecureString -String $SendGridAPIKey -AsPlainText -Force
Set-AzKeyVaultSecret -VaultName $VaultName -Name 'SendGridAPIKey' -SecretValue $Secret

# Grant access to the Key Vault to the Automation Run As account.
$connection = Get-AzAutomationConnection -ResourceGroupName $KeyVaultResourceGroupName -AutomationAccountName $AutomationAccountName -Name AzureRunAsConnection
$appID = $connection.FieldDefinitionValues.ApplicationId
Set-AzKeyVaultAccessPolicy -VaultName $VaultName -ServicePrincipalName $appID -PermissionsToSecrets Set, Get
```

Para ver otras formas de crear un almacén de Azure Key Vault y almacenar un secreto, consulte los [inicios rápidos de Key Vault](/azure/key-vault/).

## <a name="import-required-modules-to-your-automation-account"></a>Importación de los módulos necesarios a la cuenta de Automation

Para usar Azure Key Vault dentro de un runbook, la cuenta de Automation necesitará los siguientes módulos:

* [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile)
* [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault)

Haga clic en **Implementar en Azure Automation** en la pestaña Azure Automation de **Opciones de instalación**. Esta acción abre Azure Portal. En la página Importar, seleccione su cuenta de Automation y haga clic en **Aceptar**.

Para obtener métodos adicionales de agregar los módulos necesarios, consulte [Importación de módulos](/azure/automation/shared-resources/modules#importing-modules).

## <a name="create-the-runbook-to-send-an-email"></a>Creación del runbook para enviar un correo electrónico

Después de crear un almacén de claves y de almacenar la clave de API de `SendGrid`, es el momento de crear el runbook que recupere la clave de API y envíe un correo electrónico.

Este runbook usa `AzureRunAsConnection` como una [cuenta de ejecución](automation-create-runas-account.md) para autenticarse en Azure con el fin de recuperar el secreto de Azure Key Vault.

Use este ejemplo para crear un runbook llamado **Send-GridMailMessage**. Puede modificar el script de PowerShell y reutilizarlo para distintos escenarios.

1. Vaya a la cuenta de Azure Automation.
2. En **Automatización de procesos**, seleccione **Runbooks**.
3. En la parte superior de la lista de runbooks, seleccione **+ Crear un runbook**.
4. En la página **Agregar Runbook**, escriba **Send-GridMailMessage** como nombre del runbook. Como tipo de runbook, seleccione **PowerShell**. Seleccione **Crear**.
   ![Creación del runbook](./media/automation-send-email/automation-send-email-runbook.png)
5. Se crea el runbook y se abre la página **Editar Runbook de PowerShell**.
   ![Edición del runbook](./media/automation-send-email/automation-send-email-edit.png)
6. Copie el siguiente ejemplo de PowerShell en la página **Editar**. Asegúrese de que `$VaultName` es el nombre que especificó al crear la instancia del almacén de claves.

    ```powershell-interactive
    Param(
      [Parameter(Mandatory=$True)]
      [String] $destEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $fromEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $subject,
      [Parameter(Mandatory=$True)]
      [String] $content
    )

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Out-Null
    $VaultName = "<Enter your vault name>"
    $SENDGRID_API_KEY = (Get-AzKeyVaultSecret -VaultName $VaultName -Name "SendGridAPIKey").SecretValueText
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Authorization", "Bearer " + $SENDGRID_API_KEY)
    $headers.Add("Content-Type", "application/json")

    $body = @{
    personalizations = @(
        @{
            to = @(
                    @{
                        email = $destEmailAddress
                    }
            )
        }
    )
    from = @{
        email = $fromEmailAddress
    }
    subject = $subject
    content = @(
        @{
            type = "text/plain"
            value = $content
        }
    )
    }

    $bodyJson = $body | ConvertTo-Json -Depth 4

    $response = Invoke-RestMethod -Uri https://api.sendgrid.com/v3/mail/send -Method Post -Headers $headers -Body $bodyJson
    ```

7. Seleccione **Publicar** para guardar y publicar el runbook.

Para comprobar que el runbook se ejecuta correctamente, puede seguir los pasos descritos en [Prueba de un runbook](manage-runbooks.md#testing-a-runbook) o de [Inicio de un runbook](start-runbooks.md).
Si no ve inicialmente el correo electrónico de prueba, compruebe las carpetas de **correo no deseado** y de **spam**.

## <a name="clean-up"></a>Limpieza

Cuando ya no necesite el runbook, elimínelo. Para ello, seleccione el runbook en la lista de runbooks y haga clic en **Eliminar**.

Elimine el almacén de claves mediante el cmdlet [Remove-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/remove-azkeyvault?view=azps-3.7.0).

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre problemas al crear o iniciar el runbook, consulte [Solución de problemas relativos a errores con runbooks](./troubleshoot/runbooks.md).
* Para actualizar los módulos de la cuenta de Automation, consulte [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).
* Para supervisar la ejecución del runbook, consulte [Reenvío del estado de un trabajo y de transmisiones de trabajos de Automation a registros de Azure Monitor](automation-manage-send-joblogs-log-analytics.md).
* Para desencadenar un runbook mediante una alerta, consulte [Uso de una alerta para desencadenar un runbook de Azure Automation](automation-create-alert-triggered-runbook.md).
