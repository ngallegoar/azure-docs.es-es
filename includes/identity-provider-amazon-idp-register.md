---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 05/04/2020
ms.author: mimart
ms.openlocfilehash: de1b67265c5c3b3f7247b7f21506eed88abfd550
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2020
ms.locfileid: "82900362"
---
1. Inicie sesión en [Amazon Developer Console](https://developer.amazon.com/dashboard) con las credenciales de su cuenta de Amazon.
1. Si aún no lo ha hecho, haga clic en **Sign up**(Registro), siga los pasos de registro para desarrolladores y acepte la directiva.
1. En el panel, seleccione **Login with Amazon** (Iniciar sesión con Amazon).
1. Seleccione **Create a New Security Profile** (Crear perfil de seguridad).
1. Escriba un **Nombre del perfil de seguridad**, una **Descripción del perfil de seguridad** y una **URL del aviso de privacidad y consentimiento**; por ejemplo, `https://www.contoso.com/privacy`. La URL del aviso de privacidad es una página que usted administra y que proporciona información de privacidad a los usuarios. A continuación, haga clic en **Save**(Guardar).
1. En la sección **Login with Amazon Configurations** (Configuración del inicio de sesión con Amazon), seleccione el **nombre del perfil de seguridad** que creó, haga clic en el icono **Manage**(Administrar) y seleccione **Web Settings** (Configuración web).
1. En la sección **Web Settings** (Configuración web), copie los valores de **Client ID** (Identificador de cliente). Seleccione **Show secret** (Mostrar secreto) para obtener el secreto de cliente y, a continuación, cópielo. Necesitará los dos para configurar la cuenta de Amazon como proveedor de identidades de su inquilino. **secreto de cliente** es una credencial de seguridad importante.
1. En la sección **Web Settings** (Configuración web), seleccione **Edit** (Editar). En **Allowed Origins** (Orígenes permitidos) y **Allowed Return URLs** (URL de retorno permitidas), escriba las direcciones URL adecuadas (indicadas anteriormente). 
1. Haga clic en **Save**(Guardar).
