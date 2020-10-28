---
title: 'Instrucciones: Exportar la aplicación Comandos personalizados como una aptitud remota: servicio Voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, aprenderá cómo exportar la aplicación Comandos personalizados como una aptitud.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: sausin
ms.openlocfilehash: 6ab3e1e063409832de839da26eba80efd8b3f4d5
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344783"
---
# <a name="export-custom-commands-application-as-a-remote-skill"></a>Exportación de la aplicación Comandos personalizados como una aptitud remota

En este artículo, aprenderá cómo exportar una aplicación Comandos personalizados como una aptitud remota.

## <a name="prerequisites"></a>Requisitos previos
> [!div class="checklist"]
> * [Descripción de las aptitudes de Bot Framework](https://aka.ms/speech/cc-skill-overview)
> * [Descripción del manifiesto de aptitudes](https://aka.ms/speech/cc-skill-manifest)
> * [Cómo invocar una aptitud desde un bot de Bot Framework](https://aka.ms/speech/cc-skill-consumer)
> * Una aplicación Comandos personalizados existente. En caso de que no tenga ninguna aplicación Comandos personalizados, pruebe con [Inicio rápido: Creación de un asistente de voz mediante Comandos personalizados](quickstart-custom-commands-application.md)

## <a name="custom-commands-as-remote-skills"></a>Comandos personalizados como aptitudes remotas
* Las aptitudes de Bot Framework son bloques de creación de aptitudes conversacionales reutilizables que cubren casos de uso conversacionales y permiten agregar una amplia funcionalidad a un bot en cuestión de minutos. Para más información sobre esto, consulte el artículo sobre la [aptitud de Bot Framework](https://microsoft.github.io/botframework-solutions/overview/skills/).
* Una aplicación Comandos personalizados se puede exportar como aptitud. Esta aptitud se puede invocar con el protocolo de aptitudes remotas desde un bot de Bot Framework.

## <a name="configure-an-application-to-be-exposed-as-a-remote-skill"></a>Configuración de una aplicación para que se exponga como una aptitud remota

### <a name="application-level-settings"></a>Configuración de nivel de aplicación
1. En el panel izquierdo, seleccione **Settings** > **Remote Skills** (Configuración > Aptitudes remotas).
1. Establezca el botón de alternancia **Remote skills enabled** (Aptitudes remotas habilitadas) en Activado.

### <a name="authentication-to-skills"></a>Autenticación en aptitudes
1. Si desea habilitar la autenticación, agregue los identificadores de aplicación de Microsoft de los bots de Bot Framework que desea configurar para llamar a la aplicación Comandos personalizados.
      > [!div class="mx-imgBorder"]
      > ![Adición de un identificador de MSA a una aptitud](media/custom-commands/skill-add-msa-id.png)

1. Si tiene al menos una entrada agregada a la lista, se habilitará la autenticación en la aplicación y solo los bots permitidos podrán llamar a la aplicación.
> [!TIP]
>  Para deshabilitar la autenticación, elimine todos los identificadores de aplicación de Microsoft de la lista de permitidos. 

 ### <a name="enabledisable-commands-to-be-exposed-as-skills"></a>Habilitación o deshabilitación de comandos para que se expongan como aptitudes

Tiene la opción de elegir qué comandos quiere exponer mediante las aptitudes remotas.

1. Para exponer un comando mediante las aptitudes, seleccione **Enable a new command** (Habilitar un nuevo comando) en **Enable commands for skills** (Habilitar comandos para aptitudes).
1. En la lista desplegable, seleccione el comando que desea agregar.
1. Seleccione **Guardar** .

### <a name="configure-triggering-utterances-for-commands"></a>Configuración de la activación de expresiones para comandos
Comandos personalizados usa las frases de ejemplo que están configuradas para los comandos con el fin de generar las aptitudes que desencadenan expresiones. Estas **expresiones desencadenadoras** se usarán para generar la sección de **distribuidor** [**manifiesto de aptitud**](https://microsoft.github.io/botframework-solutions/skills/handbook/manifest/).

Como autor, es posible que desee controlar cuál de sus **frases de ejemplo** se usan para generar las aptitudes que desencadenan expresiones.
1. De forma predeterminada, todos los **ejemplos desencadenadores** de un comando se incluirán en el archivo de manifiesto.
1. Si desea eliminar explícitamente cualquier ejemplo, seleccione **el icono Edit** (Editar) en el comando de la sección **Enabled commands for skills** (Comandos habilitados para aptitudes).
    > [!div class="mx-imgBorder"]
    > ![Edición de un comando habilitado para la aptitud](media/custom-commands/skill-edit-enabled-command.png)

1. A continuación, en las frases de ejemplo que quiere omitir, **haga clic con el botón derecho en**  > **Disable Example Sentence** (Deshabilitar la frase de ejemplo).
    > [!div class="mx-imgBorder"]
    > ![Deshabilitación de ejemplos](media/custom-commands/skill-disable-example-sentences.png)

1. Seleccione **Guardar** .
1. Observará que no puede agregar un nuevo ejemplo en esta ventana. Si es necesario hacerlo, continúe a la salida de la sección de configuración y seleccione el comando correspondiente en el acordeón de **comandos** . Llegados a este punto, puede agregar la nueva entrada en la sección **Example sentences** (Frases de ejemplo). Este cambio se reflejará automáticamente en el valor de configuración de las aptitudes remotas del comando.

> [!IMPORTANT]
> En caso de que las frases de ejemplo existentes tengan referencias al tipo de datos **Cadena > Catálogo** , esas frases se omitirán automáticamente en la lista de aptitudes que desencadenan expresiones. 

## <a name="download-skill-manifest"></a>Descarga del manifiesto de aptitudes
1. Después de haber **publicado** la aplicación, puede descargar el archivo de manifiesto de aptitudes.
1. Use el manifiesto de aptitudes para configurar el bot de consumidor de Bot Framework para que llame a la aptitud de Comandos personalizados.
> [!IMPORTANT]
> Debe **publicar** la aplicación Comandos personalizados para poder descargar el manifiesto de aptitudes. </br>
> Además, si ha realizado **cualquier cambio** en la aplicación, deberá volver a publicarla para que los cambios más recientes se reflejen en el archivo de manifiesto.

> [!NOTE]
> Si se produce algún problema con la publicación de la aplicación y el error se dirige a las aptitudes que desencadenan expresiones, vuelva a comprobar la configuración de **Enabled commands for skills** (Comandos habilitados para aptitudes). Cada uno de los comandos expuestos debe tener al menos una expresión desencadenadora válida.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Habilite un proceso de CI/CD para su aplicación de Comandos personalizados](./how-to-custom-commands-deploy-cicd.md)