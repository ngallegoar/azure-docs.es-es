---
title: Migración al recurso de Azure para la creación
titleSuffix: Azure Cognitive Services
description: Migre a una clave de recurso de creación de Azure.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: diberry
ms.openlocfilehash: ec6f9592a4c149be382fab66cca27d929644d988
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194516"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migración a una clave de creación de recursos de Azure

La autenticación de creación de Language Understanding (LUIS) cambió de una cuenta de correo electrónico a un recurso de Azure. Aunque actualmente no es necesario, se aplicará el cambio a un recurso de Azure en el futuro.

## <a name="why-migrate"></a>¿Por qué migrar?

El uso de un recurso de Azure para la creación le permite, como propietario del recurso, controlar el acceso a la creación. Puede crear recursos de creación y asignarles nombres para administrar distintos grupos de autores.

Por ejemplo, usted es el propietario de dos aplicaciones LUIS y tiene miembros diferentes que son colaboradores en cada una de ellas. Puede crear dos recursos de creación diferentes y asignar cada aplicación a cada uno de ellos. Luego, asigne todos los miembros como colaboradores al recurso de creación adecuado en función de la aplicación en la que colaboren. El recurso de creación de Azure controla la autorización.

> [!Note]
> Antes de la migración, los coautores se conocen como _colaboradores_ en el nivel de aplicación de LUIS. Después de la migración, el rol de _colaborador_ de Azure se usa para la misma funcionalidad, pero en el nivel de recurso de Azure.

## <a name="what-is-migrating"></a>¿Qué es la migración?

La migración incluye:

* Todos los usuarios de LUIS, propietarios y colaboradores.
* **Todas** las aplicaciones.
* Una migración **unidireccional**.

El propietario no puede elegir un subconjunto de las aplicaciones que se van a migrar y el proceso no es reversible.

La migración no es:

* Un proceso que recopila colaboradores y que se mueve o agrega automáticamente al recurso de creación de Azure. Como propietario de la aplicación, usted debe completar este paso. Este paso requiere permisos para el recurso adecuado.
* Proceso para crear y asignar un recurso de predicción en tiempo de ejecución. Si necesita un recurso de predicción en tiempo de ejecución, se trata de [un proceso independiente](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) que no se modifica.

## <a name="how-are-the-apps-migrating"></a>¿Cómo se migran las aplicaciones?

El [portal de LUIS](https://www.luis.ai) proporciona el proceso de migración.

Se le pedirá que realice la migración si:

* Tiene aplicaciones en el sistema de autenticación por correo electrónico para la creación.
* Y usted es el propietario de la aplicación.

Puede retrasar el proceso de migración si cancela fuera de la ventana. Se le pedirá periódicamente que realice la migración hasta que migre o se pase la fecha límite de la migración. Puede iniciar el proceso de migración desde el icono de candado de la barra de navegación superior.

## <a name="migration-for-the-app-owner"></a>Migración para el propietario de la aplicación

### <a name="before-you-migrate"></a>Antes de la migración

* **Obligatorio**, debe tener una [suscripción a Azure](https://azure.microsoft.com/free/). Una parte del proceso de suscripción requiere información de facturación. Sin embargo, puede usar el plan de tarifa gratis (`F0`) cuando use LUIS.
* **De manera opcional**, haga una copia de seguridad de las aplicaciones desde la lista de aplicaciones del portal de LUIS; para ello, exporte cada aplicación o use la [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) de exportación.
* **Opcionalmente**, guarde la lista del colaborador de cada aplicación. Esta lista de correo electrónico se proporciona como parte del proceso de migración.


**La creación de la aplicación de LUIS es gratuita**, lo que viene indicado por el nivel `F0`. [Más información sobre planes de tarifa](luis-boundaries.md#key-limits).

Si no tiene una suscripción a Azure, [suscríbase](https://azure.microsoft.com/free/).

### <a name="migration-steps"></a>Pasos de migración

Siga [estos pasos de migración](luis-migration-authoring-steps.md).

### <a name="after-you-migrate"></a>Después de migrar

Después del proceso de migración, todas las aplicaciones de LUIS están asignadas ahora a un único recurso de creación de LUIS.

Puede crear más recursos de creación y asignar desde la página **Administración -> Recursos de Azure** del _portal de LUIS_.

Puede agregar colaboradores al recurso de creación desde _Azure Portal_, en la página **Access Control (IAM)** correspondiente a ese recurso. Para más información, consulte la sección sobre [agregar acceso para un colaborador](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

|Portal|Propósito|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Crear recursos de predicción y creación.<br>* Asignar colaboradores.|
|[LUIS](https://www.luis.ai)|* Migrar a recursos de creación nuevos.<br>* Asignar o anular la asignación de recursos de predicción y creación a las aplicaciones desde la página **Administrar -> Recursos de Azure**.|

## <a name="migration-for-the-app-contributor"></a>Migración para el colaborador de la aplicación

Se deben migrar todos los usuarios de LUIS, incluidos los colaboradores. Los colaboradores deben migrar para tener acceso a la aplicación.

> [!Note]
> Si el propietario de la aplicación de LUIS ha realizado la migración y ha agregado el usuario como colaborador en el recurso de Azure, el colaborador seguirá sin tener acceso a la aplicación, salvo que también migre.

### <a name="before-the-app-is-migrated"></a>Antes de migrar la aplicación

Puede optar por exportar una aplicación en la que sea colaborador y, a continuación, volver a importar la aplicación en LUIS. El proceso de importación crea una aplicación nueva con un nuevo identificador de aplicación, para el que usted es el propietario.

### <a name="after-the-app-is-migrated"></a>Después de migrar la aplicación

El propietario de la aplicación debe [agregar su correo electrónico al recurso de creación de Azure como colaborador](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource).

Después del proceso de migración, todas las aplicaciones que posea estarán disponibles en la página **Aplicaciones** del portal de Luis.

## <a name="troubleshooting-the-migration-process-for-luis-authoring"></a>Solución de problemas del proceso de migración para la creación de LUIS

* Las claves de creación de LUIS solo se pueden ver en el portal de LUIS cuando se haya completado el proceso de migración. Aunque cree las claves de creación, como con la CLI de LUIS, el usuario debe completar el proceso de migración en el portal de LUIS.
* Si un usuario migrado agrega un usuario no migrado como colaborador a su recurso de Azure, este no tendrá acceso a las aplicaciones, salvo que migre.
* Si un usuario no migrado no es propietario de ninguna aplicación, pero es un colaborador de otras aplicaciones que son propiedad de otros usuarios y los propietarios han pasado por el proceso de migración, este usuario tendrá que migrar para tener acceso a las aplicaciones.
* Si un usuario no migrado ha agregado otro usuario migrado como colaborador a su aplicación, se producirá un error, ya que no podrá agregar un usuario migrado como colaborador a una aplicación. El usuario no migrado tendrá que pasar por el proceso de migración y crear un recurso de Azure y agregar el usuario migrado como colaborador a ese recurso.

Recibirá un error durante el proceso de migración en los siguientes casos:
* La suscripción no le autoriza a crear recursos de Cognitive Services.
* La migración afecta negativamente a cualquier entorno de ejecución de aplicaciones. Al migrar, se quitan todos los colaboradores de sus aplicaciones y se le quita como colaborador de otras aplicaciones. Este proceso implica que también se quitan las claves asignadas. La migración se bloquea si ha asignado claves en otras aplicaciones. Quite la clave que asignó de forma segura antes de la migración. Si sabe que la clave que asignó no se utiliza en el entorno de ejecución, debe quitarla para poder continuar en la migración.

Acceda a la lista de recursos de Azure de la aplicación con el siguiente formato de dirección URL:

`https://www.luis.ai/applications/REPLACE-WITH-YOUR-APP-ID/versions/REPLACE-WITH-YOUR-VERSION-ID/manage/resources`

## <a name="next-steps"></a>Pasos siguientes

* [Migración de una aplicación a un recurso de creación](luis-migration-authoring-steps.md)
