---
title: Integración de la aplicación de MongoDB existente con la API de Azure Cosmos DB para MongoDB y Open Service Broker para Azure (OSBA)
description: En este artículo, aprenderá a integrar una aplicación de Java y MongoDB existente con la API de Azure Cosmos DB para MongoDB mediante Open Service Broker para Azure (OSBA).
author: zr-msft
ms.service: azure-dev-spaces
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: zarhoads
ms.custom: mvc
keywords: Cosmos DB, Open Service Broker, Open Service Broker para Azure
ms.openlocfilehash: ddaa3b9aa198bc142e1bcbcab6b7b1e028eff2aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78247920"
---
# <a name="integrate-existing-mongodb-application-with-azure-cosmos-db-api-for-mongodb-and-open-service-broker-for-azure-osba"></a>Integración de la aplicación de MongoDB existente con la API de Azure Cosmos DB para MongoDB y Open Service Broker para Azure (OSBA)

Azure Cosmos DB es un servicio de base de datos de varios modelos distribuido globalmente. También proporciona compatibilidad del protocolo de transferencia con varias API NoSQL como MongoDB. La API de Cosmos DB para MongoDB permite usar Cosmos DB con la aplicación de MongoDB existente sin tener que cambiar los controladores o la implementación de la base de datos de la aplicación. También se puede aprovisionar un servicio de Cosmos DB mediante Open Service Broker para Azure.

En este artículo, se tomará una aplicación Java existente que usa una base de datos de MongoDB y se actualizará para usar una base de datos de Cosmos DB mediante Open Service Broker para Azure.

## <a name="prerequisites"></a>Prerequisites

Antes de continuar, debe:
    
* Haber creado un [clúster de Azure Kubernetes Service](kubernetes-walkthrough.md).
* Tener [instalado y configurado Open Service Broker para Azure en el clúster de AKS](integrate-azure.md). 
* Tener instalada y configurada la [CLI del catálogo de servicios](https://svc-cat.io/docs/install/) para ejecutar comandos de `svcat`.
* Tener una base de datos de [MongoDB](https://www.mongodb.com/) ya existente. Por ejemplo, podría tener MongoDB en ejecución en su [máquina de desarrollo](https://docs.mongodb.com/manual/administration/install-community/) o en una [máquina virtual de Azure](../virtual-machines/linux/install-mongodb.md).
* Tener una manera de conectarse a la base de datos de MongoDB y consultarla, como el [shell de mongo](https://docs.mongodb.com/manual/mongo/).

## <a name="get-application-code"></a>Obtención del código de la aplicación
    
En este artículo, se usará la [aplicación de ejemplo Spring Music de Cloud Foundry](https://github.com/cloudfoundry-samples/spring-music) para mostrar una aplicación que usa una base de datos de MongoDB.
    
Clone la aplicación desde GitHub y vaya a su directorio:
    
```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring-music
```

## <a name="prepare-the-application-to-use-your-mongodb-database"></a>Preparación de la aplicación para usar la base de datos de MongoDB

La aplicación de ejemplo Spring Music proporciona muchas opciones de orígenes de datos. En este artículo, la configurará para usar una base de datos de MongoDB existente. 

Agregue el siguiente YAML al final de *src/main/resources/application.yml*. Con ello se crea un perfil denominado *mongodb* y se configura un URI y un nombre de base de datos. Reemplace el URI por la información de conexión a la base de datos de MongoDB existente. La incorporación del URI, que contiene un nombre de usuario y una contraseña, directamente a este archivo es **solo con fines de desarrollo**; **nunca se debe agregar al control de versiones**.

```yaml
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://user:password@serverAddress:port/musicdb"
      database: musicdb
```



Al iniciar la aplicación e indicarle que use el perfil *mongodb*, se conecta a la base de datos de MongoDB y la usa para almacenar los datos de la aplicación.

Compilación de la aplicación

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```

Inicie la aplicación e indíquele que use el perfil *mongodb*:

```cmd
java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Vaya a `http://localhost:8080` en el explorador.

![Aplicación Spring Music con datos predeterminados](media/music-app.png)

Observe que la aplicación se ha rellenado con algunos [datos predeterminados](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json). Interactúe con ellos: elimine unos cuantos álbumes existentes y cree algunos otros.

Para comprobar que la aplicación usa la base de datos de MongoDB, conéctese a ella y consulte la base de datos *musicdb*:

```cmd
mongo serverAddress:port/musicdb -u user -p password
use musicdb
db.album.find()

{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446d"), "title" : "Nevermind", "artist" : "Nirvana", "releaseYear" : "1991", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446e"), "title" : "Pet Sounds", "artist" : "The Beach Boys", "releaseYear" : "1966", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446f"), "title" : "What's Going On", "artist" : "Marvin Gaye", "releaseYear" : "1971", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
...
```

En el ejemplo anterior se usa el [shell de mongo](https://docs.mongodb.com/manual/mongo/) para conectarse a la base de datos de MongoDB y consultarla. También puede comprobar que los cambios se conservan si detiene la aplicación, la reinicia y vuelve a ella con el explorador. Observe que los cambios que ha realizado siguen estando ahí.


## <a name="create-a-cosmos-db-database"></a>Creación de una base de datos de Cosmos DB

Para crear una base de datos de Cosmos DB en Azure mediante Open Service Broker, use el comando `svcat provision`:

```cmd
svcat provision musicdb --class azure-cosmosdb-mongo-account --plan account  --params-json '{
  "location": "eastus",
  "resourceGroup": "MyResourceGroup",
  "ipFilters" : {
    "allowedIPRanges" : ["0.0.0.0/0"]
  }
}'
```

El siguiente comando aprovisiona una base de datos de Cosmos DB en Azure en el grupo de recursos *MyResourceGroup* de la región *eastus*. Más información sobre *resourceGroup*, *location* y otros parámetros JSON específicos de Azure en la [documentación de referencia del módulo de Cosmos DB](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/cosmosdb.md#provision-3).

Para comprobar que la base de datos ha completado correctamente el aprovisionamiento, use el comando: `svcat get instance`

```cmd
$ svcat get instance musicdb

   NAME     NAMESPACE              CLASS                PLAN     STATUS
+---------+-----------+------------------------------+---------+--------+
  musicdb   default     azure-cosmosdb-mongo-account   account   Ready
```

La base de datos está lista cuando vea *Ready* (Listo) en *STATUS* (ESTADO).

Una vez que la base de datos ha finalizado el aprovisionamiento, tiene que enlazar sus metadatos a un [secreto de Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/). Luego, otras aplicaciones pueden acceder a esos datos. Para enlazar los metadatos de la base de datos a un secreto, use el comando `svcat bind`:

```cmd
$ svcat bind musicdb

  Name:        musicdb
  Namespace:   default
  Status:
  Secret:      musicdb
  Instance:    musicdb

Parameters:
  No parameters defined
```


## <a name="use-the-cosmos-db-database-with-your-application"></a>Uso de la base de datos de Cosmos DB con la aplicación

Para usar la base de datos de Cosmos DB con la aplicación, debe conocer el URI para conectarse a ella. Para obtener esta información, use el comando `kubectl get secret`:

```cmd
$ kubectl get secret musicdb -o=jsonpath='{.data.uri}' | base64 --decode

mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
```

El comando anterior obtiene el secreto *musicdb* y muestra solo el URI. Los secretos se almacenan en formato base64, por lo que el comando anterior también los descodifica.

Con el URI de la base de datos de Cosmos DB, actualice *src/main/resources/application.yml* para usarlo:

```yaml
...
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
      database: musicdb
```

La actualización del URI, que contiene un nombre de usuario y una contraseña, directamente con este archivo es **solo con fines de desarrollo**; **nunca se debe agregar al control de versiones**.

Recompile e inicie la aplicación para empezar a usar la base de datos de Cosmos DB:

```cmd
./gradlew clean assemble

java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Observe que la aplicación aún utiliza el perfil *mongodb* y un URI que comienza con *mongodb: / /* para conectarse a la base de datos de Cosmos DB. La [API de Azure Cosmos DB para MongoDB](../cosmos-db/mongodb-introduction.md) ofrece esta compatibilidad. Esta API permite que la aplicación siga funcionando como si usara una base de datos de MongoDB, pero en realidad utiliza Cosmos DB.

Vaya a `http://localhost:8080` en el explorador. Observe que se han restaurado los datos predeterminados. Interactúe con ellos: elimine unos cuantos álbumes existentes y cree algunos otros. Puede comprobar que los cambios se conservan si detiene la aplicación, la reinicia y vuelve a ella con el explorador. Observe que los cambios que ha realizado siguen estando ahí. Los cambios se conservan en la base de datos de Cosmos DB que creó mediante Open Service Broker para Azure.


## <a name="run-your-application-on-your-aks-cluster"></a>Ejecución de la aplicación en el clúster de AKS

Puede usar [Azure Dev Spaces](../dev-spaces/azure-dev-spaces.md) para implementar la aplicación en el clúster de AKS. Azure Dev Spaces ayuda a generar artefactos, como gráficos de Dockerfiles y Helm, y a implementar y ejecutar una aplicación en AKS.

Para habilitar Azure Dev Spaces en el clúster de AKS:

```azurecli
az aks enable-addons --addons http_application_routing -g MyResourceGroup -n MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

Use las herramientas de Azure Dev Spaces para preparar la aplicación para ejecutarse en AKS:

```cmd
azds prep --public
```

Este comando genera varios artefactos, incluida una carpeta *charts/* , que es el gráfico de Helm, en la raíz del proyecto. Este comando no puede generar un *Dockerfile* para este proyecto concreto, por lo que debe crearlo.

Cree un archivo en la raíz del proyecto denominado *Dockerfile* con este contenido:

```dockerfile
FROM openjdk:8-jdk-alpine
EXPOSE 8080
WORKDIR /app
COPY build/libs/spring-music-1.0.jar .
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
```

Además, debe actualizar la propiedad *configurations.develop.build* de *azds.yaml* a *false*:
```yaml
...
configurations:
  develop:
    build:
      useGitIgnore: false
```

También debe actualizar el atributo *containerPort* a *8080* en *charts/spring-music/templates/deployment.yaml*:

```yaml
...
spec:
  ...
  template:
    ...
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
```

Implementación de la aplicación en AKS

```cmd
$ azds up

Using dev space 'default' with target 'MyAKS'
Synchronizing files...1m 18s
Installing Helm chart...5s
Waiting for container image build...23s
Building container image...
Step 1/5 : FROM openjdk:8-jdk-alpine
Step 2/5 : EXPOSE 8080
Step 3/5 : WORKDIR /app
Step 4/5 : COPY build/libs/spring-music-1.0.jar .
Step 5/5 : ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
Built container image in 21s
Waiting for container...8s
Service 'spring-music' port 'http' is available at http://spring-music.1234567890abcdef1234.eastus.aksapp.io/
Service 'spring-music' port 8080 (TCP) is available at http://localhost:57892
press Ctrl+C to detach
...
```

Vaya a la dirección URL mostrada en los registros. En el ejemplo anterior, se usaría *http://spring-music.1234567890abcdef1234.eastus.aksapp.io/* . 

Compruebe que puede ver la aplicación junto con los cambios.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se describe cómo actualizar una aplicación existente del uso de MongoDB al uso de la API de Cosmos DB para MongoDB. También se explica cómo aprovisionar un servicio de Cosmos DB mediante Open Service Broker para Azure y la implementación de esa aplicación en AKS con Azure Dev Spaces.

Para más información sobre Cosmos DB, Open Service Broker para Azure y Azure Dev Spaces, consulte:
* [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
* [Abrir Service Broker para Azure](https://osba.sh)
* [Desarrollo con Dev Spaces](../dev-spaces/azure-dev-spaces.md)
