---
layout: post
category : tools
tags : [postgresql, webdev, tools]
---

## PostgreSQL


### Instalação de Pacotes

    sudo apt-get install postgresql php5-pgsql phppgadmin

### Habilitar o acesso remoto ao servidor

No arquivo:

    /etc/postgresql/8.x/main/postgresql.conf

Editar o parâmetro:

    listen_addresses = '*'

### Habilitar acesso de usuários a bancos de dados

No arquivo:

	/etc/postgresql/8.x/main/pg_hba.conf

Editar as linhas:

	# TYPE  DATABASE    USER        CIDR-ADDRESS          METHOD
	host    all         all         127.0.0.1/8           password
	host    all         all         143.54.12.0/24        md5
	host    all         all         10.0.2.0/24           md5
Obs: 
O mais importante é o method: password utiliza senha sem criptografar melhor se o acesso for local (127.0.0.1/8), md5 força o envio da senha criptografada via Internet (143.54.12.0/24).

### Otimizações

http://pt.wikibooks.org/wiki/PostgreSQL_Pr%C3%A1tico/Ap%C3%AAndices/Dicas_sobre_Desempenho_e_Otimiza%C3%A7%C3%B5es_do_PostgreSQL
