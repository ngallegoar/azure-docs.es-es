---
title: Reglas de acceso del firewall
description: Configure reglas para acceder a un registro de contenedor de Azure desde detrás de un firewall. Para ello, permita el acceso a la API de REST ("inclusión en lista blanca") y los nombres de dominio del punto de conexión de almacenamiento o los intervalos de direcciones IP específicos del servicio.
ms.topic: article
ms.date: 02/11/2020
ms.openlocfilehash: 06fedea2adf5e73929f5752279f2bd7e7227e570
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77168013"
---
# <a name="configure-rules-to-access-an-azure-container-registry-behind-a-firewall"></a>Configuración de reglas para acceder a un registro de contenedor de Azure desde detrás de un firewall

En este artículo se explica cómo configurar reglas en el firewall para permitir el acceso a un registro de contenedor de Azure. Por ejemplo, es posible que un dispositivo Azure IoT Edge que esté detrás de un firewall o de un servidor proxy necesite acceder a un registro de contenedor para extraer una imagen del contenedor. O bien, que un servidor bloqueado de una red local necesite tener acceso para enviar una imagen.

Si en vez de eso desea configurar reglas de acceso de red de entrada en un registro de contenedor solo en una instancia de Azure Virtual Network o desde un intervalo de direcciones IP públicas, consulte [Restricción del acceso a un registro de contenedor de Azure desde una red virtual](container-registry-vnet.md).

## <a name="about-registry-endpoints"></a>Acerca de los puntos de conexión del registro

Para extraer imágenes u otros artefactos de un registro de contenedor de Azure (o para insertarlas en él), un cliente (por ejemplo, un demonio de Docker) debe interactuar a través de HTTPS con dos puntos de conexión distintos.

* **Punto de conexión de la API de REST del registro**: las operaciones de autenticación y administración del registro se administran a través del punto de conexión público de la API de REST del registro. Este punto de conexión es el nombre del servidor de inicio de sesión del registro, o bien un intervalo de direcciones IP asociado. 

* **Punto de conexión de almacenamiento**: Azure [asigna Blob Storage](container-registry-storage.md) en cuentas de Azure Storage en nombre de cada registro para administrar los datos de imágenes de contenedor y otros artefactos. Cuando un cliente accede a capas de imagen en un registro de contenedor de Azure, realiza solicitudes mediante un punto de conexión de la cuenta de almacenamiento proporcionado por el registro.

Si el registro está [replicado geográficamente](container-registry-geo-replication.md), es posible que el cliente tenga que interactuar con los puntos de conexión de REST y de almacenamiento en una región específica o en varias regiones replicadas.

## <a name="allow-access-to-rest-and-storage-domain-names"></a>Permitir el acceso a REST y a los nombres de dominio de almacenamiento

* **Punto de conexión de REST**: permite el acceso al nombre del servidor de inicio de sesión del registro completo, como `myregistry.azurecr.io`.
* **Punto de conexión de almacenamiento (datos)** : permite el acceso a todas las cuentas de Azure Blob Storage usando el carácter comodín `*.blob.core.windows.net`.


## <a name="allow-access-by-ip-address-range"></a>Permitir el acceso por intervalo de direcciones IP

Si su organización tiene directivas para permitir el acceso solo a direcciones o intervalos de direcciones IP específicos, descargue [Azure IP Ranges and Service Tags – Public Cloud](https://www.microsoft.com/download/details.aspx?id=56519) (Intervalos de direcciones IP y etiquetas de servicio de Azure: nube pública).

Para buscar los intervalos IP del punto de conexión de REST de ACR, busque **AzureContainerRegistry** en el archivo JSON.

> [!IMPORTANT]
> Los intervalos de direcciones IP de los servicios de Azure pueden cambiar. Estos cambios se publican semanalmente. Descargue el archivo JSON con regularidad y realice las modificaciones necesarias en las reglas de acceso. Si para acceder a Azure Container Registry, su escenario implica configurar reglas de grupo de seguridad de red en una instancia de Azure Virtual Network, use la [etiqueta de servicio](#allow-access-by-service-tag) **AzureContainerRegistry** en su lugar.
>

### <a name="rest-ip-addresses-for-all-regions"></a>Direcciones IP de REST para todas las regiones

```json
{
  "name": "AzureContainerRegistry",
  "id": "AzureContainerRegistry",
  "properties": {
    "changeNumber": 10,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.66.140.72/29",
    [...]
```

### <a name="rest-ip-addresses-for-a-specific-region"></a>Direcciones IP de REST para una región específica

Busque la región específica, como **AzureContainerRegistry.AustraliaEast**.

```json
{
  "name": "AzureContainerRegistry.AustraliaEast",
  "id": "AzureContainerRegistry.AustraliaEast",
  "properties": {
    "changeNumber": 1,
    "region": "australiaeast",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.70.72.136/29",
    [...]
```

### <a name="storage-ip-addresses-for-all-regions"></a>Direcciones IP de almacenamiento para todas las regiones

```json
{
  "name": "Storage",
  "id": "Storage",
  "properties": {
    "changeNumber": 19,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "13.65.107.32/28",
    [...]
```

### <a name="storage-ip-addresses-for-specific-regions"></a>Direcciones IP de almacenamiento para regiones específicas

Busque la región específica, como **Storage.AustraliaCentral**.

```json
{
  "name": "Storage.AustraliaCentral",
  "id": "Storage.AustraliaCentral",
  "properties": {
    "changeNumber": 1,
    "region": "australiacentral",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "52.239.216.0/23"
    [...]
```

## <a name="allow-access-by-service-tag"></a>Permitir el acceso por etiqueta de servicio

En una red virtual de Azure, use las reglas de seguridad de red para filtrar el tráfico de un recurso (por ejemplo, una máquina virtual) a un registro de contenedor. Para simplificar la creación de las reglas de red de Azure, use la [etiqueta de servicio](../virtual-network/security-overview.md#service-tags) **AzureContainerRegistry**. Una etiqueta de servicio constituye un grupo de prefijos de dirección IP para tener acceso a un servicio de Azure de manera global o por región de Azure. La etiqueta se actualiza automáticamente cuando las direcciones cambian. 

Por ejemplo, cree una regla de grupo de seguridad de red saliente con el destino **AzureContainerRegistry** para permitir el tráfico a un registro de contenedor de Azure. Para permitir el acceso a la etiqueta de servicio únicamente en una región específica, especifique la región con el siguiente formato: **AzureContainerRegistry**.[*nombre de la región*].

## <a name="configure-client-firewall-rules-for-mcr"></a>Configuración de las reglas de firewall de cliente para MCR

Si necesita obtener acceso a Microsoft Container Registry (MCR) desde detrás de un firewall, consulte las instrucciones para configurar las [reglas de firewall de cliente de MCR](https://github.com/microsoft/containerregistry/blob/master/client-firewall-rules.md). MCR es el registro principal para todas las imágenes de Docker publicadas por Microsoft, como las imágenes de Windows Server.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre los [Procedimientos recomendados de seguridad de la red de Azure](../security/fundamentals/network-best-practices.md).

* Obtenga más información sobre los [grupos de seguridad](/azure/virtual-network/security-overview) en una red virtual de Azure.



<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

