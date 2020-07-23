---
title: Uso de escalado automático para enviar notificaciones de alertas de webhook y correo electrónico
description: Aprenda a usar las acciones de escalado automático para llamar a direcciones URL web o enviar notificaciones de correo electrónico en Azure Monitor.
ms.topic: conceptual
ms.date: 04/03/2017
ms.subservice: autoscale
ms.openlocfilehash: 3b1f13fd1ce8bedcbe58385d4cee321f1d1405df
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505526"
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Uso de acciones de escalado automático para enviar notificaciones de alerta por correo electrónico y Webhook en Azure Monitor
En este artículo se muestra cómo configurar desencadenadores para que pueda llamar a direcciones URL web específicas o enviar mensajes de correo electrónico en función de las acciones de escalado automático en Azure.  

## <a name="webhooks"></a>webhooks
Los Webhook permiten enrutar las notificaciones de alerta de Azure a otros sistemas para procesarlas posteriormente o notificaciones personalizadas. Un ejemplo de esto puede ser el enrutamiento de la alerta a los servicios que pueden controlar una solicitud web entrante para enviar SMS, registrar errores o notificar a un equipo mediante servicios de chat y mensajería. El identificador URI de webhook debe ser un punto de conexión HTTP o HTTPS válido.

## <a name="email"></a>Email
Se pueden enviar mensajes de correo electrónico a cualquier dirección de correo electrónico válida. También se enviará una notificación a los administradores y los coadministradores de la suscripción en la que se ejecuta la regla.

## <a name="cloud-services-and-app-services"></a>Cloud Services y App Services
Puede participar desde Azure Portal en Cloud Services y granjas de servidores (App Services).

* Elija la **Escalar por** .

![Escalar por](./media/autoscale-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Conjuntos de escalado de máquina virtual
Para las máquinas virtuales más recientes creadas con Resource Manager (conjuntos de escala de máquina virtual), puede configurar esto con la API de REST, las plantillas de Resource Manager, PowerShell y CLI. Aún no se encuentra disponible una interfaz de portal.
Al utilizar la API REST o la plantilla de Resource Manager, incluya el elemento de notificaciones en el objeto [autoscalesettings](/azure/templates/microsoft.insights/2015-04-01/autoscalesettings) con las siguientes opciones.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```

| Campo | ¿Obligatorio? | Descripción |
| --- | --- | --- |
| operation |sí |el valor debe ser "Scale" |
| sendToSubscriptionAdministrator |sí |el valor debe ser "true" o "false" |
| sendToSubscriptionCoAdministrators |sí |el valor debe ser "true" o "false" |
| customEmails |sí |el valor puede ser null [] o una cadena de matriz de mensajes de correo electrónico |
| webhooks |sí |el valor puede ser null o un identificador URI válido |
| serviceUri |sí |un identificador URI de https válido |
| properties |sí |el valor debe ser {} vacío o puede contener pares clave-valor |

## <a name="authentication-in-webhooks"></a>Autenticación en Webhook
El webhook puede realizar la autenticación con un método basado en token, en el que el URI del webhook se guarda con un identificador de token como un parámetro de consulta. Por ejemplo, https:\//mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Esquema de carga de Webhook de notificación de escalado automático
Cuando se genera la notificación de escalado automático, los metadatos siguientes se incluyen en la carga de Webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Campo | ¿Obligatorio? | Descripción |
| --- | --- | --- |
| status |sí |Estado que indica que se ha generado una acción de escalado automático. |
| operation |sí |Para un aumento de instancias, será "Escalar horizontalmente"; para una disminución de instancias, "Reducir horizontalmente". |
| context |sí |Contexto de la acción de escalado automático. |
| timestamp |sí |Marca de tiempo cuando se desencadena la acción de escalado automático. |
| id |Sí |Id. de Resource Manager de la configuración de escalado automático |
| name |Sí |Nombre de la configuración de escalado automático. |
| detalles |Sí |Explicación de la acción que realizó el servicio de escalado automático y el cambio en el recuento de instancias. |
| subscriptionId |Sí |Id. de suscripción del recurso de destino que se está escalando. |
| resourceGroupName |Sí |Nombre del grupo de recursos del recurso de destino que se está escalando. |
| resourceName |Sí |Nombre del recurso de destino que se está escalando. |
| resourceType |Sí |Los tres valores admitidos: "microsoft.classiccompute/domainnames/slots/roles" - Roles del servicio en la nube, "microsoft.compute/virtualmachinescalesets" - Conjuntos de escalado de máquina virtual "Microsoft.Web/serverfarms" - Web App |
| resourceId |Sí |Id. de Resource Manager del recurso de destino que se está escalando |
| portalLink |Sí |Vínculo del portal de Azure a la página de resumen del recurso de destino. |
| oldCapacity |Sí |Recuento de instancias (antiguo) actual cuando el escalado automático ha realizado una acción de escalado. |
| newCapacity |Sí |Nuevo recuento de instancias al que el escalado automático escaló el recurso. |
| properties |No |Opcional. Conjunto de pares <Clave, Valor> (por ejemplo, Diccionario <Cadena, Cadena>). El campo de propiedades es opcional. En una interfaz de usuario personalizada o un flujo de trabajo basado en una aplicación lógica, puede escribir las claves y los valores que se pueden transmitir utilizando la carga. La forma alternativa para transmitir propiedades personalizadas a la llamada de Webhook de salida es mediante el propio URI de Webhook (como parámetros de consulta). |
