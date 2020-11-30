---
layout: post
title: Configurando OCI-CLI e 1º deploy de Instance VM
subtitle: Após este tutorial você será capaz de acessar o OCI-CLI localmente e criar Instances VMs na Oracle Cloud
tags: [Cloud, Oracle, OCI, CLI, Instance, VM, Windows, Linux, MacOs]
comments: true
---

## Requisitos

| Uma conta Oracle Cloud | Obrigatório |
| Python v2.7 ou superior | Obrigatório |
| Tenancy OCID | Obrigatório |
| Compartment OCID | Obrigatório |
| User OCID | Obrigatório |

## Baixando e instalando o Oracle Cloud Infrastructure CLI

Aceite as configurações default ou personalize conforme a necessidade

**MacOS/Linux**  
```javascript
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

**Windows**  
```javascript
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1'))"
```

Certifique-se que a instalação foi bem sucedida. 

```javascript
oci --help
```

Caso a instalação tenha sido bem sucedida receberá um output como esse.

![Oci-Help](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/E4YcwQoBdQsKXZm8fVrLci4xiInG0FiRaGWSQfNEXxVXJAmfiSXS-3-PPKoWV2Vr/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/oci-help.png)

## Configurando Oracle Cloud Infrastructure CLI

Com o CLI instalado, agora é preciso configura-lo. Nesta etapa será necessário compartment OCID, tenancy OCID e user OCID.

**Obtendo User OCID, Compartmet OCID e Tenancy OCID** 

User OCID: Navegue até Identity>Users>User Details

![User-Ocid](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/np_zGbe9Zaz9UINKmPcGJhRIxsGrUHuVEEcDhLG6RQow1d6SMvspygRAq_UCpzff/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/user-ocid.png)

Compartment OCID: Navegue até Identity>Compartment>Compartment Details

![Compartment-Ocid](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/JKEwDkGRSm-IbJIVUlHz89Ozcw_pjs8kkl5SeoTkl_QzlyfId2wS1EiD_AaLwXXs/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/compartment-ocid.png)

Tenancy OCID: Navegue até Administration>Compartment>Tenancy Details

![Tenancy-Ocid](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/AgiXs372OaF4Jl9P2gdjVXpgceo-c_u7So_hh2oFnDolVOGFv3VwgCEniswKKxQB/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/tenancy-ocid.png)

```javascript
oci setup config
```

```javascript
Enter a location for your config [/home/ubuntu/.oci/config]: 
Enter a user OCID: ocid1.user.oc1..
Enter a tenancy OCID: ocid1.tenancy.oc1..
Enter a region (e.g. ap-chiyoda-1, ap-chuncheon-1, ap-hyderabad-1, ap-melbourne-1, ap-mumbai-1, ap-osaka-1, ap-seoul-1, ap-sydney-1, ap-tokyo-1, ca-montreal-1, ca-toronto-1, eu-amsterdam-1, eu-frankfurt-1, eu-zurich-1, me-dubai-1, me-jeddah-1, sa-saopaulo-1, uk-cardiff-1, uk-gov-cardiff-1, uk-gov-london-1, uk-london-1, us-ashburn-1, us-gov-ashburn-1, us-gov-chicago-1, us-gov-phoenix-1, us-langley-1, us-luke-1, us-phoenix-1, us-sanjose-1): sa-saopaulo-1
Do you want to generate a new API Signing RSA key pair? (If you decline you will be asked to supply the path to an existing key.) [Y/n]: yes
Enter a directory for your keys to be created [/home/ubuntu/.oci]: 
Enter a name for your key [oci_api_key]:        
Public key written to: /home/ubuntu/.oci/oci_api_key_public.pem
Enter a passphrase for your private key (empty for no passphrase): 
Private key written to: /home/ubuntu/.oci/oci_api_key.pem
Fingerprint: 5e:b0:68:f5:d9...
Config written to /home/ubuntu/.oci/config
```

**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous numbe |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Oci-Help](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/F4ceppVSvIlaAfzCmhLvVjXEeykRzrH61nScYp9Ayn7_WBxtbfar7VaejUbW0-11/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/oci-helpoci-help.png)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
