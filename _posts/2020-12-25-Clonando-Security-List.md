---
layout: post
title: Clonando uma Security List
subtitle: Como copiar regras entre security lists com OCI CLI
tags: [Cloud, Oracle, OCI, CLI, Security List, Ingress, Egress]
comments: true
---

## Requisitos

| OCID da subnet de origem | Obrigatório |
| OCID da subnet de destino | Obrigatório |

Em algum momento trabalhando com Oracle Cloud será a necessário copiar/replicar security list para outros ambientes, seja para obter alta disponibilidade, trabalhar com múltiplas regiões ou simplesmente para criar um lab para testes. Caso tenha SL com muitas regras isso pode se tornar uma tarefa difícil, imagine adicionar 100 regras ou mais uma a uma via console da cloud.

## Obtenha o OCID das subnets de origem e destino

Navegue até **Networking > Virtual Cloud Networks > Virtual Cloud Network Details > Security Lists**, copie o OCID da Security List de Origem, observe as 133 regras entre Ingress Rules e Egress Rules.

![sl-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/q82YoxDl9YNIqQVaY_HTQcrYpJ9G-NNM_L-N2HcR45BDndmjCXtgjY1n1sQ3Tn8F/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/SL/SL-01.png)

Copie o OCID da Security List de Destino.

![sl-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/iuzMo3mnLh7s49Yb5k2La7pGaX8FXLgGeQQXoUqPZOKnFtBrGTCuqXyMwishakMh/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/SL/SL-02.png)

## Copiando regras da security list de origem

Via interface gráfica copiar e atualizar as regras de uma SL para outra leva muito tempo, mas com o OCI CLI a mesma tarefa leva apenas alguns segundos.

```javascript
$ oci network security-list get --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaabcsvprvix3wkw5p4we5cvjafpskeaio6kjr2ltbe3dhq6dr5osyq
```

Observe que o output é toda a estrutura da security list com informações de compartment, display name, egress-security-rules, ingress-security-rules. Mas vamos precisar apenas **egress-security-rules** e **ingress-security-rules.**

![sl-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/6nTOGtixADq2P2yWeMa2khOKo29kyFBktaXtNh6D3O95FgBVBLhHkpMAzAX2VnsW/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/SL/SL-03.png)

Para filtrar apenas os dados necessários, será utilizado o **[jq](https://stedolan.github.io/jq/)** que é um processador de linha de comando para JSON. Será feito em duas etapas para **egress-security-rules** e **ingress-security-rules.**

**Regras de Egress**

```javascript
$ oci network security-list get --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaabcsvprvix3wkw5p4we5cvjafpskeaio6kjr2ltbe3dhq6dr5osyq | jq '.data."egress-security-rules"'
```

Com isso o output traz apenas as regras de egress. 

![sl-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/06x4M8ETVAmHwANfB_OWD5f1-BQDde2wH1NGU6ifkyI7I90zQPBES3L_7Px8EnZ3/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/SL/SL-04.png)

Adicione as regras de egress a serem "clonadas" para o arquivo egress.json.

```javascript
$ oci network security-list get --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaabcsvprvix3wkw5p4we5cvjafpskeaio6kjr2ltbe3dhq6dr5osyq | jq '.data."egress-security-rules"' > egress.json
```

Então atualize a security list de destino com as regras contidas no arquivo egress.json, use o OCID da security list de destino. Quando solicitado confirme as alterações digitando **y** para confirmar.

```javascript
$ oci network security-list update --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaa6irvzcxv65aralnk5bt7wzinqhqkgghrbxiepoh5caeu77yafeta --egress-security-rules file://egress.json
```

**Regras de Ingress**

Basta repetir os passos da task anterior agora apontando para as regras de Ingress.

```javascript
$ oci network security-list get --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaabcsvprvix3wkw5p4we5cvjafpskeaio6kjr2ltbe3dhq6dr5osyq | jq '.data."ingress-security-rules"'
```

Com isso o output traz apenas as regras de ingress. 

![sl-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/93eRlG7Icbd1lxKhjhnQPuV-WPBOG-Nxpyd89WTRk98z4bRds3phzR8tRsqfPBW-/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/SL/SL-05.png)

Adicione as regras de ingress a serem "clonadas" para o arquivo ingress.json.

```javascript
$ oci network security-list get --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaabcsvprvix3wkw5p4we5cvjafpskeaio6kjr2ltbe3dhq6dr5osyq | jq '.data."ingress-security-rules"' > ingress.json
```

Então atualize a security list de destino com as regras contidas no arquivo ingress.json, use o OCID da security list de destino. Quando solicitado confirme as alterações digitando **y** para confirmar.

```javascript
$ oci network security-list update --security-list-id ocid1.securitylist.oc1.sa-saopaulo-1.aaaaaaaa6irvzcxv65aralnk5bt7wzinqhqkgghrbxiepoh5caeu77yafeta --ingress-security-rules file://ingress.json
```

Agora todas as regras de egress e ingress foram "clonadas" com sucesso.

![sl-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/tYmSJt3DjNxxeyYKOSWEoQxkHF3K3bWs1Oz9ooHlHjGktS1bmebXoVpkl_NYc2Dk/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/SL/SL-06.png)

