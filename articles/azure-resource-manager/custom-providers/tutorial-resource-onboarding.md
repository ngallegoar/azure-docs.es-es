---
title: 'Tutorial: Incorporación de recursos'
description: La incorporación de recursos a través de proveedores personalizados le permite manipular y ampliar los recursos existentes de Azure.
ms.topic: tutorial
ms.author: jobreen
author: jjbfour
ms.date: 09/17/2019
ms.openlocfilehash: 22d1dcd997a4ddb94aba184c5dace4c00509054d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75648612"
---
# <a name="tutorial-resource-onboarding-with-azure-custom-providers"></a>Tutorial: Incorporación de recursos con proveedores personalizados de Azure

En este tutorial, va a implementar en Azure un proveedor de recursos personalizado que amplía la API de Azure Resource Manager con el tipo de recursos Microsoft.CustomProviders/associations. En el tutorial se muestra cómo ampliar los recursos existentes que se encuentran fuera del grupo de recursos donde se encuentra la instancia del proveedor personalizado. En este tutorial, el proveedor de recursos personalizado se basa en una aplicación lógica de Azure, pero puede usar cualquier punto de conexión de API público.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita saber lo siguiente:

* Las funcionalidades de los [proveedores personalizados de Azure](overview.md).
* La información básica sobre la [incorporación de recursos con proveedores personalizados](concepts-resource-onboarding.md).

## <a name="get-started-with-resource-onboarding"></a>Introducción a la incorporación de recursos

En este tutorial, hay dos partes que se deben implementar: el proveedor personalizado y la asociación. Para facilitar el proceso, tiene la opción de usar una sola plantilla que implemente ambos.

La plantilla usará estos recursos:

* Microsoft.CustomProviders/resourceProviders
* Microsoft.Logic/workflows
* Microsoft.CustomProviders/associations

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "allowedValues": [
                "australiaeast",
                "eastus",
                "westeurope"
            ],
            "metadata": {
                "description": "Location for the resources."
            }
        },
        "logicAppName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
            "metadata": {
                "description": "Name of the logic app to be created."
            }
        },
        "customResourceProviderName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
            "metadata": {
                "description": "Name of the custom provider to be created."
            }
        },
        "customResourceProviderId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The resource ID of an existing custom provider. Provide this to skip deployment of new logic app and custom provider."
            }
        },
        "associationName": {
            "type": "string",
            "defaultValue": "myAssociationResource",
            "metadata": {
                "description": "Name of the custom resource that is being created."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2017-05-10",
            "condition": "[empty(parameters('customResourceProviderId'))]",
            "name": "customProviderInfrastructureTemplate",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "logicAppName": {
                            "type": "string",
                            "defaultValue": "[parameters('logicAppName')]"
                        }
                    },
                    "resources": [
                        {
                            "type": "Microsoft.Logic/workflows",
                            "apiVersion": "2017-07-01",
                            "name": "[parameters('logicAppName')]",
                            "location": "[parameters('location')]",
                            "properties": {
                                "state": "Enabled",
                                "definition": {
                                    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
                                    "actions": {
                                        "Switch": {
                                            "cases": {
                                                "Case": {
                                                    "actions": {
                                                        "CreateCustomResource": {
                                                            "inputs": {
                                                                "body": {
                                                                    "properties": "@addProperty(triggerBody().Body['properties'], 'myDynamicProperty', 'myDynamicValue')"
                                                                },
                                                                "statusCode": 200
                                                            },
                                                            "kind": "Http",
                                                            "type": "Response"
                                                        }
                                                    },
                                                    "case": "CREATE"
                                                }
                                            },
                                            "default": {
                                                "actions": {
                                                    "DefaultHttpResponse": {
                                                        "inputs": {
                                                            "statusCode": 200
                                                        },
                                                        "kind": "Http",
                                                        "type": "Response"
                                                    }
                                                }
                                            },
                                            "expression": "@triggerBody().operationType",
                                            "type": "Switch"
                                        }
                                    },
                                    "contentVersion": "1.0.0.0",
                                    "outputs": {},
                                    "parameters": {},
                                    "triggers": {
                                        "CustomProviderWebhook": {
                                            "inputs": {
                                                "schema": {}
                                            },
                                            "kind": "Http",
                                            "type": "Request"
                                        }
                                    }
                                }
                            }
                        },
                        {
                            "type": "Microsoft.CustomProviders/resourceProviders",
                            "apiVersion": "2018-09-01-preview",
                            "name": "[parameters('customResourceProviderName')]",
                            "location": "[parameters('location')]",
                            "properties": {
                                "resourceTypes": [
                                    {
                                        "name": "associations",
                                        "mode": "Secure",
                                        "routingType": "Webhook,Cache,Extension",
                                        "endpoint": "[[listCallbackURL(concat(resourceId('Microsoft.Logic/workflows', parameters('logicAppName')), '/triggers/CustomProviderWebhook'), '2017-07-01').value]"
                                    }
                                ]
                            }
                        }
                    ],
                    "outputs": {
                        "customProviderResourceId": {
                            "type": "string",
                            "value": "[resourceId('Microsoft.CustomProviders/resourceProviders', parameters('customResourceProviderName'))]"
                        }
                    }
                }
            }
        },
        {
            "type": "Microsoft.CustomProviders/associations",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('associationName')]",
            "location": "global",
            "properties": {
                "targetResourceId": "[if(empty(parameters('customResourceProviderId')), reference('customProviderInfrastructureTemplate').outputs.customProviderResourceId.value, parameters('customResourceProviderId'))]",
                "myCustomInputProperty": "myCustomInputValue",
                "myCustomInputObject": {
                    "Property1": "Value1"
                }
            }
        }
    ],
    "outputs": {
        "associationResource": {
            "type": "object",
            "value": "[reference(parameters('associationName'), '2018-09-01-preview', 'Full')]"
        }
    }
}
```

### <a name="deploy-the-custom-provider-infrastructure"></a>Implementación de la infraestructura de proveedores personalizados

La primera parte de la plantilla implementa la infraestructura de proveedores personalizados. Esta infraestructura define el efecto del recurso de asociaciones. Si no está familiarizado con los proveedores personalizados, consulte los [aspectos básicos sobre los proveedores personalizados](overview.md).

Vamos a implementar la infraestructura de proveedores personalizados. Copie, guarde e implemente la plantilla anterior, o bien siga e implemente la infraestructura mediante Azure Portal.

1. Vaya a [Azure Portal](https://portal.azure.com).

2. Busque **plantillasen** en **Todos los servicios** o mediante el cuadro de búsqueda principal:

   ![Buscar plantillas](media/tutorial-resource-onboarding/templates.png)

3. Seleccione **Agregar** en el panel **Plantillas**:

   ![Selección de Agregar](media/tutorial-resource-onboarding/templatesadd.png)

4. En **General**, escriba un **Nombre** y una **Descripción** para la nueva plantilla:

   ![Nombre y descripción de la plantilla](media/tutorial-resource-onboarding/templatesdescription.png)

5. Para crear la plantilla de Resource Manager, copie la plantilla JSON de la sección "Introducción a la incorporación de recursos" de este artículo:

   ![Creación de una plantilla de Resource Manager](media/tutorial-resource-onboarding/templatesarmtemplate.png)

6. Seleccione **Agregar** para crear la plantilla. Si la nueva plantilla no aparece, seleccione **Actualizar**.

7. Seleccione la plantilla recién creada y, luego, seleccione **Implementar**:

   ![Seleccione la nueva plantilla y, luego, seleccione Implementar](media/tutorial-resource-onboarding/templateselectspecific.png)

8. Escriba la configuración de los campos obligatorios y, a continuación, seleccione la suscripción y el grupo de recursos. Puede dejar vacío el cuadro **Custom Resource Provider Id** (Id. de proveedor de recursos personalizado).

   | Nombre del valor | ¿Necesario? | Descripción |
   | ------------ | -------- | ----------- |
   | Location | Sí | Ubicación de los recursos en la plantilla. |
   | Nombre de la aplicación lógica | No | nombre de la aplicación lógica. |
   | Custom Resource Provider Name (Nombre del proveedor de recursos personalizado) | No | Nombre del proveedor de recursos personalizado. |
   | Custom Resource Provider Id (Id. de proveedor de recursos personalizado) | No | Proveedor de recursos personalizado existente que admite el recurso de asociación. Si especifica un valor aquí, se omitirá la implementación de la aplicación lógica y del proveedor personalizado. |
   | Nombre de asociación | No | Nombre del recurso de asociación. |

   Parámetros de ejemplo:

   ![Escribir los parámetros de plantilla](media/tutorial-resource-onboarding/templatescustomprovider.png)

9. Vaya a la implementación y espere a que finalice. Debe ver algo parecido a la captura de pantalla siguiente. Debería ver el nuevo recurso de asociación como salida:

   ![Implementación correcta](media/tutorial-resource-onboarding/customproviderdeployment.png)

   Aquí está el grupo de recursos, con **Mostrar tipos ocultos** seleccionado:

   ![Implementación del proveedor personalizado](media/tutorial-resource-onboarding/showhidden.png)

10. Explore la pestaña **Historial de ejecuciones** de la aplicación lógica para ver las llamadas para la creación de la asociación:

    ![Historial de ejecuciones de la aplicación lógica](media/tutorial-resource-onboarding/logicapprun.png)

## <a name="deploy-additional-associations"></a>Implementar asociaciones adicionales

Una vez configurada la infraestructura de proveedores personalizados, puede implementar fácilmente más asociaciones. No es necesario que el grupo de recursos para las asociaciones adicionales sea el mismo que el grupo de recursos en el que ha implementado la infraestructura de proveedores personalizados. Para crear una asociación, debe tener permisos de Microsoft.CustomProviders/resourceproviders/write en el identificador del proveedor de recursos personalizado especificado.

1. Vaya al recurso del proveedor personalizado **Microsoft.CustomProviders/resourceProviders** en el grupo de recursos de la implementación anterior. Tendrá que activar la casilla **Mostrar tipos ocultos**:

   ![Ir al recurso](media/tutorial-resource-onboarding/showhidden.png)

2. Copie la propiedad Id. de recurso del proveedor personalizado.

3. Busque **plantillasen** en **Todos los servicios** o mediante el cuadro de búsqueda principal:

   ![Buscar plantillas](media/tutorial-resource-onboarding/templates.png)

4. Seleccione la plantilla creada anteriormente y, luego, seleccione **Implementar**:

   ![Seleccione la plantilla creada anteriormente y, luego, seleccione Implementar](media/tutorial-resource-onboarding/templateselectspecific.png)

5. Escriba la configuración de los campos obligatorios y, a continuación, seleccione la suscripción y otro grupo de recursos. Para la opción **Custom Resource Provider Id** (Id. de proveedor de recursos personalizado), escriba el identificador de recurso que copió del proveedor personalizado que implementó anteriormente.

6. Vaya a la implementación y espere a que finalice. Ahora debería implementar solo el nuevo recurso de asociaciones:

   ![Nuevo recurso de asociaciones](media/tutorial-resource-onboarding/createdassociationresource.png)

Si lo desea, puede volver al **Historial de ejecuciones** de la aplicación lógica y ver que se hizo otra llamada a la aplicación lógica. Puede actualizar la aplicación lógica para aumentar la funcionalidad adicional de cada asociación creada.

## <a name="getting-help"></a>Ayuda

Si tiene preguntas sobre los proveedores personalizados de Azure, haga su consulta en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Puede que ya se haya respondido a una pregunta similar, así que compruébelo antes de publicar la suya. Agregue la etiqueta `azure-custom-providers` para obtener una respuesta rápida.

