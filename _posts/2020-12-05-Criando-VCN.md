---
layout: post
title: Provisionando VCN, Subnet, Internet Gateway e Route Table na Oracle Cloud
subtitle: Três formas de criar uma Virtual Cloud Network, Subnet, Internet Gateway e Route Table na Oracle Cloud Infrastructure
tags: [Cloud, Oracle, OCI, CLI, VCN, Subnet, Internet Gateway, Route Table]
comments: true
---

## Requisitos

| Uma conta Oracle Cloud | Obrigatório |
| User com permissão de Manage Network-Family | Obrigatório |

## 1. Usando VCN Wizard

**Compartment:** Crie uma compartment para organizar os recursos que serão provisionados. Acesse a console administrativa da Oracle Cloud Infrastructure e vá até **Identity > Compartments > Create Compartment**.

![compartment](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/PO5B4oaCU3ZT3tgar-MMbqAVxUGtIwvDcYkCFOc5dZzHLswBoxWF16HrWjcmGpnw/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/compartment.png)

**VCN:** Navegue até **Networking > Virtual Cloud Networks** , selecione o compartment desejado e click em **Start VCN Wizard.**

![Wizard-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/GUKPr5_WgBuD-F9iel-uJStKSQ3skEYeOpJze4LmZcKNEl9gY2CaVHav7LPVJ6EQ/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-01.png)

Escolha a opção **VCN with Internet Connectivity** para criar uma VCN com conexão com a internet, assim todos os recursos necessários para conectividade externa será criada.

![Wizard-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/rUtYWUSueX0FoOCj5AN5Iqy2VDuf403UaeNEpBTN7LJEUwvdK_QTIWfGqYLCVTBB/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-02.png)

- Name: VCN-wizard
- Compartment: compartment-test
- CIDR BLOCK: 10.0.0.0/16
- Public Subnet CIDR BLOCK: 10.0.0.0/24
- Private Subnet CIDR BLOCK: 10.0.1.0/24
- DNS Resolution: VCNwizard

![Wizard-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/AGk4L8I6rXmW4EI2YzaoF-be35ElY3cQSE3H99zOplmmhAKAEiaMrl__cZG93m4y/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-04.png)

Observe os recursos que serão criados automaticamente para a VCN e clique em **Next**.

**Public Subnet**
- Security List Name: Default Security List for VCN-wizard
- Route Table Name: Default Route Table for VCN-wizard
- DNS Label: sub12050114050

**Private Subnet**
- Security List Name: Security List for Private Subnet-VCN-wizard
- Route Table Name: Route Table ffor Private Subnet-VCN-wizard
- DNS Label: sub12050114051

**Gateways**
- Internet Gateway: Internet Gateway-VCN-wizard
- NAT Gateway: NAT Gateway-VCN-wizard
- Service Gateway: Service Gateway-VCN-wizard

![Wizard-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/7HpjlyqI5YsLn426HQXWq6u1Gbk4HqR0UAgSvT5YNlJ_tkVYokvH0VgkuZQzV8an/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-05.png)

![Wizard-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/c9TY27CtiXrPlMyRIty0NwFYaVxv_HaN8RjQOg_g7ImotY3Chg5c6y1FaWxv7px3/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-06.png)

Certifique-se que todos os recursos foram criados corretamente em **Networking > Virtual Cloud Networks > VCN-wizard**.

![Wizard-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ltmZDuv5EGKkjonlNBBZk82Hm3FHwNAbGQjJIMAoN7zO-CGTrbI1tJGdLB15AvOo/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-07.png)

**Aproveite a VCN recém criada e provisione uma Instance VM seguindo [estas instruções](https://smallskills.github.io/2020-12-04-Configurando-OCI-CLI/).**

## 2. Criando recursos da VCN individualmente

**Compartment:** Crie uma compartment para organizar os recursos que serão provisionados. Acesse a console administrativa da Oracle Cloud Infrastructure e vá até **Identity > Compartments > Create Compartment**.

![compartment](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/PO5B4oaCU3ZT3tgar-MMbqAVxUGtIwvDcYkCFOc5dZzHLswBoxWF16HrWjcmGpnw/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/compartment.png)

**VCN:** Crie a VCN em até **Networking > Virtual Cloud Networks > Create VCN**.
- Name: VCN
- Compartment: compartment-test
- CIDR BLOCK: 10.0.0.0/16
- DNS Resolution: vcn

![VCN-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/EnjfQlMW-xHRUlXYq6iEvsV04fGqSEujliJzgCN0LOiV7-oEe42Hfz57o1T8HnHF/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-01.png)

**Internet Gateway:** Crie um Internet Gateway em **Networking > Virtual Cloud Networks > VCN > Internet Gateways**.
- Name: internet-gateway
- Compartment: compartment-test

![VCN-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/3KzF8mzWu87Fm3bvUiM8lIhuTrB9bRK2_QDf37fp6f5dX6wu0QEr2Xd8y7eIRJ2g/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-02.png)

**Route Table:** Crie uma Route Table que será usada para permitir acesso à internet a partir da VCN em **Networking > Virtual Cloud Networks > VCN > Route Tables**.
- Name: route-table
- Target Type: Internet Gateway
- Destination CIDR BLOCK: 0.0.0.0/0
- Compartment: compartment-test
- Target Internet Gateway: internet-gateway

![VCN-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/MpajUxDylmAttYoxg3mWr232RQsTuRYY2YLaZdfnjhxTzKtg9KiQZFaqvi45gM_c/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-03.png)

**Public Subnet:** Crie uma Public Subnet em **Networking > Virtual Cloud Networks > VCN > Subnets**.
- Name: public-subnet
- Compartment: compartment-test
- Subnet Type: Regional
- CIDR BLOCK: 10.0.0.0/24
- Route Table: route-table
- Subnet Access: Public Subnet
- DNS Label: publicsubnet
- DHCP Options: Default DHCP
- Security List: Default Security List

![VCN-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ef8b3q8rf7q3mTCu0xxWH8aURbqk9EVyKc47-wkkHjuEpQqI0QoPPSsjbRIFhtdw/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-06.png)

![VCN-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ahgQyUXqPE-x5ML31aYLHLVhhrnyW0rp8Ui1dRSo7dHtF4j2jcX0ZsmB7iR2e-rc/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-05.png)

**Private Subnet:** Crie uma Private Subnet em **Networking > Virtual Cloud Networks > VCN > Subnets**.
- Name: private-subnet
- Compartment: compartment-test
- Subnet Type: Regional
- CIDR BLOCK: 10.0.1.0/24
- Route Table: route-table
- Subnet Access: Private Subnet
- DNS Label: privatesubnet
- DHCP Options: Default DHCP
- Security List: Default Security List

![VCN-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Js4XAzGesQwjJolcIoDoSVGTE5UBD-LnvR8p1IlUGUjVX3fN5Vz66tH5fOprSgh9/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-08.png)

![VCN-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/L-EEu9N06gtgmLOf_Jvi6Um8e82Kw7Vdku7bE7_RpOmgRy1zjm2cWXSihIt0l4mF/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-07.png)

**Aproveite a VCN recém criada e provisione uma Instance VM seguindo [estas instruções](https://smallskills.github.io/2020-12-04-Configurando-OCI-CLI/).**

## 3. Usando OCI-CLI

Caso você não tenha OCI-CLI instalado clique [aqui](https://smallskills.github.io/2020-12-04-Configurando-OCI-CLI/) para configurar ou use o **Oracle Cloud Shell** a partir da console administrativa da Oracle Cloud Infrastructure.

![cloud-shell](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/_IRsnWh3PADnAuUx0e6ny-Q6acBGMLi2uw0ZMURhAgcMdwTluVwN2OMBxl4ZBMK5/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/cloud-shell.png)

Para provisionar a VCN via CLI é necessário obter o OCID de alguns recursos. É possível consultar na console administrativa, mas neste caso será usada **query** no CLI para retornar o OCID e variáveis de ambiente para automatizar o processo, observe o **comando export** no início de alguns scripts.

**Tenancy:** Obtenha o OCID do seu Tenancy.

```javascript
$ export tenancy_id=$(oci iam compartment list --all --include-root --query "data[?contains(\"id\",'tenancy')].id | [0]" --raw-output)  
```

**Compartment:** Crie o compartment a VCN será criada.
- Compartment OCID: $tenancy_id
- description: cli-test
- name: compartment-test-cli

```javascript
$ export compartment_id=$(oci iam compartment create --compartment-id $tenancy_id --description cli-test --name compartment-test-cli --query "data.id" --raw-output)
```

**VCN:** Para criar uma VCN é necessário informar o compartment, cidr block, nome da VCN e um DNS label é recomendável utilizar o nome da VCN sem os caracteres especiais.

- Compartment OCID: $compartment_id
- cidr-block: "10.0.0.0/16"
- display-name: vcn-cli-test
- dns-label: vcnclitest

```javascript
$ export vcn_id=$(oci network vcn create --compartment-id $compartment_id --cidr-block "10.0.0.0/16" --display-name vcn-cli-test --dns-label vcnclitest --query "data.id" --raw-output)
```

**Public Subnet:** Com a VNC criada adicione uma public subnet.

- VCN OCID: $vcn_id
- Compartment OCID: $compartment_id
- cidr-block: "10.0.0.0/24"
- display-name: public-subnet
- dns-label: publicsubnet

```javascript
$ export public_subnet_id=$(oci network subnet create --vcn-id $vcn_id --compartment-id $compartment_id --cidr-block "10.0.0.0/24" --display-name public-subnet --dns-label publicsubnet --query "data.id" --raw-output)
```

**Private Subnet:** Crie também uma private subnet adicionando "--prohibit-public-ip-on-vnic true".

- VCN OCID: $vcn_id
- Compartment OCID: $compartment_id
- prohibit-public-ip-on-vnic: true
- cidr-block: "10.0.1.0/24"
- display-name: private-subnet
- dns-label: privatesubnet

```javascript
$ export private_subnet_id=$(oci network subnet create --vcn-id $vcn_id --compartment-id $compartment_id --prohibit-public-ip-on-vnic true --cidr-block "10.0.1.0/24" --display-name private-subnet --dns-label privatesubnet --query "data.id" --raw-output)
```

**Intenet Gateway:** Para que a VCN tenha conexão com a internet crie um Internet Gateway.

- Compartment OCID: $compartment_id
- is-enabled: true
- VCN OCID: $vcn_id
- display-name: IG-cli-test

```javascript
$ export internet_gateway_id=$(oci network internet-gateway create --compartment-id $compartment_id --is-enabled true --vcn-id $vcn_id --display-name IG-cli-test --query "data.id" --raw-output)
```

**Route Table:** Crie uma nova route table e adicione uma regra, neste caso para toda a internet com cidr block 0.0.0.0/0.

- Compartment OCID: $compartment_id
- Route Table OCID: $rt_id
- VCN OCID: $vcn_id
- display-name: route-table-cli

```javascript
$ rt_id=$(oci network route-table create --compartment-id $compartment_id --route-rules '[{"cidrBlock":"0.0.0.0/0","networkEntityId":"'$internet_gateway_id'"}]' --vcn-id $vcn_id --display-name route-table-cli --query "data.id" --raw-output)
```

Associe a route table a public subnet.

- Subnet OCID: $subnet_id
- Route Table OCID: $rt_id

```javascript
$ oci network subnet update --subnet-id $public_subnet_id --route-table-id $rt_id
```

**Aproveite a VCN recém criada e provisione uma Instance VM seguindo [estas instruções](https://smallskills.github.io/2020-12-04-Configurando-OCI-CLI/).**
