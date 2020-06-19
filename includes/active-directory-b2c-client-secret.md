---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: f227021f6cd71686eb58b43dc16a03d6fc010b83
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2020
ms.locfileid: "84672545"
---
#### <a name="app-registrations"></a>[Registros de aplicaciones](#tab/app-reg-ga/) 

1. En **Administrar**, seleccione **Certificados y secretos**.
1. Seleccione **Nuevo secreto de cliente**.
1. Escriba una descripción para el secreto de cliente en el cuadro **Descripción**. Por ejemplo, *clientsecret1*.
1. En **Expira**, seleccione el tiempo durante el cual el secreto es válido y, a continuación, seleccione **Agregar**.
1. Registre el **Valor** del secreto. Este valor se usa para la configuración en un paso posterior.

#### <a name="applications-legacy"></a>[Aplicaciones (heredado)](#tab/applications-legacy/)

1. En **ACCESO DE API**, seleccione **Claves**.
1. Escriba una descripción para la clave en el cuadro **Descripción de la clave**. Por ejemplo, *clientsecret1*.
1. Seleccione una **Duración** para la validez y, luego, **Guardar**.
1. Anote el **VALUE** de la clave. Este valor se usa para la configuración en un paso posterior.