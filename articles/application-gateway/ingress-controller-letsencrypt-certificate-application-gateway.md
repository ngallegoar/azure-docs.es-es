---
title: Uso de certificados de LetsEncrypt.org con Application Gateway
description: Este artículo ofrece información sobre cómo obtener un certificado de LetsEncrypt.org y usarlo en su instancia de Application Gateway para los clústeres de AKS.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 92e9747865f1a0910c8bae4001cc597ae9ea3da6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73957984"
---
# <a name="use-certificates-with-letsencryptorg-on-application-gateway-for-aks-clusters"></a>Uso de certificados con LetsEncrypt.org en Application Gateway para clústeres de AKS

En esta sección se configura su instancia de AKS para aprovechar [LetsEncrypt.org](https://letsencrypt.org/) y obtener automáticamente un certificado TLS/SSL para el dominio. El certificado se instalará en Application Gateway, que realizará la terminación de SSL/TLS para el clúster de AKS. La configuración que se describe aquí usa el complemento [cert-manager](https://github.com/jetstack/cert-manager) de Kubernetes, que automatiza la creación y la administración de certificados.

Siga los pasos que se indican a continuación para instalar [cert-manager](https://docs.cert-manager.io) en su clúster de AKS existente.

1. Gráfico de Helm

    Ejecute el siguiente script para instalar el gráfico de Helm de `cert-manager`. De este modo:

    - creará un nuevo espacio de nombres de `cert-manager` en su instancia de AKS
    - creará los siguientes CRD: Certificado, Desafío, ClusterIssuer, Emisor de certificado, Orden
    - instalación del gráfico de cert-manager (desde [docs.cert-manager.io)](https://docs.cert-manager.io/en/latest/getting-started/install/kubernetes.html#steps)

    ```bash
    #!/bin/bash

    # Install the CustomResourceDefinition resources separately
    kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.8/deploy/manifests/00-crds.yaml

    # Create the namespace for cert-manager
    kubectl create namespace cert-manager

    # Label the cert-manager namespace to disable resource validation
    kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

    # Add the Jetstack Helm repository
    helm repo add jetstack https://charts.jetstack.io

    # Update your local Helm chart repository cache
    helm repo update

    # Install the cert-manager Helm chart
    helm install \
      --name cert-manager \
      --namespace cert-manager \
      --version v0.8.0 \
      jetstack/cert-manager
    ```

2. Recurso ClusterIssuer

    Cree un recurso `ClusterIssuer`. `cert-manager` lo necesita para representar a la entidad de certificación `Lets Encrypt` en la que se obtendrán los certificados firmados.

    Mediante el uso del recurso `ClusterIssuer` sin espacios de nombres, cert-manager emitirá certificados que se pueden consumir desde varios espacios de nombres. `Let’s Encrypt` usa el protocolo ACME para comprobar que controla un nombre de dominio determinado y para emitirle un certificado. Más información sobre la configuración de las propiedades de `ClusterIssuer`[aquí](https://docs.cert-manager.io/en/latest/tasks/issuers/index.html). `ClusterIssuer` indicará a `cert-manager` que emita certificados mediante el entorno de ensayo de `Lets Encrypt` que se usa para las pruebas (el certificado raíz no está presente en los almacenes de confianza del explorador o del cliente).

    El tipo de desafío predeterminado en el siguiente YAML es `http01`. Otros desafíos están documentados en [Tipos de desafío de letsencrypt.org](https://letsencrypt.org/docs/challenge-types/)

    > [!IMPORTANT] 
    > Actualice `<YOUR.EMAIL@ADDRESS>` en el YAML siguiente

    ```bash
    #!/bin/bash
    kubectl apply -f - <<EOF
    apiVersion: certmanager.k8s.io/v1alpha1
    kind: ClusterIssuer
    metadata:
    name: letsencrypt-staging
    spec:
    acme:
        # You must replace this email address with your own.
        # Let's Encrypt will use this to contact you about expiring
        # certificates, and issues related to your account.
        email: <YOUR.EMAIL@ADDRESS>
        # ACME server URL for Let’s Encrypt’s staging environment.
        # The staging environment will not issue trusted certificates but is
        # used to ensure that the verification process is working properly
        # before moving to production
        server: https://acme-staging-v02.api.letsencrypt.org/directory
        privateKeySecretRef:
        # Secret resource used to store the account's private key.
        name: example-issuer-account-key
        # Enable the HTTP-01 challenge provider
        # you prove ownership of a domain by ensuring that a particular
        # file is present at the domain
        http01: {}
    EOF
    ```

3. Implementación de una aplicación

    Cree un recurso de entrada para exponer la aplicación `guestbook` mediante la instancia de Application Gateway con el certificado de Lets Encrypt.

    Asegúrese de que su instancia de Application Gateway tiene una configuración de IP pública de front-end con un nombre DNS (ya sea mediante el dominio de `azure.com` predeterminado o aprovisione un servicio de `Azure DNS Zone` y asigne su propio dominio personalizado).
    Observe la anotación `certmanager.k8s.io/cluster-issuer: letsencrypt-staging`, que indica a cert-manager que procese el recurso de entrada etiquetado.

    > [!IMPORTANT] 
    > Actualice `<PLACEHOLDERS.COM>` en el YAML siguiente con su propio dominio (o con el de la instancia de Application Gateway, por ejemplo, "kh-aks-ingress.westeurope.cloudapp.azure.com")

    ```bash
    kubectl apply -f - <<EOF
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
    name: guestbook-letsencrypt-staging
    annotations:
        kubernetes.io/ingress.class: azure/application-gateway
        certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    spec:
    tls:
    - hosts:
        - <PLACEHOLDERS.COM>
        secretName: guestbook-secret-name
    rules:
    - host: <PLACEHOLDERS.COM>
        http:
        paths:
        - backend:
            serviceName: frontend
            servicePort: 80
    EOF
    ```

    Después de unos segundos, puede acceder al servicio de `guestbook` con la dirección URL HTTPS de Application Gateway mediante el certificado `Lets Encrypt` de **almacenamiento provisional** emitido automáticamente.
    Es posible que su explorador web le advierta de una entidad de certificación no válida. `CN=Fake LE Intermediate X1` emite el certificado de almacenamiento provisional. Esto indica que el sistema funcionó según lo esperado y está listo para el certificado de producción.

4. Certificado de producción

    Una vez que el certificado de ensayo se haya configurado correctamente, puede cambiar a un servidor de producción ACME:
    1. Reemplace la anotación de almacenamiento provisional en el recurso de entrada con: `certmanager.k8s.io/cluster-issuer: letsencrypt-prod`
    1. Elimine el `ClusterIssuer` de almacenamiento provisional existente que creó en el paso anterior y cree uno nuevo; para ello, sustituya el servidor ACME del YAML del ClusterIssuer anterior por `https://acme-v02.api.letsencrypt.org/directory`

5. Expiración y renovación del certificado

    Antes de que expire el certificado de `Lets Encrypt`, `cert-manager` lo actualizará automáticamente en el almacén de secretos de Kubernetes. En ese punto, el controlador de entrada de Application Gateway aplicará el secreto actualizado al que se hace referencia en los recursos de entrada que está usando para configurar la instancia de Application Gateway.
