---
title: Apache Kafka con el registro de esquema de Confluent en Azure HDInsight
description: Implemente un clúster de Kafka administrado por HDInsight con un nodo perimetral dentro de una red virtual y, a continuación, instale el registro de esquema de Confluent en el nodo perimetral.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 7e17cdca508db81551d988c795bd1235fa729e82
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2020
ms.locfileid: "94636867"
---
# <a name="apache-kafka-with-confluent-schema-registry-in-azure-hdinsight"></a>Apache Kafka con el registro de esquema de Confluent en Azure HDInsight

El registro de esquema de Kafka proporciona serializadores que se conectan a clientes de Apache Kafka que administran el almacenamiento y recuperación del esquema de mensajes para los mensajes de Kafka que se envían en formato Avro. Era un proyecto de software de código abierto de Confluent, pero ahora está sujeto a una [licencia de comunidad de Confluent](https://www.confluent.io/blog/license-changes-confluent-platform/). Además, el registro de esquema cumple las funciones siguientes:

* Almacenar y recuperar esquemas para productores y consumidores.
* Aplicar la compatibilidad con versiones anteriores, versiones posteriores y total en los temas.
* Reducir el tamaño de la carga útil enviada Kafka.

En un clúster de Kafka administrado por HDInsight, el registro de esquema se suele implementar en un nodo perimetral para permitir la separación de los procesos y los nodos principales.

A continuación se muestra una arquitectura que representa la implementación del registro de esquema en un clúster de HDInsight. El registro de esquema expone de forma nativa una API REST para efectuar operaciones en ella.  Los productores y consumidores pueden interactuar con el registro de esquema desde dentro de la red virtual o mediante el [proxy REST de Kafka](rest-proxy.md).

![Registro de esquema de Kafka en HDInsight](./media/schema-registry/pic1.png)

## <a name="deploy-an-hdinsight-managed-kafka-with-confluent-schema-registry"></a>Implementación de un clúster de Kafka administrado por HDInsight con el registro de esquema de Confluent

En esta sección, se implementa un clúster de Kafka administrado por HDInsight con un nodo perimetral dentro de una red virtual y, a continuación, se instala el registro de esquema de Confluent en el nodo perimetral.  

1. Seleccione el botón **Implementar en Azure** siguiente para iniciar sesión en Azure y abrir la plantilla de Resource Manager.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json" target="_blank">:::image type="icon" source="media/schema-registry/hdi-deploy-to-azure1.png":::</a>

1. En la plantilla de implementación personalizada, rellene los campos tal y como se describe a continuación:

    |Propiedad |Descripción |
    |---|---|
    |Subscription|En la lista desplegable, seleccione la suscripción de Azure que se usa para el clúster.|
    |Resource group|En la lista desplegable, seleccione el grupo de recursos existente o seleccione **Crear**.|
    |Region|En la lista desplegable, seleccione una región donde crear el clúster.|
    |Cluster Name|Escriba un nombre único global. O bien, deje el nombre predeterminado.|
    |Cluster Login User Name (Nombre de usuario de inicio de sesión del clúster)|Proporcione el nombre de usuario; el valor predeterminado es **admin**.|
    |Cluster Login Password (Contraseña de inicio de sesión del clúster)|Proporcione la contraseña.|
    |Nombre de usuario de SSH|Proporcione el nombre de usuario. El valor predeterminado es **sshuser**.|
    |Contraseña de SSH|Proporcione la contraseña.|

    Deje los demás campos tal como están. Seleccione **Revisar y crear** para continuar.

1. Revise los detalles de la implementación y, a continuación, seleccione **Crear** para inicializar la implementación. La implementación puede tardar 45 minutos en completarse.

## <a name="configure-the-confluent-schema-registry"></a>Configuración del registro de esquema de Confluent

En esta sección, vamos a configurar el registro de esquema de Kafka en Confluent que instalamos en el nodo perimetral. El registro de esquema de Confluent está ubicado en `/etc/schema-registry/schema-registry.properties`, y los mecanismos para iniciar y detener los archivos ejecutables de servicio se encuentran en la carpeta `/usr/bin/`.

El registro de esquema necesita conocer que el servicio Zookeeper puede interactuar con el clúster de Kafka en HDInsight. Siga estos pasos para obtener los detalles del cuórum de Zookeeper.

1. Use el [comando ssh](../hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al nodo perimetral del clúster. Modifique el comando siguiente: reemplace CLUSTERNAME por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@schema-registry.CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Configure una variable de contraseña. Reemplace PASSWORD por la contraseña de inicio de sesión del clúster y, después, escriba el comando:

    ```bash
    export password='PASSWORD'
    ```

1. Extraiga el nombre del clúster con las mayúsculas y minúsculas correctas. Ejecute el comando siguiente:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    ```

1. Extraiga los hosts de Zookeeper de Kafka. Ejecute el comando siguiente:

    ```bash
    export KAFKAZKHOSTS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2);

    echo $KAFKAZKHOSTS
    ```

    Tome nota de este valor, ya que se usará más adelante.

1. Extraiga los hosts de agente de Kafka. Ejecute el comando siguiente:

    ```bash
    export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);

    echo $KAFKABROKERS
    ```

1. Abra los archivos de propiedades del registro de esquema en el modo de edición. Ejecute el comando siguiente:

    ```bash
    sudo nano /etc/schema-registry/schema-registry.properties
    ```

    1. Actualice el valor de `kafkastore.connection.url` con la cadena de Zookeeper identificada anteriormente.
    1. Actualice el valor de `debug` a `true`.

    El archivo de propiedades ahora es similar a:  

    ```
    listeners=http://0.0.0.0:8081
    kafkastore.connection.url=zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
    kafkastore.topic=_schemas
    debug=true
    ```

1. Para guardar el archivo, presione **Ctr+X**, luego, **Y** y **Entrar**.

1. Inicie el registro de esquema y establézcalo para que use el archivo de propiedades del registro de esquema actualizado. Ejecute los siguientes comandos:

    ```bash
    cd /bin
    sudo schema-registry-start /etc/schema-registry/schema-registry.properties
    ```

1. Mientras el registro de esquema se ejecuta en una sesión de SSH, abra otra ventana de SSH.  Registre una nueva versión de un esquema con el asunto "Kafka-key" y tome nota de la salida:

    ```cmd
    curl -X POST -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
        --data '{"schema": "{\"type\": \"string\"}"}'
    ```

    ```output
       HTTP/1.1 200 OK
    Date: Sun, 22 Mar 2020 16:33:04 GMT
    Content-Type: application/vnd.schemaregistry.v1+json
    Content-Length: 9
    Server: Jetty(9.2.24.v20180105)
    ```

1. Registre una nueva versión de un esquema con el asunto "Kafka-value" y tome nota de la salida:

    ```cmd
    curl -X POST -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
        --data '{"schema": "{\"type\": \"string\"}"}' \
    ```

    ```output
    HTTP/1.1 200 OK
    Date: Sun, 22 Mar 2020 16:34:18 GMT
    Content-Type: application/vnd.schemaregistry.v1+json
    Content-Length: 9
    Server: Jetty(9.2.24.v20180105)
    ```

1. Enumere todos los asuntos y compruebe la salida:

    ```cmd
    curl -X GET -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
        http://localhost:8081/subjects
    ```

    ```output
    HTTP/1.1 200 OK
    Date: Sun, 22 Mar 2020 16:34:39 GMT
    Content-Type: application/vnd.schemaregistry.v1+json
    Content-Length: 27
    Server: Jetty(9.2.24.v20180105)

    ["Kafka-value","Kafka-key"]
    ```

Tal vez desee probar otros [comandos avanzados que aparecen en esta página](https://docs.confluent.io/1.0/schema-registry/docs/intro.html#quickstart).

## <a name="send-and-consume-avro-data-from-kafka-using-schema-registry"></a>Envío y consumo de datos de Avro desde Kafka mediante el registro de esquema

En esta sección, se leerán los datos de la entrada estándar y se escribirán en un tema de Kafka con un formato. A continuación, leeremos los datos de los temas de Kafka mediante un consumidor con un formateador Avro para transformar esos datos en un formato legible.

1. Cree un tema de Kafka `agkafkaschemareg`.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agkafkaschemareg --zookeeper $KAFKAZKHOSTS
    ```

1. Use **Kafka Avro Console Producer** para crear un esquema, asignar el esquema al tema y comenzar a enviar datos al tema en formato Avro. Asegúrese de que el tema de Kafka del paso anterior se haya creado correctamente y de que **$KAFKABROKERS** contenga un valor.

    El esquema que se enviará es una clave: Par de valor

    ```
    Value
    {
      "type": "record",
      "name": "example_schema",
      "namespace": "com.example",
      "fields": [
        {
          "name": "cust_id",
          "type": "int",
          "doc": "Id of the customer account"
        },
        {
          "name": "year",
          "type": "int",
          "doc": "year of expense"
        },
        {
          "name": "expenses",
          "type": {
            "type": "array",
            "items": "float"
          },
          "doc": "Expenses for the year"
        }
      ],
      "doc:": "A basic schema for storing messages"
    }
    ```

    Use el siguiente comando para iniciar **Kafka Avro Console Producer**:

    ```bash
    /usr/bin/kafka-avro-console-producer     --broker-list $KAFKABROKERS     --topic agkafkaschemareg     --property parse.key=true --property key.schema='{"type" : "int", "name" : "id"}'     --property value.schema='{ "type" : "record", "name" : "example_schema", "namespace" : "com.example", "fields" : [ { "name" : "cust_id", "type" : "int", "doc" : "Id of the customer account" }, { "name" : "year", "type" : "int", "doc" : "year of expense" }, { "name" : "expenses", "type" : {"type": "array", "items": "float"}, "doc" : "Expenses for the year" } ], "doc:" : "A basic schema for storing messages" }'
    ```

1. Cuando el productor esté preparado para aceptar mensajes, empiece a enviar los mensajes en el formato de esquema de Avro predefinido. Use la tecla TAB para crear espacio entre la clave y el valor.

    ```
    1 TAB {"cust_id":1313131, "year":2012, "expenses":[1313.13, 2424.24]}
    2 TAB {"cust_id":3535353, "year":2011, "expenses":[761.35, 92.18, 14.41]}
    3 TAB {"cust_id":7979797, "year":2011, "expenses":[4489.00]}
    ```

1. Intente escribir datos aleatorios no vinculados al esquema en el productor de consola para ver cómo ahora acepta datos que no se ajustan al esquema de Avro predefinido.

    ```
    1       {"cust_id":1313131, "year":2012, "expenses":[1313.13, 2424.24]}
    2       {"cust_id":1313131,"cust_age":34 "year":2012, "expenses":[1313.13, 2424.24,34.212]}
    org.apache.kafka.common.errors.SerializationException: Error deserializing json {"cust_id":1313131,"cust_age":34 "year":2012, "expenses":[1313.13, 2424.24,34.212]} to Avro of schema {"type":"record","name":"example_schema","namespace":"com.example","fields":[{"name":"cust_id","type":"int","doc":"Id of the customer account"},{"name":"year","type":"int","doc":"year of expense"},{"name":"expenses","type":{"type":"array","items":"float"},"doc":"Expenses for the year"}],"doc:":"A basic schema for storing messages"}
    Caused by: org.codehaus.jackson.JsonParseException: Unexpected character ('"' (code 34)): was expecting comma to separate OBJECT entries
     at [Source: java.io.StringReader@3561c410; line: 1, column: 35]
            at org.codehaus.jackson.JsonParser._constructError(JsonParser.java:1433)
            at org.codehaus.jackson.impl.JsonParserMinimalBase._reportError(JsonParserMinimalBase.java:521)
            at org.codehaus.jackson.impl.JsonParserMinimalBase._reportUnexpectedChar(JsonParserMinimalBase.java:442)
            at org.codehaus.jackson.impl.ReaderBasedParser.nextToken(ReaderBasedParser.java:406)
            at org.apache.avro.io.JsonDecoder.getVaueAsTree(JsonDecoder.java:549)
            at org.apache.avro.io.JsonDecoder.doAction(JsonDecoder.java:474)
            at org.apache.avro.io.parsing.Parser.advance(Parser.java:88)
            at org.apache.avro.io.JsonDecoder.advance(JsonDecoder.java:139)
            at org.apache.avro.io.JsonDecoder.readInt(JsonDecoder.java:166)
            at org.apache.avro.io.ValidatingDecoder.readInt(ValidatingDecoder.java:83)
            at org.apache.avro.generic.GenericDatumReader.readInt(GenericDatumReader.java:511)
            at org.apache.avro.generic.GenericDatumReader.readWithoutConversion(GenericDatumReader.java:182)
            at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:152)
            at org.apache.avro.generic.GenericDatumReader.readField(GenericDatumReader.java:240)
            at org.apache.avro.generic.GenericDatumReader.readRecord(GenericDatumReader.java:230)
            at org.apache.avro.generic.GenericDatumReader.readWithoutConversion(GenericDatumReader.java:174)
            at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:152)
            at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:144)
            at io.confluent.kafka.formatter.AvroMessageReader.jsonToAvro(AvroMessageReader.java:213)
            at io.confluent.kafka.formatter.AvroMessageReader.readMessage(AvroMessageReader.java:200)
            at kafka.tools.ConsoleProducer$.main(ConsoleProducer.scala:59)
            at kafka.tools.ConsoleProducer.main(ConsoleProducer.scala)
    
    ```

1. En una pantalla diferente, inicie Kafka Avro Console Consumer.

    ```bash
    sudo /usr/bin/kafka-avro-console-consumer --bootstrap-server $KAFKABROKERS --topic agkafkaschemareg --from-beginning
    ```

    Debería empezar a ver esta salida:

    ```output
    {"cust_id":1313131,"year":2012,"expenses":[1313.13,2424.24]}
    {"cust_id":7979797,"year":2011,"expenses":[4489.0]}
    {"cust_id":3535353,"year":2011,"expenses":[761.35,92.18,14.41]}
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Producer API y Consumer API de Apache Kafka](apache-kafka-producer-consumer-api.md)