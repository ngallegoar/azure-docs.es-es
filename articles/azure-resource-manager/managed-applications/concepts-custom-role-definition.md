---
title: Información general de definiciones de roles personalizados
description: Se describe el concepto de creación de definiciones de roles personalizadas para las aplicaciones administradas.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 09/16/2019
ms.openlocfilehash: 7b7fff913c177703f959bfa103c8e310d01059e2
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81391823"
---
# <a name="custom-role-definition-artifact-in-azure-managed-applications"></a>Artefacto de definición de roles personalizada en Azure Managed Applications

La definición de roles personalizados es un artefacto opcional en las aplicaciones administradas. Se usa para determinar los permisos que necesita la aplicación administrada para realizar sus funciones.

En este artículo se proporciona información general sobre el artefacto de definición de roles personalizada y sus funciones.

## <a name="custom-role-definition-artifact"></a>Artefacto de definición de roles personalizada

Debe asignar un nombre al artefacto de definición de roles personalizada customRoleDefinition.json. Colóquelo en el mismo nivel que createUiDefinition.json y mainTemplate.json en el paquete .zip que crea una definición de aplicación administrada. Para obtener información sobre cómo crear el paquete .zip y publicar una definición de aplicación administrada, vea [Publicación de una definición de aplicación administrada](publish-service-catalog-app.md).

## <a name="custom-role-definition-schema"></a>Esquema de definición de roles personalizada

El archivo customRoleDefinition.json tiene una propiedad `roles` de nivel superior que es una matriz de roles. Estos roles son los permisos que la aplicación administrada necesita para funcionar. Actualmente, solo se permiten roles integrados, pero se pueden especificar varios roles. El identificador de la definición de rol o el nombre de rol pueden hacer referencia a un rol.

Ejemplo de JSON para la definición de roles personalizada:

```json
{
    "contentVersion": "0.0.0.1",
    "roles": [
        {
            "properties": {
                "roleName": "Contributor"
            }
        },
        {
            "id": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
        },
        {
            "id": "/providers/Microsoft.Authorization/roledefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
        }
    ]
}
```

## <a name="roles"></a>Roles

Un rol se compone de un elemento `$.properties.roleName` o `id`:

```json
{
    "id": null,
    "properties": {
        "roleName": "Contributor"
    }
}
```

> [!NOTE]
> Se puede usar el campo `id` o `roleName`. Solo es necesario uno. Estos campos se utilizan para buscar la definición de roles que se debe aplicar. Si se proporcionan ambos, se usará el campo `id`.

|Propiedad|¿Necesario?|Descripción|
|---------|---------|---------|
|id|Sí|Identificador del rol integrado. Se puede usar el identificador completo o simplemente el GUID.|
|roleName|Sí|Nombre del rol integrado.|