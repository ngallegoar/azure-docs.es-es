---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 3ebe1ec4c0292a530e5ef2c754e9b002e931300e
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95994333"
---
#### <a name="app-registrations"></a>[Registros de aplicaciones](#tab/app-reg-ga/) 

1. Seleccione **App registrations** (Registros de aplicaciones).
1. Seleccione la aplicación *webapi1* para abrir su página **Información general**.
1. En **Administrar**, seleccione **Exponer una API**.
1. Junto a **URI de id. de aplicación**, seleccione el vínculo **Establecer**.
1. Reemplace el valor predeterminado (un GUID) por `api` y, a continuación, seleccione **Guardar**. Se muestra el URI completo, que debe tener el formato `https://your-tenant-name.onmicrosoft.com/api`. Cuando la aplicación web solicita un token de acceso para la API, debe agregar este URI como prefijo para cada ámbito que se defina para la API.
1. En **Ámbitos definidos con esta API**, seleccione **Agregar un ámbito**.
1. Escriba los valores siguientes para crear un ámbito que defina el acceso de lectura a la API y, a continuación, seleccione **Agregar ámbito**:
    1. **Nombre de ámbito**: `demo.read`
    1. **Nombre para mostrar del consentimiento del administrador**: `Read access to demo API`
    1. **Descripción del consentimiento del administrador**: `Allows read access to the demo API`
1. Seleccione **Agregar un ámbito**, escriba los valores siguientes para agregar un ámbito que defina el acceso de escritura a la API y, a continuación, seleccione **Agregar ámbito**:
    1. **Nombre de ámbito**: `demo.write`
    1. **Nombre para mostrar del consentimiento del administrador**: `Write access to demo API`
    1. **Descripción del consentimiento del administrador**: `Allows write access to the demo API`

#### <a name="applications-legacy"></a>[Aplicaciones (heredado)](#tab/applications-legacy/)

1. Seleccione **Aplicaciones (heredado)** .
1. Seleccione la aplicación *webapi1* para abrir su página **Propiedades**.
1. Seleccione **Ámbitos publicados**. Los ámbitos publicados se pueden utilizar para conceder a una aplicación cliente ciertos permisos para la API web.
1. En **ÁMBITO** escriba `demo.read` y en **DESCRIPCIÓN**, `Read access to the web API`.
1. En **ÁMBITO** escriba `demo.write` y en **DESCRIPCIÓN**, `Write access to the web API`.
1. Seleccione **Guardar**.