---
title: Lógica de procesamiento de reglas de Azure Firewall
description: Azure Firewall tiene reglas NAT, reglas de red y reglas de aplicaciones. Las reglas se procesan en función del tipo de regla.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 03/10/2020
ms.author: victorh
ms.openlocfilehash: d3f8e52b4582c9467ae3ec61ee984771b801fe4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231256"
---
# <a name="azure-firewall-rule-processing-logic"></a>Lógica de procesamiento de reglas de Azure Firewall
Puede configurar reglas NAT, reglas de red y reglas de aplicaciones en Azure Firewall. Las reglas se procesan en función del tipo de regla. 

> [!NOTE]
> Si habilita el filtrado basado en la inteligencia sobre amenazas, esas reglas tienen la prioridad más alta y siempre se procesan primero. El filtrado de inteligencia sobre amenazas puede denegar el tráfico antes de que se procesen las reglas configuradas. Para más información, consulte [Filtrado basado en inteligencia sobre amenazas de Azure Firewall](threat-intel.md).

## <a name="outbound"></a>Salida

### <a name="network-rules-and-applications-rules"></a>Reglas de red y reglas de aplicaciones

Si configura reglas de red y reglas de aplicación, las reglas de red se aplican en orden de prioridad antes que las reglas de aplicación. Las reglas están terminando. Por lo tanto, si se encuentra una coincidencia en una regla de red, no se procesa ninguna otra regla.  Si no coincide ninguna regla de red, y el protocolo es HTTP, HTTPS o MSSQL, las reglas de aplicación evalúan el paquete en orden de prioridad. Si sigue sin haber coincidencias, el paquete se evalúa con la [colección de reglas de infraestructura](infrastructure-fqdns.md). Si sigue sin haber coincidencias, el paquete se deniega de forma predeterminada.

## <a name="inbound"></a>Entrada

### <a name="nat-rules"></a>Reglas NAT

Para habilitar la conectividad a Internet entrante, configure Traducción de direcciones de red de destino (DNAT) como se describe en el [Tutorial: Filtro del tráfico entrante con la DNAT de Azure Firewall mediante Azure Portal](tutorial-firewall-dnat.md). Las reglas NAT se aplican en orden de prioridad antes que las reglas de red. Si se encuentra alguna coincidencia, se agrega una regla de red correspondiente implícita para permitir el tráfico traducido. Para invalidar este comportamiento, agregue explícitamente una colección de reglas de red con reglas de denegación que coinciden con el tráfico traducido.

No se aplican reglas de aplicación a las conexiones entrantes. Por lo tanto, si quiere filtrar el tráfico de HTTP/S entrante, debe usar el firewall de aplicaciones web (WAF). Para más información, consulte [¿Qué es el firewall de aplicaciones web?](../web-application-firewall/overview.md)

## <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestran los resultados de algunas de estas combinaciones de reglas.

### <a name="example-1"></a>Ejemplo 1

La conexión a google.com se permite debido a una regla de red coincidente.

**Regla de red**

- Acción: Allow


|name  |Protocolo  |Tipo de origen  |Source  |Tipo de destino  |Dirección de destino  |Puertos de destino|
|---------|---------|---------|---------|----------|----------|--------|
|Allow-web     |TCP|Dirección IP|*|Dirección IP|*|80 443

**Regla de aplicación**

- Acción: Denegar

|name  |Tipo de origen  |Source  |Protocolo:Puerto|FQDN de destino|
|---------|---------|---------|---------|----------|----------|
|Deny-google     |Dirección IP|*|http:80,https:443|google.com

**Resultado**

La conexión a google.com se permite porque el paquete coincide con la regla de red *Allow-web*. El procesamiento de reglas se detiene en este momento.

### <a name="example-2"></a>Ejemplo 2

Se deniega el tráfico SSH porque una colección de reglas de red *Denegar* de prioridad más alta lo bloquea.

**Colección de reglas de red 1**

- Nombre: Allow-collection
- Prioridad: 200
- Acción: Allow

|name  |Protocolo  |Tipo de origen  |Source  |Tipo de destino  |Dirección de destino  |Puertos de destino|
|---------|---------|---------|---------|----------|----------|--------|
|Allow-SSH     |TCP|Dirección IP|*|Dirección IP|*|22

**Colección de reglas de red 2**

- Nombre: Deny-collection
- Prioridad: 100
- Acción: Denegar

|name  |Protocolo  |Tipo de origen  |Source  |Tipo de destino  |Dirección de destino  |Puertos de destino|
|---------|---------|---------|---------|----------|----------|--------|
|Deny-SSH     |TCP|Dirección IP|*|Dirección IP|*|22

**Resultado**

Se deniegan las conexiones SSH porque una colección de reglas de red de prioridad más alta las bloquea. El procesamiento de reglas se detiene en este momento.

## <a name="rule-changes"></a>Cambios de reglas

Si cambia una regla para denegar el tráfico permitido anteriormente, se quitará cualquier sesión existente pertinente.

## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).