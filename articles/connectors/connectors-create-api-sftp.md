---
title: Conexión con una cuenta SFTP (en desuso)
description: Automatice tareas y procesos que supervisan, crean, administran, envían y reciben archivos en un servidor SFTP mediante Azure Logic Apps.
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 11/01/2019
tags: connectors
ROBOTS: NOINDEX
ms.openlocfilehash: 70fb956af7ff45c7b54f04d7ed441ec39f9d80a5
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92673821"
---
# <a name="monitor-create-and-manage-sftp-files-in-azure-logic-apps"></a>Supervisión, creación y administración de archivos SFTP en Azure Logic Apps

> [!IMPORTANT]
> Use el [conector SFTP-SSH](../connectors/connectors-sftp-ssh.md), ya que el conector SFTP está en desuso. Ya no puede seleccionar desencadenadores y acciones de SFTP en el diseñador de aplicación lógica.

Para automatizar las tareas que supervisan, crear, envían y reciben archivos en un servidor [Secure File Transfer Protocol (SFTP)](https://www.ssh.com/ssh/sftp/), puede crear y automatizar los flujos de trabajo de integración con Azure Logic Apps y el conector SFTP. SFTP es un protocolo de red que proporciona acceso a archivos, transferencia de archivos y administración de archivos a través de cualquier flujo de datos confiable. Estas son algunas tareas de ejemplo que se pueden automatizar:

* Supervisar cuándo se agregan o cambian archivos
* Obtener, crear, copiar, actualizar, enumerar y eliminar archivos
* Obtener contenido de archivos y metadatos
* Extraer archivos en carpetas

Puede usar desencadenadores que supervisen eventos en el servidor SFTP y permitir que la salida esté disponible para otras acciones. Puede usar acciones que realicen diversas tareas en el servidor SFTP. También puede hacer que otras acciones de la aplicación lógica usen la salida de las acciones SFTP. Por ejemplo, si recupera regularmente archivos del servidor SFTP, puede enviar por correo electrónico alertas sobre esos archivos y su contenido mediante el conector Office 365 Outlook o el conector Outlook.com. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>límites

El conector SFTP controla solo los archivos que tengan un tamaño de *50 MB o menos* y no admite la [fragmentación de mensajes](../logic-apps/logic-apps-handle-large-messages.md). Para archivos de mayor tamaño, use el [conector SFTP-SSH](../connectors/connectors-sftp-ssh.md). Para conocer otras diferencias entre el conector SFTP y el conector SFTP-SSH, consulte [Comparación de SFTP-SSH y SFTP](../connectors/connectors-sftp-ssh.md#comparison) en el artículo sobre SFTP-SSH.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La dirección del servidor SFTP y las credenciales de cuenta, que permiten que la aplicación lógica acceda a la cuenta de SFTP. Para usar el protocolo [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/), también necesita acceso a una clave privada SSH y a la contraseña de clave privada SSH.

  > [!NOTE]
  >
  > El conector SFTP admite estos formatos de clave privada: OpenSSH, ssh.com y PuTTY
  >
  > Al crear la aplicación lógica, después de agregar el desencadenador o la acción SFTP que desee, deberá proporcionar información de conexión para el servidor SFTP. 
  > Si usa una clave privada SSH, asegúrese de * **copiar** _ la clave del archivo de clave privada SSH y _*_pegarla_*_ en los datos de conexión. _*_No escriba ni edite manualmente la clave_*_ , ya que se podría producir un error en la conexión. 
  > Para más información, consulte los pasos más adelante en este artículo.

_ Conocimientos básicos sobre [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde quiere acceder a la cuenta de SFTP. Para comenzar con un desencadenador de SFTP, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción de SFTP, inicie la aplicación lógica con otro desencadenador, por ejemplo, el desencadenador **Recurrence**.

## <a name="how-sftp-triggers-work"></a>Funcionamiento de los desencadenadores de SFTP

Los desencadenadores de SFTP funcionan al sondear el sistema de archivos SFTP y buscar archivos que se hayan modificado desde el último sondeo. Algunas herramientas le permiten conservar la marca de tiempo cuando cambian los archivos. En estos casos, tiene que deshabilitar esta característica para que el desencadenador funcione. Estas son algunas opciones de configuración habituales:

| Cliente SFTP | Acción |
|-------------|--------|
| WinSCP | Vaya a **Options** > **Preferences** > **Transfer** > **Edit** > **Preserve timestamp** > **Disable** (Opciones > Preferencias > Transferir > Editar > Conservar marca de tiempo >Deshabilitar). |
| FileZilla | Vaya a **Transfer** > **Preserve timestamps of transferred files** > **Disable** (Transferir > Conservar marca de tiempo de archivos transferidos > Deshabilitar). |
|||

Cuando un desencadenador encuentra un nuevo archivo, el desencadenador comprueba que el nuevo archivo se ha escrito por completo y no parcialmente. Por ejemplo, un archivo podría tener los cambios en curso cuando el desencadenador comprueba el servidor de archivos. Para evitar devolver un archivo parcialmente escrito, el desencadenador anota la marca de tiempo del archivo que tiene cambios recientes, pero no devuelve inmediatamente dicho archivo. El desencadenador devuelve el archivo solo al volver a sondear el servidor. En ocasiones, este comportamiento puede provocar un retraso de hasta dos veces el intervalo de sondeo del desencadenador.

## <a name="connect-to-sftp"></a>Conexión a SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "sftp" como filtro. En la lista de desencadenadores, seleccione el que desee.

   O bien

   Para las aplicaciones lógicas existentes, en el último paso donde desea agregar una acción, elija **Nuevo paso**. En el cuadro de búsqueda, escriba "sftp" como filtro. En la lista de acciones, seleccione la que desee.

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. Elija el signo más ( **+** ) que aparece y seleccione **Agregar una acción**.

1. Proporcione la información necesaria para la conexión.

   > [!IMPORTANT]
   >
   > Al escribir la clave privada SSH en la propiedad **SSH private key** , siga estos pasos adicionales para asegurarse de proporcionar el valor completo y correcto para esta propiedad. 
   > Una clave no válida provoca un error de conexión.

   Aunque puede usar cualquier editor de texto, estos son los pasos de ejemplo que muestran cómo copiar y pegar correctamente la clave mediante Notepad.exe.

   1. Abra el archivo de clave privada SSH en un editor de texto. Estos pasos usan Bloc de notas como ejemplo.

   1. En el menú **Edición** del Bloc de notas, elija **Seleccionar todo**.

   1. Seleccione **Editar** > **Copiar**.

   1. En el desencadenador o la acción SFTP que ha agregado, pegue la clave *completa* que copió en la propiedad **SSH private key** , que admite varias líneas. **_Asegúrese de pegar_* _ la clave. _*_No escriba ni edite la clave manualmente_*_.

1. Cuando haya terminado de especificar los detalles de conexión, elija _*Crear**.

1. Proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="examples"></a>Ejemplos

<a name="file-add-modified"></a>

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>Desencadenador SFTP: When a file is added or modified (Cuando se agrega o modifica un archivo)

Este desencadenador inicia un flujo de trabajo de aplicación lógica cuando se agrega o se modifica un archivo en un servidor SFTP. Por ejemplo, puede agregar una condición que compruebe el contenido del archivo y obtenga el contenido en función de si cumple una condición especificada. Luego, puede agregar una acción que obtenga el contenido del archivo y lo coloque en una carpeta en el servidor SFTP.

**Ejemplo Enterprise** : Puede usar este desencadenador para supervisar nuevos archivos en una carpeta de SFTP que representan los pedidos de los clientes. Seguidamente, puede usar una acción SFTP, como **Get file content** para obtener el contenido del pedido para su posterior procesamiento y almacenar ese pedido en una base de datos de pedidos.

<a name="get-content"></a>

### <a name="sftp-action-get-content"></a>Acción de SFTP: Obtener contenido

Esta acción obtiene el contenido de un archivo en un servidor SFTP. Por ejemplo, puede agregar el desencadenador del ejemplo anterior y una condición que debe cumplir el contenido del archivo. Si la condición es verdadera, se puede ejecutar la acción que obtiene el contenido.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](../data-factory/connector-sftp.md) del conector.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)