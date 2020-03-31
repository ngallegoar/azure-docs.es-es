---
title: Transformación Aplanar en el flujo de datos de asignación
description: Desnormalice los datos jerárquicos mediante la transformación Aplanar.
author: kromerm
ms.author: makromer
ms.review: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: b19aae8ab6730936a826f5bb069bfdb7d696cdfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246643"
---
# <a name="flatten-transformation-in-mapping-data-flow"></a>Transformación Aplanar en el flujo de datos de asignación

Use la transformación Aplanar para tomar valores de matriz incluidos en estructuras jerárquicas, como JSON, y expandirlos en filas individuales. Este proceso se conoce como desnormalización.

## <a name="configuration"></a>Configuración

La transformación Aplanar contiene los siguientes valores de configuración:

![Configuración de Aplanar](media/data-flow/flatten1.png "Configuración de Aplanar")

### <a name="unroll-by"></a>Expandir por

Seleccione la matriz que se va a expandir. Los datos de salida tendrán una fila por cada elemento de cada matriz. Si la matriz que se va a expandir de la fila de entrada tiene un valor NULL o está vacía, habrá una fila de salida con valores no expandidos con valor NULL.

### <a name="unroll-root"></a>Expandir la raíz

De forma predeterminada, la transformación Aplanar expande una matriz sobre la jerarquía en la que existe. Opcionalmente, puede seleccionar una matriz como raíz de la expansión. La raíz de la expansión debe ser una matriz de objetos complejos que sea o que contenga la matriz que se va a expandir. Si se selecciona una raíz de expansión, los datos de salida contendrán al menos una fila por cada elemento de dicha raíz. Si la fila de entrada no tiene ningún elemento de la raíz de expansión, se quitará de los datos de salida. Al elegir una raíz de expansión, siempre se generará un número de filas menor o igual que el comportamiento predeterminado.

### <a name="flatten-mapping"></a>Asignación de aplanamiento

De forma similar a la transformación de selección, elija la proyección de la nueva estructura a partir de los campos de entrada y la matriz desnormalizada. Si se asigna una matriz desnormalizada, la columna de salida tendrá el mismo tipo de datos que la matriz. Si la matriz de expansión es una matriz de objetos complejos que contiene submatrices, la asignación de un elemento de dicha submatriz producirá una matriz.

Consulte la pestaña de inspección y la vista previa de los datos para comprobar la salida de la asignación.

## <a name="examples"></a>Ejemplos

Consulte el siguiente objeto JSON como referencia para los ejemplos siguientes de la transformación Aplanar.

``` json
{
  "name":"MSFT","location":"Redmond", "satellites": ["Bay Area", "Shanghai"],
  "goods": {
    "trade":true, "customers":["government", "distributer", "retail"],
    "orders":[
        {"orderId":1,"orderTotal":123.34,"shipped":{"orderItems":[{"itemName":"Laptop","itemQty":20},{"itemName":"Charger","itemQty":2}]}},
        {"orderId":2,"orderTotal":323.34,"shipped":{"orderItems":[{"itemName":"Mice","itemQty":2},{"itemName":"Keyboard","itemQty":1}]}}
    ]}}
{"name":"Company1","location":"Seattle", "satellites": ["New York"],
  "goods":{"trade":false, "customers":["store1", "store2"],
  "orders":[
      {"orderId":4,"orderTotal":123.34,"shipped":{"orderItems":[{"itemName":"Laptop","itemQty":20},{"itemName":"Charger","itemQty":3}]}},
      {"orderId":5,"orderTotal":343.24,"shipped":{"orderItems":[{"itemName":"Chair","itemQty":4},{"itemName":"Lamp","itemQty":2}]}}
    ]}}
{"name": "Company2", "location": "Bellevue",
  "goods": {"trade": true, "customers":["Bank"], "orders": [{"orderId": 4, "orderTotal": 123.34}]}}
{"name": "Company3", "location": "Kirkland"}
```

### <a name="no-unroll-root-with-string-array"></a>Sin raíz de expansión, con matriz de cadenas

| Expandir por | Expandir la raíz | Proyección |
| --------- | ----------- | ---------- |
| goods.customers | None | name <br> customer = goods.customer |

#### <a name="output"></a>Output

```
{ 'MSFT', 'government'}
{ 'MSFT', 'distributer'}
{ 'MSFT', 'retail'}
{ 'Company1', 'store'}
{ 'Company1', 'store2'}
{ 'Company2', 'Bank'}
{ 'Company3', null}
```

### <a name="no-unroll-root-with-complex-array"></a>Sin raíz de expansión, con matriz compleja

| Expandir por | Expandir la raíz | Proyección |
| --------- | ----------- | ---------- |
| goods.orders.shipped.orderItems | None | name <br> orderId = goods.orders.orderId <br> itemName = goods.orders.shipped.orderItems.itemName <br> itemQty = goods.orders.shipped.orderItems.itemQty <br> location = location |

#### <a name="output"></a>Output

```
{ 'MSFT', 1, 'Laptop', 20, 'Redmond'}
{ 'MSFT', 1, 'Charger', 2, 'Redmond'}
{ 'MSFT', 2, 'Mice', 2, 'Redmond'}
{ 'MSFT', 2, 'Keyboard', 1, 'Redmond'}
{ 'Company1', 4, 'Laptop', 20, 'Seattle'}
{ 'Company1', 4, 'Charger', 3, 'Seattle'}
{ 'Company1', 5, 'Chair', 4, 'Seattle'}
{ 'Company1', 5, 'Lamp', 2, 'Seattle'}
{ 'Company2', 4, null, null, 'Bellevue'}
{ 'Company3', null, null, null, 'Kirkland'}
```

### <a name="same-root-as-unroll-array"></a>Misma raíz que la matriz de expansión

| Expandir por | Expandir la raíz | Proyección |
| --------- | ----------- | ---------- |
| goods.orders | goods.orders | name <br> goods.orders.shipped.orderItems.itemName <br> goods.customers <br> ubicación |

#### <a name="output"></a>Output

```
{ 'MSFT', ['Laptop','Charger'], ['government','distributer','retail'], 'Redmond'}
{ 'MSFT', ['Mice', 'Keyboard'], ['government','distributer','retail'], 'Redmond'}
{ 'Company1', ['Laptop','Charger'], ['store', 'store2'], 'Seattle'}
{ 'Company1', ['Chair', 'Lamp'], ['store', 'store2'], 'Seattle'}
{ 'Company2', null, ['Bank'], 'Bellevue'}
```

### <a name="unroll-root-with-complex-array"></a>Raíz de expansión, con matriz compleja

| Expandir por | Expandir la raíz | Proyección |
| --------- | ----------- | ---------- |
| goods.orders.shipped.orderItem | goods.orders |name <br> orderId = goods.orders.orderId <br> itemName = goods.orders.shipped.orderItems.itemName <br> itemQty = goods.orders.shipped.orderItems.itemQty <br> location = location |

#### <a name="output"></a>Output

```
{ 'MSFT', 1, 'Laptop', 20, 'Redmond'}
{ 'MSFT', 1, 'Charger', 2, 'Redmond'}
{ 'MSFT', 2, 'Mice', 2, 'Redmond'}
{ 'MSFT', 2, 'Keyboard', 1, 'Redmond'}
{ 'Company1', 4, 'Laptop', 20, 'Seattle'}
{ 'Company1', 4, 'Charger', 3, 'Seattle'}
{ 'Company1', 5, 'Chair', 4, 'Seattle'}
{ 'Company1', 5, 'Lamp', 2, 'Seattle'}
{ 'Company2', 4, null, null, 'Bellevue'}
```

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
foldDown(unroll(<unroll cols>),
    mapColumn(
        name,
        each(<array>(type == '<arrayDataType>')),
        each(<array>, match(true())),
        location
    )) ~> <transformationName>
```

### <a name="example"></a>Ejemplo

```
source foldDown(unroll(goods.orders.shipped.orderItems, goods.orders),
    mapColumn(
        name,
        orderId = goods.orders.orderId,
        itemName = goods.orders.shipped.orderItems.itemName,
        itemQty = goods.orders.shipped.orderItems.itemQty,
        location = location
    ),
    skipDuplicateMapInputs: false,
    skipDuplicateMapOutputs: false) 
```    

## <a name="next-steps"></a>Pasos siguientes

* Use la [transformación Dinamizar](data-flow-pivot.md) para dinamizar las filas en columnas.
* Use la [transformación Anular dinamización](data-flow-unpivot.md) para dinamizar las columnas a filas.
