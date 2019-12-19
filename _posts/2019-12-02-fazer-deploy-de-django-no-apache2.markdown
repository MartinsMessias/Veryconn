---
title: Fazer deploy de Django no Apache2
date: 2019-12-02 15:16:00 Z
tags:
- django
- python
- apache2
- deploy
---

<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://raw.githubusercontent.com/sindresorhus/github-markdown-css/gh-pages/github-markdown.css">
<style>
* {
    box-sizing: border-box;
    /* min-width: 200px; */
    max-width: 100%;
    margin: 0 auto;
    padding: 15px;
    font-family: sans-serif;
text-align: justify;
background-color: darkslategrey;
    color: black;
}

	@media (max-width: 767px) {
		.markdown-body {
			padding: 15px;
		}
	}
h2 {
    /* text-decoration: underline; */
    text-shadow: 0px 0px 35px #00ff0a;
    color: black;
}
code {
    font-family: monospace;
    color: #00d427;
    background-color: #000000ab;
    padding-top: 0px;
    padding-bottom: 0px;
    border-radius: 4px;
    font-weight: bold;
}
</style>
## Procedimento realizado no Debian 9

#### Preparando o sistema

`sudo apt update && apt dist-upgrade`

#### Instalando os requerimentos

`sudo apt install apache2`

`sudo apt install python3-pip python-dev build-essential`

`sudo apt install libapache2-mod-wsgi-py3`

#### Instalando virtualenv

`sudo python3 -m pip install virtualenv`

#### Pasta raiz onde tudo vai acontecer

`sudo cd /var/www/`

#### Criando o enviroment

`sudo python3 -m virtualenv env` 

#### Ativando o env

`source env/bin/activate`

#### Instalando os módulos necessários

`pip3 install django`

### Crie um projeto ou baixe dentro da pasta /var/www/env/ 

#### Crie uma pasta “src”

`sudo mkdir env/src && cd env/src`

#### Baixando o projeto direto do Github (link da opção baixar zip)

`sudo django-admin.py startproject --template=https://github.com/MartinsMessias/SeuProjeto/archive/master.zip --name=Procfile SeuProjeto`

### A partir daqui é django então você sabe. Vamos pro apache2

### Configurando o apache

Por default, o arquivo de configuração do apache fica localizado em
**/etc/apache2/sites-available/000-default.conf**

Com um editor no console de sua preferência, apague tudo o que estiver dentro.
Eu curto o nano.

#### Vamos configurar:

`nano /etc/apache2/sites-available/000-default.conf`

    <VirtualHost *:80>  
	    ServerName localhost  
	    ServerAdmin webmaster@localhost  
      
	    # Substituir pelo endereço da sua pasta "static"
	    Alias /static "/var/www/env/src/SeuProjeto/static"  
	    <Directory "/var/www/env/src/SeuProjeto/static">  
		    Require all granted  
	    </Directory>  
	      
	    # Substituir pelo endereço da sua pasta de midias caso possua
	    Alias /media "/var/www/env/src/SeuProjeto/media"  
	    <Directory "/var/www/env/src/SeuProjeto/media">  
		    Require all granted  
	    </Directory>  
	      
	    # Substituir pelo endereço da pasta do seu "wsgi.py"
	    <Directory /var/www/env/src/SeuProjeto/SeuProjeto>  
		    <Files wsgi.py>  
			    Require all granted
		    </Files>  
	    </Directory>  
	    
	    # Pasta do projeto:Pasta do Python
            WSGIDaemonProcess SeuProjeto python-path=/var/www/env/src/SeuProjeto:/var/www/env/lib/python3.7/site-packages  
	    WSGIProcessGroup SeuProjeto
	    # Pasta onde esta o wsgi.py
	    WSGIScriptAlias / /var/www/env/src/SeuProjeto/PastaDo/wsgi.py  
	    WSGIPassAuthorization On  
	     
	    ErrorLog ${APACHE_LOG_DIR}/error.log  
	    CustomLog ${APACHE_LOG_DIR}/access.log combined  
    </VirtualHost>


#### Salve o arquivo e dê um:
`sudo a2enmod wsgi` e `service apache2 restart`

#### Tente acessar e deve estar correndo tudo bem.
#### Agora você já tem sua aplicação rodando.
