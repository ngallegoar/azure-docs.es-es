---
title: Configurar modos de red para servicios de contenedor
description: Aprenda a configurar los diferentes modos de red que admite Azure Service Fabric.
author: athinanthny
ms.topic: conceptual
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: e6174f35bd54b3ca0b2c5240a663369350b30ce8
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86241903"
---
# <a name="service-fabric-container-networking-modes"></a>Modos de redes de contenedor de Service Fabric

Un clúster de Azure Service Fabric de servicios de contenedor usa de forma predeterminada el modo de red **nat**. Cuando más de un servicio de contenedor escucha en el mismo puerto y se usa el modo nat, pueden producirse errores de implementación. Para permitir que varios servicios de contenedor escuchen en el mismo puerto, Service Fabric ofrece el modo de red **abierto** (versión 5.7 y posterior). En modo abierto, cada servicio de contenedor tiene una dirección IP interna asignada dinámicamente que admite que varios servicios escuchen en el mismo puerto.  

Si tiene un servicio de contenedor con un punto de conexión estático en el manifiesto del servicio, puede crear y eliminar nuevos servicios mediante el modo abierto sin errores de implementación. El mismo archivo docker-compose.yml se puede usar también con asignaciones de puertos estáticos para crear varios servicios.

Cuando un servicio de contenedor se reinicia o se mueve a otro nodo del clúster, la dirección IP cambia. Por esta razón, no se recomienda usar la dirección IP asignada dinámicamente para detectar servicios de contenedor. Solo se debe usar para ello el servicio de nombres de Service Fabric o el servicio DNS. 

>[!WARNING]
>Azure permite hasta un total de 65 356 direcciones IP por red virtual. La suma del número de nodos y el número de instancias de servicio de contenedor (que usan el modo abierto) no puede superar 65 356 direcciones IP en una red virtual. En escenarios de alta densidad, se recomienda el modo de red nat. Además, otras dependencias, como el equilibrador de carga, tendrán otra [limitaciones](../azure-resource-manager/management/azure-subscription-service-limits.md) a tener en cuenta. Actualmente se han probado hasta 50 direcciones IP por nodo, y han demostrar ser estables. 
>

## <a name="set-up-open-networking-mode"></a>Configuración del modo de red abierto

1. Configure la plantilla de Azure Resource Manager. En la sección **fabricSettings** del recurso de clúster, habilite el servicio DNS y el proveedor de IP: 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```
    
2. Configure la sección de perfil de red del recurso de conjunto de escalado de máquinas virtuales. Esto permite la configuración de varias direcciones IP en cada nodo del clúster. En el ejemplo siguiente se configuran cinco direcciones IP por nodo de un clúster de Service Fabric de Windows o Linux. Puede tener cinco instancias de servicio que escuchen en el puerto en cada nodo. Para que Azure Load Balancer pueda acceder a las cinco direcciones IP, inscríbalas en el grupo de direcciones de back-end de Azure Load Balancer como se indica a continuación.  También deberá agregar las variables a la parte superior de la plantilla en la sección de variables.

    Agregue esta sección a Variables:

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    ```
    
    Agregue esta sección al recurso del conjunto de escalado de máquinas virtuales:

    ```json   
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
   ```
 
3. Solo en clústeres de Windows, configure una regla de grupo de seguridad de red (NSG) de Azure que abra el puerto UDP/53 para la red virtual con los siguientes valores:

   |Configuración |Value |
   | --- | --- |
   |Priority |2000 |
   |Nombre |Custom_Dns  |
   |Source |VirtualNetwork |
   |Destination | VirtualNetwork |
   |Servicio | DNS (UDP/53) |
   |Acción | Allow  |

4. Especifique el modo de red en el manifiesto de aplicación para cada servicio `<NetworkConfig NetworkType="Open">`. El modo **abierto** da lugar a que el servicio obtenga una dirección IP dedicada. Si no se especifica un modo, el servicio adopta como valor predeterminado el modo **nat**. En el siguiente ejemplo de manifiesto, los servicios `NodeContainerServicePackage1` y `NodeContainerServicePackage2` pueden escuchar en el mismo puerto (ambos servicios escuchan en `Endpoint1`). Cuando se especifica el modo de red abierto, no se pueden especificar configuraciones de `PortBinding`.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```

    Puede mezclar y combinar diferentes modos de redes en todos los servicios dentro de una aplicación para un clúster de Windows. Algunos servicios pueden usar el modo abierto y otros el modo nat. Cuando un servicio está configurado para usar el modo nat, el puerto en el que escucha el servicio debe ser único.

    >[!NOTE]
    >En clústeres de Linux, no se admite la combinación de modos de red para diferentes servicios. 
    >

5. Si el modo **Abierto** está seleccionado, la definición **Endpoint** del manifiesto de servicio debe apuntar de forma explícita al paquete de código correspondiente del punto de conexión, incluso si dicho paquete de servicio solo contiene un paquete de código. 
   
   ```xml
   <Resources>
     <Endpoints>
       <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" CodePackageRef="Code"/>
     </Endpoints>
   </Resources>
   ```
   
6. Para Windows, un reinicio de la máquina virtual hará que la red abierta se vuelva a crear. Esto sirve para mitigar un problema subyacente en la pila de redes. El comportamiento predeterminado consiste en volver a crear la red. Si tiene que desactivar este comportamiento, puede usar la configuración siguiente seguida de una actualización de configuración.

```json
"fabricSettings": [
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "SkipContainerNetworkResetOnReboot",
                            "value": "true"
                    }
                    ]
                }
            ],          
 ``` 
 
## <a name="next-steps"></a>Pasos siguientes
* [Entender el modelo de aplicación de Service Fabric](service-fabric-application-model.md)
* [Aprenda más sobre los recursos del manifiesto de servicio de Service Fabric](./service-fabric-service-manifest-resources.md)
* [Implementación de un contenedor de Windows en Service fabric en Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementación de un contenedor de Docker en Service Fabric en Linux](service-fabric-get-started-containers-linux.md)
