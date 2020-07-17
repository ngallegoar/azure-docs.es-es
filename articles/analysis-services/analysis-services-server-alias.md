---
title: Alias de nombre de servidor de Azure Analysis Services | Microsoft Docs
description: Aprenda a crear alias de nombre del servidor de Azure Analysis Services. Los usuarios pueden conectarse a su servidor con un nombre de alias más corto en lugar del nombre del servidor.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 435649c5431ff14461245fee88cebe4a2c571663
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85361445"
---
# <a name="alias-server-names"></a>Alias de nombre de servidor

Mediante un alias de nombre de servidor, los usuarios pueden conectarse a su servidor de Azure Analysis Services con un *alias* más corto en lugar del nombre del servidor. Al conectarse desde una aplicación cliente, el alias se especifica como un punto de conexión mediante el formato de protocolo **link://** . El punto de conexión devuelve entonces el nombre real del servidor para conectarse.

Los alias de nombre de servidor son muy adecuados en los siguientes casos:

- Migrar modelos entre servidores sin que los usuarios se vean afectados. 
- Los nombres de servidor descriptivos son más fáciles de recordar para los usuarios. 
- Dirigir a los usuarios a diferentes servidores en diferentes momentos del día. 
- Dirigir a los usuarios de diferentes regiones a instancias que se encuentran geográficamente próximas, como cuando se usa Azure Traffic Manager. 

Cualquier punto de conexión HTTPS que devuelva un nombre de servidor válido de Azure Analysis Services podrá servir como alias. El punto de conexión debe admitir HTTPS a través del puerto 443 y no se debe especificar el puerto en el URI.

![Alias mediante formato de vínculo](media/analysis-services-alias/aas-alias-browser.png)

Al conectarse desde un cliente, el alias del nombre de servidor de especifica mediante el formato de protocolo **link://** . Por ejemplo, en Power BI Desktop:

![Conexión de Power BI Desktop](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Creación de un alias

Para crear un alias de punto de conexión, puede usar cualquier método que devuelva un nombre de servidor válido de Azure Analysis Services. Por ejemplo, una referencia a un archivo en Azure Blob Storage que contenga el nombre real del servidor, o crear y publicar una aplicación de Web Forms ASP.NET.

En este ejemplo, se crea una aplicación Web Forms ASP.NET en Visual Studio. La referencia a la página y el control de usuario se eliminan de la página Default.aspx. El contenido de Default.aspx es simplemente la directiva de página siguiente:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

El evento Page_Load de Default.aspx.cs usa el método Response.Write() para devolver el nombre del servidor de Azure Analysis Services.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Consulte también

[Bibliotecas de cliente](https://docs.microsoft.com/analysis-services/client-libraries?view=azure-analysis-services-current)   
[Conexión con Power BI Desktop](analysis-services-connect-pbi.md)
