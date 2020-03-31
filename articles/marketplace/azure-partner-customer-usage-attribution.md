---
title: Atribución de uso de los clientes y asociados de Azure | Azure Marketplace
description: Información general sobre cómo hacer seguimiento del uso del cliente de las soluciones de Azure Marketplace
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 9/23/2019
ms.author: dsindona
ms.openlocfilehash: 348633ffc91bc25a226b05743a18d2c87533a01d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280650"
---
# <a name="azure-partner-customer-usage-attribution"></a>Atribución de uso del cliente para asociados de Azure

Como asociado de software de Azure, sus soluciones requieren componentes de Azure o necesitan implementarse directamente en la infraestructura de Azure. A los clientes que implementen una solución de asociado y aprovisionen sus propios recursos de Azure puede que les resulte difícil ver el estado de la implementación y lo que esta afecta al crecimiento de Azure. Al agregar un mayor nivel de visibilidad, se alinea con el equipo de ventas de Microsoft y obtiene crédito para los programas para partners de Microsoft.

Ahora, Microsoft ofrece un método que ayuda a los asociados a mejorar el seguimiento del uso de Azure de las implementaciones de clientes de su software en Azure. Este nuevo método usa Azure Resource Manager para organizar la implementación de los servicios de Azure.

Como asociado de Microsoft, puede asociar el uso de Azure con todos los recursos de Azure que aprovisione en nombre de un cliente. La asociación puede formarla a través de Azure Marketplace, el repositorio del Inicio rápido, repositorios privados de GitHub y la involucración del cliente cara a cara. La atribución de uso del cliente admite tres opciones de implementación:

- Plantillas de Azure Resource Manager: los asociados pueden usar plantillas de Resource Manager para implementar los servicios de Azure para ejecutar su software. Los asociados pueden crear una plantilla de Resource Manager para definir la infraestructura y configuración de su solución de Azure. Una plantilla de Resource Manager les permite tanto a usted como a sus clientes implementar la solución a lo largo de su ciclo de vida. Puede estar seguro de que los recursos se implementan de forma consistente.
- API de Azure Resource Manager: los asociados pueden llamar a las API de Resource Manager directamente para implementar una plantilla de Azure Resource Manager o para generar las llamadas a API para aprovisionar directamente los servicios de Azure.
- Terraform: los asociados pueden usar orquestador en la nube, como Terraform, para implementar una plantilla de Resource Manager o servicios de Azure directamente.

La atribución de uso del cliente es para implementaciones nuevas y NO admite el etiquetado de recursos existentes que ya se hayan implementado.

Se requiere la atribución de uso del cliente en [Aplicación de Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer): la oferta de plantilla de solución está publicada en Azure Marketplace.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-resource-manager-templates"></a>Uso de plantillas de Resource Manager
Muchas soluciones de los partners se implementan en una suscripción de cliente mediante plantillas de Resource Manager. Si tiene una plantilla de Resource Manager disponible en Azure Marketplace, en GitHub o como Inicio rápido, el proceso de modificación de esta para habilitar la atribución de uso del cliente debería ser sencillo.

Para más información sobre la creación y publicación de plantillas de solución, consulte

* [Creación e implementación de plantillas de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).
* [Oferta de aplicación de Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer).
* Vídeo: [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Creación de plantillas de soluciones y aplicaciones administradas para Azure Marketplace).


## <a name="add-a-guid-to-your-template"></a>Adición de un identificador único global a una plantilla

Para agregar un identificador único global (GUID), se realizas una modificación única en el archivo de plantilla principal:

1. [Cree un GUID](#create-guids) mediante el método sugerido y [registre el GUID](#register-guids-and-offers).

1. Abra la plantilla de Resource Manager.

1. Agregue un nuevo recurso en el archivo de plantilla principal. El recurso solo debe estar en los archivos **mainTemplate.json** o **azuredeploy.json**, no en ninguna de las plantillas vinculadas o anidadas.

1. Escriba el valor de GUID después del prefijo **pid -** (por ejemplo, pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. Compruebe si la plantilla tiene errores.

1. Vuelva a publicar la plantilla en los repositorios adecuados.

1. [Compruebe que el GUID es correcto en la implementación de plantillas](#verify-the-guid-deployment).

### <a name="sample-resource-manager-template-code"></a>Código de ejemplo de plantilla de Resource Manager

Para habilitar el seguimiento de recursos de la plantilla, deberá agregar los siguientes recursos adicionales en la sección de recursos. Asegúrese de modificar el siguiente código de ejemplo con sus propias entradas cuando lo agregue al archivo de plantilla principal.
El recurso solo se debe agregar al archivo **mainTemplate.json** o **azuredeploy.json**, y no a plantillas vinculadas o anidadas.

```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the resources section in the mainTemplate.json (do not add the entire file)
    "apiVersion": "2018-02-01",
    "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your generated GUID here
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "template": {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": []
        }
    }
} // remove all comments from the file when complete
```

## <a name="use-the-resource-manager-apis"></a>Uso de las API de Resource Manager

En algunos casos, es posible que prefiera realizar llamadas directamente a las API REST de Resource Manager para implementar los servicios de Azure. [Azure admite varios SDK](https://docs.microsoft.com/azure/?pivot=sdkstools) para habilitar estas llamadas. Puede usar uno de los SDK, o llamar a las API REST directamente para implementar los recursos.

Si usa una plantilla de Resource Manager, para etiquetar la solución debe seguir las instrucciones que se han proporcionado anteriormente. Si no usa una plantilla de Resource Manager y realiza llamadas API directas, puede etiquetar la implementación para asociar el uso de los recursos de Azure.

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Etiquetado de una implementación con las API de Resource Manager

Para habilitar la atribución de uso del cliente, al diseñar las llamadas API, se incluye un identificador único global en el encabezado del agente de usuario de la solicitud. Agregue el identificador único global a cada oferta o SKU. Dé formato a la cadena con el prefijo **pid -** e incluya el identificador único global generado por el asociado. Este es un ejemplo del formato del identificador único global para la inserción en el agente de usuario:

![Formato de GUID de ejemplo](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> El formato de la cadena es importante. Si no se incluye el prefijo **pid-** , no es posible consultar los datos. El seguimiento de los distintos SDK se hace de forma diferente. Para implementar este método, revise la compatibilidad y el enfoque del seguimiento de su SDK de Azure preferido.

#### <a name="example-the-python-sdk"></a>Ejemplo: SDK de Python

Para Python, use el atributo **config**. El atributo solo se puede agregar a un UserAgent. Este es un ejemplo:

![Agregar el atributo a un agente de usuario](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> Agregue el atributo a cada cliente. No hay ninguna configuración estática global. Puede etiquetar una fábrica de cliente para asegurarse de que todos los clientes realizan el seguimiento. Para más información, consulte este [ejemplo de fábrica de cliente en GitHub](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79).

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Etiquetado de una implementación mediante Azure PowerShell

Si implementa recursos a través de Azure PowerShell, use el siguiente método para anexar el identificador único global:

```powershell
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Etiquetado de una implementación mediante la CLI de Azure

Cuando use la CLI de Azure para anexar un identificador único global, establezca la variable de entorno **AZURE_HTTP_USER_AGENT**. Dicha variable se puede establecer en el ámbito de un script. Pero también se puede establecer globalmente para el ámbito de una shell:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```
Para más información, consulte [Azure SDK para Go](https://docs.microsoft.com/azure/go/).

## <a name="use-terraform"></a>Uso de Terraform

La compatibilidad con Terraform está disponible a partir de la versión 1.21.0 del proveedor de Azure: [https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019](https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019).  Esta compatibilidad se aplica a todos los asociados que implementen su solución mediante Terraform y a todos los recursos que implemente y mida el proveedor de Azure (versión 1.21.0 o posteriores).

El proveedor de Azure para Terraform agrega un nuevo campo opcional denominado [*partner_id*](https://www.terraform.io/docs/providers/azurerm/#partner_id) que es donde se especifica el identificador único global de seguimiento que usa para la solución. El valor de este campo también puede proceder de la variable de entorno *ARM_PARTNER_ID*.

```
provider "azurerm" {
          subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          ……
          # new stuff for ISV attribution
          partner_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"}
```
Los asociados que deseen que la implementación mediante Terraform tenga un seguimiento de la atribución de uso del cliente deben hacer lo siguiente:

* Crear un identificador único global (que se debe agregar para cada oferta o SKU).
* Actualizar el proveedor de Azure para establecer el valor de *partner_id* con el identificador único global (NO añadir el prefijo "pid-" al identificador único global, simplemente establecerlo con el valor real).

## <a name="create-guids"></a>Creación de los identificadores únicos globales

Un GUID es un número de referencia único que tiene 32 dígitos hexadecimales. Para crear identificadores únicos globales para el seguimiento, debe usar un generador de GUID. El equipo de Azure Storage ha creado un [formulario de generación de GUID](https://aka.ms/StoragePartners) que enviará por correo electrónico un GUID en el formato correcto. Este GUID se puede reutilizar en los distintos sistemas de seguimiento.

> [!Note]
> Es muy recomendable usar el [formulario del generador de GUID de Azure Storage](https://aka.ms/StoragePartners) para crear el identificador único global. Para más información, consulte las [P+F](#faq).

Se recomienda crear un GUID único para todos los canales de distribución y oferta de cada producto. Puede optar por usar un solo GUID para los múltiples canales de distribución del producto si no quiere que los informes se dividan.

Si implementa un producto mediante una plantilla y está disponible tanto en Azure Marketplace como en GitHub, puede crear y registrar 2 GUID distintos:

*   Producto A en Azure Marketplace
*   Producto A en GitHub

La creación de informes se realiza en función del valor de asociado comercial (id. de Partner de Microsoft) y de los GUID.

Asimismo, si lo desea también puede realizar un seguimiento de los identificadores únicos globales a un nivel más detallado como el SKU donde las SKU son variantes de una oferta.

## <a name="register-guids-and-offers"></a>Registro de identificadores únicos globales y ofertas

El identificador único global debe estar registrado para permitir la atribución de uso del cliente.

Todos los registros de los GUID de plantilla se realizan en el Centro de partners.

Después de agregar el GUID a la plantilla o en el agente de usuario, y registrarlo en el Centro de partners, se realizará un seguimiento de todas las implementaciones.

1. Regístrese como [publicador comercial de Marketplace](https://aka.ms/JoinMarketplace).

   * Los partners deben [tener un perfil en el Centro de partners](https://docs.microsoft.com/azure/marketplace/become-publisher). Le recomendamos que vea la oferta en Azure Marketplace o AppSource.
   * Los asociados pueden registrar varios identificadores únicos globales.
   * Los asociados puede registrar un identificador único global tanto para las plantillas de solución que no sean de Marketplace como para las ofertas.

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).

1. En la esquina superior derecha, seleccione el icono de engranaje de la configuración y, a continuación, **Configuración de desarrollador**.

1. En la **página de configuración de la cuenta**, seleccione **Add Tracking GUID** (Agregar GUID de seguimiento).

1. En el cuadro **GUID**, escriba su identificador único global de seguimiento. Escriba solo el GUID, sin el prefijo **pid -** . En el cuadro **Descripción**, escriba el nombre o la descripción de la oferta.

1. Para registrar varios identificadores únicos globales, vuelva a seleccionar **Add Tracking GUID** (Agregar GUID de seguimiento). Aparecen más cuadros en la página.

1. Seleccione **Guardar**.


## <a name="verify-the-guid-deployment"></a>Comprobación de la implementación de GUID

Tras modificar la plantilla y realizar una implementación de prueba, use el siguiente script de PowerShell para recuperar los recursos que implementó y etiquetó.

Puede usar dicho script para comprobar que el GUID se ha agregado correctamente a la plantilla de Resource Manager. El script no se aplica a la implementación de la API de Resource Manager ni Terraform.

Inicie sesión en Azure. Antes de ejecutar el script seleccione la suscripción con la implementación que desea comprobar. Ejecute el script en el contexto de la suscripción de la implementación.

Tanto el **GUID** como el nombre **resourceGroup** son parámetros necesarios.

[El script original](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) se puede obtener en GitHub.

```powershell
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzResourceGroupDeployment -ResourceGroupName
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="report"></a>Informe

Puede encontrar el informe de atribución de uso del cliente en el panel de análisis del Centro de asociados ([https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure](https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure)). Para ver el informe, tendrá que iniciar sesión con las credenciales del Centro de asociados. Si tiene problemas con los informes o el inicio de sesión, cree una solicitud de soporte técnico siguiendo las instrucciones de la sección de soporte técnico de Get.

Elija Tracked Templates (Plantillas en seguimiento) en la lista desplegable de Partner Association Type (Tipo de asociación del asociado) para ver el informe.

![Informe de la atribución de uso del cliente](media/marketplace-publishers-guide/customer-usage-attribution-report.png)

## <a name="notify-your-customers"></a>Notificación a los clientes

Los asociados deben informar a sus clientes de las implementaciones que usan atribución de uso del cliente. Microsoft notifica al asociado el uso de Azure que está asociado con estas implementaciones. Los ejemplos siguientes incluyen contenido que puede usar para notificar a los clientes estas implementaciones. En los ejemplos, reemplace \<PARTNER > por el nombre de su empresa. Los asociados deben asegurarse de que la notificación es compatible con sus directivas de privacidad y recopilación de datos, incluyendo las opciones que permiten excluir a los clientes del seguimiento.

### <a name="notification-for-resource-manager-template-deployments"></a>Notificación de implementaciones de plantillas de Resource Manager

Al implementar esta plantilla, Microsoft puede identificar la instalación del software de \<PARTNER> con los recursos de Azure que se implementan. Microsoft puede correlacionar los recursos de Azure que se usan para admitir el software. Microsoft recopila esta información para proporcionar las mejores experiencias con sus productos y conseguir que sus negocios funcionen. Los datos se recopilan en función de las directivas de privacidad de Microsoft, que se encuentran en https://www.microsoft.com/trustcenter, y se rigen por ellas.

### <a name="notification-for-sdk-or-api-deployments"></a>Notificación para las implementaciones de SDK o API

Al implementar el software \<PARTNER>, Microsoft puede identificar la instalación del software de \<PARTNER> con los recursos de Azure que se implementan. Microsoft puede correlacionar los recursos de Azure que se usan para admitir el software. Microsoft recopila esta información para proporcionar las mejores experiencias con sus productos y conseguir que sus negocios funcionen. Los datos se recopilan en función de las directivas de privacidad de Microsoft, que se encuentran en https://www.microsoft.com/trustcenter, y se rigen por ellas.

## <a name="get-support"></a>Obtención de soporte técnico

Existen dos canales de soporte técnico según los problemas que se encuentre.

Si tiene algún problema en el Centro de asociados, como consultar un informe de atribución de uso del cliente o iniciar sesión, cree una solicitud de soporte técnico para el equipo de soporte técnico del Centro de asociados aquí: [https://partner.microsoft.com/support](https://partner.microsoft.com/support)

![](./media/marketplace-publishers-guide/partner-center-log-in-support.png)

Si necesita ayuda para la atribución de uso de del cliente o la incorporación a Marketplace, por ejemplo, cómo configurar la primera, siga estos pasos:

1. Vaya a la [página de soporte técnico](https://go.microsoft.com/fwlink/?linkid=844975).

1. En **Problem type** (Tipo de problema), seleccione **Marketplace Onboarding** (Incorporación a Marketplace).

1. En **Category** (Categoría), elija la categoría del problema:

   - Para problemas de asociación de uso, seleccione **Other** (Otros).
   - Para problemas de acceso al CPP de Azure Marketplace, seleccione **Access Problem** (Problema de acceso).

     ![Elegir la categoría de problema](media/marketplace-publishers-guide/lu-article-incident.png)

1. Haga clic en **Start Request** (Iniciar solicitud).

1. En la página siguiente, especifique los valores necesarios. Seleccione **Continuar**.

1. En la página siguiente, especifique los valores necesarios.

   > [!Important]
   > En el cuadro **Incident title** (Título de la incidencia), escriba **Seguimiento del uso de ISV**. Describa el problema de forma detallada.

   ![Escribir Seguimiento del uso de ISV como título de la incidencia](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Complete el formulario y seleccione **Submit** (Enviar).

También puede recibir orientación técnica de un asesor técnico para asociados de Microsoft sobre escenarios técnicos antes de la venta, escenarios de implementación y escenarios de desarrollo de aplicaciones para entender e incorporar la atribución de uso del cliente.

### <a name="how-to-submit-a-technical-consultation-request"></a>Envío de una solicitud de consulta técnica

1. Visite [https://aka.ms/TechnicalJourney](https://aka.ms/TechnicalJourney).
1. Seleccione Cloud infrastructure and management (Infraestructura en la nube y administración) y una nueva página se abrirá para que pueda ver el trayecto técnico.
1. En Deployment Services (Servicios de implementación), haga clic en el botón Submit a request (Enviar una solicitud).
1. Inicie sesión con su cuenta de servicio administrada (cuenta de MPN) o de AAD (cuenta de panel del asociado); se abrirá un formulario de solicitud en línea en función de sus credenciales de inicio de sesión:
    * Complete/revise la información de contacto.
    * Los detalles de la consulta pueden haberse rellenado previamente o quizá deba seleccionar de las listas desplegables.
    * Escriba un título y la descripción del problema (proporcione el máximo de detalle).
1. Haga clic en Enviar.

Consulte las instrucciones paso a paso con capturas de pantalla en [https://aka.ms/TechConsultInstructions](https://aka.ms/TechConsultInstructions).

### <a name="whats-next"></a>Pasos siguientes

Un asesor técnico para los asociados de Microsoft le contactará para programar una llamada y determinar sus necesidades.

## <a name="faq"></a>Preguntas más frecuentes

**¿Cuál es la ventaja de agregar el identificador único global a la plantilla?**

Microsoft proporciona a sus asociados una vista de las implementaciones por parte de los clientes de sus soluciones, así como información detallada sobre su uso de influencia. Tanto Microsoft como el asociado pueden usar dicha información para impulsar una participación más estrecha entre los equipos de ventas. Tanto Microsoft como el asociado pueden usar los datos para obtener una vista más coherente del impacto que un asociado individual tiene en el crecimiento de Azure.

**Una vez que se ha agregado un identificador único global, ¿se puede cambiar?**

Sí, cualquier cliente o asociado de implementación puede personalizar la plantilla y cambiar o quitar el identificador único global. Se recomienda que los asociados describan de forma proactiva la función de los recursos y el identificador único global a sus clientes y asociados, para evitar la eliminación o modificación de este. El cambio del identificador único global solo afecta a las implementaciones y a los recursos nuevos, no a los existentes.

**¿Puedo realizar un seguimiento de las plantillas implementadas desde un repositorio que no sea de Microsoft, como GitHub?**

Sí, si el GUID está presente cuando se implemente la plantilla, se realiza el seguimiento. Los asociados deben tener un perfil en CPP para registrar los identificadores únicos globales que se usen para la implementación fuera de Azure Marketplace.

**¿El cliente recibe también un informe?**

Los clientes pueden realizar el seguimiento del uso tanto de recursos individuales como de grupos de recursos definidos por los usuarios en Azure Portal.

**¿Es esta metodología similar al asociado digital de registro (DPOR)?**

Este nuevo método de conexión de la implementación y el uso en la solución de un asociado proporciona un mecanismo que permite vincular una solución de asociado al uso de Azure. DPOR está pensado para asociar un asociado de consultoría (integrador de sistemas) o de administración (proveedor de servicios administrados) con la suscripción de Azure de un cliente.

**¿Cuál es la ventaja de usar el formulario del generador de GUID de Azure Storage?**

El formulario del generador de GUID de Azure Storage garantiza la generación de un GUID del formato necesario. Además, si usa alguno de los métodos de seguimiento de planos de datos de Azure Storage, puede aprovechar el mismo GUID para el seguimiento de planos de control de Marketplace. Esto le permite aprovechar un único GUID unificado para la atribución de asociado sin tener que mantener distintos GUID.

**¿Puedo usar un disco duro virtual privado personalizado para una oferta de plantilla de solución en Azure Marketplace?**

Lamentablemente, no. La imagen de máquina virtual debe proceder de Azure Marketplace; consulte [https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines).

Puede crear una oferta de máquina virtual en Marketplace mediante el disco duro virtual personalizado y marcarla como privada para que nadie la vea y hacer referencia a esta máquina virtual en la plantilla de solución.

**¿No se pudo actualizar la propiedad *contentVersion* para la plantilla principal?**

A veces se puede producir un error al implementar la plantilla mediante TemplateLink desde otra plantilla que espere una versión de contentVersion anterior por alguna razón. La solución alternativa es usar la propiedad de metadatos:

```
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "contentVersion": "1.0.1.0"
    },
    "parameters": {
```
