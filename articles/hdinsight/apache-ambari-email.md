---
title: 'Tutorial: Configuración de notificaciones por correo electrónico de Apache Ambari en Azure HDInsight'
description: En este artículo se describe cómo usar SendGrid con Apache Ambari para las notificaciones por correo electrónico.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/10/2020
ms.openlocfilehash: bdce735bdacbe7ff0752650c6949fdb361342c73
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92542563"
---
# <a name="tutorial-configure-apache-ambari-email-notifications-in-azure-hdinsight"></a>Tutorial: Configuración de notificaciones por correo electrónico de Apache Ambari en Azure HDInsight

En este tutorial, configurará las notificaciones por correo electrónico de Apache Ambari mediante SendGrid. [Apache Ambari](./hdinsight-hadoop-manage-ambari.md) simplifica la administración y la supervisión de un clúster de HDInsight, ya que ofrece una API REST y una interfaz de usuario web fácil de usar. Ambari se incluye en los clústeres de HDInsight y, además, se usa para supervisar el clúster y realizar cambios en la configuración. [SendGrid](https://sendgrid.com/solutions/) es un servicio de correo electrónico gratuito basado en la nube que ofrece un sistema confiable de entrega de correo electrónico transaccional, escalabilidad y análisis en tiempo real, junto con API flexibles que facilitan la integración personalizada. Los clientes de Azure pueden desbloquear 25.000 correos electrónicos gratuitos cada mes.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Obtener un nombre de usuario de Sendgrid
> * Configurar notificaciones por correo electrónico de Apache Ambari

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta de correo electrónico de SendGrid. Para obtener instrucciones, consulte [Envío de correos electrónicos mediante SendGrid con Azure](../sendgrid-dotnet-how-to-send-email.md).

* Un clúster de HDInsight. Consulte [Creación de clústeres de Apache Hadoop mediante Azure Portal](./hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="obtain-sendgrid-username"></a>Obtención de un nombre de usuario de SendGrid

1. En [Azure Portal](https://portal.azure.com), vaya al recurso de SendGrid.

1. En la página de información general, seleccione **Administrar** para ir a la página web de SendGrid para su cuenta.

    ![Información general de SendGrid en Azure Portal](./media/apache-ambari-email/azure-portal-sendgrid-manage.png)

1. En el menú de la izquierda, vaya al nombre de la cuenta y, después, seleccione **Detalles de la cuenta** .

    ![Navegación por el panel de SendGrid](./media/apache-ambari-email/sendgrid-dashboard-navigation.png)

1. En la página **Detalles de la cuenta** , grabe el valor de **Nombre de usuario** .

    ![Detalles de la cuenta de SendGrid](./media/apache-ambari-email/sendgrid-account-details.png)

## <a name="configure-ambari-e-mail-notification"></a>Configuración de la notificación por correo electrónico de Ambari

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net/#/main/alerts`, donde `CLUSTERNAME` es el nombre del clúster.

1. En la lista desplegable **Acciones** , seleccione **Administrar notificaciones** .

1. En la ventana **Manage Alert Notifications** (Administrar notificaciones de alerta), seleccione el icono **+** .

    ![Captura de pantalla que muestra el cuadro de diálogo de administración de notificaciones de alerta.](./media/apache-ambari-email/azure-portal-create-notification.png)

1. En el cuadro de diálogo **Create Alert Notification** (Crear notificación de alerta), especifique la siguiente información:

    |Propiedad |Descripción |
    |---|---|
    |Nombre|Especifique el nombre de la notificación.|
    |Grupos|Configúrela como quiera.|
    |severity|Configúrela como quiera.|
    |Descripción|Opcional.|
    |Método|Deje el valor **EMAIL** (CORREO ELECTRÓNICO).|
    |Correo electrónico para|Especifique las direcciones de correo electrónico que van a recibir las notificaciones, separadas por una coma.|
    |Servidor SMTP|`smtp.sendgrid.net`|
    |Puerto SMTP|25 o 587 (para conexiones no cifradas o TLS).|
    |Correo electrónico de|Especifique una dirección de correo electrónico. No es preciso que la dirección sea auténtica.|
    |Usar autenticación|Seleccione esta casilla.|
    |Nombre de usuario|Especifique el nombre de usuario de SendGrid.|
    |Contraseña|Especifique la contraseña que usó al crear el recurso SendGrid en Azure.|
    |Confirmación de contraseña|Vuelva a escribir la contraseña.|
    |Iniciar TLS|Seleccione esta casilla|

    ![Captura de pantalla que muestra el cuadro de diálogo de creación de notificación de alerta.](./media/apache-ambari-email/ambari-create-alert-notification.png)

    Seleccione **Guardar** . Volverá a la ventana **Manage Alert Notifications** (Administrar notificaciones de alerta).

1. En la ventana **Manage Alert Notifications** (Administrar notificaciones de alerta), seleccione **Cerrar** .

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a configurar las notificaciones por correo electrónico de Apache Ambari mediante SendGrid. Use los recursos siguientes para obtener más información sobre cómo Apache Ambari:

* [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](./hdinsight-hadoop-manage-ambari.md)

* [Creación de una notificación de alerta](https://docs.cloudera.com/HDPDocuments/Ambari-latest/managing-and-monitoring-ambari/content/amb_create_an_alert_notification.html)