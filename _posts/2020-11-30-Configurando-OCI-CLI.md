---
layout: post
title: Configurando OCI-CLI e 1º deploy de Instance VM
subtitle: Após este tutorial você será capaz de acessar o OCI-CLI localmente e criar Instances VMs na Oracle Cloud
tags: [Cloud, Oracle, OCI, CLI, Instance, VM, Windows, Linux, MacOs, Setup]
comments: true
---


**Requisitos**

| Uma conta Oracle Cloud | Obrigatório |
| Python v2.7 ou superior | Obrigatório |
| Tenancy OCID | Obrigatório |
| Compartment OCID | Obrigatório |
| User OCID | Obrigatório |

**Baixando e instalando o Oracle Cloud Infrastructure CLI**

Aceite as configurações default ou personalize conforme a necessidade

MacOS/Linux  
```javascript
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

Windows  
```javascript
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1'))"
```

Verifique que a instalação foi bem sucedida. 

```javascript
oci --help
```

**Configurando Oracle Cloud Infrastructure CLI**

Com o CLI instalado, agora é preciso configura-lo.

```javascript
oci setup config
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

![Crepe](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/dv6MDY-DTGQtoeo26DVjErDksfva4g8eJbFILaPg87ZPSpIDgFHTio5F89ZO_MtH/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/photo01Captura%20de%20Tela%202020-11-30%20a%CC%80s%2010.56.39.png)

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
