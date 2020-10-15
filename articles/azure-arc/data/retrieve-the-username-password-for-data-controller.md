---
title: Recuperación del nombre de usuario y la contraseña para conectarse al controlador de datos de Arc
description: Recuperación del nombre de usuario y la contraseña para conectarse al controlador de datos de Arc
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 8cabb48e3620f1a17d1bb9b87e1646ce2793143b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761709"
---
# <a name="retrieve-the-user-name-and-password-to-connect-to-the-arc-data-controller"></a>Recuperación del nombre de usuario y la contraseña para conectarse al controlador de datos de Arc

Es posible que se produzca una situación en la que sea necesario recuperar el nombre de usuario y la contraseña del controlador de datos. Estos son los comandos que necesitará cuando lo ejecute. 

```console
azdata login
```

Si es el administrador de Kubernetes del clúster. Como tal, cuenta con privilegios para ejecutar comandos con el fin de recuperar de Kubernetes el secreto que almacena la información que Azure Arc conserva allí.

> [!NOTE]
>  Si ha usado un nombre diferente para el espacio de nombres en el que se ha creado el controlador de datos, asegúrese de cambiar el parámetro `-n arc` en los comandos siguientes a fin de usar el nombre del espacio de nombres en el que ha creado el controlador de datos.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="linux"></a>Linux

Ejecute el comando siguiente para recuperar el nombre de usuario:

```console
kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['username']}" | base64 -d
```

Ejecute el comando siguiente para recuperar la contraseña:

```console
kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['password']}" | base64 -d
```

## <a name="powershell"></a>PowerShell

Ejecute el comando siguiente para recuperar el nombre de usuario:

```console
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String((kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['username']}")))
```

Ejecute el comando siguiente para recuperar la contraseña:

```console
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String((kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['password']}")))
```

## <a name="next-steps"></a>Pasos siguientes

Pruebe otros [escenarios](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory-domain-services/scenarios.md).
