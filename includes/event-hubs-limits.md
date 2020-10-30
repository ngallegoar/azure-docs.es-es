---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 09/10/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: ea1ac064799b0cede1de82851a514a2b389f20aa
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92499063"
---
En las tablas siguientes se enumeran las cuotas y los límites específicos de [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Para más información sobre los precios de Event Hubs, consulte los [precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="common-limits-for-all-tiers"></a>Límites comunes para todos los niveles
Los límites siguientes son comunes en todos los niveles. 

| Límite |  Notas | Value |
| --- |  --- | --- |
| Número de espacios de nombres de Event Hubs por suscripción |- |100 |
| Número de centros de eventos por espacio de nombres | Las solicitudes posteriores para la creación de un nuevo centro de eventos se rechazan. |10 |
| Número de particiones por centro de eventos |- |32 |
| Tamaño del nombre de un centro de eventos |- | 256 caracteres |
| Tamaño del nombre de un grupo de consumidores |- | 256 caracteres |
| Número de destinatarios no de época por grupo de consumidores |- |5 |
| Número de reglas de autorización por espacio de nombres | Las solicitudes posteriores de creación de reglas de autorización se rechazan.|12 |
| Número de llamadas al método GetRuntimeInformation |  - | 50 por segundo | 
| Número de reglas de red virtual (VNet) y de configuración IP | - | 128 | 


### <a name="basic-and-standard-tiers"></a>Niveles Basic y Estándar
En la tabla siguiente se muestran los límites que pueden ser diferentes para los niveles básico y estándar. 

| Límite | Notas | Básica | Estándar |
| --- |  --- | -- | --- |
| Tamaño máximo de evento de Event Hubs| &nbsp; | 256 KB | 1 MB |
| Número de grupos de consumidores por centro de eventos | &nbsp; |1 |20 |
| Número de conexiones de AMQP por espacio de nombres | Las solicitudes posteriores de conexiones adicionales se rechazan y el código que llama recibe una excepción. |100 |5\.000|
| Período de retención máximo de datos de eventos | &nbsp; |1 día |1-7 días |
| Unidades de rendimiento máximo |Si se supera el límite de unidades de procesamiento, los datos se limitan y se genera un excepción [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Para solicitar un número mayor de unidades de procesamiento para el nivel Estándar rellene una [solicitud de soporte técnico](/azure/azure-portal/supportability/how-to-create-azure-support-request). Las [unidades de rendimiento adicionales](../articles/event-hubs/event-hubs-auto-inflate.md) se encuentran disponibles en bloques de 20 y están sujetas a un compromiso de compra. |20 | 20 | 

### <a name="dedicated-tier"></a>Nivel dedicado
La oferta de Event Hubs dedicado se factura aplicando una tarifa mensual fija con un uso mínimo de 4 horas. El nivel Dedicado ofrece todas las características del plan Estándar, pero con una capacidad de escalado de nivel empresarial para aquellos clientes que tienen cargas de trabajo muy exigentes. 

Consulte este [documento](https://docs.microsoft.com/azure/event-hubs/event-hubs-dedicated-cluster-create-portal), en el que se explica cómo crear un clúster de Event Hubs dedicado mediante Azure Portal.

| Característica | Límites |
| --- | ---|
| Ancho de banda |  20 CU |
| Espacios de nombres | 50 por CU |
| Centros de eventos |  1000 por espacio de nombres |
| Tamaño del mensaje | 1 MB |
| Particiones | 2000 por CU |
| Grupos de consumidores | Sin límite por CU, 1000 por centro de eventos |
| Conexiones asincrónicas | 100 000 incluidos |
| Tiempo de retención de mensajes | 90 días, 10 TB incluidas por TU |
| Eventos de entrada | Se incluye |
| Capturar | Se incluye |


### <a name="schema-registry-limitations"></a>Limitaciones del registro de esquema

#### <a name="limits-that-are-the-same-for-standard-and-dedicated-tiers"></a>Límites que son iguales para los niveles **estándar** y **dedicado** 
| Característica | Límite | 
| --- |  --- | -- |
| Longitud máxima de un nombre de grupo de esquemas | 50 |  
| Longitud máxima de un nombre de esquemas | 100 |    
| Tamaño, en bytes, por esquema | 1 MB |   
| Número de propiedades por grupo de esquemas | 1024 |
| Tamaño, en bytes, por clave de propiedad del grupo | 256 | 
| Tamaño, en bytes, por valor de propiedad del grupo | 1024 | 


#### <a name="limits-that-are-different-for-standard-and-dedicated-tiers"></a>Límites que son distintos para los niveles **estándar** y **dedicado** 

| Límite | Estándar | Dedicado | 
| --- |  --- | -- | --- |
| Tamaño del registro de esquema (espacio de nombres), en megabytes | 25 |  1024 |
| Número de grupos de esquema en un registro de esquema (espacio de nombres)| 1 (sin incluir el predeterminado) | 1000 |
| Número de versiones del esquema en todos los grupos de esquemas | 25 | 10000 |





