---
title: Conexión a servidores de Azure Analysis Services | Microsoft Docs
description: Aprenda a conectarse y obtener datos de un servidor de Analysis Services en Azure.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: owend
ms.reviewer: minewiskan
ms.custom: references_regions
ms.openlocfilehash: 10e091ed58146d992d7b9c1f65b8b64f881a41b9
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2020
ms.locfileid: "91400288"
---
# <a name="connecting-to-servers"></a>Conexión a servidores

En este artículo se describe la conexión a un servidor mediante aplicaciones de modelado y administración de datos, como SQL Server Management Studio (SSMS) o Visual Studio con proyectos de Analysis Services, o con aplicaciones de informes de cliente como Microsoft Excel, Power BI Desktop o aplicaciones personalizadas. Las conexiones a Azure Analysis Services usan HTTPS.

## <a name="client-libraries"></a>Bibliotecas de clientes

[Obtener las bibliotecas de cliente más recientes](https://docs.microsoft.com/analysis-services/client-libraries?view=azure-analysis-services-current)

Todas las conexiones a un servidor, con independencia del tipo, requieren las bibliotecas de cliente AMO, ADOMD.NET, y OLEDB actualizadas para conectarse a un servidor de Analysis Services e interaccionar con él. En el caso de SSMS, Visual Studio, Excel 2016 y versiones posteriores y Power BI, las bibliotecas cliente más recientes se instalan o actualizan con las versiones mensuales. Sin embargo, en algunos casos, es posible que una aplicación no tenga la más reciente. Por ejemplo, cuando las directivas retrasan las actualizaciones o las actualizaciones de Microsoft 365 están en el canal diferido.

> [!NOTE]
> Las bibliotecas de cliente no se pueden conectar a Azure Analysis Services a través de servidores proxy que requieran un nombre de usuario y contraseña. 

## <a name="server-name"></a>Nombre de servidor

Al crear un servidor de Analysis Services en Azure, especifique un nombre único y la región donde se creará el servidor. Al especificar el nombre del servidor en una conexión, el esquema de nombres del servidor es:

```
<protocol>://<region>/<servername>
```
 Donde protocol es la cadena **asazure**, region es el URI donde se creó el servidor (por ejemplo, westus.asazure.windows.net) y servername es el nombre de su servidor único dentro de la región.

### <a name="get-the-server-name"></a>Obtención del nombre del servidor

En **Azure Portal** > servidor > **Información general** > **Nombre de servidor**, copie el nombre del servidor completo. Si otros usuarios de su organización también se conectan a este servidor, podrá compartir dicho nombre con ellos. Al especificar un nombre de servidor, se debe usar la ruta de acceso completa.

![Obtención del nombre del servidor en Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

> [!NOTE]
> El protocolo para la región Este de EE. UU. 2 es **aspaaseastus2**.

## <a name="connection-string"></a>Cadena de conexión

Al conectarse a Azure Analysis Services mediante el modelo de objetos tabular, utilice los siguientes formatos de cadena de conexión:

###### <a name="integrated-azure-active-directory-authentication"></a>Autenticación integrada de Azure Active Directory

La autenticación integrada selecciona la memoria caché de credenciales de Azure Active Directory, si está disponible. De lo contrario, se muestra la ventana de inicio de sesión de Azure.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Autenticación de Azure Active Directory con el nombre de usuario y la contraseña

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Autenticación de Windows (seguridad integrada)

Use la cuenta de Windows que ejecuta el proceso actual.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>Conexión mediante un archivo .odc

Con versiones anteriores de Excel, los usuarios pueden conectarse a un servidor de Azure Analysis Services mediante un archivo de conexión de datos de Office (.odc). Para más información, consulte [Creación de un archivo de conexión de datos de Office (.odc)](analysis-services-odc.md).

## <a name="connect-as-a-linked-server-from-sql-server"></a>Conectividad como un servidor vinculado desde SQL Server

SQL Server puede conectarse a un recurso de Azure Analysis Services como un [servidor vinculado](https://docs.microsoft.com/sql/relational-databases/linked-servers/create-linked-servers-sql-server-database-engine) especificando MSOLAP como proveedor de origen de datos. Antes de configurar una conexión de servidor vinculado, asegúrese de instalar la [biblioteca de cliente de MSOLAP](https://docs.microsoft.com/analysis-services/client-libraries?view=azure-analysis-services-current) (proveedor) más reciente. 

En el caso de las conexiones del servidor vinculado a Azure Analysis Services, se debe crear una instancia del proveedor MSOLAP fuera del proceso de SQL Server. Al configurar las opciones de servidor vinculado, asegúrese de que la opción **Permitir InProcess** **no esté seleccionada**.

Si se selecciona **Permitir InProcess** y se crea una instancia del proveedor en el proceso de SQL Server, se devuelve el siguiente error:

```
OLE DB provider "MSOLAP" for linked server "(null)" returned message "The following system error occurred: ".

OLE DB provider "MSOLAP" for linked server "(null)" returned message "The connection failed because user credentials are needed and Sign-In UI is not allowed.".

Msg 7303, Level 16, State 1, Line 2
Cannot initialize the data source object of OLE DB provider "MSOLAP" for linked server "(null)".
```



## <a name="next-steps"></a>Pasos siguientes

[Connect with Excel](analysis-services-connect-excel.md)   (Conexión con Excel)  
[Connect with Power BI](analysis-services-connect-pbi.md)  (Conexión con Power BI)  
[Administración del servidor](analysis-services-manage.md)   

