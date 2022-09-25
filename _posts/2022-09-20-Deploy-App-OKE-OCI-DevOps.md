---
layout: post
title: Deploy App com OCI DevOps Pipeline
subtitle: Usando OCI DevOps Pipeline para realizar o deploy de um aplicativo em um cluster OKE.
share-img: /assets/img/oci.jpg
thumbnail-img: /assets/img/oci.jpg
tags: [Cloud, Oracle, OCI, DevOps, Pipeline, OKE, Cluster, LoadBalancer]
comments: true
---

OCI DevOps é um serviço Oracle Cloud Native para Integração e Entrega Contínua (CI/CD), neste tutorial usaremos para criar uma pipeline para implantar uma aplicação num cluster OKE. 

## Criando um Repository para armazenar Imagens.

Na Console de OCI navegue até **Developer Services** e clique em **Container Registry**. 

- Escolha o compartment e região (neste exemplo, br-est-sao-work e Brazil East, São Paulo).

![devops-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-01.png)

Clique em **Create Repository.**

- Escolha o compartment.
- Insira um nome para o repositório (neste exemplo, helloworld).
- Selecione a opção **Private.**

Então clique em **Create Repository** para finalizar.

![devops-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-02.png)

- Insira uma descrição e novamente clique em **Generate Token.**
- Copie o auth token imediatamente para um notepad, pois não é possível recuperá-lo após fechar.

## Push uma Imagem para o OCIR (Oracle Cloud Infrastructure Registry).

Para fazer o push da imagem docker para o OCIR voçe vai precisar das seguintes informações sobre o tenancy OCI:

- Tenancy namespace.
- Username.
- Auth Token (necessário gerar).
- Region Key.

Para obter o **Tenancy Namespace** clique em **Governance & Administration** e navegue até **Tenancy details**.

![devops-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-03.png)

Para obter o **Username** vá até **Profile** e clique em **User Settings**.

![devops-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-04.png)

Caso seu tenancy seja federado com o Oracle Identity Cloud Service, copie o texto completo incluído o oracleidentitycloudservice.

Ainda em **User Settings** clique em **Auth Tokens** e depois em  **Generate Token** para gerar um novo **Auth Token**.

- Insira uma descrição e novamente clique em **Generate Token.**
- Copie o auth token imediatamente para um notepad, pois não é possível recuperá-lo após fechar.

![devops-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-05.png)

Para obter a Region Key acesse [Regiões e Domínios de Disponibilidade](https://docs.oracle.com/pt-br/iaas/Content/General/Concepts/regions.htm)

Com isso temos todos as informações para os próximos passos.

Abra uma janela do terminal num desktop com Docker instalado, faça login no Oracle Cloud Infrastructure Registry usando o seguinte comando:

```javascript
$ docker login <region-key>.ocir.io
```

Insira o username com o seguinte formato \<tenancy-namespace\>/oracleidentitycloudservice/\<username\> para usuários federados ou \<tenancy-namespace\>/\<username\> para **usuários não federados.**
  
Então insira o Auth Token copiado anteriomente no lugar da senha.
  
![devops-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-06.png)

Ainda no terminal faça o pull da última versão de uma imagem hello-world do DockerHub executando o comando abaixo.

```javascript
$ docker pull karthequian/helloworld:latest
```

![devops-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-07.png)

Adicione uma tag à imagem do helloworld antes de envia-la para o OCIR (Oracle Cloud Infrastructure Registry).

```javascript
$ docker tag karthequian/helloworld:latest <region-key>.ocir.io/tenancy-namespace/helloworld:latest
```

Agora verifique se ambas as imagens estão disponíveis usando o comando abaixo.

```javascript
$ docker images
```

![devops-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-08.png)

Faça o push imagem para o OCIR (Oracle Cloud Infrastructure Registry) utilizando o seguinte comando:

```javascript
$ docker push <region-key>.ocir.io/<tenancy-namespace>/helloworld:latest
```

![devops-09](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-09.png)

Verifique se a imagem consta no Oracle Cloud Infrastructure Registry

![devops-10](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-10.png)

## Deploy APP.

Conecte-se ao seu cluster OKE e teste o kubectl.

```javascript
$ kubectl get nodes
```

![devops-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-11.png)

Crie uma secret 

```javascript
$ kubectl create secret docker-registry ocirsecret
--docker-server=<region-key>.ocir.io --docker-username='<tenancy-namespace>/oracleidentitycloudservice/<username>' --docker-password='<oci-auth-token>' --docker-email='<email-address>'
```

Verifique se a secret foi criada corretamente.

```javascript
$ kubectl get secrets
```

![devops-12](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-12.png)

Para criar um tópico:

Na console clique em **Developer Services**. Em **Application Intergration**, clique em **Notifications**.

![devops-13](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-13.png)

Na página **Topics**, clique em **Create Topic**.

- Nome do topico: **hello-world-topic**.

Clique em **Create**.

![devops-15](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-15.png)




Na Console, navegue até **Developer Services**, em **DevOps** clique em **Projects**.

![devops-14](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-14.png)

Na página **DevOps Projects**, clique em **Create DevOps Project**.

- Nome do Projeto: **helloworld**.
- Selecione o hello-world-topic criado anteriomente.

Clique em **Create DevOps Project**.

![devops-16](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-16.png)

Clique em **Artifact** e então em **Add Artifact**.

- Nome: helloworld-artifact.
- Type: General Artifact.
- Artifact source: Inline
- Value: Cole o manifest abaixo alterando os campos **region-key**, **tenancy-namespace**, **repo-name**, **tag** e **secret-nama**.

Editando Manifest File para deploy.

```javascript
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-deployment
spec:
  selector:
    matchLabels:
      app: helloworld
  replicas: 1
  template:
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: helloworld   
        image: <region-key>.ocir.io/<tenancy-namespace>/<repo-name>:<tag>
        ports:
        - containerPort: 80
      imagePullSecrets: 
      - name: <secret-name>
---
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
  annotations:
    service.beta.kubernetes.io/oci-load-balancer-shape: "flexible"
    service.beta.kubernetes.io/oci-load-balancer-shape-flex-min: "10"
    service.beta.kubernetes.io/oci-load-balancer-shape-flex-max: "10"
spec:
  type: LoadBalancer
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: helloworld
```

![devops-17](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-17.png)

Em **Environments** clique em **Create environment.**

![devops-20](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-20.png)

Selecione 

- Environment type: Oracle Kubernetes Engine
- Name: oke-env

Clique em Next.

![devops-21](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-21.png)

Selecione 

- Region
- Compartment
- Cluster: Selecione seu Cluster OKE
- VCN do Cluster OKE
- Subnet do Cluster OKE

Clique em **Create Enviroment.**

![devops-22](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-22.png)

Em Deployment Pipeline

Clique em **Create Pipeline.**

- Pipeline type: **Create a deployment pipeline**.

![devops-18](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-18.png)

Adicione um estágio ao pipeline, clique no ícone + e selecione **Add Stage**.

Selecione a opção **OKE: Default** e clique em Next.

![devops-19](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-19.png)

Verifique se deployment foi realizado com sucesso no cluster.

```javascript
$ kubectl get deployments
```

![devops-25](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-25.png)

Verifique os services para obter o IP do LoadBalancer para o helloworld-service.

```javascript
$ kubectl get services
```

![devops-26](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-26.png)

```javascript
$ kubectl get pods
```

![devops-27](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-27.png)
