---
title: Restringir el tráfico de salida en Azure Kubernetes Service (AKS)
description: Obtenga información sobre qué puertos y direcciones son necesarios para controlar el tráfico de salida en Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: d723f7b1e7331e65d17dca5873b891ec46d76c0e
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82207180"
---
# <a name="control-egress-traffic-for-cluster-nodes-in-azure-kubernetes-service-aks"></a>Control del tráfico de salida de los nodos de clúster en Azure Kubernetes Service (AKS)

De forma predeterminada, los clústeres de AKS tienen acceso de salida a Internet ilimitado. Este nivel de acceso a la red permite que los nodos y servicios que ejecuta accedan a recursos externos según sea necesario. Si desea restringir el tráfico de salida, es necesario el acceso a un número limitado de puertos y direcciones para mantener las tareas de mantenimiento del clúster en buen estado. De manera predeterminada, el clúster se configura para que solo use las imágenes de contenedor del sistema base de Microsoft Container Registry (MCR) o Azure Container Registry (ACR). Configure las reglas de seguridad y de firewall que prefiera para permitir estos puertos y direcciones necesarios.

En este artículo se detallan los puertos de red y los nombres de dominio completo (FQDN) obligatorios y opcionales al restringir el tráfico de salida de un clúster de AKS.

> [!IMPORTANT]
> En este documento solo se explica cómo bloquear el tráfico que sale de la subred de AKS. AKS no tiene requisitos de entrada.  No se admite el bloqueo del tráfico de subred interna mediante grupos de seguridad de red (NSG) y firewalls. Para controlar y bloquear el tráfico dentro del clúster, use las [directivas de red][network-policy].

## <a name="before-you-begin"></a>Antes de empezar

Es necesario tener instalada y configurada la versión 2.0.66 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-azure-cli].

## <a name="egress-traffic-overview"></a>Descripción del tráfico de salida

Para fines operativos y de administración, los nodos de un clúster de AKS deben tener acceso a determinados puertos y nombres de dominio completo (FQDN). Estas acciones podrían consistir en la comunicación con el servidor de la API o la descarga e instalación de actualizaciones de seguridad de nodos y componentes principales del clúster de Kubernetes. De forma predeterminada, el tráfico de Internet (saliente) de salida no está restringido a los nodos de un clúster de AKS. El clúster puede extraer imágenes de contenedor del sistema base de repositorios externos.

Para aumentar la seguridad de su clúster de AKS, le recomendamos que restrinja el tráfico de salida. El clúster está configurado para extraer imágenes de contenedor del sistema base de MCR o ACR. Si bloquea el tráfico de salida de esta forma, defina puertos específicos y FQDN para permitir que los nodos de AKS se comuniquen correctamente con los servicios externos necesarios. Sin estos FQDN y puertos autorizados, los nodos de AKS no podrán comunicarse con el servidor de API ni instalar los componentes principales.

Puede usar [Azure Firewall][azure-firewall] o un dispositivo de firewall de terceros para proteger el tráfico de salida y definir estos puertos y direcciones obligatorios. AKS no crea automáticamente estas reglas para usted. Los siguientes puertos y direcciones sirven como referencia para crear las reglas correspondientes en el firewall de red.

> [!IMPORTANT]
> Cuando use Azure Firewall para restringir el tráfico de salida y cree una ruta definida por el usuario (UDR) para forzar todo el tráfico de salida, asegúrese de crear una regla DNAT adecuada en el firewall para permitir correctamente el tráfico de entrada. El uso de Azure Firewall con una UDR interrumpe la configuración de entrada debido al enrutamiento asimétrico. (El problema se produce si la subred de AKS tiene una ruta predeterminada que va a la dirección IP privada del firewall, pero está usando un equilibrador de carga público, de entrada o de servicio de Kubernetes del tipo: LoadBalancer). En este caso, el tráfico entrante del equilibrador de carga se recibe a través de su dirección IP pública, pero la ruta de vuelta pasa a través de la dirección IP privada del firewall. Dado que el firewall es con estado, quita el paquete de vuelta porque el firewall no tiene conocimiento de una sesión establecida. Para aprender a integrar Azure Firewall con el equilibrador de carga de entrada o de servicio, consulte [Integración de Azure Firewall con Azure Standard Load Balancer](https://docs.microsoft.com/azure/firewall/integrate-lb).
> Puede bloquear el tráfico del puerto TCP 9000, el puerto TCP 22 y el puerto UDP 1194 con una regla de red entre las direcciones IP del nodo de trabajo de salida y la dirección IP del servidor de API.

En AKS, hay dos conjuntos de puertos y direcciones:

* Los [puertos y direcciones obligatorios para los clústeres de AKS](#required-ports-and-addresses-for-aks-clusters), que son los requisitos mínimos para el tráfico de salida autorizado.
* Los [puertos y direcciones recomendados opcionales para los clústeres de AKS](#optional-recommended-addresses-and-ports-for-aks-clusters), que no son obligatorios en todos los escenarios, pero implican que la integración con otros servicios como Azure Monitor no funcione correctamente. Consulte esta lista de FQDN y puertos opcionales y autorice cualquiera de los servicios y componentes que se usan en el clúster de AKS.

> [!NOTE]
> Limitar el tráfico de salida solo funciona en clústeres de AKS nuevos. En los clústeres existentes, [realice una operación de actualización de clústeres][aks-upgrade] utilizando el comando `az aks upgrade` antes de limitar el tráfico de salida.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Puertos y direcciones obligatorios para los clústeres de AKS

Un clúster de AKS requiere los siguientes puertos de salida / reglas de red:

* Puerto TCP *443*
* TCP [IPAddrOfYourAPIServer]:443 es necesario si tiene una aplicación que deba comunicarse con el servidor de API.  Este cambio se puede establecer después de crear el clúster.
* Puerto TCP *9000*, puerto TCP *22* y puerto UDP *1194* del pod de la parte delantera del túnel que va a comunicarse con el extremo del túnel del servidor de la API.
    * Para obtener información más concreta, vea las direcciones * *.hcp.\<ubicación\>.azmk8s.io* y * *.tun.\<ubicación\>.azmk8s.io* en la tabla siguiente.
* Puerto UDP *123* para la sincronización de hora del protocolo de tiempo de red (NTP) (nodos de Linux).
* También se requiere el puerto UDP *53* para DNS si tiene pods que acceden directamente al servidor de API.

Se requieren las siguientes reglas de aplicación / FQDN:

> [!IMPORTANT]
> * **.blob.core.windows.net y aksrepos.azurecr.io** ya no son reglas necesarias de FQDN para el bloqueo de salida  En el caso de los clústeres existentes, [realice una operación de actualización de clúster][aks-upgrade] con el comando `az aks upgrade` para quitar estas reglas.

> [!IMPORTANT]
> *.cdn.mscr.io se han sustituido por *.data.mcr.microsoft.com en las regiones de nube pública de Azure. Actualice las reglas de firewall existentes para que los cambios surtan efecto.

- Azure Global

| FQDN                       | Port      | Uso      |
|----------------------------|-----------|----------|
| *.hcp.\<ubicación\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Esta dirección es necesaria para la comunicación bidireccional entre el nodo y el servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| *.tun.\<ubicación\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Esta dirección es necesaria para la comunicación bidireccional entre el nodo y el servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| *.cdn.mscr.io       | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por Azure Content Delivery Network (CDN). |
| mcr.microsoft.com          | HTTPS:443 | Esta dirección es necesaria para acceder a las imágenes de Microsoft Container Registry (MCR). Este registro contiene imágenes y gráficos de propios (por ejemplo, Moby, etc.) necesarios para el funcionamiento del clúster durante la actualización y la escala del clúster. |
| *.data.mcr.microsoft.com             | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por la red de entrega de contenido (CDN) de Azure. |
| management.azure.com       | HTTPS:443 | Esta dirección es necesaria para las operaciones de Kubernetes GET o PUT. |
| login.microsoftonline.com  | HTTPS:443 | Esta dirección es necesaria para la autenticación de Azure Active Directory. |
| ntp.ubuntu.com             | UDP:123   | Esta dirección es necesaria para la sincronización de la hora NTP en nodos de Linux. |
| packages.microsoft.com     | HTTPS:443 | Esta dirección es el repositorio de paquetes de Microsoft que se usa para las operaciones *apt-get* almacenadas en caché.  Los paquetes de ejemplo incluyen Moby, PowerShell y la CLI de Azure. |
| acs-mirror.azureedge.net      | HTTPS:443 | Esta dirección es para el repositorio necesario para instalar los archivos binarios necesarios, como kubenet y Azure CNI. |

- Azure China 21Vianet

| FQDN                       | Port      | Uso      |
|----------------------------|-----------|----------|
| *.hcp.\<ubicación\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Esta dirección es necesaria para la comunicación bidireccional entre el nodo y el servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| *.tun.\<ubicación\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Esta dirección es necesaria para la comunicación bidireccional entre el nodo y el servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| *.azk8s.cn        | HTTPS:443 | Esta dirección es necesaria para descargar las imágenes y los archivos binarios necesarios.|
| mcr.microsoft.com          | HTTPS:443 | Esta dirección es necesaria para acceder a las imágenes de Microsoft Container Registry (MCR). Este registro contiene imágenes y gráficos de propios (por ejemplo, Moby, etc.) necesarios para el funcionamiento del clúster durante la actualización y la escala del clúster. |
| *.cdn.mscr.io       | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por Azure Content Delivery Network (CDN). |
| *.data.mcr.microsoft.com             | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por la red de entrega de contenido (CDN) de Azure. |
| management.chinacloudapi.cn       | HTTPS:443 | Esta dirección es necesaria para las operaciones de Kubernetes GET o PUT. |
| login.chinacloudapi.cn  | HTTPS:443 | Esta dirección es necesaria para la autenticación de Azure Active Directory. |
| ntp.ubuntu.com             | UDP:123   | Esta dirección es necesaria para la sincronización de la hora NTP en nodos de Linux. |
| packages.microsoft.com     | HTTPS:443 | Esta dirección es el repositorio de paquetes de Microsoft que se usa para las operaciones *apt-get* almacenadas en caché.  Los paquetes de ejemplo incluyen Moby, PowerShell y la CLI de Azure. |

- Azure Government

| FQDN                       | Port      | Uso      |
|----------------------------|-----------|----------|
| *.hcp.\<ubicación\>.cx.aks.containerservice.azure.us | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Esta dirección es necesaria para la comunicación bidireccional entre el nodo y el servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| *.tun.\<ubicación\>.cx.aks.containerservice.azure.us | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Esta dirección es necesaria para la comunicación bidireccional entre el nodo y el servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| mcr.microsoft.com          | HTTPS:443 | Esta dirección es necesaria para acceder a las imágenes de Microsoft Container Registry (MCR). Este registro contiene imágenes y gráficos de propios (por ejemplo, Moby, etc.) necesarios para el funcionamiento del clúster durante la actualización y la escala del clúster. |
|*.cdn.mscr.io              | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por Azure Content Delivery Network (CDN). |
| *.data.mcr.microsoft.com             | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por la red de entrega de contenido (CDN) de Azure. |
| management.usgovcloudapi.net       | HTTPS:443 | Esta dirección es necesaria para las operaciones de Kubernetes GET o PUT. |
| login.microsoftonline.us  | HTTPS:443 | Esta dirección es necesaria para la autenticación de Azure Active Directory. |
| ntp.ubuntu.com             | UDP:123   | Esta dirección es necesaria para la sincronización de la hora NTP en nodos de Linux. |
| packages.microsoft.com     | HTTPS:443 | Esta dirección es el repositorio de paquetes de Microsoft que se usa para las operaciones *apt-get* almacenadas en caché.  Los paquetes de ejemplo incluyen Moby, PowerShell y la CLI de Azure. |
| acs-mirror.azureedge.net      | HTTPS:443 | Esta dirección es para el repositorio necesario para instalar los archivos binarios necesarios, como kubenet y Azure CNI. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Direcciones y puertos recomendados opcionales para clústeres de AKS

Los puertos de salida / reglas de red siguientes son opcionales para un clúster de AKS:

Se recomiendan las siguientes reglas de aplicación / FQDN para que los clústeres de AKS funcionen correctamente:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| security.ubuntu.com, azure.archive.ubuntu.com, changelogs.ubuntu.com | HTTP:80   | Esta dirección permite a los nodos del clúster de Linux descargar las revisiones de seguridad y actualizaciones necesarias. |

## <a name="required-addresses-and-ports-for-gpu-enabled-aks-clusters"></a>Direcciones y puertos necesarios para clústeres AKS habilitados para GPU

Los clústeres de AKS que tienen GPU habilitado necesitan las reglas de aplicación / FQDN siguientes:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| nvidia.github.io | HTTPS:443 | Esta dirección se utiliza para la instalación y el funcionamiento correctos del controlador en nodos basados en GPU. |
| us.download.nvidia.com | HTTPS:443 | Esta dirección se utiliza para la instalación y el funcionamiento correctos del controlador en nodos basados en GPU. |
| apt.dockerproject.org | HTTPS:443 | Esta dirección se utiliza para la instalación y el funcionamiento correctos del controlador en nodos basados en GPU. |

## <a name="required-addresses-and-ports-with-azure-monitor-for-containers-enabled"></a>Direcciones y puertos necesarios que tienen habilitado Azure Monitor para contenedores

Los clústeres de AKS que tienen habilitado Azure Monitor para contenedores necesitan las reglas de aplicación / FQDN siguientes:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| dc.services.visualstudio.com | HTTPS:443    | Es para la telemetría de supervisión y las métricas correctas mediante Azure Monitor. |
| *.ods.opinsights.azure.com    | HTTPS:443    | Azure Monitor lo usa para la ingesta de los datos de análisis de registros. |
| *.oms.opinsights.azure.com | HTTPS:443 | Esta dirección la usa omsagent, que se usa para autenticar el servicio de análisis de registros. |
|*.microsoftonline.com | HTTPS:443 | Se usa para autenticar y enviar métricas a Azure Monitor. |
|*.monitoring.azure.com | HTTPS:443 | Se usa para enviar datos de métricas a Azure Monitor. |

## <a name="required-addresses-and-ports-with-azure-dev-spaces-enabled"></a>Direcciones y puertos necesarios que tienen habilitado Azure Dev Spaces

Los clústeres de AKS que tienen habilitado Azure Dev Spaces necesitan las reglas de aplicación/FQDN siguientes:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS:443 | Esta dirección se usa para extraer Linux Alpine y otras imágenes de Azure Dev Spaces |
| gcr.io | HTTP:443 | Esta dirección se usa para extraer imágenes de Helm/Tiller |
| storage.googleapis.com | HTTP:443 | Esta dirección se usa para extraer imágenes de Helm/Tiller |
| azds-\<guid\>.\<location\>.azds.io | HTTPS:443 | Comunicarse con los servicios de back-end de Azure Dev Spaces para el controlador. El FQDN exacto se puede encontrar en "dataplaneFqdn" en %USERPROFILE%\.azds\settings.json |

## <a name="required-addresses-and-ports-for-aks-clusters-with-azure-policy-in-public-preview-enabled"></a>Direcciones y puertos necesarios para los clústeres de AKS que tienen habilitado Azure Policy (en versión preliminar pública)

> [!CAUTION]
> Algunas de las características siguientes se encuentran en versión preliminar.  Las sugerencias de este artículo están sujetas a cambio a medida que la característica avanza a la versión preliminar pública y a las fases de versiones futuras.

Los clústeres de AKS que tienen habilitado Azure Policy necesitan las reglas de aplicación / FQDN siguientes.

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 | Esta dirección se usa para el funcionamiento correcto de Azure Policy. (Actualmente en versión preliminar en AKS) |
| raw.githubusercontent.com | HTTPS:443 | Esta dirección se usa para extraer las directivas integradas de GitHub para garantizar el funcionamiento correcto de Azure Policy. (Actualmente en versión preliminar en AKS) |
| *.gk.\<location\>.azmk8s.io | HTTPS:443    | Complemento de Azure Policy que se comunica con el punto de conexión de auditoría de Gatekeeper que se ejecuta en el servidor maestro para obtener los resultados de la auditoría. |
| dc.services.visualstudio.com | HTTPS:443 | Complemento de Azure Policy que envía datos de telemetría al punto de conexión de Application Insights. |

## <a name="required-by-windows-server-based-nodes-enabled"></a>Requerido por los nodos basados en Windows Server habilitados

Los grupos de nodos basados en Windows Server necesitan las reglas de aplicación/FQDN siguientes:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| onegetcdn.azureedge.net, winlayers.blob.core.windows.net, winlayers.cdn.mscr.io, go.microsoft.com | HTTPS:443 | Para instalar archivos binarios relacionados con Windows |
| mp.microsoft.com, www<span></span>.msftconnecttest.com, ctldl.windowsupdate.com | HTTP:80 | Para instalar archivos binarios relacionados con Windows |
| kms.core.windows.net | TCP:1688 | Para instalar archivos binarios relacionados con Windows |

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido qué puertos y direcciones hay que permitir si restringe el tráfico de salida para el clúster. También puede definir cómo se pueden comunicar los pods y qué restricciones tienen en el clúster. Para más información, consulte [Protección del tráfico entre pods mediante directivas de red en Azure Kubernetes Service (AKS)][network-policy].

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
