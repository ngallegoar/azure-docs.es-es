---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 2d0e171807985deaebe8faa625d6b767c2a3efd7
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218255"
---
La siguiente configuración se usó para los pasos siguientes:

- Equipo: Ubuntu Server 18.04
- Dependencias: strongSwan


Ejecute los comandos siguientes para instalar la configuración necesaria de strongSwan:

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

Use el siguiente comando para instalar la interfaz de la línea de comandos de Azure:

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[Instrucciones adicionales sobre cómo instalar la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)
