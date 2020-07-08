---
title: Configuración de herramientas de Azure Stream Analytics para Visual Studio
description: En este artículo se describen los requisitos de instalación de las herramientas de Azure Stream Analytics para Visual Studio y cómo configurarlas.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 05/22/2018
ms.openlocfilehash: 464e8fbf624dfa84ec54bf6a7e0ce202d047e15d
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045117"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Instalación de las herramientas de Azure Stream Analytics para Visual Studio

Visual Studio 2019 y Visual Studio 2017 admiten herramientas de Azure Data Lake y Stream Analytics. En este artículo se describe cómo instalar y desinstalar las herramientas.

Para más información sobre el uso de las herramientas, consulte [Inicio rápido: Creación de un trabajo de Azure Stream Analytics con Visual Studio](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Instalar

Las ediciones Visual Studio Enterprise (Ultimate y Premium), Professional y Community admiten las herramientas. La edición Express y Visual Studio para Mac no las admiten.

Se recomienda Visual Studio 2019.

### <a name="install-for-visual-studio-2019-and-2017"></a>Instalación de Visual Studio 2019 y 2017<a name="recommended-visual-studio-2019-and-2017"></a>

Las herramientas de Azure Data Lake y Stream Analytics forman parte de las cargas de trabajo **Desarrollo de Azure** y **Almacenamiento y procesamiento de datos**. Habilite una de estas dos cargas de trabajo durante la instalación. Si Visual Studio ya está instalado, seleccione **Herramientas** > **Obtener herramientas y características** para agregar cargas de trabajo.

Descargue [Visual Studio 2019 (Preview 2 o superior) o Visual Studio 2017 (15.3 o superior) ](https://www.visualstudio.com/) y siga las instrucciones de instalación.

Seleccione la carga de trabajo **Almacenamiento y procesamiento de datos** como se muestra:

![Se ha seleccionado la carga de trabajo de procesamiento y el almacenamiento de datos](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-01.png)

Seleccione la carga de trabajo **Desarrollo de Azure** como se muestra:

![Se ha seleccionado la carga de trabajo de desarrollo de Azure](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-02.png)

Después de agregar la carga de trabajo, actualice las herramientas. Este procedimiento hace referencia a Visual Studio 2019:

1. Seleccione **Extensiones** > **Administrar extensiones**.

1. En **Administrar extensiones**, seleccione **Actualizaciones** y elija **Herramientas de Azure Data Lake y Stream Analytics**.

1. Seleccione **Actualizar** para instalar la extensión más reciente.

![Extensiones y actualizaciones de Visual Studio](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-vs2019-extensions-updates.png)

### <a name="install-for-visual-studio-2015-and-2013"></a>Instalación de Visual Studio 2015 y 2013<a name="visual-studio-2015-2013"></a>

Las ediciones Visual Studio Enterprise (Ultimate y Premium), Professional y Community admiten las herramientas. La edición Express no las admite.

* Instale Visual Studio 2015 o Visual Studio 2013 Update 4.
* Instale Microsoft Azure SDK para .NET versión 2.7.1 o superior con el [instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx).
* Instale [Herramientas de Microsoft Azure Data Lake y Stream Analytics para Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## <a name="update"></a>Actualización<a name="visual-studio-2019-and-2017"></a><a name="visual-studio-2015-and-2013"></a>

Para Visual Studio 2019 y Visual Studio 2017, se muestra un nuevo recordatorio de versión en forma de una notificación de Visual Studio.

Para Visual Studio 2015 y Visual Studio 2013, las herramientas compruebas si hay nuevas versiones automáticamente. Siga las instrucciones para instalar la versión más reciente.

## <a name="uninstall"></a>Desinstalación

Puede desinstalar las herramientas de Azure Data Lake y Stream Analytics. En Visual Studio 2019 o Visual Studio 2017, seleccione **Herramientas** > **Obtener herramientas y características**. En **Modificando**, anule la selección de **Herramientas de Azure Data Lake y Stream Analytics**. Aparece en las cargas de trabajo **Almacenamiento y procesamiento de datos** o **Desarrollo de Azure**.

Para desinstalar desde Visual Studio 2015 o Visual Studio 2013, vaya al **Panel de control** > **Programas y características**. Desinstale **Herramientas de Microsoft Azure Data Lake y Stream Analytics para Visual Studio**.
