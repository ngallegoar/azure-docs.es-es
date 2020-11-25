---
title: Análisis de datos de Twitter con Apache Hive en Azure HDInsight
description: Obtenga información sobre cómo usar Apache Hive y Apache Hadoop en HDInsight para transformar datos sin procesar de Twitter en una tabla de Hive que permite realizar búsquedas.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 12/16/2019
ms.openlocfilehash: fe511ed2d6b724c1215f9986c9d6c50aae076935
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95971916"
---
# <a name="analyze-twitter-data-using-apache-hive-and-apache-hadoop-on-hdinsight"></a>Análisis de datos de Twitter con Apache Hive y Apache Hadoop en HDInsight

Aprenda a usar [Apache Hive](https://hive.apache.org/) para procesar los datos de Twitter. El resultado es una lista de usuarios de Twitter que enviaron la mayoría de los tweets que contienen una palabra determinada.

> [!IMPORTANT]  
> Los pasos de este documento se probaron en HDInsight 3.6.

## <a name="get-the-data"></a>Obtener los datos

Twitter permite recuperar los datos de cada tweet como un documento con formato de notación de objetos JavaScript (JSON) mediante una API REST. [OAuth](https://oauth.net) para autenticación en la API.

### <a name="create-a-twitter-application"></a>Crear una aplicación de Twitter

1. Desde un explorador web, inicie sesión en [https://developer.twitter.com/apps/](https://developer.twitter.com/apps/). Seleccione el vínculo **Regístrate ahora** si no tiene una cuenta de Twitter.

2. Seleccione **Crear nueva aplicación**.

3. Especifique los campos **Name** (Nombre), **Description** (Descripción), **Website** (Sitio web). Puede conformar una dirección URL para el campo **Website** (Sitio web). La siguiente tabla muestra algunos valores de ejemplo para utilizar:

   | Campo | Value |
   |--- |--- |
   | Nombre |MyHDInsightApp |
   | Descripción |MyHDInsightApp |
   | Website |`https://www.myhdinsightapp.com` |

4. Seleccione **Yes, I agree** (Acepto) y, a continuación, elija **Create your Twitter application** (Crear la aplicación de Twitter).

5. Seleccione la pestaña **Permisos**. El permiso predeterminado es **Read only**(Solo lectura).

6. Seleccione la pestaña **Claves y tokens de acceso** .

7. Seleccione **Create my access token** (Crear mi token de acceso).

8. Seleccione **Test OAuth** (Probar OAuth) en la esquina superior derecha de la página.

9. Rellene los campos **consumer key** (clave del consumidor), **Consumer secret** (Secreto del consumidor), **Access token** (Token de acceso) y **Access token secret** (Secreto del token de acceso).

### <a name="download-tweets"></a>Descarga de tweets

El siguiente código Python descarga 10 000 tweets de Twitter y los guarda en un archivo denominado **tweets.txt**.

> [!NOTE]  
> Los siguientes pasos se realizan en el clúster de HDInsight, puesto que Python ya está instalada.

1. Use el [comando SSH](./hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al clúster. Modifique el comando siguiente: reemplace CLUSTERNAME por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Use los comandos siguientes para instalar [Tweepy](https://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2) y otros paquetes requeridos:

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   python -m pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

1. Use el comando siguiente para crear un archivo denominado **gettweets.py**:

   ```bash
   nano gettweets.py
   ```

1. Para modificar el comando siguiente, reemplace `Your consumer secret`, `Your consumer key`, `Your access token` y `Your access token secret` por la información pertinente de su aplicación de Twitter. A continuación, pegue el código editado como el contenido del archivo **gettweets.py**.

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #The number of tweets we want to get
   max_tweets=100

   #Create the listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set the counter to zero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append the tweet to the 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment the number of tweets
               self.num_tweets += 1
               #Check to see if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment the progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get the OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use the listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!TIP]  
    > Ajuste el filtro de temas en la última línea para realizar un seguimiento de las palabras clave populares. El uso de palabras clave populares en el momento de ejecutar el script permite la captura más rápida de los datos.

1. Use **Ctrl+X** y luego **Y** para guardar el archivo.

1. Use el siguiente comando para ejecutar el archivo y descargar tweets:

    ```bash
    python gettweets.py
    ```

    Aparece un indicador de progreso. Cuenta hasta el 100% a medida que se descargan los tweets.

   > [!NOTE]  
   > Si la barra de progreso tarda mucho en avanzar, debería cambiar el filtro de seguimiento de las tendencias. Cuando hay muchos tweets sobre el tema en el filtro, puede obtener rápidamente los 100 tweets necesarios.

### <a name="upload-the-data"></a>Carga de los datos

Para cargar los datos de almacenamiento para HDInsight, use los comandos siguientes:

```bash
hdfs dfs -mkdir -p /tutorials/twitter/data
hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

Estos comandos almacenan los datos en una ubicación a la que pueden tener acceso todos los nodos del clúster.

## <a name="run-the-hiveql-job"></a>Ejecución del trabajo de HiveQL

1. Use el siguiente comando para crear un archivo que contenga instrucciones de [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual):

   ```bash
   nano twitter.hql
   ```

    Use el texto siguiente como contenido del archivo:

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward the tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate the destination table
   DROP TABLE tweets;
   CREATE TABLE tweets
   (
       id BIGINT,
       created_at STRING,
       created_at_date STRING,
       created_at_year STRING,
       created_at_month STRING,
       created_at_day STRING,
       created_at_time STRING,
       in_reply_to_user_id_str STRING,
       text STRING,
       contributors STRING,
       retweeted STRING,
       truncated STRING,
       coordinates STRING,
       source STRING,
       retweet_count INT,
       url STRING,
       hashtags array<STRING>,
       user_mentions array<STRING>,
       first_hashtag STRING,
       first_user_mention STRING,
       screen_name STRING,
       name STRING,
       followers_count INT,
       listed_count INT,
       friends_count INT,
       lang STRING,
       user_location STRING,
       time_zone STRING,
       profile_image_url STRING,
       json_response STRING
   );
   -- Select tweets from the imported data, parse the JSON,
   -- and insert into the tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
           when "Jan" then "01"
           when "Feb" then "02"
           when "Mar" then "03"
           when "Apr" then "04"
           when "May" then "05"
           when "Jun" then "06"
           when "Jul" then "07"
           when "Aug" then "08"
           when "Sep" then "09"
           when "Oct" then "10"
           when "Nov" then "11"
           when "Dec" then "12" end,
       substr (get_json_object(json_response, '$.created_at'),9,2),
       substr (get_json_object(json_response, '$.created_at'),12,8),
       get_json_object(json_response, '$.in_reply_to_user_id_str'),
       get_json_object(json_response, '$.text'),
       get_json_object(json_response, '$.contributors'),
       get_json_object(json_response, '$.retweeted'),
       get_json_object(json_response, '$.truncated'),
       get_json_object(json_response, '$.coordinates'),
       get_json_object(json_response, '$.source'),
       cast (get_json_object(json_response, '$.retweet_count') as INT),
       get_json_object(json_response, '$.entities.display_url'),
       array(
           trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
       array(
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
       trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
       trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
       get_json_object(json_response, '$.user.screen_name'),
       get_json_object(json_response, '$.user.name'),
       cast (get_json_object(json_response, '$.user.followers_count') as INT),
       cast (get_json_object(json_response, '$.user.listed_count') as INT),
       cast (get_json_object(json_response, '$.user.friends_count') as INT),
       get_json_object(json_response, '$.user.lang'),
       get_json_object(json_response, '$.user.location'),
       get_json_object(json_response, '$.user.time_zone'),
       get_json_object(json_response, '$.user.profile_image_url'),
       json_response
   WHERE (length(json_response) > 500);
   ```

1. Presione **Ctrl+X** y luego **Y** para guardar el archivo.

1. Use el siguiente comando para ejecutar el HiveQL incluido en el archivo:

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    Este comando ejecuta el archivo **twitter.hql**. Una vez que se completa la consulta, verá el aviso `jdbc:hive2//localhost:10001/>`.

1. Desde el símbolo del sistema de beeline, use la siguiente consulta para comprobar que se importaron datos:

   ```hiveql
   SELECT name, screen_name, count(1) as cc
   FROM tweets
   WHERE text like "%Azure%"
   GROUP BY name,screen_name
   ORDER BY cc DESC LIMIT 10;
   ```

    Esta consulta devolverá un máximo de 10 tweets que contienen la palabra **Azure** en el texto del mensaje.

    > [!NOTE]  
    > Si cambió el filtro en el script `gettweets.py`, reemplace **Azure** por uno de los filtros que usó.

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido cómo transformar un conjunto de datos JSON no estructurado en una tabla de [Apache Hive](https://hive.apache.org/) estructurada. Para obtener más información sobre Hive en HDInsight, consulte los siguientes documentos:

* [Introducción a HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Análisis de la información de retraso de vuelos con HDInsight](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)