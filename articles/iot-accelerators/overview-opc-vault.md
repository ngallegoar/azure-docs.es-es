---
title: 'Qué es OPC Vault: Azure | Microsoft Docs'
description: En este artículo se proporciona información general de OPC Vault. Puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones de OPC UA en la nube.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 715ed204e28d6260c28fa099b40fc78aa12de44d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91281548"
---
# <a name="what-is-opc-vault"></a>¿Qué es OPC Vault?

> [!IMPORTANT]
> Mientras actualizamos este artículo, consulte el artículo sobre [Azure Industrial IoT](https://azure.github.io/Industrial-IoT/), donde encontrará el contenido más actualizado.

OPC Vault es un microservicio que puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones cliente y servidor de OPC UA en la nube. En este artículo se describen los casos de uso sencillos de OPC Vault.

## <a name="certificate-management"></a>Administración de certificados

Por ejemplo, una empresa de fabricación debe conectar su máquina servidor de OPC UA a su aplicación cliente recién creada. Cuando el fabricante crea el acceso inicial de la máquina servidor, se muestra inmediatamente un mensaje de error en la aplicación servidor de OPC UA para indicar que la aplicación cliente no es segura. Este mecanismo se incorpora en la máquina servidor de OPC UA para evitar el acceso no autorizado a las aplicaciones, y así impedir la piratería feroz en la planta.

## <a name="application-security-management"></a>Administración de la seguridad de aplicaciones
Un profesional de seguridad usa el microservicio OPC Vault para permitir que el servidor OPC UA se comunique fácilmente con cualquier aplicación cliente, porque OPC Vault tiene todas las funciones para el registro, el almacenamiento y la administración del ciclo de vida de los certificados. Ahora que el servidor de OPC UA está conectado de forma segura, se puede comunicar con la aplicación cliente recién creada.

## <a name="the-complete-opc-vault-architecture"></a>La arquitectura completa de OPC Vault
El siguiente diagrama ilustra la arquitectura completa de OPC Vault.

![Arquitectura de OPC Vault](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre OPC Vault y sus usos, este es el siguiente paso sugerido:

[Arquitectura de OPC Vault](overview-opc-vault-architecture.md)