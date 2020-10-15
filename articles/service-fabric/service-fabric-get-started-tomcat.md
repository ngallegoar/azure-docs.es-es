---
title: Creación de un contenedor para Apache Tomcat en Linux
description: Cree un contenedor de Linux para exponer una aplicación que se ejecuta en un servidor de Apache Tomcat en Azure Service Fabric. Compile una imagen de Docker con la aplicación y el servidor de Apache Tomcat, inserte la imagen en un registro de contenedor y compile e implemente una aplicación contenedora de Service Fabric.
ms.topic: conceptual
ms.date: 6/08/2018
ms.author: pepogors
ms.openlocfilehash: 1a699f3b35970270a9800162a6d8717682a168ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75614424"
---
# <a name="create-service-fabric-container-running-apache-tomcat-server-on-linux"></a>Creación de un contenedor de Service Fabric que se ejecuta en un servidor de Apache Tomcat en Linux
Apache Tomcat es una conocida implementación de código abierto de las tecnologías Java Servlet y Java Server. En este artículo se muestra cómo crear un contenedor con Apache Tomcat y una sencilla aplicación web, cómo implementar el contenedor en un clúster de Service Fabric con Linux y cómo conectarse a la aplicación web.  

Para más información sobre Apache Tomcat, consulte la [página principal de Apache Tomcat](https://tomcat.apache.org/). 

## <a name="prerequisites"></a>Prerrequisitos
* Un equipo de desarrollo en el que se ejecute:
  * [SDK y herramientas de Service Fabric](service-fabric-get-started-linux.md).
  * [Docker CE para Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [CLI de Service Fabric](service-fabric-cli.md)

* Un registro de contenedor en Azure Container Registry. Puede crear un registro de contenedor en su suscripción de Azure mediante [Azure Portal](../container-registry/container-registry-get-started-portal.md) o la [CLI de Azure](./service-fabric-tutorial-create-container-images.md#deploy-azure-container-registry). 

## <a name="build-a-tomcat-image-and-run-it-locally"></a>Creación de una imagen de Tomcat y su ejecución de forma local
Siga los pasos descritos en esta sección para crear una imagen de Docker basada en una imagen de Apache Tomcat y una aplicación web sencilla y, luego, ejecútela en un contenedor en el sistema local. 
 
1. Clone el repositorio de ejemplos de [introducción a Service Fabric con Java](https://github.com/Azure-Samples/service-fabric-java-getting-started) en su equipo de desarrollo.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   ```

1. Cambie los directorios al directorio de ejemplos del servidor de Apache Tomcat (*service-fabric-java-getting-started/container-apache-tomcat-web-server-sample*):

   ```bash
   cd service-fabric-java-getting-started/container-apache-tomcat-web-server-sample
   ```

1. Cree un archivo de Docker basado en la [imagen de Tomcat](https://hub.docker.com/_/tomcat/) oficial ubicada en Docker Hub y en el ejemplo de servidor de Tomcat. En el directorio *service-fabric-java-getting-started/container-apache-tomcat-web-server-sample*, cree un archivo llamado *Dockerfile* (sin extensión de archivo). Agregue lo siguiente al archivo *Dockerfile* y guarde los cambios:

   ```
   FROM library/tomcat

   EXPOSE 8080

   COPY ./ApacheTomcat /usr/local/tomcat
   ```

   Para más información, consulte la [referencia de Dockerfile](https://docs.docker.com/engine/reference/builder/).


4. Ejecute el comando `docker build` para crear la imagen que ejecuta la aplicación web:

   ```bash
   docker build . -t tomcattest
   ```

   Este comando crea la imagen nueva con las instrucciones del Dockerfile y asigna el nombre (etiqueta -t) `tomcattest` a la imagen. Para crear una imagen de contenedor, primero se descarga la imagen base de Docker Hub y se agrega a él la aplicación. 

   Una vez que se complete el comando de compilación, ejecute el comando `docker images` para ver información sobre la nueva imagen:

   ```bash
   $ docker images
    
   REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
   tomcattest                    latest              86838648aab6        2 minutes ago       194 MB
   ```

5. Compruebe que la aplicación en contenedor se ejecuta localmente antes de insertarla en el registro de contenedor:
 
   ```bash
   docker run -itd --name tomcat-site -p 8080:8080 tomcattest.
   ```
   
   * `--name` asigna un nombre al contenedor, de modo que puede hacer referencia a él con un nombre descriptivo en lugar de su identificador.
   * `-p` especifica la asignación de puertos entre el contenedor y el sistema operativo host. 

   > [!Note]
   > El puerto que se abre con el parámetro `-p` debe ser el puerto en el que la aplicación Tomcat escucha las solicitudes. En el ejemplo actual, hay un conector configurado en el archivo *ApacheTomcat/conf/server.xml* para que escuche en el puerto 8080 las solicitudes HTTP. Este puerto se asigna al puerto 8080 en el host. 

   Para información sobre otros parámetros, consulte la [documentación de ejecución de Docker](https://docs.docker.com/engine/reference/commandline/run/).

1. Para probar el contenedor, abra un explorador y escriba una de las siguientes direcciones URL. Verá una variante de la pantalla de bienvenida "Hello World!" para cada dirección URL.

   - `http://localhost:8080/hello` 
   - `http://localhost:8080/hello/sayhello` 
   - `http://localhost:8080/hello/sayhi` 

   ![Hello world /sayhi](./media/service-fabric-get-started-tomcat/hello.png)

2. Detenga el contenedor y elimínelo del equipo de desarrollo:

   ```bash
   docker stop tomcat-site
   docker rm tomcat-site
   ```

## <a name="push-the-tomcat-image-to-your-container-registry"></a>Inserción de la imagen de Tomcat en el registro de contenedor
Ahora que ha comprobado que la imagen de Tomcat se ejecuta en un contenedor en el equipo de desarrollo, insértela en un repositorio de un registro de contenedor. En este artículo se usa Azure Container Registry para almacenar la imagen, pero puede usar cualquier registro de contenedor que prefiera modificando algunos pasos. En este artículo se supone que el nombre del registro es *myregistry* y el nombre de registro completo es myregistry.azurecr.io. Cambie estas propiedades según sea adecuado para su escenario. 

1. Ejecute `docker login` para iniciar sesión en el registro de contenedor con sus [credenciales de registro](../container-registry/container-registry-authentication.md).

   En el ejemplo siguiente se pasa el identificador y la contraseña de una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) de Azure Active Directory. Por ejemplo, puede que haya asignado una entidad de servicio al registro para ver un escenario de automatización. O bien, puede iniciar sesión con su nombre de usuario y contraseña del registro.

   ```bash
   docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
   ```

2. El siguiente comando crea una etiqueta, o alias, de la imagen, con una ruta de acceso completa al registro. Este ejemplo coloca la imagen en el espacio de nombres `samples` para evitar el desorden en la raíz del registro.

   ```bash
   docker tag tomcattest myregistry.azurecr.io/samples/tomcattest
   ```

3. Inserte la imagen en el registro de contenedor:

   ```bash
   docker push myregistry.azurecr.io/samples/tomcattest
   ```

## <a name="build-and-deploy-the-service-fabric-container-application"></a>Compilación e implementación de la aplicación contenedora de Service Fabric
Ahora que ha insertado la imagen de Tomcat en un registro de contenedor, puede compilar e implementar una aplicación contenedora de Service Fabric que extraiga la imagen de Tomcat del registro y la ejecute como un servicio en contenedor en el clúster. 

1. Cree un nuevo directorio fuera del clon local (fuera del árbol de directorios *service-fabric-java-getting-started*). Cambie a este directorio y use Yeoman para crear el scaffolding para una aplicación contenedora: 

   ```bash
   yo azuresfcontainer 
   ```
   Escriba los siguientes valores cuando se le solicite:

   * Asigne un nombre a la aplicación: ServiceFabricTomcat
   * Nombre del servicio de aplicación: TomcatService
   * Escriba el nombre de la imagen: proporcione la dirección URL de la imagen de contenedor en un registro de contenedor; por ejemplo, myregistry.azurecr.io/samples/tomcattest.
   * Comandos: Déjelo en blanco. Como dicha imagen tiene un punto de entrada de la carga de trabajo definido, no es preciso especificar explícitamente comandos de entrada (los comandos se ejecutarán dentro del contenedor, por lo que este seguirá en ejecución después del inicio).
   * Número de instancias de la aplicación contenedora invitada: 1

   ![Generador Yeoman de Service Fabric para contenedores](./media/service-fabric-get-started-tomcat/yo-generator.png)

10. En el manifiesto de servicio (*ServiceFabricTomcat/ServiceFabricTomcat/TomcatServicePkg/ServiceManifest.xml*), agregue el siguiente código XML bajo la etiqueta raíz **ServiceManfest** para abrir el puerto en el que la aplicación escucha las solicitudes. La etiqueta **Endpoint** declara el protocolo y el puerto del punto de conexión. En este artículo, el servicio en contenedor realiza escuchas en el puerto 8080: 

   ```xml
   <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
       listen. Please note that if your service is partitioned, this port is shared with 
       replicas of different partitions that are placed in your code. -->
      <Endpoint Name="endpointTest" Port="8080" Protocol="tcp"/>
    </Endpoints>
   </Resources>
   ```

11. En el manifiesto de aplicación (*ServiceFabricTomcat/ServiceFabricTomcat/ApplicationManifest.xml*), bajo la etiqueta **ServiceManifestImport**, agregue el siguiente código XML. Reemplace los valores de **AccountName** y **Password** de la etiqueta **RepositoryCredentials** por el nombre del registro de contenedor y la contraseña necesaria para iniciar sesión en él.

   ```xml
   <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="8080" EndpointRef="endpointTest"/>
      <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
    </ContainerHostPolicies>
   </Policies>
   ```

   La etiqueta **ContainerHostPolicies** especifica las directivas para activar los hosts de contenedor.
    
   * La etiqueta **PortBinding** configura la directiva de asignación de puertos de puerto a host del contenedor. El atributo **ContainerPort** se establece en 8080 porque el contenedor expone el puerto 8080, tal como se especifica en el Dockerfile. El atributo **EndpointRef** se establece en "endpointTest", el punto de conexión definido en el manifiesto de servicio en el paso anterior. Por tanto, las solicitudes entrantes al servicio en el puerto 8080 se asignan al puerto 8080 en el contenedor. 
   * La etiqueta **RepositoryCredentials** especifica las credenciales con las que debe autenticarse el contenedor en el repositorio (privado) del que se extrae la imagen. Esta directiva no es necesaria si la imagen se va a extraer de un repositorio público.
    

12. En la carpeta *ServiceFabricTomcat*, conéctese al clúster de Service Fabric. 

   * Para conectarse al clúster de Service Fabric local, ejecute:

     ```bash
     sfctl cluster select --endpoint http://localhost:19080
     ```
    
   * Para conectarse a un clúster de Azure seguro, asegúrese de que el certificado de cliente exista como un archivo .pem en el directorio *ServiceFabricTomcat* y ejecute: 

     ```bash
     sfctl cluster select --endpoint https://PublicIPorFQDN:19080 -pem your-certificate.pem -no-verify
     ```
     En el comando anterior, reemplace `your-certificate.pem` por el nombre del archivo de certificado de cliente. En entornos de desarrollo y pruebas, el certificado de clúster se usa a menudo como certificado de cliente. Si el certificado no es autofirmado, omita el parámetro `-no-verify`. 
       
     Normalmente, los certificados de clúster se descargan localmente como archivos .pfx. Si aún no tiene el certificado en formato PEM, puede ejecutar el siguiente comando para crear un archivo .pem a partir de un archivo .pfx:

     ```bash
     openssl pkcs12 -in your-certificate.pfx -out your-certificate.pem -nodes -passin pass:your-pfx-password
     ```

     Si el archivo .pfx no está protegido con contraseña, use `-passin pass:` para el último parámetro.


13. Ejecute el script de instalación proporcionado en la plantilla para implementar la aplicación en el clúster. El script copia el paquete de aplicación en el almacén de imágenes del clúster, registra el tipo de aplicación y crea una instancia de la aplicación.

     ```bash
     ./install.sh
     ```

   Después de haber ejecutado el script de instalación, abra un explorador y vaya a Service Fabric Explorer:
    
   * En un clúster local, use `http://localhost:19080/Explorer` (reemplace *localhost* por la IP privada de la máquina virtual si se usa Vagrant en Mac OS X).
   * En un clúster seguro de Azure, use `https://PublicIPorFQDN:19080/Explorer`. 
    
   Expanda el nodo **Applications** y observe que ahora hay una entrada para su tipo de aplicación, **ServiceFabricTomcatType** y otra para la primera instancia de ese tipo. La aplicación puede tardar unos minutos en implementarse completamente, así que tenga paciencia.

   ![Service Fabric Explorer](./media/service-fabric-get-started-tomcat/service-fabric-explorer.png)


1. Para acceder a la aplicación en el servidor de Tomcat, abra una ventana del explorador y escriba cualquiera de las siguientes direcciones URL. Si ha realizado la implementación en el clúster local, use *localhost* para *PublicIPorFQDN*. Verá una variante de la pantalla de bienvenida "Hello World!" para cada dirección URL.

   * http://PublicIPorFQDN:8080/hello  
   * http://PublicIPorFQDN:8080/hello/sayhello
   * http://PublicIPorFQDN:8080/hello/sayhi

## <a name="clean-up"></a>Limpieza
Para eliminar la instancia de aplicación del clúster y anular el registro del tipo de aplicación, use el script de desinstalación proporcionado en la plantilla.

```bash
./uninstall.sh
```

Después de insertar la imagen en el registro de contenedor puede eliminar la imagen local del equipo de desarrollo:

```
docker rmi tomcattest
docker rmi myregistry.azurecr.io/samples/tomcattest
```

## <a name="next-steps"></a>Pasos siguientes
* Puede encontrar pasos rápidos sobre características adicionales de contenedores de Linux en [Cree la primera aplicación contenedora de Service Fabric en Linux](service-fabric-get-started-containers-linux.md).
* Si desea pasos más detallados sobre contenedores de Linux, lea el tutorial [Creación de una aplicación contenedora de Linux](service-fabric-tutorial-create-container-images.md).
* Más información acerca de cómo ejecutar [contenedores en Service Fabric](service-fabric-containers-overview.md).


