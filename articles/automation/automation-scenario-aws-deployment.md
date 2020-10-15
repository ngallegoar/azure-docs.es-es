---
title: Implementación de una máquina virtual de Amazon Web Services con un runbook de Azure Automation
description: En este artículo se explica cómo automatizar la creación de una máquina virtual de Amazon Web Services.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 456a7e4f07b2416e1d2037205574f2e7149e70e2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86185949"
---
# <a name="deploy-an-amazon-web-services-vm-with-a-runbook"></a>Implementación de una máquina virtual de Amazon Web Services con un runbook

En este artículo aprenderá cómo se puede aprovechar Azure Automation para aprovisionar una máquina virtual en su suscripción de Amazon Web Services (AWS) y asignar a dicha máquina virtual un nombre específico, lo que AWS denomina "etiquetar" la máquina virtual.

## <a name="prerequisites"></a>Prerrequisitos

Debe tener una cuenta de Azure Automation y una suscripción de Amazon Web Services (AWS). Para más información sobre cómo configurar una cuenta de Azure Automation con sus credenciales de la suscripción de AWS, consulte [Configuración de la autenticación con Amazon Web Services](automation-config-aws-account.md). Antes de continuar, debe crear o actualizar esta cuenta con las credenciales de suscripción de AWS, ya que en las secciones siguientes haremos referencia a esta cuenta.

## <a name="deploy-amazon-web-services-powershell-module"></a>Implementar un módulo Amazon Web Services de PowerShell

El runbook de aprovisionamiento de la VM usa el módulo AWS de PowerShell para realizar su trabajo. Utilice los pasos siguientes para agregar el módulo a la cuenta de Automation que esté configurada con sus credenciales de suscripción de AWS.  

1. Abra el explorador web, navegue a la [Galería de PowerShell](https://www.powershellgallery.com/packages/AWSPowerShell/) y haga clic en el **botón Deploy to Azure Automation** (Implementar en Azure Automation).<br><br> ![Importación del módulo AWS de PS](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Aparecerá la página de inicio de sesión de Azure y, una vez que se autentique, se le dirigirá a Azure Portal y se mostrará la página siguiente:<br><br> ![Página de importación de módulo](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Seleccione la cuenta de Automation que usar y haga clic en **Aceptar** para iniciar la implementación.

   > [!NOTE]
   > Cuando Azure Automation importa un módulo de PowerShell, extrae los cmdlets. Las actividades no aparecen hasta que Automation haya terminado de importar el módulo y extraer los cmdlets. Este proceso puede tardar unos minutos.  
   > <br>

1. En Azure Portal, abra su cuenta de Automation.
2. Haga clic en el mosaico **Recursos** y, en el panel Recursos, seleccione el icono **Módulos**.
3. En la página Módulos, verá el módulo **AWSPowerShell** en la lista.

## <a name="create-aws-deploy-vm-runbook"></a>Implementar AWS y crear runbook de VM

Una vez que haya implementado el módulo AWS de PowerShell, puede crear un runbook para automatizar el aprovisionamiento de una máquina virtual en AWS mediante un script de PowerShell. Los pasos siguientes muestran cómo usar un script nativo de PowerShell en Azure Automation.  

> [!NOTE]
> Para obtener más información y conocer las opciones de este script, visite la [Galería de PowerShell](https://www.powershellgallery.com/packages/New-AwsVM/).
> 

1. Descargue el script de New-AwsVM de PowerShell de la Galería de PowerShell; para ello, abra una sesión de PowerShell y escriba el comando siguiente:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. En Azure Portal, abra la cuenta de Automation y seleccione **Runbooks** en **Automatización de procesos**.  
3. En la página Runbooks, seleccione **Agregar un Runbook**.
4. En el panel Agregar un Runbook, seleccione **Creación rápida** para crear un nuevo runbook.
5. En el panel Propiedades de libro de ejecuciones, escriba un nombre para el runbook.
6. En la lista desplegable **Tipo de Runbook**, seleccione **PowerShell** y, a continuación, haga clic en **Crear**.<br><br> ![Panel de creación de runbooks](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
7. Cuando aparezca la página Editar Runbook de PowerShell, copie y pegue el script de PowerShell en el lienzo de creación de runbooks.<br><br> ![Script de PowerShell de Runbook](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    Tenga en cuenta lo siguiente al trabajar con el script de PowerShell de ejemplo:

    * El Runbook contiene una serie de valores de parámetro predeterminados. Evalúe todos los valores predeterminados y actualícelos cuando sea necesario.
    * Si ha almacenado las credenciales de AWS como un recurso de credencial con un nombre diferente de `AWScred`, debe actualizar el script en la línea 57 como corresponda.  
    * Cuando trabaje con los comandos de la CLI de AWS en PowerShell, especialmente con este Runbook de ejemplo, debe especificar la región de AWS. En caso contrario, se producirá un error en los cmdlets. Consulte el tema de AWS [Specify AWS Region](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) (Especificación de la región de AWS) en las herramientas de AWS para documentos de PowerShell para más información.  

8. Para recuperar una lista de nombres de imagen de su suscripción de AWS, inicie PowerShell ISE e importe el módulo AWS de PowerShell. Para autenticar en AWS, reemplace `Get-AutomationPSCredential` en el entorno de ISE con `AWScred = Get-Credential`. Esta instrucción solicita las credenciales. Puede proporcionar el identificador de la clave de acceso como nombre de usuario y la clave de acceso secreta como contraseña. 

      ```powershell
        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile
      ```
        
    Se devuelve el siguiente resultado:<br><br>
   ![Obtener imágenes AWS](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
9. Copie y pegue el nombre de una de las imágenes en una variable de Automation que se indica en el runbook como `$InstanceType`. Dado que en este ejemplo utiliza la suscripción gratuita en capas de AWS, usará **t2.micro** en el ejemplo de runbook.  
10. Guarde el runbook, haga clic en **Publish** (Publicar) para publicarlo y, después, en **Yes** (Sí), cuando se le solicite.

### <a name="test-the-aws-vm-runbook"></a>Prueba del runbook de la máquina virtual de AWS

1. Compruebe que el runbook crea un recurso denominado `AWScred` para su autenticación en AWS, o bien actualice el script para que haga referencia al nombre del recurso de credenciales.    
2. Compruebe el nuevo runbook y asegúrese de que se han actualizado todos los valores de parámetro necesarios.
El módulo AWS de PowerShell se ha importado a Azure Automation.  
3. En Azure Automation, establezca **Registrar registros detallados** y, opcionalmente, **Registrar registros de progreso** de la operación **Registro y seguimiento** del runbook en **Activado**.<br><br> ![Seguimiento y registro de Runbook](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png).  
4. Para iniciar el runbook, haga clic en **Iniciar** y, después, en **Aceptar** cuando se abra el panel Iniciar runbook.
5. En el panel Iniciar runbook, proporcione un nombre de VM. Acepte los valores predeterminados de los demás parámetros que configuró previamente en el script. Haga clic en **Aceptar** para iniciar el trabajo del runbook.<br><br> ![Iniciar nuevo runbook AWS VM](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
6. Se abre un panel Trabajo para el trabajo de runbook que acaba de crear. Cierre este panel.
7. Para ver el progreso del trabajo y las transmisiones de salida, seleccione **Todos los registros** en el panel de trabajo de runbook.<br><br> ![Salida de transmisiones](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
8. Para confirmar que la VM se está aprovisionando, inicie sesión en la Consola de administración de AWS si todavía no ha iniciado sesión.<br><br> ![Máquinas virtual implementada en consola AWS](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Pasos siguientes
 
* Para averiguar qué runbooks se admiten, consulte [Tipos de runbooks de Azure Automation](automation-runbook-types.md).
* Para trabajar con runbooks, consulte [Administración de runbooks en Azure Automation](manage-runbooks.md).
* Para más información sobre PowerShell, consulte [PowerShell Docs](/powershell/scripting/overview).
* Con respecto a la compatibilidad con scripts, consulte [Compatibilidad nativa con scripts de PowerShell en Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](/powershell/module/az.automation/?view=azps-3.7.0#automation).
