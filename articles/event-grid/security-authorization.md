---
title: Seguridad y autenticación de Azure Event Grid
description: Describe Azure Event Grid y sus conceptos.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 09317b310a5934d27b82d265ec7f9b276135f882
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86119096"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Autorización de acceso a recursos de Event Grid
Azure Event Grid permite controlar el nivel de acceso dado a distintos usuarios para realizar diversas operaciones de administración, como enumerar las suscripciones a eventos, crear otras nuevas y generar claves. Event Grid usa el control de acceso basado en rol (RBAC) de Azure.


## <a name="operation-types"></a>Tipos de operación

Event Grid admite las siguientes acciones:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Las tres últimas operaciones devuelven información potencialmente confidencial, la cual se filtra de las operaciones de lectura normales. Se recomienda restringir el acceso a estas operaciones. 

## <a name="built-in-roles"></a>Roles integrados

Event Grid proporciona dos roles integrados para administrar las suscripciones de eventos. Son importantes al implementar [dominios de eventos](event-domains.md) porque proporcionan a los usuarios los permisos que necesitan para suscribirse a temas en el dominio del evento. Estos roles se centran en las suscripciones de eventos y no conceden acceso para acciones como la creación de temas.

También puede [asignar estos roles a un usuario o grupo](../role-based-access-control/quickstart-assign-role-user-portal.md).

**Colaborador de EventGrid EventSubscription**: administre las operaciones de suscripción de Event Grid.

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**Lector de EventGrid EventSubscription**: lea las suscripciones de Event Grid.

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="custom-roles"></a>Roles personalizados

Si tiene que especificar permisos distintos a los de los roles integrados, puede crear roles personalizados.

Las siguientes son definiciones de roles de Event Grid de ejemplo que permiten a los usuarios realizar distintas acciones. Estos roles personalizados son diferentes de los roles integrados, ya que conceden acceso más amplio que las suscripciones a eventos.

**EventGridReadOnlyRole.json**: Únicamente se permiten operaciones de solo lectura.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: permitir acciones de publicación restringidas pero denegar acciones de eliminación.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: Permite todas las acciones de Event Grid.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

Puede crear roles personalizados con [PowerShell](../role-based-access-control/custom-roles-powershell.md), la [CLI de Azure](../role-based-access-control/custom-roles-cli.md) o [REST](../role-based-access-control/custom-roles-rest.md).



### <a name="encryption-at-rest"></a>Cifrado en reposo

Todos los eventos o datos escritos en el disco por el servicio Event Grid se cifran mediante una clave administrada por Microsoft, lo que garantiza que se cifran en reposo. Además, el período máximo que se conservan los eventos o los datos es de 24 horas, conforme a la [directiva de reintentos de Event Grid](delivery-and-retry.md). Event Grid elimina automáticamente todos los eventos o datos tras 24 horas, o el período de vida del evento, lo que sea menor.

## <a name="permissions-for-event-subscriptions"></a>Permisos para suscripciones a eventos
Si usa un controlador de eventos que no sea un WebHook (como un almacenamiento de cola o un centro de eventos), necesita acceso de escritura a ese recurso. Esta comprobación de permisos impide que un usuario no autorizado envíe eventos al recurso.

Debe tener el permiso **Microsoft.EventGrid/EventSubscriptions/Write** en el recurso que constituya el origen del evento. Necesita este permiso porque está escribiendo una nueva suscripción en el ámbito del recurso. El recurso requerido difiere en función de si se suscribe a un tema del sistema o a un tema personalizado. Ambos tipos se describen en esta sección.

### <a name="system-topics-azure-service-publishers"></a>Temas del sistema (editores del servicio de Azure)
Para temas del sistema, necesita permiso para escribir una nueva suscripción a eventos en el ámbito del recurso que publica el evento. El formato del recurso es: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Por ejemplo, para suscribirse a un evento en una cuenta de almacenamiento denominada **myacct**, necesita el permiso Microsoft.EventGrid/EventSubscriptions/Write en:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Temas personalizados
Para temas personalizados, necesita permiso para escribir una nueva suscripción a eventos en el ámbito del tema de Event Grid. El formato del recurso es: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Por ejemplo, para suscribirse a un tema personalizado denominado **mytopic**, necesita el permiso Microsoft.EventGrid/EventSubscriptions/Write en: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`



## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a Event Grid, vea [About Event Grid](overview.md) (Acerca de Event Grid).
