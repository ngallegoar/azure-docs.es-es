---
title: Cliente de prueba automática para la validación previa de una máquina virtual | Azure Marketplace
description: Creación de un cliente de prueba automática para la validación previa de una imagen de máquina virtual para Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: dsindona
ms.openlocfilehash: af42476f9d04f7f2bfc275c731b02aa5a9b8ecf6
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273161"
---
# <a name="create-a-self-test-client-to-pre-validate-an-azure-virtual-machine-image"></a>Creación de un cliente de prueba automática para la validación previa de una imagen de máquina virtual de Azure

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Certificación de imágenes de máquina virtual de Azure](https://aks.ms/CertifyVMimage) para administrar las ofertas migradas.

Use este artículo como guía para crear un servicio de cliente que use la API de prueba automática. Puede usar la API de prueba automática para validar previamente una máquina virtual para asegurarse de que cumple con los requisitos de publicación más recientes de Azure Marketplace. Este servicio de cliente le permite probar una máquina virtual antes de enviar la oferta para la certificación de Microsoft.

## <a name="development-and-testing-overview"></a>Introducción sobre el desarrollo y las pruebas

Como parte del proceso de prueba automática, se crea un cliente local que se conecta a Azure Marketplace para validar una máquina virtual que se ejecuta en la suscripción de Azure. La máquina virtual puede ejecutar el sistema operativo Windows o Linux.

El cliente local ejecuta un script que permite autenticarse con la API de prueba automática, envía información de conexión y recibe los resultados de las pruebas.

Los pasos de alto nivel para la creación de un cliente de prueba automática son:

1. Elija el inquilino de Azure Active Directory (AD) para la aplicación.
2. Registre la aplicación cliente.
3. Cree un token para la aplicación cliente de Azure AD.
4. Pase el token a la API de prueba automática.

Después de crear el cliente, puede probarlo en la máquina virtual.

### <a name="self-test-client-authorization"></a>Autorización del cliente de prueba automática

El diagrama siguiente muestra cómo funciona la autorización para las llamadas de servicio a servicio mediante las credenciales del cliente (secreto o certificado compartido).

![Proceso de autorización del cliente](./media/stclient-dev-process.png)

## <a name="the-self-test-client-api"></a>La API de cliente de prueba automática

La API de prueba automática contiene un punto de conexión único que solo admite el método POST.  Tiene la estructura siguiente.

```
Uri:             https://isvapp.azurewebsites.net/selftest-vm
Method:          Post
Request Header:  Content-Type: "application/json"
Authorization:   "Bearer xxxx-xxxx-xxxx-xxxxx"
Request body:    The Request body parameters should use the following JSON format:
                 {
                   "DNSName":"XXXX.westus.cloudapp.azure.com",
                   "User":"XXX",
                   "Password":"XXX@1234567",
                   "OS":"XXX",
                   "PortNo":"22",
                   "CompanyName":"ABCD",
                 }
```

En la siguiente tabla se describen los campos de la API.


|      Campo         |    Descripción    |
|  ---------------   |  ---------------  |
|  Authorization     |  La cadena "Bearer xxxx-xxxx-xxxx-xxxxx" contiene el token de cliente de Azure Active Directory (AD), que se puede crear mediante PowerShell.          |
|  DNSName           |  Nombre DNS de la máquina virtual que se va a probar    |
|  Usuario              |  Nombre de usuario para iniciar sesión en la máquina virtual         |
|  Contraseña          |  Contraseña para iniciar sesión en la máquina virtual          |
|  SO                |  Sistema operativo de la máquina virtual: ya sea `Linux` o `Windows`          |
|  PortNo            |  Número de puerto abierto para conectarse a la máquina virtual. Suele ser el número de puerto `22` para Linux y `5986` para Windows.          |
|  |  |

## <a name="consuming-the-api"></a>Uso de la API

Puede usar la API de prueba automática con PowerShell o cURL.

### <a name="use-powershell-to-consume-the-api-on-the-linux-os"></a>Uso de PowerShell para utilizar la API en el sistema operativo Linux

Para llamar a la API en PowerShell, siga estos pasos:

1. Use el comando `Invoke-WebRequest` para llamar a la API.
2. El método es Post y el tipo de contenido es JSON, tal como se muestra en la siguiente captura de pantalla y el ejemplo de código.
3. Especifique los parámetros del cuerpo en formato JSON.

El ejemplo de código siguiente muestra una llamada de PowerShell a la API.

```powershell
$accesstoken = "Get token for your Client AAD App"
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Linux"
    "PortNo" = "22"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" -Headers $headers;
$Content = $res | ConvertFrom-Json
```
La captura de pantalla siguiente muestra un ejemplo para llamar a la API en PowerShell.

![Llamada a la API con PowerShell para el sistema operativo Linux](./media/stclient-call-api-ps-linuxvm.png)

Mediante el ejemplo anterior, puede recuperar el archivo JSON y analizarlo para obtener los detalles siguientes:

```powershell
$testresult = ConvertFrom-Json -InputObject (ConvertFrom-Json -InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++)
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

La siguiente captura de pantalla, que muestra `$res.Content`, proporciona los detalles de los resultados de las pruebas en formato JSON.

![Resultados de la llamada de PowerShell a Linux en formato JSON](./media/stclient-pslinux-rescontent-json.png)

La captura de pantalla siguiente muestra un ejemplo de los resultados de las pruebas que se ven en un visor JSON en línea (por ejemplo, [Code Beautify](https://codebeautify.org/jsonviewer) o [Visor de JSON](https://jsonformatter.org/json-viewer)).

![Resultados de la llamada de PowerShell a la máquina virtual Linux en formato JSON](./media/stclient-consume-api-pslinux-json.png)

### <a name="use-powershell-to-consume-the-api-on-the-windows-os"></a>Uso de PowerShell para utilizar la API en el sistema operativo Windows

Para llamar a la API en PowerShell, siga estos pasos:

1. Use el comando `Invoke-WebRequest` para llamar a la API.
2. El método es Post y el tipo de contenido es JSON, tal como se muestra en la siguiente captura de pantalla y el ejemplo de código.
3. Cree los parámetros del cuerpo en formato JSON.

El ejemplo de código siguiente muestra una llamada de PowerShell a la API.

```powershell
$accesstoken = "Get token for your Client AAD App"
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Windows"
    "PortNo" = "5986"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" -Headers $headers;
$Content = $res | ConvertFrom-Json
```

La captura de pantalla siguiente muestra un ejemplo para llamar a la API en PowerShell.

![Llamada a la API con PowerShell para una máquina virtual Windows](./media/stclient-call-api-ps-windowsvm.png)

Mediante el ejemplo anterior, puede recuperar el archivo JSON y analizarlo para obtener los detalles siguientes:

```powershell
$testresult = ConvertFrom-Json -InputObject (ConvertFrom-Json -InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++)
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

La siguiente captura de pantalla, que muestra `$res.Content`, proporciona los detalles de los resultados de las pruebas en formato JSON.

![Resultados de la llamada de PowerShell a Windows en formato JSON](./media/stclient-pswindows-rescontent-json.png)

La captura de pantalla siguiente muestra los resultados de las pruebas que se ven en un visor JSON en línea.
(Por ejemplo, [Code Beautify](https://codebeautify.org/jsonviewer), [Visor JSON](https://jsonformatter.org/json-viewer))

![Resultados de la llamada de PowerShell a una máquina virtual Windows en formato JSON](./media/stclient-consume-api-pswindows-json.png)

### <a name="use-curl-to-consume-the-api-on-the-linux-os"></a>Uso de cURL para utilizar la API en el sistema operativo Linux

Para llamar a la API con cURL, siga estos pasos:

1. Use el comando curl para llamar a la API.
2. El método es Post y el tipo de contenido es JSON, tal como se muestra en el siguiente fragmento de código.

```
CURL POST -H "Content-Type:application/json"
-H "Authorization: Bearer XXXXXX-Token-XXXXXXXX"
https://isvapp.azurewebsites.net/selftest-vm
-d '{ "DNSName":"XXXX.westus.cloudapp.azure.com", "User":"XXX", "Password":"XXXX@123456", "OS":"Linux", "PortNo":"22", "CompanyName":"ABCD"}'
```

La siguiente pantalla muestra un ejemplo del uso de curl para llamar a la API.

![Llamada a la API con el comando curl](./media/stclient-consume-api-curl.png)

La siguiente captura de pantalla muestra los resultados de la llamada de curl en formato JSON.

![Resultados de la llamada de curl en formato JSON](./media/stclient-consume-api-curl-json.png)


## <a name="choose-the-azure-ad-tenant-for-the-app"></a>Elección del inquilino de Azure AD para la aplicación

Use los pasos siguientes para elegir el inquilino de Azure AD en el que desea crear la aplicación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En la barra de menús superior, seleccione su cuenta y, en la lista Directorio, elija el inquilino de Active Directory en el que desea registrar la aplicación. O bien, seleccione el icono **Directorio + suscripción** para ver el filtro de suscripción Global. La captura de pantalla siguiente muestra un ejemplo de este filtro.

   ![Selección del filtro de suscripción](./media/stclient-subscription-filter.png)

3. En la barra de navegación situada a la izquierda, seleccione **Todos los servicios** y, después, **Azure Active Directory**.

   En los pasos siguientes, puede que necesite el nombre del inquilino (o del directorio) o el identificador de este (o el identificador del directorio).

   **Para obtener la información del inquilino:**

   En **Información general sobre Azure Active Directory**, busque "Propiedades" y seleccione **Propiedades**. Use la captura de pantalla siguiente como ejemplo:

   - **Nombre**: el nombre del inquilino o del directorio
   - **Identificador de directorio**: el identificador del inquilino o del directorio, o use la barra de desplazamiento para buscar Propiedades.

   ![Página de propiedades de Azure Active Directory](./media/stclient-aad-properties.png)

## <a name="register-the-client-app"></a>Registro de la aplicación cliente

Para registrar la aplicación cliente, realice los pasos siguientes.

1. En la barra de navegación situada a la izquierda, seleccione **Todos los servicios** y, después, **Registros de aplicaciones**.
2. En **Registros de aplicaciones**, seleccione **+ Nuevo registro de aplicaciones**.
3. En **Crear**, proporcione la información necesaria para los campos siguientes:

   - **Nombre**: escriba un nombre descriptivo para la aplicación. Por ejemplo, "SelfTestClient".
   - **Tipo de aplicación**: seleccione **Aplicación web o API**
   - **URL de inicio de sesión**: escriba "https:\//isvapp.azurewebsites.net/selftest-vm"

4. Seleccione **Crear**.
5. En **Registros de aplicaciones** o **Aplicación registrada**, copia el **Id. de la aplicación**.

   ![Obtención del identificador de la aplicación](./media/stclient-app-id.png)

6. En la barra de herramientas Aplicación registrada, seleccione **Configuración**.
7. Seleccione **Permisos necesarios** para configurar los permisos de la aplicación.
8. En **Permisos necesarios**, seleccione **+ Agregar**.
9. En **Agregar acceso de API**, elija **Seleccionar una API**.
10. En **Seleccionar una API**, escriba "Modelo de implementación clásica de Windows Azure" para buscar la API.
11. En los resultados de búsqueda, seleccione **Modelo de implementación clásica de Windows Azure** y **Seleccionar**.

    ![Configuración de la compatibilidad multiinquilino para la aplicación](./media/stclient-select-api.png)

12. En **Agregar acceso de API**, haga clic en **Seleccionar permisos**.
13. Seleccione **Acceder a "Windows Azure Service Management API"** .

    ![Habilitar acceso a la API para la aplicación](./media/stclient-enable-api-access.png)

14. Haga clic en **Seleccionar**.
15. Seleccione **Listo**.
16. En **Configuración**, seleccione **Propiedades**.
17. En **Propiedades**, desplácese hacia abajo hasta **Multiinquilino**. Seleccione **Sí**.

    ![Configuración de la compatibilidad multiinquilino para la aplicación](./media/stclient-yes-multitenant.png)

18. Seleccione **Guardar**.
19. En **Configuración**, seleccione **Claves**.
20. Cree una clave secreta seleccionando el cuadro de texto **DESCRIPCIÓN** de la clave. Configure los campos siguientes:

    - Escriba un nombre de clave. Por ejemplo, "selftestclient"
    - En la lista desplegable **EXPIRA**, seleccione "En 1 año".
    - Seleccione **Guardar** para generar la clave.
    - En **VALOR**, copie la clave.

      >[!Important]
      >No puede ver el valor de la clave después de salir del formulario **Claves**.

    ![Formulario del valor de clave](./media/stclient-create-key.png)

## <a name="create-the-token-for-the-client-app"></a>Creación del token para la aplicación cliente

Puede usar cualquiera de los siguientes programas para crear y obtener un token con la API REST de OAuth:

- postman
- cURL en Linux
- C&#35;
- PowerShell

### <a name="to-create-and-get-a-token-using-postman"></a>Creación y obtención de un token con Postman

 Para solicitar tokens a Auth0 para todas las aplicaciones autorizadas, realice una operación POST en el punto de conexión [https://login.microsoftonline.com/common/oauth2/token](https://login.microsoftonline.com/common/oauth2/token) con una carga en el formato siguiente:

```
Method Type : POST
Base Url: https://login.microsoftonline.com/common/oauth2/token
```

Use los siguientes parámetros en el cuerpo de la solicitud:

```
Body Content-Type: x-www-form-urlencoded
client_id: XXX (Paste your Application ID of Web App/API Type client AD App)
grant_type: client_credentials
client_secret: XXX (Paste your Secret Key of Web App/API Type client AD App)
resource: https://management.core.windows.net
```

Use los siguientes parámetros en el encabezado de la solicitud:

```
Content-Type: application/x-www-form-urlencoded
```

La captura de pantalla siguiente muestra un ejemplo de uso de Postman para obtener un token.

![Obtención de un token con Postman](./media/stclient-postman-get-token.png)

### <a name="to-create-and-get-a-token-using-curl-in-linux"></a>Creación y obtención de un token con cURL en Linux

Para solicitar tokens a Auth0 para todas las aplicaciones autorizadas, realice una operación POST en el punto de conexión [https://login.microsoftonline.com/common/oauth2/token](https://login.microsoftonline.com/common/oauth2/token) con una carga en el formato siguiente:

```
Request:

curl --request POST \
                --url 'https://login.microsoftonline.com/common/oauth2/token' \
                --header "Content-Type:application/x-www-form-urlencoded" \
                --data "grant_type=client_credentials&client_id=XXXXX-XXXX-XXXX-XXXXXXX&client_secret=XXXXXXXXX&resource=https://management.core.windows.net"

Response:

{"token":"UClCUUKxUlkdbhE1cHLz3kyjbIZYVh9eB34A5Q21Y3FPqKGSJs","expires":"2014-02-17 18:46:08"}
```

La captura de pantalla siguiente muestra un ejemplo de uso del comando curl para obtener un token.

![Obtención de un token con el comando curl](./media/stclient-curl-get-token.png)

### <a name="to-create-and-get-a-token-using-c35"></a>Creación y obtención de un token con C#

Para solicitar tokens a Auth0 para todas las aplicaciones autorizadas, realice una operación POST en el punto de conexión \//soamtenant.auth0.com/oauth/token con una carga en el formato siguiente:

```csharp
string clientId = "Your Application Id";
string clientSecret = "Your Application Secret";
string audience = "https://management.core.windows.net";
string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, "https://login.microsoftonline.com/common/oauth2/token");
string grantType = "client_credentials";
var client = new RestClient(authority);
var request = new RestRequest(Method.POST);
       request.AddHeader("content-type", "application/x-www-form- urlencoded");

string requestBody = "grant_type=" + grantType + "&" + "client_id=" +   clientId + "&" + "client_secret=" + clientSecret + "&" + "resource=" +  audience;

request.AddParameter("application/x-www-form-urlencoded", requestBody,  ParameterType.RequestBody);

IRestResponse response = client.Execute(request);
var content = response.Content;
var token = JObject.Parse(content)["access_token"];
```

### <a name="to-create-and-get-a-token-using-powershell"></a>Creación y obtención de un token con PowerShell

Para solicitar tokens a Auth0 para todas las aplicaciones autorizadas, realice una operación POST en el punto de conexión \//soamtenant.auth0.com/oauth/token con una carga en el formato siguiente:

```powershell
$clientId = "Application Id of AD Client APP";
$clientSecret = "Secret Key of AD Client APP "
$audience = "https://management.core.windows.net";
$authority = "https://login.microsoftonline.com/common/oauth2/token"
$grantType = "client_credentials";

$requestBody = "grant_type=" + $grantType + "&" + "client_id=" +  $clientId   + "&" + "client_secret=" + $clientSecret + "&" + "resource=" + $audience;

$headers = New-Object  "System.Collections.Generic.Dictionary[[String],[String]]"
             $headers.Add("ContentType", "application/x-www-form-urlencoded")
resp = Invoke-WebRequest -Method Post -Uri $authority -Headers $headers -ContentType 'application/x-www-form-urlencoded' -Body $requestBody

$token = $resp.Content | ConvertFrom-Json
$token.AccessToken
```

## <a name="pass-the-client-app-token-to-the-api"></a>Paso del token de la aplicación cliente a la API

Pase el token a la API de prueba automática mediante el código siguiente en el encabezado de autorización:

```powershell
$redirectUri = 'https://isvapp.azurewebsites.net/selftest-vm'
$accesstoken = 'place your token here'

$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body =
@{
      'DNSName'="XXXX.cloudapp.azure.com";
      'User'="XXX";
      'Password'="XXXX@12345";
      'OS'="Linux";
      'PortNo'="22"
  } | ConvertTo-Json

$result=Invoke-WebRequest -Method Post -Uri $redirectUri -Headers $headers -ContentType 'application/json' -Body $Body
$result
Write-Output 'Test Results:'
$result.Content
```

## <a name="test-your-self-test-client"></a>Comprobación del cliente de prueba automática

Para probar el cliente, siga estos pasos:

1. Implemente la máquina virtual que desea probar.
2. Llame a la API de prueba automática mediante el token de la aplicación cliente para obtener autorización.
3. Obtenga los resultados de la prueba en formato JSON.

### <a name="test-result-examples"></a>Ejemplos de resultado de pruebas

Los siguientes fragmentos de código muestran los resultados de las pruebas en formato JSON.

**Resultados de las pruebas para una máquina virtual Windows:**

```json
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Microsoft Windows Server 2016 Datacenter",
  "OSVersion": "10.0.14393",
  "CreatedDate": "06/01/2018 07:48:04",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "Tes tID": "1.1.4",
      "TestCaseName": "OS Architecture",
      "Description": "Azure supports 64bit Operating System Only.",
      "Result": "Passed",
      "ActualValue": "64 Bit",
      "Re quiredValue": "OS Should be 64 bit"
    },
    {
      "TestID": "1.1.5",
      "TestCaseName": "User account dependency",
      "Description": "Application execution should not have de pendency on administrator account.",
      "Result": "Passed",
      "ActualValue": "isv",
      "RequiredValue": "Logged in user should not be an administrator"
    },
    {
      "TestID": " 1.1.6",
      "TestCaseName": "Failover Cluster",
      "Description": "Windows Server Failover Clustering feature is not yet supported, Application should not have dependency on this feature.",
      "Result": "Passed",
      "ActualValue": "Disabled",
      "RequiredValue": "Failover Clustering feature is disabled"
    },
```

**Resultados de las pruebas para una máquina virtual Linux:**

```json
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Ubuntu  16.04",
  "OSVersion": "16.04",
  "CreatedDate": "06/01/2018 07:04:24",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "TestID": "48",
      "TestCaseName": "Bash Hi story",
      "Description": "Bash history files should be cleared before creating the VM image.",
      "Result": "Passed",
      "ActualValue": "No file Exist",
      "RequiredVal ue": "1024"
    },
    {
      "TestID": "39",
      "TestCaseName": "Linux Agent Version",
      "Description": "Azure Linux Agent 2.2.10 and above should be installed. ",
      "Result": "Pas sed",
      "ActualValue": "2.2.20",
      "RequiredValue": "2.2.10"
    },
    {
      "TestID": "40",
      "TestCaseName": "Required Kernel Parameters",
      "Description": "Verifies the following  kernel parameters are set console=ttyS0, earlyprintk=ttyS0, rootdelay=300",
      "Result": "Passed",
      "ActualValue": "Matched Parameter: console=ttyS0,earlypri ntk=ttyS0,rootdelay=300",
      "RequiredValue": "console=ttyS0#earlyprintk=ttyS0#rootdelay=300"
    },
```

## <a name="next-steps"></a>Pasos siguientes

Cuando haya probado correctamente la máquina virtual de Azure, podrá [publicar la oferta](./cpp-publish-offer.md).
