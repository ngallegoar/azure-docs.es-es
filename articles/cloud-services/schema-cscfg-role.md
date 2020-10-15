---
title: Esquema de rol de Azure Cloud Services | Microsoft Docs
description: El elemento Rol de un archivo de configuración de servicio especifica el número de instancias de rol que se implementarán para cada rol, los valores de configuración y las huellas digitales de certificado.
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 12
author: tgore03
ms.author: tagore
ms.openlocfilehash: b64f9d27e382a39b132593502fed32c565af473a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "79528428"
---
# <a name="azure-cloud-services-config-role-schema"></a>Esquema de rol de configuración de Azure Cloud Services

El elemento `Role` del archivo de configuración especifica el número de instancias de rol que se implementan para cada rol del servicio, los valores de los parámetros de configuración y las huellas digitales de los certificados asociados a un rol.

Para más información sobre el esquema de configuración de servicios de Azure, consulte el [esquema de configuración de servicio en la nube (clásico)](schema-cscfg-file.md). Para más información sobre el esquema de definición de servicio de Azure, consulte [Cloud Service (classic) Definition Schema](schema-csdef-file.md) (Esquema de definición de servicio en la nube [clásico]).

##  <a name="role-element"></a><a name="Role"></a>Elemento Role
En el ejemplo siguiente se muestra el elemento `Role` y sus elementos secundarios.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

En la tabla siguiente se describen los atributos del elemento `Role`.

| Atributo | Descripción |
| --------- | ----------- |
| name   | Necesario. Especifica el nombre del rol. El nombre debe coincidir con el nombre proporcionado en el rol en el archivo de definición de servicio.|
| vmName | Opcional. Especifica el nombre DNS de una máquina virtual. El nombre debe tener 10 caracteres como máximo.|

En la siguiente tabla se describen los elementos secundarios del elemento `Role`.

| Elemento | Descripción |
| ------- | ----------- |
| Instancias | Necesario. Especifica el número de instancias para implementar en el rol. El número de instancias se define mediante un entero en el atributo `count`.|
| Configuración   | Opcional. Especifica un nombre y un valor de configuración en una colección de valores para un rol. El nombre de la configuración se define mediante una cadena en el atributo `name` y el valor de configuración se define mediante una cadena en el atributo `value`.|
| Certificado | Opcional. Especifica el nombre, la huella digital y el algoritmo de un certificado de servicio que se va a asociar con el rol. El nombre del certificado se define mediante una cadena en el atributo `name`. La huella digital del certificado se define mediante una cadena de números hexadecimales que no contienen espacios en el atributo `thumbprint`. Los números hexadecimales deben estar representados por dígitos y caracteres alfanuméricos en mayúscula. El algoritmo de certificado se define mediante una cadena en el atributo `thumbprintAlgorithm`.|

## <a name="see-also"></a>Consulte también
[Cloud Service (classic) Configuration Schema](schema-cscfg-file.md) (Esquema de configuración de Cloud Service [clásico])