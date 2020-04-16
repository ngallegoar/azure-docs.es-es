---
title: 'Descodificación lógica: Azure Database for PostgreSQL: servidor único'
description: 'Describe la descodificación lógica y wal2json para la captura de datos modificados en Azure Database for PostgreSQL: servidor único'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 1213b38f2b67e8fed179cfda4308943808893e1b
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522576"
---
# <a name="logical-decoding"></a>Descodificación lógica
 
La [descodificación lógica en PostgreSQL](https://www.postgresql.org/docs/current/logicaldecoding.html) permite transmitir los cambios de los datos a consumidores externos. La descodificación lógica se usa frecuentemente para escenarios de streaming de eventos y de captura de datos modificados.

La descodificación lógica usa un complemento de salida para convertir el registro de escritura previa (WAL) de Postgre en un formato legible. Azure Database for PostgreSQL proporciona dos complementos de salida: [test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) y [wal2json](https://github.com/eulerto/wal2json).
 

> [!NOTE]
> La descodificación lógica se encuentra en versión preliminar pública en Azure Database for PostgreSQL: servidor único.


## <a name="set-up-your-server"></a>Configuración del servidor
Para empezar a usar la descodificación lógica, habilite el servidor para guardar y transmitir el registro de escritura previa. 

1. Establezca azure.replication_support en `logical` mediante la CLI de Azure. 
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ```

   > [!NOTE]
   > Si usa réplicas de lectura, azure.replication_support establecido en `logical` también permite que se ejecuten las réplicas. Si deja de usar la descodificación lógica, vuelva a cambiar la configuración a `replica`. 


2. Reinicie el servidor para aplicar los cambios.
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

## <a name="start-logical-decoding"></a>Inicio de la descodificación lógica

La descodificación lógica se puede consumir mediante el protocolo de streaming o la interfaz SQL. Ambos métodos usan [ranuras de replicación](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS). Una ranura representa una secuencia de cambios de una sola base de datos.

El uso de una ranura de replicación requiere privilegios de replicación de Postgre. En este momento, el privilegio de replicación solo está disponible para el usuario administrador del servidor. 

### <a name="streaming-protocol"></a>Protocolo de streaming
El consumo de los cambios mediante el protocolo de streaming suele ser preferible. Puede crear su propio consumidor o conector o usar una herramienta como [Debezium](https://debezium.io/). 

Consulte la documentación de wal2json para obtener [un ejemplo de uso del protocolo de streaming con pg_recvlogical](https://github.com/eulerto/wal2json#pg_recvlogical).


### <a name="sql-interface"></a>Interfaz SQL
En el ejemplo siguiente, usamos la interfaz SQL con el complemento wal2json.
 
1. Cree una ranura.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. Emita comandos SQL. Por ejemplo:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Consuma los cambios.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   La salida tendrá el siguiente aspecto:
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. Elimine la ranura cuando haya terminado de usarla.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```


## <a name="monitoring-slots"></a>Supervisión de las ranuras

Debe supervisar la descodificación lógica. Se debe eliminar cualquier ranura de replicación no utilizada. Las ranuras se mantienen en los registros de WAL de Postgres y los catálogos del sistema correspondientes hasta que un consumidor ha leído los cambios. Si se produce un error en el consumidor o este no se ha configurado correctamente, los registros no consumidos se acumularán y llenarán el almacenamiento. Además, los registros no consumidos aumentan el riesgo de un salto del identificador de transacción. Ambas situaciones pueden hacer que el servidor deje de estar disponible. Por lo tanto, es fundamental que las ranuras de replicación lógicas se consuman continuamente. Si ya no se usa una ranura de replicación lógica, elimínela inmediatamente.

La columna "active" (activo) en la vista pg_replication_slots indicará si hay un consumidor conectado a una ranura.
```SQL
SELECT * FROM pg_replication_slots;
```

[Establezca alertas](howto-alert-on-metric.md) en las métricas de *almacenamiento utilizado* y *retraso máximo entre réplicas* para recibir una notificación cuando los valores superen los umbrales normales. 

> [!IMPORTANT]
> Debe eliminar las ranuras de replicación no usadas. Si no lo hace, puede provocar la falta de disponibilidad del servidor.

## <a name="how-to-drop-a-slot"></a>Cómo eliminar una ranura
Si no está consumiendo activamente una ranura de replicación, debe eliminarla.

Para eliminar una ranura de replicación llamada `test_slot` mediante SQL:
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> Si deja de usar la descodificación lógica, cambie azure.replication_support a `replica` u `off`. Los detalles de WAL retenidos con la opción `logical` son más detallados y se deben deshabilitar cuando la descodificación lógica no está en uso. 

 
## <a name="next-steps"></a>Pasos siguientes

* Consulte la documentación de Postgres [para más información sobre la descodificación lógica](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html).
* Póngase en contacto con [nuestro equipo](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) si tiene preguntas sobre la descodificación lógica.
* Más información sobre [réplicas de lectura](concepts-read-replicas.md).

