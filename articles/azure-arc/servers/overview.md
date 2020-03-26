---
title: Introducción a Azure Arc para servidores (versión preliminar)
description: Aprenda a usar Azure Arc para servidores para administrar máquinas hospedadas fuera de Azure, como si se tratase de un recurso de Azure.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: azure automation, DSC, powershell, desired state configuration, update management, change tracking, inventory, runbooks, python, graphical, hybrid
ms.date: 02/24/2020
ms.topic: overview
ms.openlocfilehash: 57b44db9c1bb9a607ad8478b7208df40441020c2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79290133"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>¿Qué es Azure Arc para servidores (versión preliminar)?

Azure Arc para servidores (versión preliminar) le permite administrar las máquinas Windows y Linux hospedadas fuera de Azure en la red corporativa o en otro proveedor en la nube, de forma similar a cómo se administran las máquinas virtuales nativas de Azure. Cuando una máquina híbrida se conecta a Azure, se convierte en una máquina conectada y se trata como un recurso de Azure. Cada máquina conectada tiene un identificador de recurso, se administra como parte de un grupo de recursos de una suscripción y se beneficia de las construcciones estándar de Azure como Azure Policy y la aplicación de etiquetas.

Para ofrecer esta experiencia con las máquinas híbridas hospedadas fuera de Azure, el agente de Azure Connected Machine debe estar instalado en cada una de las máquinas que planea conectar con Azure. Este agente no proporciona ninguna otra funcionalidad y no reemplaza al [agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md). El agente de Log Analytics para Windows y Linux es necesario si desea supervisar de forma proactiva el sistema operativo y las cargas de trabajo que se ejecutan en la máquina, administrarlos mediante runbooks de Automation o soluciones como Update Management, o usar otros servicios de Azure como [Azure Security Center](../../security-center/security-center-intro.md).

>[!NOTE]
>Esta versión preliminar está pensada para fines de evaluación y se recomienda no administrar máquinas de producción críticas.
>

## <a name="supported-scenarios"></a>Escenarios admitidos

Azure Arc para servidores (versión preliminar) admite los siguientes escenarios con máquinas conectadas:

- Asigne [configuraciones de invitado de Azure Policy](../../governance/policy/concepts/guest-configuration.md) de la misma manera que en la asignación de directivas para máquinas virtuales de Azure.
- Los datos de registros que recopila el agente de Log Analytics y que se almacenan en el área de trabajo de Log Analytics en la que la máquina está registrada contienen ahora propiedades específicas de la máquina como el identificador de recurso, el cual se puede usar para admitir el acceso al registro del [contexto del recurso](../../azure-monitor/platform/design-logs-deployment.md#access-mode).

## <a name="supported-regions"></a>Regiones admitidas

En Azure Arc para servidores (versión preliminar), solo se admiten determinadas regiones:

- WestUS2
- Oeste de Europa
- WestAsia

En la mayoría de los casos, la ubicación que seleccione al crear el script de instalación debe ser la región de Azure más cercana geográficamente a la ubicación de la máquina. Los datos en reposo se almacenarán en la ubicación geográfica de Azure que contiene la región que especifique, lo que también puede afectar a su elección de región si tiene requisitos de residencia de datos. Aunque la región de Azure a la que está conectada la máquina experimente una interrupción, la máquina conectada no se verá afectada, aunque es posible que las operaciones de administración que usan Azure no se puedan completar. Para lograr resistencia en caso de una interrupción regional, si dispone de varias ubicaciones que proporcionan un servicio con redundancia geográfica, es mejor conectar las máquinas de cada ubicación a una región diferente de Azure.

## <a name="prerequisites"></a>Prerequisites

### <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Las siguientes versiones de los sistemas operativos Windows y Linux son compatibles oficialmente con el agente de Azure Connected Machine: 

- Windows Server 2012 R2 y versiones posteriores (incluido Windows Server Core)
- Ubuntu 16.04 y 18.04
- CentOS Linux 7
- SUSE Linux Enterprise Server (SLES) 15
- Red Hat Enterprise Linux (RHEL) 7
- Amazon Linux 7

>[!NOTE]
>Esta versión preliminar del agente de Connected Machine para Windows solo admite la configuración de Windows Server para usar el idioma inglés.
>

### <a name="required-permissions"></a>Permisos necesarios

- Para incorporar máquinas, debe ser miembro del rol **Incorporación de Azure Connected Machine**.

- Para leer, modificar, volver a incorporar y eliminar una máquina, debe ser miembro del rol **Administrador de recursos de Azure Connected Machine**. 

### <a name="azure-subscription-and-service-limits"></a>Límites del servicio y la suscripción de Azure

Antes de configurar las máquinas con Azure Arc para servidores (versión preliminar), debe revisar los [límites de suscripción](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) y los [límites del grupo de recursos](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) de Azure Resource Manager para planear el número de máquinas que se van a conectar.

## <a name="tls-12-protocol"></a>Protocolo TLS 1.2

Para garantizar la seguridad de los datos en tránsito hacia Azure, se recomienda encarecidamente configurar la máquina para que use Seguridad de la capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**. 

|Plataforma/lenguaje | Soporte técnico | Más información |
| --- | --- | --- |
|Linux | Las distribuciones de Linux tienden a basarse en [OpenSSL](https://www.openssl.org) para la compatibilidad con TLS 1.2. | Compruebe el [registro de cambios de OpenSSL](https://www.openssl.org/news/changelog.html) para confirmar si su versión de OpenSSL es compatible.|
| Windows Server 2012 R2 y versiones posteriores | Compatible y habilitado de manera predeterminada. | Para confirmar que aún usa la [configuración predeterminada](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings).|

### <a name="networking-configuration"></a>Configuración de redes

El agente de Connected Machine para Linux y Windows se comunica de forma segura con la salida de Azure Arc a través del puerto TCP 443. Si la máquina se conecta mediante un servidor proxy o firewall para comunicarse a través de Internet, consulte los requisitos siguientes para comprender qué configuración de red debe aplicarse.

Si la conectividad saliente está restringida por el firewall o el servidor proxy, asegúrese de que las direcciones URL que se muestran a continuación no estén bloqueadas. Si solo permite los intervalos IP o los nombres de dominio necesarios para que el agente se comunique con el servicio, también debe permitir el acceso a las siguientes etiquetas y direcciones URL del servicio.

Etiquetas de servicio:

- AzureActiveDirectory
- AzureTrafficManager

Direcciones URL:

| Recurso del agente | Descripción |
|---------|---------|
|management.azure.com|Azure Resource Manager|
|login.windows.net|Azure Active Directory|
|dc.services.visualstudio.com|Application Insights|
|agentserviceapi.azure-automation.net|Configuración de invitado|
|*-agentservice-prod-1.azure-automation.net|Configuración de invitado|
|*.his.hybridcompute.azure-automation.net|Servicio de identidad híbrida|

Para obtener una lista de direcciones IP para cada etiqueta o región de servicio, consulte el archivo JSON [Rangos de direcciones IP y etiquetas de servicio de Azure: nube pública](https://www.microsoft.com/download/details.aspx?id=56519). Microsoft publica actualizaciones semanales que incluyen cada uno de los servicios de Azure y los intervalos IP que usan. Para más información, consulte [Etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Se requieren las direcciones URL de la tabla anterior junto con la información del intervalo IP de la etiqueta de servicio, ya que la mayoría de los servicios no tienen actualmente un registro de etiquetas de servicio. Por este motivo, las direcciones IP están sujetas a cambios. Si se necesitan intervalos de direcciones IP para la configuración del firewall, se debe usar la etiqueta de servicio **AzureCloud** para permitir el acceso a todos los servicios de Azure. No deshabilite la supervisión de seguridad ni la inspección de estas direcciones URL, pero permítalas como haría con otro tráfico de Internet.

### <a name="register-azure-resource-providers"></a>Registro de proveedores de recursos de Azure

Azure Arc para servidores (versión preliminar) depende de los siguientes proveedores de recursos de Azure de la suscripción para poder usar este servicio:

- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**

Si no están registrados, puede registrarlos con los comandos siguientes:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

CLI de Azure:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Para registrar también los proveedores de recursos en Azure Portal siga los pasos que se describen en [Azure Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

## <a name="connected-machine-agent"></a>Agente de Connected Machine

Puede descargar el paquete del agente de Azure Connected Machine para Windows y Linux desde las ubicaciones que se indican a continuación.

- [Paquete de Windows Installer con el agente](https://aka.ms/AzureConnectedMachineAgent) del centro de descarga de Microsoft.
- En el caso del paquete del agente de Linux, este se distribuye a través del [repositorio de paquetes](https://packages.microsoft.com/) de Microsoft con el formato preferido para la distribución (.RPM o .DEB).

>[!NOTE]
>Durante esta versión preliminar, solo se ha lanzado un paquete, que es adecuado para Ubuntu 16.04 o 18.04.

El agente de Azure Connected Machine para Windows y Linux se puede actualizar a la versión más reciente de forma manual o automática según sus necesidades. En Windows, la actualización del agente se puede realizar automáticamente mediante Windows Update y, para Ubuntu, mediante la herramienta de línea de comandos [apt](https://help.ubuntu.com/lts/serverguide/apt.html).

### <a name="agent-status"></a>Estado del agente

El agente Connected Machine envía un mensaje de latido al servicio cada 5 minutos. Si no se recibe uno durante 15 minutos, se considera la máquina sin conexión y el estado se cambiará automáticamente a **Desconectado** en el portal. Al recibir un mensaje de latido subsiguiente del agente de Connected Machine, su estado se cambiará automáticamente a **Conectado**.

## <a name="install-and-configure-agent"></a>Instalación y configuración del agente

La conexión de máquinas del entorno híbrido directamente con Azure se puede lograr mediante diferentes métodos según sus requisitos. En la tabla siguiente se resalta cada método para determinar cuál se adapta mejor a su organización.

| Método | Descripción |
|--------|-------------|
| interactivamente, | Instale manualmente el agente en una máquina, o en un grupo reducido de estas, siguiendo los pasos que se indican en [Conexión de máquinas desde Azure Portal](onboard-portal.md).<br> En Azure Portal, puede generar un script y ejecutarlo en la máquina para automatizar los pasos de instalación y configuración del agente.|
| A escala | Instale y configure el agente para varias máquinas según lo que se indica en [Conexión de máquinas mediante una entidad de servicio](onboard-service-principal.md).<br> Este método crea una entidad de servicio para conectar máquinas de forma no interactiva.|

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a evaluar Azure Arc para servidores (versión preliminar), siga el artículo [Conexión de máquinas híbridas a Azure desde Azure Portal](onboard-portal.md). 