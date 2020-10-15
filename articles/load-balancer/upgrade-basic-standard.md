---
title: Actualización de Azure Load Balancer de público básico a público estándar
description: En este artículo se muestra cómo actualizar Azure Public Load Balancer de una SKU básica a una estándar.
services: load-balancer
author: irenehua
ms.service: load-balancer
ms.topic: how-to
ms.date: 01/23/2020
ms.author: irenehua
ms.openlocfilehash: 66c56ae6730043022a0d8bf3c94f7c6ce14d9852
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84809349"
---
# <a name="upgrade-azure-public-load-balancer"></a>Actualización de Azure Load Balancer público
[Azure Standard Load Balancer](load-balancer-overview.md) ofrece un amplio conjunto de funcionalidades y alta disponibilidad gracias a la redundancia de zona. Para más información acerca de la SKU de Load Balancer, consulte la [tabla de comparación](https://docs.microsoft.com/azure/load-balancer/skus#skus).

Hay tres fases en una actualización:

1. Migración de la configuración
2. Incorporación de máquinas virtuales a los grupos de back-end de Standard Load Balancer

Este artículo trata sobre la migración de la configuración. La incorporación de máquinas virtuales a los grupos de back-end puede variar en función de su entorno específico. pero [se proporcionan](#add-vms-to-backend-pools-of-standard-load-balancer) algunas recomendaciones generales de alto nivel.

## <a name="upgrade-overview"></a>Información general sobre la actualización

Existe un script de Azure PowerShell que hace lo siguiente:

* Crea una instancia de Load Balancer con una SKU estándar en el grupo de recursos y la ubicación que especifique.
* Copia perfectamente las configuraciones de Load Balancer de SKU básica en la instancia de Standard Load Balancer recién creada.
* Crea una regla de salida predeterminada que permite la conectividad saliente.

### <a name="caveatslimitations"></a>Advertencias y limitaciones

* El script solo admite la actualización de Public Load Balancer. En el caso de la actualización interna de la instancia básica de Load Balancer, puede encontrar instrucciones en [esta página](https://docs.microsoft.com/azure/load-balancer/upgrade-basicinternal-standard).
* Standard Load Balancer tiene una nueva dirección pública. No es posible trasladar las direcciones IP asociadas a las instancias de Basic Load Balancer existentes sin problemas a las instancias de Standard Load Balancer, ya que tienen diferentes SKU.
* Si se crea la instancia de Standard Load Balancer en una región diferente, no podrá asociar las máquinas virtuales existentes de la región antigua a la instancia de Standard Load Balancer recién creada. Para solucionar esta limitación, asegúrese de crear una nueva máquina virtual en la nueva región.
* Si Load Balancer no tiene ninguna configuración de IP de front-end ni grupo de back-end, es probable que se produzca un error al ejecutar el script. Asegúrese de que no están vacíos

## <a name="download-the-script"></a>Descarga del script

Descargue el script de migración de la [Galería de PowerShell](https://www.powershellgallery.com/packages/AzurePublicLBUpgrade/2.0).
## <a name="use-the-script"></a>Uso del script

Dispone de dos opciones en función de sus preferencias y de la configuración del entorno de PowerShell local:

* Si no tiene instalados los módulos de Azure Az, o si no le importa desinstalarlos, la mejor alternativa es usar la opción `Install-Script` para ejecutar el script.
* Si necesita conservar los módulos de Azure Az, lo mejor es que descargue el script y lo ejecute directamente.

Para determinar si tiene instalados los módulos de Azure Az, ejecute `Get-InstalledModule -Name az`. Si no ve ningún módulo de Az instalado, puede usar el método `Install-Script`.

### <a name="install-using-the-install-script-method"></a>Instalación con el método Install-Script

Para usar esta opción, los módulos de Azure Az no deben estar instalados en el equipo. En caso de que lo estén, el comando siguiente mostrará un error. Puede desinstalar los módulos de Azure Az o usar la otra opción para descargar manualmente el script y ejecutarlo.
  
Ejecute el script con el siguiente comando:

`Install-Script -Name AzurePublicLBUpgrade`

Este comando también instala los módulos de Az necesarios.  

### <a name="install-using-the-script-directly"></a>Instalación directamente con el script

Si tiene instalados algunos módulos de Azure Az y no puede desinstalarlos (o no le interesa hacerlo), puede descargar manualmente el script mediante la pestaña **Descarga manual** en el vínculo de descarga del script. El script se descarga como un archivo nupkg sin procesar. Para instalar el script desde este archivo nupkg, consulte [Descarga manual del paquete](/powershell/scripting/gallery/how-to/working-with-packages/manual-download).

Para ejecutar el script:

1. Use `Connect-AzAccount` para conectarse a Azure.

1. Use `Import-Module Az` para importar los módulos de Az.

1. Examine los parámetros obligatorios:

   * **oldRgName: [String]: Required**: es el grupo de recursos de la instancia existente de Basic Load Balancer que desea actualizar. Para encontrar este valor de cadena, vaya a Azure Portal, seleccione el origen Basic Load Balancer y haga clic en la sección **Información general** del equilibrador de carga. El grupo de recursos se encuentra en esa página.
   * **oldLBName: [String]: Required**: es el nombre de la instancia de Basic Load Balancer que desea actualizar. 
   * **newrgName: [String]: Required**: es el grupo de recursos en el que se creará la instancia de Standard Load Balancer. Puede tratarse de un nuevo grupo de recursos o de uno ya existente. Si elige un grupo de recursos existente, tenga en cuenta que el nombre de la instancia de Load Balancer debe ser único dentro del grupo de recursos. 
   * **newlocation: [String]: Required**: es la ubicación en la que se creará la instancia de Standard Load Balancer. Se recomienda heredar la misma ubicación de la instancia elegida de Basic Load Balancer para la instancia de Standard Load Balancer para una mejor asociación con otros recursos existentes.
   * **newLBName: [String]: Required**: es el nombre de la instancia de Standard Load Balancer que se va a crear.
1. Ejecute el script con los parámetros adecuados. Podría tardar entre cinco y siete minutos en finalizar.

    **Ejemplo**

   ```azurepowershell
   AzurePublicLBUpgrade.ps1 -oldRgName "test_publicUpgrade_rg" -oldLBName "LBForPublic" -newrgName "test_userInput3_rg" -newlocation "centralus" -newLbName "LBForUpgrade"
   ```

### <a name="add-vms-to-backend-pools-of-standard-load-balancer"></a>Incorporación de máquinas virtuales a los grupos de back-end de Standard Load Balancer

En primer lugar, asegúrese de que el script ha creado correctamente una instancia de Standard Public Load Balancer con la configuración exacta migrada a partir de la instancia de Basic Public Load Balancer. Puede comprobarlo desde Azure Portal.

Asegúrese de que envía una pequeña cantidad de tráfico mediante Standard Load Balancer como una prueba manual.
  
Estos son algunos escenarios en los que puede agregar máquinas virtuales a los grupos de back-end de la instancia de Standard Public Load Balancer recién creada y cómo se pueden configurar y nuestras recomendaciones para cada uno de ellos:

* **Traslado de las máquinas virtuales existentes desde los grupos de back-end de las instancias antiguas de Basic Public Load Balancer a los grupos de back-end de la instancia de Standard Public Load Balancer recién creada**.
    1. Para realizar las tareas de esta guía de inicio rápido, inicie sesión en [Azure Portal](https://portal.azure.com).
 
    1. Seleccione **Todos los recursos** en el menú de la izquierda y, a continuación, seleccione la **instancia recién creada de Standard Load Balancer** de la lista de recursos.
   
    1. En **Configuración**, seleccione **Grupos de back-end**.
   
    1. Seleccione el grupo de back-end que coincida con el grupo de back-end de Basic Load Balancer y seleccione el valor siguiente: 
      - **Máquina virtual**: Despliegue y seleccione las máquinas virtuales del grupo de back-end coincidente de la instancia de Basic Load Balancer.
    1. Seleccione **Guardar**.
    >[!NOTE]
    >En el caso de las máquinas virtuales que tienen direcciones IP públicas, deberá crear direcciones IP estándar primero en aquellos casos en los que no se garantice la misma dirección IP. Desasocie las máquinas virtuales de las direcciones IP básicas y asócielas con las direcciones IP estándar recién creadas. A continuación, podrá seguir las instrucciones para agregar máquinas virtuales al grupo de back-end de Standard Load Balancer. 

* **Creación de nuevas máquinas virtuales para agregarlas a los grupos de back-end de la instancia de Standard Public Load Balancer recién creada**.
    * [Aquí](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal#create-virtual-machines) encontrará más instrucciones sobre cómo crear una máquina virtual y cómo asociarla con Standard Load Balancer.

### <a name="create-an-outbound-rule-for-outbound-connection"></a>Creación de una regla de salida para la conexión de salida

Siga las [instrucciones ](https://docs.microsoft.com/azure/load-balancer/configure-load-balancer-outbound-portal#create-outbound-rule-configuration) para crear una regla de salida para poder:
* Definir la NAT de salida desde cero.
* Escalar y ajustar el comportamiento de la NAT de salida existente.

## <a name="common-questions"></a>Preguntas frecuentes

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>¿Hay alguna limitación en el script de Azure PowerShell para migrar la configuración de v1 a v2?

Sí. Consulte [Advertencias y limitaciones](#caveatslimitations).

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-basic-load-balancer-to-the-newly-created-standard-load-balancer"></a>¿Puede el script de Azure PowerShell cambiar el tráfico de la instancia de Basic Load Balancer a la instancia de Standard Load Balancer recién creada?

No. El script de Azure PowerShell solo migra la configuración. Usted es el responsable de realizar y controlar la migración real del tráfico.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Se han producido algunos problemas al usar este script. ¿Cómo puedo obtener ayuda?
  
Puede enviar un correo electrónico a slbupgradesupport@microsoft.com, abrir una incidencia con el Soporte técnico de Azure o hacer ambas cosas.

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre Load Balancer estándar](load-balancer-overview.md)
