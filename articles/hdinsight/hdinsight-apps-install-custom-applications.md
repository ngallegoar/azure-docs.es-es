---
title: Instalación de aplicaciones personalizadas de Apache Hadoop en Azure HDInsight
description: Aprenda a instalar aplicaciones de HDInsight para clústeres de Apache Hadoop en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: 9fd8152b4180d44d3b822feef7e74e267b6b948a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86086507"
---
# <a name="install-custom-apache-hadoop-applications-on-azure-hdinsight"></a>Instalación de aplicaciones de Apache Hadoop propias en Azure HDInsight

En este artículo, aprenderá a instalar una aplicación de [Apache Hadoop](https://hadoop.apache.org/) en Azure HDInsight que no se ha publicado en Azure Portal. La aplicación que instalará en este artículo es [Hue](https://gethue.com/).

Una aplicación de HDInsight es una aplicación que los usuarios pueden instalar en un clúster de HDInsight.  Estas aplicaciones puede desarrollarlas Microsoft, fabricantes de software independientes (ISV) o el propio usuario.  

## <a name="prerequisites"></a>Prerrequisitos

Si desea instalar aplicaciones de HDInsight en un clúster de HDInsight existente, debe tener un clúster de HDInsight. Para crearlo, consulte [Crear clúster](hadoop/apache-hadoop-linux-tutorial-get-started.md). También puede instalar aplicaciones de HDInsight al crear un clúster de HDInsight.

## <a name="install-hdinsight-applications"></a>Install HDInsight applications

Las aplicaciones de HDInsight pueden instalarse cuando se crea un clúster o en un clúster de HDInsight existente. Para definir plantillas en Azure Resource Manager, consulte el artículo sobre [MSDN: instalación de una aplicación de HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).

Los archivos necesarios para implementar esta aplicación (Hue):

* [azuredeploy.json](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): plantilla de Resource Manager para instalar una aplicación de HDInsight. Consulte el artículo sobre [MSDN: instalación de una aplicación de HDInsight](https://msdn.microsoft.com/library/mt706515.aspx) para desarrollar su propia plantilla de Resource Manager.
* [hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): acción de Script a la que llama la plantilla de Resource Manager para configurar el nodo perimetral.
* [hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): archivo binario de Hue al que se llama desde hui install_v0.sh.
* [hue-binaries-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): archivo binario de Hue al que se llama desde hui install_v0.sh.
* [webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): aplicación web de ejemplo (Tomcat) a la que se llama desde hui install_v0.sh.

### <a name="to-install-hue-to-an-existing-hdinsight-cluster"></a>Para instalar Hue en un clúster de HDInsight existente

1. Seleccione la imagen siguiente para iniciar sesión en Azure y abrir la plantilla de Resource Manager en Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    La plantilla de Resource Manager se encuentra en [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  Para aprender a escribir esta plantilla de Resource Manager, consulte el artículo sobre [MSDN: instalación de una aplicación de HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).

1. Seleccione el **Grupo de recursos** existente que contiene el clúster en la lista desplegable. Es necesario usar el mismo grupo de recursos que el clúster.

1. especifique el nombre del clúster en el que desea instalar la aplicación. Debe ser un clúster existente.

1. Seleccione la casilla **Acepto los términos y condiciones indicados anteriormente**.

1. Seleccione **Comprar**.

El estado de la instalación se puede ver desde el icono anclado al panel del portal y la notificación del portal (haga clic en el icono de campana de la parte superior del portal).  La aplicación tarda aproximadamente 10 minutos en instalarse.

### <a name="to-install-hue-while-creating-a-cluster"></a>Para instalar Hue durante la creación de un clúster

1. Seleccione la imagen siguiente para iniciar sesión en Azure y abrir la plantilla de Resource Manager en Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    La plantilla de Resource Manager se encuentra en [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  Para aprender a escribir esta plantilla de Resource Manager, consulte el artículo sobre [MSDN: instalación de una aplicación de HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).

2. Siga las instrucciones para crear un clúster e instalar Hue. Para más información acerca de cómo crear clústeres de HDInsight, consulte [Creación de clústeres de Hadoop basados en Linux en HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="other-installation-methods"></a>Otros métodos de instalación

Además de Azure Portal, también puede usar [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-using-powershell) y la [CLI de Azure](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-using-azure-cli) para invocar plantillas de Resource Manager.

## <a name="validate-the-installation"></a>Validación de la instalación

El estado de la aplicación se puede comprobar en el Portal de Azure para validar la instalación de la aplicación. Además, también se pueden validar todos los puntos de conexión HTTP que han aparecido según lo previsto y la página web, si hay.

En el caso de **Hue**, puede seguir estos pasos:

### <a name="azure-portal"></a>Portal de Azure

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el clúster en el que instaló la aplicación.
1. En el menú **Configuración**, seleccione **Aplicaciones**.
1. Seleccione **hue** en la lista para ver las propiedades.  
1. Seleccione el vínculo de la página web para validar el sitio web.

### <a name="azure-cli"></a>Azure CLI

Reemplace `CLUSTERNAME`y `RESOURCEGROUP` por los valores pertinentes y, a continuación, escriba los siguientes comandos:

* Para enumerar todas las aplicaciones del clúster de HDInsight.

    ```azurecli
    az hdinsight application list --cluster-name CLUSTERNAME --resource-group RESOURCEGROUP
    ```

* Para recuperar las propiedades de la aplicación especificada.

    ```azurecli
    az hdinsight application show --name hue --cluster-name CLUSTERNAME --resource-group RESOURCEGROUP
    ```

## <a name="troubleshoot-the-installation"></a>Solución de problemas de instalación

En el portal aparece una notificación donde puede comprobar el estado de la instalación de la aplicación (haga clic en el icono de campana que encontrará en la parte superior del portal).

Si se produce algún error al instalar la aplicación, puede consultar los mensajes de error y la información de depuración en tres sitios:

* Aplicaciones de HDInsight: información general sobre los errores.

    Abra el clúster desde el portal y seleccione Aplicaciones en Configuración:

    ![error de instalación de aplicaciones de hdinsight](./media/hdinsight-apps-install-custom-applications/hdinsight-apps-error.png)

* Acción de script de HDInsight: si el mensaje de error de las aplicaciones de HDInsight indica que se ha producido un problema en una acción del script, aparecerá más información sobre este error en el panel de acciones del script.

    Seleccione Acción de script en Configuración. En el historial de acciones de script aparecen los mensajes de error.

    ![error de acción de script de aplicaciones de hdinsight](./media/hdinsight-apps-install-custom-applications/hdinsight-apps-script-action-error.png)

* Interfaz de usuario web de Apache Ambari: si el script de instalación fue la causa del error, utilice la interfaz de usuario web de Ambari para consultar los registros completos de los scripts de instalación.

    Para más información, consulte [Solución de problemas de acciones de script](./troubleshoot-script-action.md).

## <a name="remove-hdinsight-applications"></a>Eliminación de aplicaciones de HDInsight

### <a name="azure-portal"></a>Portal de Azure

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el clúster en el que instaló la aplicación.
1. En el menú **Configuración**, seleccione **Aplicaciones**.
1. Haga clic con el botón derecho en la aplicación que desea eliminar y, luego, seleccione **Eliminar**.
1. Seleccione **Sí** para confirmar la acción.

### <a name="azure-cli"></a>Azure CLI

Reemplace `NAME`, `CLUSTERNAME` y `RESOURCEGROUP` por los valores pertinentes y, a continuación, escriba el siguiente comando:

```azurecli
az hdinsight application delete --name NAME --cluster-name CLUSTERNAME --resource-group RESOURCEGROUP
```

## <a name="next-steps"></a>Pasos siguientes

* [MSDN: instalación de una aplicación de HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): aprenda a desarrollar plantillas de Resource Manager para implementar aplicaciones de HDInsight.
* [Instalación de aplicaciones de HDInsight](hdinsight-apps-install-applications.md): aprenda a instalar una aplicación de HDInsight en sus clústeres.
* [Publicación de aplicaciones de HDInsight en Azure Marketplace](hdinsight-apps-publish-applications.md): aprenda a publicar aplicaciones de HDInsight personalizadas en Azure Marketplace.
* [Personalización de clústeres de HDInsight mediante la acción de scripts (Linux)](hdinsight-hadoop-customize-cluster-linux.md): aprenda a usar acciones de script para instalar otras aplicaciones.
* [Creación de clústeres de Apache Hadoop basados en Linux en HDInsight con plantillas de Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md): aprenda a llamar a plantillas de Resource Manager para crear clústeres de HDInsight.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md)(Utilización de nodos perimetrales vacíos en HDInsight): aprenda a usar un nodo perimetral vacío para acceder a los clústeres de HDInsight, probar aplicaciones de este y hospedarlas.
