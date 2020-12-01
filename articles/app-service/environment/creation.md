---
title: Creación de una instancia de App Service Environment
description: Aprenda a crear una instancia de App Service Environment.
author: ccompy
ms.assetid: 7690d846-8da3-4692-8647-0bf5adfd862a
ms.topic: article
ms.date: 11/16/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 12c9fd43933f9f54e013752c5fe482ce31650536
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95238401"
---
# <a name="create-an-app-service-environment"></a>Creación de una instancia de App Service Environment

> [!NOTE]
> Este artículo trata sobre App Service Environment v3 (versión preliminar).
> 

[App Service Environment (ASE)][Intro] es una implementación de inquilino único de Azure App Service que se ejecuta en una instancia de Azure Virtual Network (VNet).  ASEv3 solo admite la exposición de aplicaciones en una dirección privada de la red virtual. Cuando se crea una instancia de ASEv3 durante la versión preliminar, se agregan tres recursos a la suscripción.

- Entorno de App Service
- Zona privada de Azure DNS
- Punto de conexión privado

Para implementar un ASE, será necesario el uso de dos subredes.  Una subred contendrá el punto de conexión privado.  Esta subred se puede usar para otras cosas, como las máquinas virtuales.  La otra subred se utiliza para las llamadas salientes realizadas desde el ASE.  Esta subred no se puede usar para ninguna otra cosa que no sea el ASE. 

## <a name="before-you-create-your-ase"></a>Antes de crear su ASE

Después de crear el ASE, no se puede cambiar:

- Location
- Suscripción
- Resource group
- Instancia de Azure Virtual Network (red virtual) usada
- Subredes usadas
- Tamaño de la subred
- Nombre del ASE

La subred de salida debe ser lo suficientemente grande como para contener el tamaño máximo al que se escalará el ASE. Elija una subred lo suficientemente grande como para satisfacer sus necesidades de escalado máximo, ya que no se podrá cambiar una vez creada. El tamaño recomendado es /24 con 256 direcciones.

Después de crear el ASE, puede agregarle aplicaciones. Cuando su instancia de ASEv3 no contiene planes de App Service, se le cobra como si tuviera una instancia de un plan de App Service I1v2 en su ASE.  

ASEv3 solo se ofrece en determinadas regiones. A medida que la versión preliminar se mueva a disponibilidad general, se agregarán más regiones. 

## <a name="creating-an-ase-in-the-portal"></a>Creación de un ASE en el portal

1. Para crear una instancia de ASEv3, busque **App Service Environment (versión preliminar)** en el marketplace.  
2. Aspectos básicos:  Seleccione la suscripción, seleccione o cree el grupo de recursos y escriba el nombre del ASE.  El nombre del ASE también se usará como sufijo de dominio del ASE.  Si el nombre del ASE es *contoso*, el sufijo de dominio será *contoso.appserviceenvironment.net*.  Este nombre se establecerá automáticamente en la zona privada de Azure DNS usada por la red virtual en la que se implementa el ASE. 

    ![Pestaña Aspectos básicos de creación de una instancia de App Service Environment](./media/creation/creation-basics.png)

3. Hospedaje: seleccione OS Preference (Preferencia de SO), Host Group deployment (Implementación del grupo host). La preferencia de SO indica el sistema operativo que se usará inicialmente con las aplicaciones de este ASE. Puede agregar aplicaciones de otro sistema operativo después de la creación del ASE. La implementación del grupo host se usa para seleccionar el hardware dedicado. Con ASEv3, puede seleccionar Habilitado y, luego, colocarlo en hardware dedicado. Se le cobra por todo el host dedicado con la creación del ASE y, luego, un precio reducido por las instancias del plan de App Service. 

    ![Selecciones de hospedaje de App Service Environment](./media/creation/creation-hosting.png)

4. Redes:  seleccione o cree la red virtual, seleccione o cree la subred de entrada y seleccione o cree la subred de salida. Cualquier subred usada para la salida debe estar vacía y delegarse en Microsoft.Web/hostingEnvironments. Si crea la subred mediante el portal, esta se delegará automáticamente.

    ![Selecciones de red de App Service Environment](./media/creation/creation-networking.png)

5. Revisión y creación; compruebe que la configuración sea correcta y seleccione Crear. El ASE tardará aproximadamente una hora en crearse. 

    ![Revisión y creación de App Service Environment](./media/creation/creation-review.png)

Una vez completada la creación del ASE, puede seleccionarlo como ubicación al crear las aplicaciones. Para más información sobre la creación de aplicaciones en el nuevo ASE, lea [Uso de App Service Environment][UsingASE].

## <a name="os-preference"></a>Preferencia de sistema operativo
En un ASE, puede tener aplicaciones de Windows, Linux o ambas. En ASEv2, la preferencia inicial seleccionada durante la creación agrega la infraestructura de alta disponibilidad para ese sistema operativo durante la creación del ASE. Para agregar aplicaciones de otro sistema operativo, solo tiene que crear las aplicaciones como de costumbre y seleccionar el sistema operativo que prefiera. En ASEv3, esto no afectará apenas al comportamiento del back-end.  

## <a name="dedicated-hosts"></a>Hosts dedicados
El ASE se implementa normalmente en máquinas virtuales que se aprovisionan en un hipervisor multiinquilino. Si necesita implementarlo en sistemas dedicados, incluido el hardware, puede aprovisionar su ASE en hosts dedicados. En la versión preliminar inicial de ASEv3, los hosts dedicados vienen en pares. Cada host dedicado está en una zona de disponibilidad independiente, si la región lo permite. Las implementaciones del ASE basadas en hosts dedicados tienen un precio distinto del normal. Hay un cargo por el host dedicado y, luego, otro cargo por cada instancia del plan de App Service.  

<!--Links-->
[Intro]: ./overview.md
[MakeASE]: ./creation.md
[ASENetwork]: ./networking.md
[UsingASE]: ./using.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/network-security-groups-overview.md
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../web-application-firewall/ag/ag-overview.md
[logalerts]: ../../azure-monitor/platform/alerts-log.md
