---
title: Implementación de asociaciones de aplicaciones administradas mediante Azure Policy
description: Aprenda a implementar asociaciones para aplicaciones administradas mediante el servicio Azure Policy.
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: ec0fe8f66ef2ad2458b4ffad0e848591793e5b05
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75649348"
---
# <a name="deploy-associations-for-a-managed-application-using-azure-policy"></a>Implementación de asociaciones para aplicaciones administradas mediante Azure Policy

Se pueden usar directivas de Azure para implementar asociaciones con el fin de asociar recursos a aplicaciones administradas. En este artículo se describe una directiva integrada que implementa asociaciones y el modo de usarla.

## <a name="built-in-policy-to-deploy-associations"></a>Directiva integrada para implementar asociaciones

La implementación de asociaciones para aplicaciones administradas es una directiva integrada que se puede usar para asociar un recurso a una aplicación administrada. La directiva acepta tres parámetros:

- Identificador de la aplicación administrada: es el identificador de recurso de la aplicación administrada al que se deben asociar los recursos.
- Tipos de recursos que se van a asociar: son la lista de tipos de recursos que se van a asociar a la aplicación administrada. Puede asociar varios tipos de recursos a una aplicación administrada con la misma directiva.
- Prefijo de nombre de asociación: esta cadena es el prefijo que se va a agregar al nombre del recurso de asociación que se va a crear. El valor predeterminado es "DeployedByPolicy".

La directiva usa la evaluación DeployIfNotExists, que se ejecuta después de que un proveedor de recursos ha administrado una solicitud de creación o actualización de recursos del tipo de recurso seleccionado y la evaluación ha devuelto un código de estado correcto. Luego, el recurso de asociación se implementa mediante una implementación de plantilla.
Para más información sobre las asociaciones, consulte [Información general sobre la incorporación de recursos de proveedores personalizados de Azure](../custom-providers/concepts-resource-onboarding.md).

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Uso de la directiva integrada de implementación de asociaciones 

### <a name="prerequisites"></a>Prerrequisitos
Si la aplicación administrada necesita permisos sobre la suscripción para realizar una acción, la implementación de los recursos de asociación de la directiva no funcionará sin conceder los permisos.

### <a name="policy-assignment"></a>Asignación de directiva
Para usar la directiva integrada, cree una asignación de directiva y asigne las asociaciones de implementación a una directiva de aplicación administrada. Cuando la directiva se haya asignado correctamente, identificará los recursos no compatibles e implementará la asociación de esos recursos.

![Asignación de la directiva integrada](media/concepts-built-in-policy/assign-builtin-policy-managedapp.png)

## <a name="getting-help"></a>Ayuda

Si tiene preguntas sobre el desarrollo de proveedores de recursos personalizados de Azure, haga su consulta en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Puede que ya se haya respondido a una pregunta similar, así que compruébelo antes de publicar la suya. Agregue la etiqueta ```azure-custom-providers``` para obtener una respuesta rápida.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprenderá a usar la directiva integrada para implementar asociaciones. Para obtener más información, vea estos artículos:

- [Conceptos: Incorporación de recursos de proveedores personalizados de Azure](../custom-providers/concepts-resource-onboarding.md)
- [Tutorial: Incorporación de recursos con proveedores personalizados](../custom-providers/tutorial-resource-onboarding.md)
- [Tutorial: Creación de acciones y recursos personalizados en Azure](../custom-providers/tutorial-get-started-with-custom-providers.md)
- [Inicio rápido: Creación de un proveedor de recursos personalizados e implementación de recursos personalizados](../custom-providers/create-custom-provider.md)
- [Cómo: Incorporación de acciones personalizadas a una API REST de Azure](../custom-providers/custom-providers-action-endpoint-how-to.md)
- [Cómo: Incorporación de recursos personalizados a una API REST de Azure](../custom-providers/custom-providers-resources-endpoint-how-to.md)
