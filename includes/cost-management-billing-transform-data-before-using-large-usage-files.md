---
title: archivo de inclusión
description: archivo de inclusión
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: include
ms.date: 10/09/2020
ms.author: banders
ms.reviwer: ''
ms.custom: include file
ms.openlocfilehash: fb34b4ef1cf209a15c7e6ac9a59cc11dd4b0056e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92026736"
---
## <a name="transform-data-before-using-large-usage-files"></a>Transformación de datos antes de usar archivos de uso grande

A veces, el archivo de uso o conciliación es demasiado grande para abrirse. O bien, puede que necesite solo una parte de la información para solucionar un problema. Por ejemplo, es posible que solo desee información sobre un recurso determinado o solo el consumo de pocos servicios o grupos de recursos. Puede transformar los datos para resumirlos antes de crear tablas dinámicas.

1. Abra un libro en blanco en Excel.
1. En el menú superior, seleccione **Datos** > **A partir de texto o CSV**, seleccione el archivo de uso y, a continuación, seleccione **Importar**.
1. En la parte inferior de la ventana, seleccione **Transformar datos**. Una nueva ventana muestra un resumen de los datos.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/summarized-data.png" alt-text="Ejemplo que muestra los datos resumidos" lightbox="./media/cost-management-billing-transform-data-before-using-large-usage-files/summarized-data.png" :::
1. Si tiene un Contrato de cliente de Microsoft, omita este paso y continúe con el siguiente porque los archivos de uso de MCA normalmente tienen los títulos de columna en las primeras filas. Prepare los datos mediante la creación de la tabla. Quite las primeras filas y deje solo los títulos. Seleccione **Quitar filas** > **Quitar filas superiores**.  
     :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/remove-top-rows.png" alt-text="Ejemplo que muestra los datos resumidos" :::
1. En la ventana Quitar filas superiores, escriba el número de filas que desea quitar en la parte superior. Para EA normalmente son dos, para CSP normalmente es una. Seleccione **Aceptar**.
1. Seleccione **Uso de la primera fila como encabezado**.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/use-first-row-as-header.png" alt-text="Ejemplo que muestra los datos resumidos" :::
    
    La vista de tabla muestra los títulos de columna en la parte superior.
1. A continuación, agregue un filtro. Use las flechas de selector a la derecha de cada título de columna para filtrar. Los filtros sugeridos son el identificador de la suscripción, el nombre del servicio (categoría de medición), el identificador de instancia y el grupo de recursos. Puede usar varios filtros en el mismo documento. Se recomienda aplicar todos los filtros posibles para reducir el tamaño del documento y ayudar a trabajar posteriormente.
1. Una vez aplicados los filtros, seleccione **Cerrar y cargar**.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/close-and-load.png" alt-text="Ejemplo que muestra los datos resumidos" :::

El archivo se carga y muestra una tabla con los datos de uso filtrados. Ahora puede crear una nueva tabla dinámica para solucionar los problemas de uso.