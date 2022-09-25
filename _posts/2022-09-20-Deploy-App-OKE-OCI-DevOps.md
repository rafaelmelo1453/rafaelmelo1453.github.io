---
layout: post
title: Deploy App com OCI DevOps Pipeline
subtitle: Usando OCI DevOps Pipeline para realizar o deploy de um aplicativo em um cluster OKE.
share-img: /assets/img/oci.jpg
thumbnail-img: /assets/img/oci.jpg
tags: [Cloud, Oracle, OCI, DevOps, Pipeline, OKE, Cluster, LoadBalancer]
comments: true
---

OCI DevOps é um serviço Oracle Cloud Native para Integração e Entrega Contínua (CI/CD), neste tutorial usaremos para criar um pipeline e implantar uma aplicação simple de Hello World num cluster OKE com Load Balancer. 

## O que será criado?

| Criar um repositório no OCIR |
| Push do HelloWorld Image do DockerHub |
| Pull do HelloWorld Imagepara OCIR |
| Criar Secret no OKE |
| Criar Pipeline Deployment |
| Deploy e Acesso ao HelloWorld |

## Crie um (OCIR) Oracle Cloud Infrastructure Registry para armazenar imagens.

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

## Enviando HelloWorld Image para o OCIR (Oracle Cloud Infrastructure Registry).

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

```
$ docker login <region-key>.ocir.io
```

Insira o username com o seguinte formato \<tenancy-namespace\>/oracleidentitycloudservice/\<username\> para usuários federados ou \<tenancy-namespace\>/\<username\> para **usuários não federados.**
  
Então insira o Auth Token copiado anteriomente no lugar da senha.
  
![devops-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-06.png)

Ainda no terminal faça o pull da última versão de uma imagem helloworld do DockerHub executando o comando abaixo.

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

## Verificando e Preparando OKE para Deployment.

Conecte-se ao seu cluster OKE e verifique que o funcionamento kubectl está operacional.

```javascript
$ kubectl get nodes
```

![devops-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-11.png)

Gere uma secret que será usada no Kubernetes Manifest durante o deploy.

```javascript
$ kubectl create secret docker-registry ocirsecret
--docker-server=<region-key>.ocir.io --docker-username='<tenancy-namespace>/oracleidentitycloudservice/<username>' --docker-password='<oci-auth-token>' --docker-email='<email-address>'
```

Depois de gerada verifique se a secret foi criada corretamente.

```javascript
$ kubectl get secrets
```

![devops-12](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-12.png)

## Criando Pipeline Deployment com OCI DevOps.

#### Topic Notifications

Topic Notifications pode ser utilizado para enviar notificações sobre os eventos que ocorrem no OCI DevOps, Topic Notifications é um requisito para criar um novo projeto no OCI DevOps.

Na console clique em **Developer Services**. Em **Application Intergration**, clique em **Notifications**.

![devops-13](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-13.png)

Na página **Topics**, clique em **Create Topic**.

- Nome do topico: **hello-world-topic**.

Clique em **Create**.

![devops-15](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-15.png)

### OCI DevOps Project

Na Console, navegue até **Developer Services**, em **DevOps** clique em **Projects**.

![devops-14](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-14.png)

Na página **DevOps Projects**, clique em **Create DevOps Project**.

- Nome do Projeto: **helloworld**.
- Selecione o hello-world-topic criado anteriomente.

Clique em **Create DevOps Project**.

![devops-16](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-16.png)

### OCI DevOps Logs

Ative o OCI DevOps Logs para que seja possível acompanhar os logs passa a passo durante o deployment.

Clique em **Logs** e então habilite o DevOps **Logs**.

![devops-30](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-30.png)

### Artifact

Artifact é o Kubernetes Manifest que será usado para realizar o deployment no OKE e substituirá o arquivo .yaml que seria utilizado num deployment manual. 

Clique em **Artifact** e então em **Add Artifact**.

- Nome: helloworld-artifact.
- Type: Kubernetes Manifest.
- Artifact source: Inline
- Value: Cole o manifest abaixo alterando os campos **region-key**, **tenancy-namespace**, **repo-name**, **tag** e **secret-name** de acordo com as informações do seu ambiente.

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

### Environments

Criando um Environment é possível setar o cluster OKE onde será realizado o deployment do HelloWorld App.  

Em **Environments** clique em **Create environment.**

![devops-20](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-20.png)

Em **Environment type** selecione a opção **Oracle Kubernetes Engine**, adicione o nome **oke-env** e clique em **Next**.

![devops-21](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-21.png)

Então selecione o Cluster OKE onde será realizado o deployment. 

- Region
- Compartment
- Cluster: Selecione seu Cluster OKE
- VCN do Cluster OKE
- Subnet do Cluster OKE

Clique em **Create Enviroment.**

![devops-22](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-22.png)

### Criando Deployment Pipeline

Clique em **Create Pipeline** e então escolha a opção **Create a deployment pipeline** em Pipeline type.

![devops-18](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-18.png)

Adicione um estágio ao pipeline, clique no ícone + e selecione **Add Stage**, então selecione a opção **OKE: Default** e clique em Next.

![devops-19](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-19.png)

Na tela seguinte configure o pipeline deployment com as seguintes informações.

- Stage Name: helloworld-deploy.
- Environment: **oke-env** criado anteriomente.

Clique em **Add.**

![devops-23](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-23.png)

### Executando Deployment Pipeline

Clique em **Run Pipeline**

![devops-24](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-24.png)

Então clique em **Start Manual Run**.

![devops-28](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-28.png)

Acompanhe o deployment do App helloworld via logs no terminal que será aberto.

![devops-29](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-29.png)

Após finalização do deployment realize um double check do deployment, dessa vez direto no Cluster OKE via kubectl.

```javascript
$ kubectl get deployments
```

![devops-25](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-25.png)

### Acesse o site HelloWorld

Com deployment realizado com sucesso verifique os services via kubectl para obter o IP do LoadBalancer que foi criado para o App helloworld-service.

```javascript
$ kubectl get services
```

![devops-26](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-26.png)

No navegador acesse o App HelloWorld com o IP do loadbalancer, obtido no passo anterior.

![devops-31](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-31.png)

Note que o Container ID no site HelloWorl confere com o nome do POD no cluster OKE.

```javascript
$ kubectl get pods
```

![devops-27](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-27.png)

## Housekeeping/Limpeza (Opcional)

#### Deployment

Delete o deployment via kubectl.

```javascript
$ kubectl delete deployment helloworld-deployment
```

![devops-32](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-32.png)

#### Load Balancer

Delete o Load Balancer via kubectl.

```javascript
$ kubectl delete service helloworld-service
```

![devops-32](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-32.png)

#### (OCIR) Oracle Cloud Infrastructure Registry

Na Console de OCI navegue até **Developer Services** e clique em **Container Registry**.

Escolha o compartment onde o Registry foi criado.

Clique em **Actions** ao lado do nome do repositório e selecione **Delete Repository**, na tela seguinte confirme.

![devops-33](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-33.png)
