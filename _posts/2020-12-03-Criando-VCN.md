---
layout: post
title: Como criar VCN, Subnet, Internet Gateway e Route Table na Oracle Cloud
subtitle: Três formas de criar uma Virtual Cloud Network, Subnet, Internet Gateway e Route Table na Oracle Cloud Infrastructure
tags: [Cloud, Oracle, OCI, CLI, VCN, Subnet, Internet Gateway, Route Table]
comments: true
---

## Requisitos

| Uma conta Oracle Cloud | Obrigatório |
| User com permissão de Manage Network-Family | Obrigatório |

## 1. Usando VCN Wizard

**Compartment:** Crie uma compartment para organizar os recursos que serão provisionados. Acesse a console administrativa da Oracle Cloud Infrastructure e vá até **Identity > Compartments > Create Compartment**.

![compartment](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ZLAwcM1XYlT_lkBcSn0NeJ4dOM-wi1abvj3xyDBawvFVT4umlvc4P6fdGDNc7G-c/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/compartment.png)

**VCN:** Navegue até **Networking > Virtual Cloud Networks** , selecione o compartment desejado e click em **Start VCN Wizard.**

![Wizard-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/elm_Z_qoSXZhZzQFIe9F8qfSgRVY4bWRIZ7bTfFIzbgJL7x-gij3be0vB2CVqMx7/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-01.png)

Escolha a opção **VCN with Internet Connectivity** para criar uma VCN com conexão com a internet, ou seja todos os recursos necessários para conectividade externa será criada.

![Wizard-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Gqq3Wwt2GxPnZZLt9AqBb2fomv24IQ9Mx4Pp_Ha67X5ON_m3Ohqzz2qtWyqmiDA1/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-02.png)

- Name: VCN-wizard
- Compartment: compartment-test
- CIDR BLOCK: 10.0.0.0/16
- Public Subnet CIDR BLOCK: 10.0.0.0/24
- Private Subnet CIDR BLOCK: 10.0.1.0/24
- DNS Resolution: VCNwizard

![Wizard-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/o8Ayy2Z9WKCcFjHG2UF3cXBN5-eG3nbIlzGXoR-FLhhyhILGpAeqV9CdOd1S3iG4/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-04.png)

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

![Wizard-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/yRQHxfPW4z3oS4qc5dQCKN6cBs-z8YjfHT2xjmB1gQ_yx8keelpj04t4Rl8blvd8/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-05.png)

![Wizard-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/SxD8rWfmkBRgGbOADp40wuNzRpxaOoOA5Ui7ekG0b8Bczp1XlWPfN-IJt5hI8aHj/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-06.png)

Certifique-se que todos os recursos foram criados corretamente em **Networking > Virtual Cloud Networks > VCN-wizard**.

![Wizard-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ENEs-ejd6RoNUvU6-KLqmMZjW6GuftqiYh41TbRhsYmp05skG4vWeS9mqtBNqIqE/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/wizard-07.png)

**Aproveite a VCN recém criada e provisione uma Instance VM seguindo [estas instruções](https://smallskills.github.io/2020-12-03-Configurando-OCI-CLI/).**

## 2. Criando recursos da VCN individualmente

**Compartment:** Crie uma compartment para organizar os recursos que serão provisionados. Acesse a console administrativa da Oracle Cloud Infrastructure e vá até **Identity > Compartments > Create Compartment**.

![compartment](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ZLAwcM1XYlT_lkBcSn0NeJ4dOM-wi1abvj3xyDBawvFVT4umlvc4P6fdGDNc7G-c/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/compartment.png)

**VCN:** Crie a VCN em até **Networking > Virtual Cloud Networks > Create VCN**.
- Name: VCN
- Compartment: compartment-test
- CIDR BLOCK: 10.0.0.0/16
- DNS Resolution: vcn

![VCN-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Y3zwEO7ChkKdwPX28xYrRnzwn7Mzk5FukxIKLdG2RAFqAMjm91mqxVbm_Whr5SM0/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-01.png)

**Internet Gateway:** Crie um Internet Gateway em **Networking > Virtual Cloud Networks > VCN > Internet Gateways**.
- Name: internet-gateway
- Compartment: compartment-test

![VCN-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/KR8sBCs6F_AsXkj5FFdyP9GlrTxIeANwe6wAN2w5psW1OnIBBUjHyLTW98seSmUu/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-02.png)

**Route Table:** Crie uma Route Table que será usada para permitir acesso a internet a partir da VCN em **Networking > Virtual Cloud Networks > VCN > Route Tables**.
- Name: route-table
- Target Type: Internet Gateway
- Destination CIDR BLOCK: 0.0.0.0/0
- Compartment: compartment-test
- Target Internet Gateway: internet-gateway

![VCN-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/8-A3V1NkDc_UVsrIepjYAUp_EtRfs-QisMFiRK1LKjFkJes7feAHBuqX0OvynlpS/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-03.png)

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

![VCN-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/1XjGtfCjr1vXO0giXh_q3W7v-vnHxchU_0lWsCybz1zvWKddnsp3ErnRP0U3iIdU/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-06.png)

![VCN-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/DWlmOTjzAzyA37jUHUlLBesHJlv5_PTpLCU9w7i_G_Qg400l1vRN0Ilt5ADS7X_H/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-05.png)

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

![VCN-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/MJb5NYrF2fELQLlHL9qIbh49uXOIkyA9nMjciptSKeNzMsORtzlOhoz99ukXiIV2/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-08.png)

![VCN-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/8zAyR16Qv-DKWT7c-givfYWoRYEt5_DebHebHfRKha4UNYXkxGxCfAm7A9WD9xaL/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/VCN-07.png)

**Aproveite a VCN recém criada e provisione uma Instance VM seguindo [estas instruções](https://smallskills.github.io/2020-12-03-Configurando-OCI-CLI/).**

## 3. Usando OCI-CLI

Caso você não tenha OCI-CLI instalado clique [aqui](https://smallskills.github.io/2020-12-03-Configurando-OCI-CLI/) para configura-lo ou continue usando o **Oracle Cloud Shell** a partir da console administrativa da Oracle Cloud Infrastructure.

![cloud-shell](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/mD3PmxYZP3IKYEsOnT4pAEkJOnHxNkPa-F-4DwaY6aFglc0oY37f44ZTuyW_IN3q/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-VCN/cloud-shell.png)

Para provisionar a VCN via CLI é necessário obter o OCID de alguns recursos. É possível consultar na console administrativa, mas neste caso será usada **query** no CLI para retornar o OCID e variavéis de ambiente para automatizar o processo, observe o **comando export** no início de alguns scripts.

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

**Aproveite a VCN recém criada e provisione uma Instance VM seguindo [estas instruções](https://smallskills.github.io/2020-12-03-Configurando-OCI-CLI/).**
