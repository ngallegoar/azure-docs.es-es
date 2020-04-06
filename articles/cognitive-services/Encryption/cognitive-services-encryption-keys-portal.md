---
title: Uso de Azure Portal para configurar las claves administradas por el cliente
titleSuffix: Cognitive Services
description: Aprenda a usar Azure Portal para configurar las claves administradas por el cliente con Azure Key Vault. Las claves administradas por el cliente le permiten crear, rotar, deshabilitar y revocar los controles de acceso.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 22bd3afcf30b8b8ebce18b22d5419d49ec8c3b4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053603"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Configuración de claves administradas por el cliente con Azure Key Vault mediante Azure Portal

Debe usar Azure Key Vault para almacenar las claves administradas por el cliente. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. El recurso de Cognitive Services y el almacén de claves deben estar en la misma región y en el mismo inquilino de Azure Active Directory (Azure AD), pero pueden estar en distintas suscripciones. Para más información sobre Azure Key Vault, vea [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

En este artículo se muestra cómo configurar Azure Key Vault con claves administradas del cliente mediante [Azure Portal](https://portal.azure.com/). Para obtener información sobre cómo crear un almacén de claves mediante Azure Portal, consulte [Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure Portal](../../key-vault/quick-create-portal.md).

## <a name="configure-azure-key-vault"></a>Configuración de Azure Key Vault

Para usar las claves administradas por el cliente, es necesario establecer dos propiedades en el almacén de claves, **Eliminación temporal** y **No purgar**. Estas propiedades no están habilitadas de forma predeterminada, pero se pueden habilitar mediante PowerShell o la CLI de Azure tanto en un almacén de claves nuevo como en uno existente.

> [!IMPORTANT]
> Si no tiene habilitadas las propiedades **Eliminación temporal** y **No purgar** y elimina la clave, no podrá recuperar los datos en el recurso de Cognitive Services.

Para aprender a habilitar estas propiedades en un almacén de claves existente, consulte las secciones **Habilitación de la eliminación temporal** y **Habilitación de la protección de purgas** en cualquiera de los siguientes artículos:

- [Uso de la eliminación temporal con PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell).
- [Uso de la eliminación temporal con la CLI](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli).

Las claves RSA de tamaño 2048 son las únicas que admite el cifrado de Azure Storage. Para más información acerca de las claves, consulte la sección **Claves en Key Vault** en [Información acerca de claves, secretos y certificados de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys).

## <a name="enable-customer-managed-keys"></a>Habilitar claves administradas del cliente

Para habilitar claves administradas del cliente en Azure Portal, siga estos pasos:

1. Vaya al recurso de Cognitive Services.
1. En la hoja **Configuración** del recurso de Cognitive Services, haga clic en **Cifrado**. Seleccione la opción **Claves administradas de cliente**, como se muestra en la ilustración siguiente.

    ![Captura de pantalla que muestra cómo seleccionar Claves administradas por el cliente](../media/cognitive-services-encryption/selectcmk.png)

## <a name="specify-a-key"></a>Especificar una clave

Después de habilitar las claves administradas por el cliente, tendrá la oportunidad de especificar una clave para asociarla con el recurso de Cognitive Services.

### <a name="specify-a-key-as-a-uri"></a>Especificación de una clave como URI

Para especificar una clave como URI, siga estos pasos:

1. Para buscar el URI de la clave en Azure Portal, vaya al almacén de claves y seleccione la opción de configuración **Claves**. Seleccione la clave que desee y luego haga clic en ella para ver sus versiones. Seleccione cualquiera de las versiones de clave para ver su configuración.
1. Copie el valor del campo **Identificador de clave**, que proporciona el URI.

    ![Captura de pantalla en que se muestra el URI de la clave del almacén de claves](../media/cognitive-services-encryption/key-uri-portal.png)

1. En las opciones de configuración de **cifrado** de la cuenta de almacenamiento, elija la opción **Introducir URI de la clave**.
1. Pegue el identificador URI que ha copiado en el campo **URI de clave**.

   ![Captura de pantalla en que se muestra cómo introducir el URI de la clave](../media/cognitive-services-encryption/ssecmk2.png)

1. Especifique el identificador de la suscripción que contiene el almacén de claves.
1. Guarde los cambios.

### <a name="specify-a-key-from-a-key-vault"></a>Especificación de una clave a partir de un almacén de claves

Para especificar una clave a partir de un almacén de claves, asegúrese de tener un almacén de claves que contenga una clave. Para especificar una clave a partir de un almacén de claves, siga estos pasos:

1. Elija la opción **Select from Key Vault** (Seleccionar desde almacén de claves).
1. Seleccione el almacén de claves que contiene la clave que desea usar.
1. Seleccione la clave en el almacén de claves.

   ![Captura de pantalla en que se muestra la opción de clave administrada del cliente](../media/cognitive-services-encryption/ssecmk3.png)

1. Guarde los cambios.

## <a name="update-the-key-version"></a>Actualización de la versión de la clave

Al crear una versión de una clave, actualice el recurso de Cognitive Services para usar la nueva versión. Siga estos pasos:

1. Vaya al recurso de Cognitive Services y muestre la configuración de **Cifrado**.
1. Escriba el identificador URI de la nueva versión de la clave. Como alternativa, puede volver a seleccionar el almacén de claves y la clave para actualizar la versión.
1. Guarde los cambios.

## <a name="use-a-different-key"></a>Uso de una clave distinta

Para cambiar la clave que se usa para el cifrado, siga estos pasos:

1. Vaya al recurso de Cognitive Services y muestre la configuración de **Cifrado**.
1. Escriba el identificador URI de la nueva clave. Otra opción es seleccionar el almacén de claves y elegir una clave nueva.
1. Guarde los cambios.

## <a name="disable-customer-managed-keys"></a>Deshabilitación de claves administradas por el cliente

Cuando se deshabilitan las claves administradas por el cliente, el recurso de Cognitive Services se cifra entonces con claves administradas por Microsoft. Para deshabilitar las claves administradas por el cliente, siga estos pasos:

1. Vaya al recurso de Cognitive Services y muestre la configuración de **Cifrado**.
1. Anule la selección de la casilla que se encuentra junto al valor **Usar su propia clave**.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Formulario de solicitud de claves administradas por el cliente de Cognitive Services](https://aka.ms/cogsvc-cmk)
* [Cifrado de datos en reposo de los servicios Face](../Face/face-encryption-of-data-at-rest.md)
* [Cifrado de datos en reposo de QnA Maker](../QnAMaker/qna-maker-encryption-of-data-at-rest.md)
* [Cifrado de datos en reposo del servicio Language Understanding](../LUIS/luis-encryption-of-data-at-rest.md)
* [Cifrado de datos en reposo de Content Moderator](../Content-Moderator/content-moderator-encryption-of-data-at-rest.md)
* [Cifrado de datos en reposo de Translator](../translator/translator-encryption-of-data-at-rest.md)
* [Cifrado de datos en reposo de Personalizer](../personalizer/personalizer-encryption-of-data-at-rest.md)
