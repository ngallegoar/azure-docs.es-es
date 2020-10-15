---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: de79ea50d12ab322d1e28d0ad650df30ecc0c222
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74806596"
---
## <a name="install-client-library-packages"></a>Instalación de los paquetes de biblioteca cliente

> [!NOTE]
> Los ejemplos que se muestran aquí usan la biblioteca cliente de Azure Storage versión 12. La biblioteca cliente de la versión 12 forma parte de Azure SDK. Para más información sobre Azure SDK, consulte el repositorio de Azure SDK en [GitHub](https://github.com/Azure/azure-sdk).

Para instalar el paquete de almacenamiento de blobs, ejecute el siguiente comando desde la consola del administrador de paquetes NuGet:

```powershell
Install-Package Azure.Storage.Blobs
```

En los ejemplos que se muestran aquí también se usa la versión más reciente de la [biblioteca cliente de identidades de Azure para .NET](https://www.nuget.org/packages/Azure.Identity/) para autenticarse con credenciales de Azure AD. Para instalar el paquete, ejecute el siguiente comando desde la consola del administrador de paquetes NuGet:

```powershell
Install-Package Azure.Identity
```
