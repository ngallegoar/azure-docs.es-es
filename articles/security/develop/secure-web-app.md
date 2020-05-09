---
title: Desarrollo de una aplicación web segura | Microsoft Docs
description: Esta sencilla aplicación de ejemplo implementa procedimientos recomendados de seguridad que mejoran la seguridad de la aplicación y la organización al desarrollar en Azure.
keywords: na
services: security
documentationcenter: na
author: isaiah-msft
manager: barbkess
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.subservice: security-develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: terrylan
ms.custom: has-adal-ref
ms.openlocfilehash: 690cb37df4a5d195bfce6ee792f7565a6f7f1768
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612782"
---
# <a name="develop-a-secure-web-app"></a>Desarrollar una aplicación web segura

Este ejemplo es una aplicación de Python sencilla que muestra una página web que contiene vínculos a recursos de seguridad para el desarrollo de aplicaciones en Azure. Esta aplicación implementa procedimientos recomendados de seguridad que pueden ayudar a mejorar seguridad de la aplicación y la organización al desarrollar aplicaciones en Azure.

Debe seguir los pasos que se describen en este artículo secuencialmente para asegurarse de que los componentes de la aplicación estén configurados correctamente. La base de datos, Azure App Service, la instancia de Azure Key Vault y la instancia de Azure Application Gateway son dependientes entre sí.

Los scripts de implementación configuran la infraestructura. Después de ejecutar los scripts de implementación, deberá realizar cierta configuración manual en Azure Portal para vincular los componentes y los servicios.

La aplicación de ejemplo está orientada a principiantes que desarrollan aplicaciones en Azure y quieren implementar medidas de seguridad en sus aplicaciones.

Al desarrollar e implementar esta aplicación, aprenderá a:

- Crear una instancia de Azure Key Vault, y almacenar y recuperar sus secretos.
- Implementar Azure Database for PostgreSQL, configurar contraseñas seguras y autorizar el acceso a este.
- Ejecute un contenedor de Alpine Linux en Azure Web Apps para Linux y habilite las identidades administradas para los recursos de Azure.
- Cree y configure una instancia de Azure Application Gateway con un firewall que use [el conjunto de 10 reglas principales de OWASP](https://coreruleset.org/).
- Habilite el cifrado de datos en tránsito y en reposo mediante los servicios de Azure.

Después de desarrollar e implementar esta aplicación, habrá configurado la aplicación web de ejemplo siguiente junto con las medidas de configuración y seguridad que se describen.

![Aplicación web de ejemplo](./media/secure-web-app/demo-app.png)

## <a name="architecture"></a>Architecture

La aplicación es una aplicación de n niveles típica con tres niveles. El front-end, el back-end y el nivel de base de datos con los componentes de supervisión y administración de secretos integrados se muestran aquí:

![Arquitectura de la aplicación](./media/secure-web-app/architecture.png)

La arquitectura consta de los siguientes componentes:

- [Azure Application Gateway](../../application-gateway/index.yml). Proporciona la puerta de enlace y el firewall para la arquitectura de la aplicación.
- [Azure Web Apps en Linux](../../app-service/containers/app-service-linux-intro.md). Proporciona el entorno en tiempo de ejecución del contenedor para ejecutar la aplicación Python en un entorno Linux.
- [Azure Key Vault](../../key-vault/index.yml) Almacena y cifra los secretos de la aplicación y administra la creación de directivas de acceso en torno a ellos.
- [Azure Database for PostgreSQL](https://azure.microsoft.com/services/postgresql/). Almacena de forma segura los datos de la aplicación.
- [Azure Security Center](../../security-center/index.yml) y [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Proporciona supervisión y alertas sobre el funcionamiento de la aplicación.

## <a name="threat-model"></a>Modelo de amenazas

El proceso de modelado de amenazas consiste en identificar posibles amenazas de seguridad para la empresa y la aplicación y, luego, asegurarse que exista un plan de mitigación adecuado.

En este ejemplo, se usó [Microsoft Threat Modeling Tool](threat-modeling-tool.md) para implementar el modelado de amenazas para la aplicación de ejemplo segura. Mediante la creación de diagramas de los componentes y los flujos de datos, puede identificar los problemas y las amenazas al principio del proceso de desarrollo. Esto ahorra tiempo y dinero más tarde.

Este es el modelo de amenazas para la aplicación de ejemplo:

![Modelo de amenazas](./media/secure-web-app/threat-model.png)

En la captura de pantalla siguiente se muestran algunas amenazas de ejemplo y posibles vulnerabilidades que genera Threat Modeling Tool. El modelo de amenazas ofrece información general sobre la superficie expuesta a ataques y solicita a los desarrolladores que piensen en cómo mitigar los problemas.

![Salida del modelo de amenazas](./media/secure-web-app/threat-model-output.png)

Por ejemplo, la inyección de SQL en la salida del modelo de amenazas anterior se mitiga mediante la limpieza de las entradas del usuario y el uso de funciones almacenadas en Azure Database for PostgreSQL. Esta mitigación evita la ejecución arbitraria de consultas durante las lecturas y escrituras de datos.

Los desarrolladores mejoran la seguridad general del sistema mediante la mitigación de cada una de las amenazas de la salida del modelo de amenazas.

## <a name="deployment"></a>Implementación

Las siguientes opciones le permiten ejecutar Linux en Azure App Service:

- Elija un contenedor de la lista de contenedores de Microsoft precompilados en Azure creados con tecnologías auxiliares (Python, Ruby, PHP, Java, Node.js, .NET Core).
- Use un contenedor compilado de forma personalizada. Seleccione sus propios registros de contenedor como el origen de la imagen y use las numerosas tecnologías disponibles que admiten HTTP.

En este ejemplo, ejecutará el script de implementación que implementará la aplicación web en App Service y creará los recursos.

La aplicación puede usar los diferentes modelos de implementación que se muestran a continuación:

![Diagrama de flujo de datos de implementación](./media/secure-web-app/deployment.png)

Hay muchas maneras de implementar aplicaciones en Azure, entre las que se incluyen las siguientes:

- Plantillas del Administrador de recursos de Azure
- PowerShell
- Azure CLI
- Portal de Azure
- Azure DevOps

En esta aplicación, se ha usado:

- [Docker](https://docs.docker.com/) para crear y compilar las imágenes de contenedor.
- La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para la implementación.
- [Docker Hub](https://hub.docker.com/) como registro de contenedor.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

### <a name="network"></a>Red

La aplicación de ejemplo usa el cifrado TLS/SSL de un extremo a otro para el flujo de datos en tránsito que entra y sale de la red. La puerta de enlace se ha configurado con un certificado autofirmado.
> [!IMPORTANT]
> En esta demostración, se usa un certificado autofirmado. En un entorno de producción, debe obtener los certificados de una entidad de certificación (CA) verificada.

El firewall de la aplicación también inspecciona el tráfico entrante y los administradores de alertas cuando se detecta tráfico malintencionado en el tráfico de red.
Application Gateway reduce la posibilidad de que se detecten amenazas de DDoS e inyección de SQL en el modelo de amenazas.

### <a name="identity"></a>Identidad

Para iniciar sesión en el portal, la aplicación de ejemplo usa Multi-Factor Authentication para los administradores de Azure Active Directory (Azure AD) que tienen asignado acceso a los recursos.
La aplicación de ejemplo usa identidades administradas para obtener permisos para leer y recuperar secretos de Azure Key Vault, lo que garantiza que la aplicación no necesita codificar las credenciales y los tokens para leer los secretos. Azure AD crea automáticamente las entidades de servicio que la aplicación necesita leer y modifica los secretos cuando se usan identidades administradas.

Las identidades administradas para los recursos de Azure y MFA hacen que sea más difícil para los adversarios obtener privilegios y escalar sus privilegios en el sistema. Esta amenaza se señaló en el modelo de amenazas.
La aplicación usa OAuth, que permite a los usuarios registrados en la aplicación OAuth iniciar sesión en la aplicación.

### <a name="storage"></a>Storage

Azure Database for PostgreSQL cifra los datos de la base de datos PostgreSQL en reposo automáticamente. La base de datos autoriza las direcciones IP de App Service para que solo la aplicación web de App Service implementada pueda acceder a los recursos de la base de datos con las credenciales de autenticación correctas.

### <a name="logging-and-auditing"></a>Registro y auditoría

La aplicación implementa el registro mediante el uso de Application Insights para realizar un seguimiento de las métricas, los registros y las excepciones que se producen. Este registro proporciona suficientes metadatos de aplicación para informar a los desarrolladores y a los miembros del equipo de operaciones sobre el estado de la aplicación. También proporciona suficientes datos para retroceder en caso de incidentes de seguridad.

## <a name="cost-considerations"></a>Consideraciones sobre el costo

En caso de no tener ninguna cuenta de Azure, puede crear una gratuitamente. Vaya a la [página de la cuenta gratuita](https://azure.microsoft.com/free/) para comenzar, ver lo que puede hacer con una cuenta gratuita de Azure y averiguar qué productos son gratis durante 12 meses.

Para implementar los recursos de la aplicación de ejemplo con las características de seguridad, debe pagar algunas características prémium. A medida que la aplicación se escala y los niveles gratis y las pruebas que ofrece Azure deben actualizarse para cumplir los requisitos de la aplicación, es posible que los costos aumenten. Puede usar la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) de Azure para calcular los costos.

## <a name="deploy-the-solution"></a>Implementación de la solución

### <a name="prerequisites"></a>Prerrequisitos

Para poner en marcha la aplicación, debe instalar estas herramientas:

- Un editor de código para modificar y ver el código de la aplicación.[Visual Studio Code](https://code.visualstudio.com/) es una opción de código abierto.
- La [CLI de Azure](/cli/azure/install-azure-cli) en el equipo de desarrollo.
- [GIT](https://git-scm.com/) en el sistema. GIT se usa para clonar el código fuente localmente.
- [jq](https://stedolan.github.io/jq/), una herramienta de UNIX fácil de usar para consultar JSON.

Necesita una suscripción a Azure para implementar los recursos de la aplicación de ejemplo. Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/) para probar la aplicación de ejemplo.

Después de instalar estas herramientas, ya puede implementar la aplicación en Azure.

### <a name="environment-setup"></a>Configuración del entorno

Ejecute los scripts de implementación para configurar el entorno y la suscripción:

1. Para clonar el repositorio de código fuente, use este comando de GIT:

   ```shell
   git clone https://github.com/Azure-Samples/sample-linux-python-app tutorial-project
   ```

2. Para ir al directorio, use este comando:

   ```shell
   cd tutorial-project/scripts
   ```

3. Hay archivos en la carpeta de scripts que son específicos de la plataforma que usa (Windows o Linux). Como ya se ha instalado la CLI de Azure, inicie sesión en la cuenta de Azure en el símbolo del sistema mediante la ejecución de este comando de la CLI de Azure:

   ```azurecli-interactive
   az login
   ```

Cuando se abra el explorador, inicie sesión con sus credenciales. Después de iniciar sesión, puede empezar a implementar los recursos desde el símbolo del sistema.

Los scripts `deploy-powershell.ps1` y `deploy-bash.sh` de desarrollo contienen código que implementa toda la aplicación.
Para implementar la solución:

1. Si está en PowerShell, ejecute el archivo `deploy-powershell.ps1` escribiendo `./deploy-powershell.ps1 REGION RESOURCE_GROUP_NAME` y reemplazando el nombre de la región y el grupo de recursos con las regiones de Azure adecuadas y un nombre para el grupo de recursos.
2. Si está en Linux, ejecute el archivo `deploy-bash.sh` escribiendo `/deploy-bash.sh REGION RESOURCE_GROUP_NAME`. Puede que tenga que escribir `chmod +x deploy-bash.sh` para poder ejecutar el archivo.

En los siguientes ejemplos se muestran fragmentos de código de los componentes clave. Puede implementar los ejemplos individualmente o con el resto de los componentes mediante la ejecución de los archivos de implementación.

### <a name="implementation-guidance"></a>Guía de implementación

El script de implementación es un script que se puede dividir en cuatro fases. Cada fase implementa y configura un recurso de Azure que se encuentra en el [diagrama de arquitectura](#architecture).

Las cuatro fases son:

- Implementar Azure Key Vault
- Implementar Azure Database for PostgreSQL
- Implementar Azure Web Apps en Linux
- Implementar Application Gateway con un firewall de aplicaciones web.

Cada fase se basa en la anterior mediante la configuración de los recursos implementados previamente.

Para completar los pasos de implementación, asegúrese de que ha instalado las herramientas que se indican en [Requisitos previos](#prerequisites).

#### <a name="deploy-azure-key-vault"></a>Implementar Azure Key Vault

En esta sección, creará e implementará una instancia de Azure Key Vault que se usa para almacenar secretos y certificados.

Después de completar la implementación, tendrá una instancia de Azure Key Vault implementada en Azure.

Para implementar Azure Key Vault mediante la CLI de Azure:

1. Declare las variables para Azure Key Vault.
2. Registre el proveedor de Azure Key Vault.
3. Cree el grupo de recursos para la instancia.
4. Cree la instancia de Azure Key Vault en el grupo de recursos creado en el paso 3.

   ```powershell-interactive

    function Get-Hash() {
        return (New-Guid).Guid.Split('-')[4]
    }

   az provider register -n Microsoft.KeyVault

   # Create the Azure Key Vault instance
   Write-Host "Creating Azure Key Vault instance: $($kvName)"
   az keyvault create --name $kvName `
       --resource-group $ResourceGroup `
       --location $Location `
       --verbose

   # Generate usernames and passwords using system functions and environment variables
   Write-Host "Generating PostgreSQL username and password"
   $pgUsername = "$($env:Username)$(Get-Hash)"
   $pgPassword = (New-Guid).Guid

   # Set the username secret in the Azure Key Vault instance
   Write-Host "Setting PostgreSQL username in KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGUSERNAME `
       --value $pgUsername `
       --verbose

   # Set the password secret in the Azure Key Vault instance
   Write-Host "Setting PostgreSQL password in KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGPASSWORD `
       --value $pgPassword `
       --verbose

   ```

El procedimiento recomendado es usar identidades administradas para los recursos de Azure en las aplicaciones que usan Key Vault para acceder a los recursos. La seguridad aumenta cuando las claves de acceso a Key Vault no se almacenan en el código ni en la configuración.

#### <a name="deploy-azure-database-for-postgresql"></a>Implementar Azure Database for PostgreSQL

Azure Database for PostgreSQL funciona de la siguiente manera: cree primero el servidor de bases de datos y, a continuación, cree la base de datos en la que se almacenará el esquema y los datos.

Una vez completada la implementación, tendrá un servidor y una base de datos PostgreSQL en ejecución en Azure.

Para implementar Azure Database for PostgreSQL mediante la CLI de Azure:

1. Abra un terminal con la CLI de Azure y la configuración de suscripción de Azure.
2. Genere una combinación de nombre de usuario y contraseña segura que se usará para acceder a la base de datos. Deben almacenarse en Azure Key Vault para las aplicaciones que los usan.
3. Cree la instancia del servidor PostgreSQL.
4. Cree una base de datos en la instancia del servidor que creó en el paso 3.
5. Ejecute scripts de PostgreSQL en la instancia de PostgreSQL.

El código siguiente se basa en los secretos PGUSERNAME y PGPASSWORD almacenados en Azure Key Vault en el paso de implementación de Key Vault anterior.

   ```powershell-interactive
   $pgUsername = $(az keyvault secret show --name PGUSERNAME --vault-name $kvName --query value) -replace '"',''
   $pgPassword = $(az keyvault secret show --name PGPASSWORD --vault-name $kvName --query value) -replace '"',''

   # Create an Azure Database for PostgreSQL server
   Write-Host "Creating the PostreSQL server: $($dbServer)"
   az  postgres server create -l $Location `
       --resource-group $ResourceGroup `
       --name $dbServer `
       --admin-user $pgUsername `
       --admin-password $pgPassword `
       --sku-name B_Gen5_1 `
       --verbose

   # Create a database in the server instance created above
   Write-Host "Creating the PostgreSQL database: $($dbName)"
   az postgres db create --resource-group $ResourceGroup `
       --server-name $dbServer `
       --name $dbName `
       --verbose

   # Retrieve the database information above
   $db = (az postgres server show --resource-group $ResourceGroup `
           --name $dbServer)

   $db = $db | ConvertFrom-Json

   # Get the Fully Qualified Domain Name (FQDN) of the database to use in
   # the database connection strings to be stored in Azure Key Vault
   Write-Host "Generating the PostgreSQL connection string"
   $PGCONNECTIONSTRING = "postgresql+psycopg2://${pgUsername}@$($db.fullyQualifiedDomainName):${pgPassword}@$($db.fullyQualifiedDomainName):$dbPort/$($dbName)?sslmode=$($dbSSLMode)"
   $PGCONNECTIONSTRING = $PGCONNECTIONSTRING + '"&"' + "sslrootcert=$($dbRootCertPath)"

   # Set the database connection string above into Azure Key Vault
   Write-Host "Setting the PostgreSQL connection string into KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGCONNECTIONSTRING `
       --value $PGCONNECTIONSTRING `
       --verbose
   ```

Después de implementar la base de datos, debe almacenar sus credenciales y la cadena de conexión en Azure Key Vault.
En la carpeta de scripts, hay un archivo `functions.sql` que contiene el código PL/pgSQL que crea funciones almacenadas cuando lo ejecuta. La ejecución de este archivo parametriza las entradas para limitar la inyección de SQL.

PostgreSQL está agrupado con una herramienta llamada `psql` que se usa para conectar con la base de datos. Para ejecutar `functions.sql`, debe conectarse a la instancia de Azure Database for PostgreSQL desde el equipo local y ejecutarla desde allí. La instalación de la herramienta psql se incluye en la instalación predeterminada de PostgreSQL en cada sistema operativo.
Para obtener más información, consulte la [documentación de psql](https://www.postgresql.org/docs/9.3/app-psql.html).

Azure Cloud Shell también incluye la herramienta `psql`. Para usar Cloud Shell directamente desde Azure Portal, seleccione el icono de Cloud Shell.

Para habilitar el acceso remoto a la instancia de PostgreSQL, debe autorizar la dirección IP en PostgreSQL.
Para habilitar este acceso, vaya a la pestaña **Seguridad de conexión**, seleccione **Agregar IP de cliente** y guarde la nueva configuración.

![Autorizar la dirección IP de cliente](./media/secure-web-app/add-client-ip-postgres.png)

Si usa Cloud Shell en lugar de la herramienta psql local, seleccione **Permitir el acceso a servicios de Azure** y cambie su valor a **Activado** para permitir el acceso de Cloud Shell.

A continuación, conéctese a la instancia ejecutando el siguiente comando psql con los parámetros de la cadena de conexión de la pestaña **Cadenas de conexión** de la instancia de PostgreSQL en Azure Portal.
Reemplace las llaves vacías por parámetros de la hoja Cadena de conexión de la base de datos y la contraseña por la contraseña de Azure Key Vault.

```shell
psql "host={} port=5432 dbname=hellodb user={} password=PGPASSWORD sslmode=require"
```

Ejecute el siguiente script PL/pgSQL después de asegurarse de que está conectado a la base de datos. El script crea las funciones almacenadas que se usan para insertar datos en la base de datos.

```shell
CREATE OR REPLACE FUNCTION insert_visitor(country VARCHAR(40), browser VARCHAR(40), operating_system VARCHAR(40)) RETURNS void AS $$
BEGIN
    INSERT INTO visitor(
        country,
        browser,
        operating_system,
        date_visited)
    VALUES (
        country,
        browser,
        operating_system,
        NOW()
    );
END;
$$ LANGUAGE PLPGSQL;

CREATE OR REPLACE FUNCTION insert_azure_document(title VARCHAR(40), url VARCHAR(100), category VARCHAR(40)) RETURNS void AS $$
BEGIN
    INSERT INTO azure_document(
        title,
        url,
        category)
    VALUES (
        title,
        url,
        category
    );
END;
$$ LANGUAGE PLPGSQL;
```

Para más información sobre cómo configurar TLS y la comprobación de la entidad de certificación (CA) para PostgreSQL, consulte [Configuración de la conectividad TLS en Azure Database for PostgreSQL](/azure/postgresql/concepts-ssl-connection-security).

En el contenedor se incluye un certificado raíz. Los pasos que se siguen para obtener el certificado son:

1. Descargar el archivo de certificado de la [entidad de certificación](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).
2. [Descargar e instalar OpenSSL en la máquina](/azure/postgresql/concepts-ssl-connection-security).
3. Descodificar el archivo del certificado:

   ```shell
   openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
   ```

Para más información sobre cómo configurar la seguridad TLS para PostgreSQL, consulte [Configuración de la seguridad de la conexión TLS](/azure/postgresql/concepts-ssl-connection-security).

#### <a name="deploy-azure-web-apps-on-linux"></a>Implementar Azure Web Apps en Linux

Puede compilar fácilmente servicios de Linux basados en Azure App Service, ya que Azure proporciona un conjunto de imágenes y contenedores precompilados para lenguajes comunes como Python, C#, Ruby y Java. Azure también admite contenedores personalizados, lo que permite que prácticamente todos los lenguajes de programación se ejecuten en la plataforma Azure App Service.

La aplicación que se va a implementar es una sencilla aplicación de Python que se ejecuta en la distribución de Ubuntu Linux más reciente. Se conecta a las instancias de Azure Key Vault y PostgreSQL creadas en las secciones anteriores para la administración de credenciales y el almacenamiento de datos, respectivamente.

El siguiente archivo de Docker se proporciona en la carpeta raíz de la aplicación:

```dockerfile
# Docker file for the basic web app
# Using the latest Alpine Linux

FROM alpine:latest

# Copy requirements to the container
COPY requirements.txt /tmp/requirements.txt

# Install Python and PostgreSQL dependencies
RUN apk update && \
    apk add --update bash gcc libffi-dev musl-dev  postgresql-dev python3 python3-dev && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --upgrade pip setuptools && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \
    pip3 install --no-cache-dir -r /tmp/requirements.txt && \
    rm -rf /.wh /root/.cache /var/cache /tmp/requirements.txt

# Change the working directory inside the container to /hello
WORKDIR /hello

# Copy the application code into the container
COPY . /hello

# Set the FLASK_APP environment variable used by flask migrate
ENV FLASK_APP=app.py

# Copy the init script to the container
COPY init.sh /usr/local/bin/

# Make the init script executable
RUN  chmod u+x /usr/local/bin/init.sh

# Expose the container web service endpoint
EXPOSE 8000

# Run the app with a non root user
RUN addgroup -g 1000 -S appgroup && \
    adduser  -u 1000 -S appuser -G appgroup

# Allow the non root user to access the folder
RUN chown -R appuser:appgroup /hello

# Switch to the non root user
USER appuser

# Set the init script as the file to be run during container startups
ENTRYPOINT ["/usr/local/bin/init.sh"]
```

El archivo Dockerfile anterior se utiliza para compilar el contenedor que se hospeda en Azure Container Registry en `mcr.microsoft.com/samples/basic-linux-app`.

El código siguiente:

1. Declara las variables y los nombres de la instancia de App Service.
2. Crea el grupo de recursos para el plan de App Service.
3. Aprovisiona una instancia de contenedores de Azure Web Apps en Linux.
4. Habilita el registro para el contenedor de la aplicación web.
5. Establece algunas configuraciones de aplicación en la configuración de la aplicación del contenedor.

   ```powershell-interactive
   Write-Host "Retrieving the Azure Key Vault URL"
   $kvURI = $(az keyvault show --name $kvName --query properties.vaultUri)

   # Create the App Service plan, using --linux for running containers on Web Apps on Linux
   Write-Host "Creating App Service Plan: $($appName)"
   az appservice plan create --name $appServicePlanName `
       --resource-group $ResourceGroup `
       --location $Location `
       --number-of-workers 1 `
       --sku B1 `
       --is-linux `
       --verbose

   # Create the web app
   Write-Host "Creating Azure Web App for Linux: $($appName)"
   az webapp create --name $appName `
       --resource-group $ResourceGroup `
       --plan $appServicePlanName `
       --deployment-container-image-name $containerName `
       --verbose

   # Assign a system-assigned identity
   # This creates a service principal to be used for managed identities for Azure resources allowing access to Key Vault Secrets without using auth keys/tokens
   Write-Host "Assigning Service Principal Identity to webapp: $($appName)"
   az webapp identity assign --name $appName `
       --resource-group $ResourceGroup `
       --verbose

   # Configure logging for the Docker container on App Service
   Write-Host "Configuring logging for the web app: $($appName)"
   az webapp log config --name $appName `
       --resource-group $ResourceGroup `
       --application-logging true `
       --detailed-error-messages true `
       --docker-container-logging filesystem `
       --verbose

   # Set app configuration settings to be set in the container's environment variables
   Write-Host "Setting app settings for our web app: $($appName)"
   az webapp config appsettings set --name $appName `
       --resource-group $ResourceGroup `
       --settings WEBSITE_TIME_ZONE=$timezone KEY_VAULT_URI=$kvURI `
       --verbose

   # Stop the web app to enable us to first allow the created managed identities for Azure resources service principals access to the Key Vault instance
   az webapp stop --name $appName `
       --resource-group $ResourceGroup `
       --verbose

   # Get all the outbound IPs the App Service instance might use
   Write-Host "Adding outbound Azure App Service IP's to the PostgreSQL database firewall."
   $outboundIps = (az webapp show --resource-group $ResourceGroup `
       --name $appName `
       --query outboundIpAddresses `
       --output tsv)

   # Loop over all the outbound IP addresses and authorize them in the PostgreSQL firewall
   $outboundIps = $outboundIps.Split(',')
   for($i=0; $i -lt $outboundIps.length; $i++) {
       Write-Output "Adding IP Rule $($outboundIps[$i]) on PostgreSQL for App Service"

       az postgres server firewall-rule create --name "OUTBOUND_IP_RULE$i" `
           --resource-group $ResourceGroup `
           --server-name $dbServer `
           --start-ip-address $outboundIps[$i] `
           --end-ip-address $outboundIps[$i] `
           --verbose
   }
   ```

Este script crea una identidad asignada para la instancia de App Service que se puede usar con MSI para interactuar con Azure Key Vault sin codificar de forma rígida los secretos en el código ni en la configuración.

Vaya a la instancia de Azure Key Vault en el portal para autorizar la identidad asignada en la pestaña de la directiva de acceso. Seleccione **Agregar una nueva directiva de acceso**. En **Seleccionar entidad**, busque el nombre de la aplicación similar al nombre de la instancia de App Service creada.
Una entidad de servicio adjuntada a la aplicación debe estar visible. Selecciónela y guarde la página de directiva de acceso, tal y como se muestra en la siguiente captura de pantalla.

Dado que la aplicación solo necesita recuperar claves, seleccione el permiso **Get** en las opciones de secretos para permitir el acceso y, al mismo tiempo, reducir los privilegios concedidos.

![Directiva de acceso de Key Vault](./media/secure-web-app/kv-access-policy.png)

*Crear una directiva de acceso a Key Vault*

Guarde la directiva de acceso y, a continuación, guarde el nuevo cambio en la pestaña **Directivas de acceso** para actualizar las directivas.

#### <a name="deploy-application-gateway-with-web-application-firewall-enabled"></a>Implementar Application Gateway con un firewall de aplicaciones web habilitado

En las aplicaciones web, no se recomienda exponer los servicios directamente al mundo exterior en Internet.
Las reglas de equilibrio de carga y firewall proporcionan más seguridad y control sobre el tráfico entrante y le ayudan a administrarlo.

Para implementar una instancia de Application Gateway:

1. Cree el grupo de recursos para hospedar Application Gateway.
2. Aprovisione una red virtual para conectarla a la puerta de enlace.
3. Cree una subred para la puerta de enlace en la red virtual.
4. Aprovisione una dirección IP pública.
5. Aprovisione Application Gateway.
6. Habilite el firewall de aplicaciones web en la puerta de enlace.

   ```powershell-interactive
   az keyvault certificate create --vault-name $kvName `
       --name $certName `
       --policy `@policy.json `
       --verbose

   az keyvault secret download --file $filePath `
       --encoding base64 `
       --name $certName `
       --vault-name $kvName `
       --verbose

   $pfxFile = Get-PfxData -FilePath $filePath

   $certPassword = Get-Random

   az keyvault secret set --vault-name $kvName `
       --name CERTPASSWORD `
       --value $certPassword `
       --verbose

   $signPassword = ConvertTo-SecureString $certPassword -Force -AsPlainText
   Export-PfxCertificate -PFXData $pfxFile -FilePath $certPath -Password $signPassword
   ```

El script anterior:

1. Crea un nuevo certificado autofirmado en Azure.
2. Descarga el certificado autofirmado como un archivo codificado en Base64.
3. Genera una contraseña para el certificado autofirmado.
4. Exporta el certificado como archivo PFX firmado con la contraseña.
5. Almacena la contraseña del certificado en Azure Key Vault.

En esta sección se implementa Application Gateway:

```powershell-interactive
# Create a virtual network required by the gateway
Write-Host "Creating the Azure Virtual Network: $($vnetName)"
az network vnet create --name $vnetName `
    --resource-group $ResourceGroup `
    --location $Location `
    --address-prefix $vnetAddressPrefix `
    --verbose

# Add a subnet to the virtual network
Write-Host "Creating the Subnet: $($gwSubnet)"
az network vnet subnet create --name $gwSubnet `
    --resource-group $ResourceGroup `
    --vnet-name $vnetName `
    --address-prefix $gatewayAddressPrefix `
    --verbose

# Create a public IP address that will be used by clients to access the application gateway
Write-Host "Creating the Public IP Address: $($publicIpName)"
az network public-ip create --resource-group $ResourceGroup `
    --name $publicIpName `
    --verbose

# Create the application gateway
Write-Host "Creating the Application Gateway: $($gwName)"
az network application-gateway create `
    --name $gwName `
    --resource-group $ResourceGroup `
    --location $Location `
    --vnet-name $vnetName `
    --subnet $gwSubnet `
    --public-ip-address $publicIpName `
    --http-settings-cookie-based-affinity Disabled `
    --frontend-port 443 `
    --http-settings-protocol Https `
    --http-settings-port 443 `
    --capacity 2 `
    --sku WAF_Medium `
    --cert-file $certPath `
    --cert-password $certPassword `
    --verbose

# Enable the firewall with OWASP Ruleset 3.0 on the application gateway
Write-Host "Creating the Application Gateway WAF Configuration"
az network application-gateway waf-config set `
    --enabled true `
    --gateway-name $gwName `
    --resource-group $ResourceGroup `
    --firewall-mode Detection `
    --rule-set-version 3.0 `
    --verbose

# Retrieve the name of the HTTP settings that will be updated below
$gwHTTPSettings = $(az network application-gateway http-settings list --resource-group $ResourceGroup `
    --gateway-name $gwName)

$gwHTTPSettings = $gwHTTPSettings | ConvertFrom-Json
$gwHTTPSettingsName = $gwHTTPSettings.name

# Retrieve the name of the backend address pool that will be updated below
$gwAddressPool = $(az network application-gateway address-pool list --resource-group $ResourceGroup `
    --gateway-name $gwName)

$gwAddressPool = $gwAddressPool | ConvertFrom-Json
$gwAddressPoolName = $gwAddressPool.name

# Update the backend pool with the App Service host name
Write-Host "Updating the Azure Application Gateway backend pool host name: $($appHostName)"
az network application-gateway address-pool update --name $gwAddressPoolName `
    --resource-group $ResourceGroup `
    --gateway-name $gwName `
    --servers $appHostName `
    --verbose

# Create a probe that will check the backend pool's availability
Write-Host "Updating the Azure Application Gateway Probe: $($gwProbe)"
az network application-gateway probe create --gateway-name $gwName `
    --name $gwProbe `
    --resource-group $ResourceGroup `
    --protocol Https `
    --path $gwProbePath `
    --host-name-from-http-settings true `
    --verbose

# Update the app to user https and to pick the host name from the backend settings
Write-Host "Deploying the updated application gateway"
az network application-gateway http-settings update --gateway-name $gwName `
    --resource-group $ResourceGroup `
    --name $gwHTTPSettingsName `
    --connection-draining-timeout 0 `
    --enable-probe true `
    --host-name-from-backend-pool true `
    --probe $gwProbe `
    --protocol Https `
    --port 443 `
    --verbose
```

Una vez completada la implementación, tendrá una instancia de Application Gateway con el firewall de aplicaciones web habilitado.

La instancia de la puerta de enlace expone el puerto 443 para HTTPS. Esta configuración garantiza que la aplicación solo esté accesible en el puerto 443 a través de HTTPS.

El bloqueo de los puertos que no se usan y la limitación de la exposición de la superficie de ataque es una práctica de seguridad recomendada.

#### <a name="add-network-security-groups-to-the-app-service-instance"></a>Agregar grupos de seguridad de red a la instancia de App Service

Las instancias de App Service se pueden integrar con redes virtuales. Esta integración permite que se configuren con directivas de grupo de seguridad de red que administran el tráfico entrante y saliente de la aplicación.

1. Para habilitar esta característica, en la hoja de la instancia de Azure App Service, en **Configuración**, seleccione **Redes**. En el panel derecho, en **Integración de VNET**, seleccione **Haga clic aquí para configurar**.

   ![Integración de una nueva red virtual](./media/secure-web-app/app-vnet-menu.png)

    *Integración de una nueva red virtual para App Service*

1. En la página siguiente, seleccione **Agregar VNET (versión preliminar)** .

1. En el menú siguiente, seleccione la red virtual creada en la implementación que empieza por `hello-vnet`. Puede crear un subconjunto nuevo o seleccionar uno existente.
   En este caso, cree una nueva subred. Defina el **Intervalo de direcciones** como **10.0.3.0/24** y asigne a la subred el nombre **app-subnet**.

   ![Configuración de la red virtual de App Service](./media/secure-web-app/app-vnet-config.png)

    *Configuración de la red virtual para App Service*

Ahora que ha habilitado la integración de la red virtual, puede agregar grupos de seguridad de red a nuestra aplicación.

1. En el cuadro de búsqueda, busque **grupos de seguridad de red**. Seleccione **Grupos de seguridad de red** en los resultados.

    ![Buscar grupos de seguridad de red](./media/secure-web-app/nsg-search-menu.png)

    *Buscar grupos de seguridad de red*

2. En el menú siguiente, seleccione **Agregar**. Escriba el **Nombre** del NSG y el **Grupo de recursos** en el que se debe ubicar. Este NSG se aplicará a la subred de la puerta de enlace de la aplicación.

    ![Crear un NSG](./media/secure-web-app/nsg-create-new.png)

    *Crear un NSG*

3. Una vez creado el NSG, selecciónelo. En su hoja, en **Configuración**, seleccione **Reglas de seguridad de entrada**. Configure estas opciones para permitir conexiones entrantes en la puerta de enlace de la aplicación a través del puerto 443.

   ![Configurar el NSG](./media/secure-web-app/nsg-gateway-config.png)

   *Configurar el NSG*

4. En las reglas de salida para el NSG de la puerta de enlace, agregue una regla que permita las conexiones salientes con la instancia de App Service creando una regla orientada a la etiqueta de servicio `AppService`:

   ![Agregar reglas de salida para el NSG](./media/secure-web-app/nsg-outbound-allowappserviceout.png)

   *Agregar reglas de salida para el NSG*

    Agregue otra regla de salida para permitir que la puerta de enlace envíe reglas de salida a una red virtual.

   ![Agregar otra regla de salida](./media/secure-web-app/nsg-outbound-vnet.png)

    *Agregar otra regla de salida*

5. En la hoja de subredes del NSG, seleccione **Asociar**, seleccione la red virtual creada en la implementación y seleccione la subred de puerta de enlace denominada **gw-subnet**. El NSG se aplica a la subred.

6. Cree otro NSG como en el paso anterior; esta vez, para la instancia de App Service. Asígnele un nombre. Agregue la regla de entrada para el puerto 443 como lo hizo para el NSG de la puerta de enlace de la aplicación.

   Si tiene una instancia de App Service implementada en una instancia de App Service Environment, que no es el caso de esta aplicación, puede agregar reglas de entrada para permitir sondeos de Azure Service Health abriendo los puertos 454-455 en los grupos de seguridad de entrada de su NSG de App Service. Esta es la configuración:

   ![Agregar reglas para sondeos de Azure Service Health](./media/secure-web-app/nsg-create-healthprobes.png)

    *Agregar reglas para sondeos de Azure Service Health (solo App Service Environment)*

7. En las reglas de seguridad de salida, cree una nueva regla de seguridad de salida que permita a la instancia de App Service comunicarse con la base de datos de PostgreSQL. Configúrela como se indica a continuación:

   ![Regla para permitir conexiones de salida de PostgreSQL](./media/secure-web-app/nsg-outbound-postgresql.png)

   *Agregar una regla para permitir conexiones de salida de PostgreSQL*

Para limitar la superficie expuesta a ataques, modifique la configuración de red de App Service para que solo la puerta de enlace de la aplicación tenga acceso a la aplicación.
Para ello, vaya a la pestaña de red de App Service, seleccione la pestaña **Restricciones de IP** y cree una regla de permiso que solo permita a la dirección IP de Application Gateway acceder directamente al servicio.

Puede recuperar la dirección IP de la puerta de enlace desde esta página de información general. En la pestaña **Dirección IP o CIDR**, escriba la dirección IP en este formato: `<GATEWAY_IP_ADDRESS>/32`.

![Permitir solo la puerta de enlace](./media/secure-web-app/app-allow-gw-only.png)

*Permitir que solo la dirección IP de la puerta de enlace tenga acceso a App Service*

#### <a name="implement-azure-active-directory-oauth"></a>Implementación de OAuth en Azure Active Directory

Los documentos de Azure distribuidos en la página de la aplicación web de ejemplo son recursos de la aplicación que podrían necesitar protección. Puede usar Azure Active Directory (Azure AD) para implementar la autenticación de aplicaciones web, de escritorio y móviles mediante diferentes flujos de autenticación.
La aplicación usa el **inicio de sesión con Microsoft**, que permite a la aplicación leer los perfiles de los usuarios que se han agregado a la lista de usuarios de un solo inquilino de Azure AD.

En Azure Portal, configure la aplicación para que use las credenciales necesarias:

1. Seleccione **Azure Active Directory** o búsquelo desde el cuadro de búsqueda.

2. Seleccione **Nuevo registro**:

   ![Crear un registro](./media/secure-web-app/ad-auth-create.png)

   *Creación de un registro de aplicaciones de Azure AD*

3. En la página siguiente, especifique el nombre de la aplicación. En **Tipos de cuenta admitidos**, seleccione **Solo las cuentas de este directorio organizativo**.
    En **URI de redirección**, escriba el dominio base en el que se ejecutará la aplicación más uno con el punto de conexión del token. Por ejemplo: *GATEWAY_HASH*.cloudapp.net/token.

   ![Configuración del registro de aplicaciones de Azure AD](./media/secure-web-app/ad-auth-type.png)

   *Configuración del registro de aplicaciones de Azure AD*

4. Aparece una pantalla que muestra la aplicación registrada y su información. Debe agregar esta información a la instancia de Azure Key Vault.
   1. Copie el identificador de la aplicación (cliente) y guárdelo en Key Vault como `CLIENTID`.
   2. Copie el URI de redirección que especificó en el paso anterior y guárdelo como `REDIRECTURI`.
   3. Copie el nombre del directorio predeterminado de Azure AD, que tiene el formato *nombre*.microsoftonline.com y guárdelo en Key Vault como `TENANT`.
   4. Vaya a la pestaña **Certificados y secretos** de la aplicación de Azure AD que creó anteriormente y seleccione **Nuevo secreto de cliente**, como se muestra en la siguiente captura de pantalla. Defina una fecha de expiración y, a continuación, copie el valor generado y guárdelo en Key Vault como `CLIENTSECRET`.

      ![Secreto de autorización de Azure AD](./media/secure-web-app/ad-auth-secrets.png)

      *Secreto de autorización de Azure AD*

   5. Genere una clave secreta aleatoria segura con cualquier herramienta de línea de comandos o en línea. Guárdela en Key Vault como `FLASKSECRETKEY`. El marco de trabajo de la aplicación usa esta clave para crear sesiones.
        Para obtener información sobre cómo generar una clave secreta, consulte [Objetos session de Flask](http://flask.pocoo.org/docs/1.0/quickstart/#sessions).

5. Después de configurar el inicio de sesión, debe agregar usuarios al vínculo de Azure AD vínculo para permitirles iniciar sesión en el recurso. Para agregarlos, vaya a la pestaña **Usuarios** de Azure AD, seleccione **Todos los usuarios** y, a continuación, seleccione **Nuevo usuario** o **Nuevo usuario invitado**. Para las pruebas, puede agregar un usuario invitado e invitarlo al directorio. También puede agregar un nuevo usuario si el dominio en el que se ejecuta la aplicación se ha validado. En este ejemplo, solo se pueden registrar para el acceso los usuarios registrados en el inquilino de Azure AD. Para obtener información sobre el acceso de inicio de sesión en varios inquilinos, consulte la documentación.

   ![Agregar usuarios al dominio predeterminado](./media/secure-web-app/ad-auth-add-user.png)

   *Agregar usuarios al dominio predeterminado de Azure Active Directory*

Después de agregar la configuración de Azure AD y los secretos a Key Vault, los usuarios se pueden autenticar en la aplicación mediante la autenticación de OAuth de Azure.
En el código de la aplicación, se controla mediante la biblioteca de autenticación de Azure Active Directory (ADAL).

Una vez que los secretos están en Key Vault y la aplicación tiene acceso a estos y a la base de datos, se puede acceder al servicio de aplicación a través de la URL de la aplicación de la puerta de enlace (https:\//GATEWAY_HASH.cloudapp.net), que se puede obtener de su hoja.

Si, al iniciar sesión en Azure AD, recibe un error que indica que "El usuario no está registrado en el directorio en el que intenta iniciar sesión", debe agregar al usuario. Para agregar al usuario, vaya a la pestaña **Usuarios** de Azure AD y agregue el usuario manualmente. Para ello, escriba sus detalles o invite al usuario escribiendo su dirección de correo electrónico como usuario invitado en Azure AD en la hoja **Invitar a un invitado**.

#### <a name="deploy-application-insights"></a>Implementar Application Insights
Ahora que la aplicación está implementada y en funcionamiento, debe controlar los errores que se producen dentro de la aplicación junto con el registro y la recopilación de datos de seguimiento.
El registro y la recopilación de datos de seguimiento proporcionan una vista de los eventos de auditoría que se producen en la aplicación.

Application Insights es un servicio que recopila registros que pueden generar los usuarios o el sistema.

Para crear una instancia de Application Insights:

1. Para buscar **Application Insights**, use el cuadro de búsqueda de Azure Portal.
2. Seleccione **Application Insights**. Proporcione los detalles que se muestran aquí para crear una instancia.

   ![Crear una instancia de Application Insights](./media/secure-web-app/app-insights-data.png)

Una vez finalizada la implementación, tendrá una instancia de Application Insights.

Después de crear la instancia de Application Insights, debe hacer que la aplicación reconozca la clave de instrumentación que le permite enviar registros a la nube. Para ello, se recupera la clave de Application Insights y se usa en las bibliotecas de aplicación que proporciona Azure para Application Insights. El procedimiento recomendado es almacenar claves y secretos en Azure Key Vault para protegerlos.

Para la aplicación básica de ejemplo, después de crear la instancia de Application Insights, debe hacer que la aplicación reconozca la clave de instrumentación que le permite enviar registros a la nube.
En Key Vault, establezca un secreto `APPINSIGHTSKEY` y defina su valor como la clave de instrumentación. Esto permite que la aplicación envíe registros y métricas a Application Insights.

#### <a name="implement-multi-factor-authentication-for-azure-active-directory"></a>Implementar Multi-Factor Authentication para Azure Active Directory

Los administradores deben asegurarse de que las cuentas de suscripción del portal están protegidas. La suscripción es vulnerable a los ataques porque administra los recursos creados. Para proteger la suscripción, habilite Multi-Factor Authentication en la pestaña **Azure Active Directory** de la suscripción.

Azure AD funciona en función de las directivas que se aplican a un usuario o a grupos de usuarios que se ajustan a determinados criterios.
Azure crea una directiva predeterminada que especifica que los administradores necesitan la autenticación en dos fases para iniciar sesión en el portal.
Después de habilitar esta directiva, es posible que se le pida que cierre sesión y vuelva a iniciarla en Azure Portal.

Para habilitar MFA para los inicios de sesión de administrador:

1. Vaya a la pestaña **Azure Active Directory** en Azure Portal.
2. En la categoría de seguridad, seleccione el acceso condicional. Verá esta pantalla:

   ![Acceso condicional: directivas](./media/secure-web-app/ad-mfa-conditional-add.png)

Si no puede crear una directiva nueva:

1. Vaya a la pestaña **MFA**.
2. Seleccione el vínculo **Prueba gratuita** de Azure AD Premium para suscribirse a la evaluación gratuita.

   ![Evaluación gratuita de Azure AD Premium](./media/secure-web-app/ad-trial-premium.png)

Vuelva a la pestaña de acceso condicional.

1. Seleccione la pestaña de nueva directiva.
2. Escriba el nombre de la directiva.
3. Seleccione los usuarios o grupos para los que quiere habilitar MFA.
4. En **Controles de acceso**, seleccione la pestaña **Conceder** y, a continuación, seleccione **Requerir autenticación multifactor** (y cualquier otra opción que quiera).

   ![Requerir MFA](./media/secure-web-app/ad-mfa-conditional-add.png)

Para habilitar la directiva, seleccione la casilla de la parte superior de la pantalla. También puede hacerlo desde la pestaña **Acceso condicional**. Si la directiva está habilitada, los usuarios necesitan MFA para iniciar sesión en el portal.

Hay una directiva de línea de base que requiere MFA para todos los administradores de Azure. Puede habilitarla inmediatamente en el portal. Al habilitar esta directiva, puede que se invalide la sesión actual y que tenga que volver a iniciar sesión.

Si la directiva de línea de base no está habilitada:

1. Seleccione **Requerir MFA para los administradores**.
2. Seleccione **Usar la directiva inmediatamente**.

   ![Seleccione Usar la directiva inmediatamente.](./media/secure-web-app/ad-mfa-conditional-enable.png)

#### <a name="use-azure-sentinel-to-monitor-apps-and-resources"></a>Usar Azure Sentinel para supervisar aplicaciones y recursos

A medida que crece una aplicación, resulta difícil agregar todas las señales y métricas de seguridad recibidas de los recursos y hacerlas útiles de forma orientada a la acción.

Azure Sentinel está diseñado para recopilar datos, detectar los tipos de amenazas posibles y proporcionar visibilidad sobre los incidentes de seguridad.
Mientras espera la intervención manual, Azure Sentinel puede basarse en cuadernos de estrategias escritos previamente para iniciar las alertas y los procesos de administración de incidentes.

La aplicación de ejemplo se compone de varios recursos que Azure Sentinel puede supervisar.
Para configurar Azure Sentinel, primero debe crear un área de trabajo de Log Analytics que almacene todos los datos recopilados de los distintos recursos.

Para crear esta área de trabajo:

1. En el cuadro de búsqueda de Azure Portal, busque **Log Analytics**. Seleccione **Áreas de trabajo de Log Analytics**.

   ![Buscar áreas de trabajo de Log Analytics](./media/secure-web-app/sentinel-log-analytics.png)

    *Buscar áreas de trabajo de Log Analytics*

2. En la página siguiente, seleccione **Agregar** y, a continuación, proporcione un nombre, un grupo de recursos y una ubicación para el área de trabajo.
   ![Creación de un área de trabajo de Log Analytics](./media/secure-web-app/sentinel-log-analytics-create.png)

   *Creación de un área de trabajo de Log Analytics*

3. Use el cuadro de búsqueda para buscar **Azure Sentinel**.

   ![Buscar Azure Sentinel](./media/secure-web-app/sentinel-add.png)

    *Buscar Azure Sentinel*

4. Seleccione **Agregar** y, a continuación, seleccione el área de trabajo de Log Analytics que creó antes.

   ![Agregar un área de trabajo de Log Analytics](./media/secure-web-app/sentinel-workspace-add.png)

    *Agregar un área de trabajo de Log Analytics*

5. En la página **Azure Sentinel: conectores de datos**, en **Configuración**, seleccione **Conectores de datos**. Verá una matriz de servicios de Azure que puede vincular a la instancia de almacenamiento de Log Analytics para su análisis en Azure Sentinel.

   ![Conectores de datos de Log Analytics](./media/secure-web-app/sentinel-connectors.png)

    *Agregar un conector de datos a Azure Sentinel*

   Por ejemplo, para conectar Application Gateway, siga estos pasos:

   1. Abra la hoja de la instancia de Azure Application Gateway.
   2. En **Supervisión**, seleccione **Configuración de diagnóstico**.
   3. Seleccione **Agregar configuración de diagnóstico**.

      ![Agregar diagnósticos de Application Gateway](./media/secure-web-app/sentinel-gateway-connector.png)

      *Agregar diagnósticos de Application Gateway*

   4. En la página **Configuración de diagnóstico** , seleccione el área de trabajo de Log Analytics que creó y, a continuación, seleccione todas las métricas que quiere recopilar y enviar a Azure Sentinel. Seleccione **Guardar**.

        ![Configuración del conector de Azure Sentinel](./media/secure-web-app/sentinel-connector-settings.png)

        *Configuración del conector de Azure Sentinel*

  Las métricas del recurso están en Azure Sentinel, donde puede consultarlas e investigarlas.

   Agregue las mismas métricas en la configuración de diagnóstico para Azure Key Vault, la dirección IP pública, Azure Database for PostgreSQL y cualquier servicio que admita registros de diagnóstico en su cuenta.

Después de configurar las métricas, Azure Sentinel tiene datos que analizar.

## <a name="evaluate-and-verify"></a>Evaluación y comprobación

Después de desarrollar e implementar la arquitectura, debe asegurarse de que el código y los servicios implementados cumplan los estándares de seguridad. Estos son algunos de los pasos que puede seguir para comprobar el software:

- Análisis de código estático
- Examen de vulnerabilidades
- Búsqueda y corrección de vulnerabilidades en las dependencias de la aplicación

Estos son los bloques de creación básicos para los procedimientos recomendados de desarrollo seguro.

### <a name="static-code-analysis"></a>Análisis de código estático

En el caso de la aplicación de ejemplo, la comprobación con las herramientas de análisis estático implica buscar vulnerabilidades en el código de la aplicación mediante técnicas como la comprobación de los datos y el análisis del flujo de datos. Las herramientas de análisis estático de Python ofrecen mayor confianza en la seguridad de la aplicación.

**Búsqueda de errores**

PyFlakes, una biblioteca de búsqueda de errores de Python, le ayuda a quitar el código no alcanzado y las funciones no usadas de las aplicaciones, como se muestra aquí:

![PyFlakes](./media/secure-web-app/pyflakes.png)

La búsqueda de errores proporciona sugerencias y posibles cambios que pueden hacer que el código sea más limpio y menos propenso a errores en tiempo de ejecución.

**PyLint**

PyLint proporcionó el mayor valor a este proyecto. Realiza comprobaciones estándar de código, comprobación de errores y sugerencias de refactorización para asegurarse de que el código que se ejecuta en el servidor sea seguro. Al usar PyLint para actualizar el código, puede eliminar los errores y mejorar la clasificación de PyLint, como muestran las siguientes imágenes.

![Antes de PyLint](./media/secure-web-app/before-pylint.png)

*Antes de PyLint*

Después de corregir algunos de los errores de código encontrados por las herramientas de búsqueda de errores, tendrá más confianza en que el código no es propenso a errores. Corregir los errores reduce de forma significativa los riesgos de seguridad que pueden producirse cuando el código se implementa en entornos de producción.

![Después de Pylint](./media/secure-web-app/after-pylint.png)

*Después de Pylint*

### <a name="vulnerability-scanning"></a>Examen de vulnerabilidades

[La herramienta ZAP de OWASP](https://www.zaproxy.org/) es un escáner de vulnerabilidades de aplicaciones web de código abierto que puede usar para comprobar si hay vulnerabilidades en la aplicación de ejemplo. La ejecución de la herramienta en la aplicación de ejemplo revela algunos posibles errores y vectores de ataque.

![Herramienta ZAP](./media/secure-web-app/zap-tool.png)

*Herramienta ZAP*

### <a name="find-and-fix-vulnerabilities-in-app-dependencies"></a>Búsqueda y corrección de vulnerabilidades en las dependencias de la aplicación

Para buscar y corregir las dependencias de la aplicación, puede usar la [comprobación de dependencias de OWASP](https://owasp.org/www-project-dependency-check/).

La seguridad es una aplicación similar que comprueba las dependencias. Puede encontrarla en [GitHub](https://github.com/pyupio/safety). Los exámenes de seguridad para detectar vulnerabilidades en bases de datos de vulnerabilidades conocidas.

![Seguridad](./media/secure-web-app/pysafety.png)

*Seguridad*

## <a name="next-steps"></a>Pasos siguientes

Los siguientes artículos pueden ayudarle a diseñar, desarrollar e implementar aplicaciones seguras.

- [Diseño](secure-design.md)
- [Desarrollo](secure-develop.md)
- [Implementación](secure-deploy.md)
