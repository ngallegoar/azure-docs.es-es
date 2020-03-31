---
title: archivo de inclusión
description: archivo de inclusión
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 12/13/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: d700dfcf5a7b6e9ada2a755335689ffa571e4c3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334613"
---
#### <a name="process-automation"></a>Automatización de procesos

| Resource | Límite |Notas|
| --- | --- |---|
| Cantidad máxima de trabajos nuevos que se puede enviar cada 30 segundos por cuenta de Automation (trabajos no programados) |100 |Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. El cliente recibe una respuesta de error.|
| Cantidad máxima de trabajos en ejecución simultáneos en la misma instancia de tiempo por cuenta de Automation (trabajos no programados) |200 |Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. El cliente recibe una respuesta de error.|
| Tamaño máximo de almacenamiento de metadatos de trabajo para un período sucesivo de 30 días | 10 GB (aproximadamente, 4 millones de trabajos)|Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. |
| Límite de flujo de trabajo máximo|1MB|Una sola secuencia no puede ser superior a 1 MB.|
| Cantidad máxima de módulos que se puede importar cada 30 segundos por cuenta de Automation |5 ||
| Tamaño máximo de un módulo |100 MB ||
| Tiempo de ejecución de trabajos, nivel Gratis |500 minutos por suscripción por mes del calendario ||
| Cantidad máxima de espacio en disco permitida por espacio aislado<sup>1</sup> |1 GB |Se aplica solo a los espacios aislados de Azure.|
| Cantidad máxima de memoria que se asigna a un espacio aislado<sup>1</sup> |400 MB |Se aplica solo a los espacios aislados de Azure.|
| Número máximo de sockets de red permitido por espacio aislado<sup>1</sup> |1,000 |Se aplica solo a los espacios aislados de Azure.|
| Tiempo de ejecución máximo permitido por runbook <sup>1</sup> |3 horas |Se aplica solo a los espacios aislados de Azure.|
| Número máximo de cuentas de Automation en una suscripción |Sin límite ||
| Número máximo de grupos de Hybrid Worker por cuenta de Automation|4\.000||
|Número máximo de trabajos simultáneos que se pueden ejecutar en un único Hybrid Runbook Worker|50 ||
| Tamaño máximo de los parámetros de un trabajo de runbook   | 512 kilobits||
| Parámetros máximos de runbook   | 50|Si alcanza el límite de 50 parámetros, puede pasar una cadena JSON o XML a un parámetro y analizarlo con el runbook.|
| Tamaño máximo de carga útil de webhook |  512 kilobits|
| Máximo de días que se conservan los datos de trabajo|30 días|
| Tamaño máximo del estado de flujo de trabajo de PowerShell |5 MB| Se aplica a los runbooks del flujo de trabajo de PowerShell al aplicar el punto de control del flujo de trabajo.|

<sup>1</sup>Un espacio aislado es un entorno compartido que puede usarse en varios trabajos. Los trabajos con el mismo espacio aislado están sujetos a las limitaciones de recursos de dicho espacio.

#### <a name="change-tracking-and-inventory"></a>Change Tracking e Inventario

En la siguiente tabla se muestran los límites del elemento sometido a seguimiento por máquina para el seguimiento de cambios.

| **Recurso** | **Límite**| **Notas** |
|---|---|---|
|Archivo|500||
|Registro|250||
|Software de Windows|250|No se incluye el costo de las actualizaciones de software.|
|Paquetes Linux|1250||
|Servicios|250||
|Demonio|250||

#### <a name="update-management"></a>Administración de actualizaciones

En la tabla siguiente se muestran los límites de la administración de actualizaciones.

| **Recurso** | **Límite**| **Notas** |
|---|---|---|
|Número de máquinas por implementación de actualizaciones|1000||