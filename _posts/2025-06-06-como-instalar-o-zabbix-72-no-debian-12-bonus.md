---
title: Como instalar o Zabbix 7.2 no Debian 12 + bônus
categories:
- tutoriais
tags:
- zabbix
- debian12
---

Buenas pessoal, tudo certo? Segue um tutorial de como instalar o Zabbix 7.2 no Debian 12!

Primeiramente acesse como superuser:<br>
```
su -
```

Acesse o diretório temporário:<br>
```
cd /tmp
```

Dentro do diretório temporário, baixe o .deb do Zabbix:<br>
```
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
```

Instale o repositório oficial do Zabbix: (Ele adiciona os arquivos necessários para que seu gerenciador de pacotes saiba onde encontrar os pacotes do Zabbix.)<br>
```
dpkg -i zabbix-release_latest_7.2+debian12_all.deb
```

Atualize a lista de pacotes disponíveis:<br>
```
apt update
```

Instale o Zabbix e seus componentes:<br>
```
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mariadb-server
```

Acesse o MySQL com o root:<br>
```
mysql -uroot -p
```

(Irá solicitar a criação uma senha de root, nesse caso irei utilizar passw0rd@r00t, crie a sua senha e a guarde em segurança)<br>
<iframe src="https://drive.google.com/file/d/1Ptp-4dnO3rpy28Izz-w5eaqLULoErNPD/preview" width="311" height="auto" allow="autoplay"></iframe>

Aplique os seguintes comandos, mas **linha por linha**, e em *passw0rdzabb1x*, crie sua senha para o user zabbix do MySQL e a guarde em segurança:<br>
```
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'passw0rdzabb1x';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```
<iframe src="https://drive.google.com/file/d/1RxOTyr1Jzf7lJR4RdytpRMkUH0RIRJT2/preview" width="719" height="auto" allow="autoplay"></iframe>

Execute o comando abaixo, ele serve para criar toda a estrutura do banco de dados que o Zabbix Server precisa para funcionar: (Irá solicitar a senha do user zabbix criado no passo anterior, no meu caso, a senha *passw0rdzabb1x*)<br>
```
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

*(Esse comando leva um certo tempo, irá parecer que travou, mas tenha paciencia e aguarde.)*
<iframe src="https://drive.google.com/file/d/1ixq6_tGt6IHxZJufaTPJHk13pGQ8C9cW/preview" width="719" height="auto" allow="autoplay"></iframe>


Acesse o MySQL novamente: (No meu caso, a senha *passw0rd@r00t*)<br>
```
mysql -uroot -p
```

Execute os seguintes comandos:<br>
```
set global log_bin_trust_function_creators = 0;
quit;
```

Edite o arquivo zabbix_server.conf:<br>
```
nano /etc/zabbix/zabbix_server.conf
```

Localize a linha DBPassword que está comentada, descomente-a e insira a senha do user Zabbix do MySQL: (No meu caso, a senha *passw0rdzabb1x*)<br>
*Exemplo:*
```
DBPassword=passw0rdzabb1x
```

Reinicie os serviços do Zabbix e habilite para iniciar automaticamente:<br>
```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

Abra o navegador e insira o IP do seu zabbix, seguido pelo /zabbix:<br>
*Exemplo:*<br>
http://192.168.3.166/zabbix

Escolha a linguagem que preferir e clique em *Próximo passo*:<br>
<iframe src="https://drive.google.com/file/d/1Ujrp7KtnmnGuAI5_rjigs-CUzZ7gcj6W/preview" width="719" height="427" allow="autoplay"></iframe>

Na tela de pré-requisitos, clique em *Próximo passo*:<br>
<iframe src="https://drive.google.com/file/d/1_lZPpUP5GBKO-CPL-jorIvHT72_IDCH8/preview" width="719" height="426" allow="autoplay"></iframe>

Agora na tela de conexão com o BD, insira o login e senha do user zabbix do MySQL: (No meu caso *zabbix* e *passw0rdzabb1x*)<br>
<iframe src="https://drive.google.com/file/d/1MM8YxztSIJcjLGP3zLDJL6p9D4P3GGr-/preview" width="719" height="427" allow="autoplay"></iframe>

Insira o nome do seu servidor Zabbix, o fuso horário e selecione um tema de sua preferencia, após, clique em *Próximo passo*:<br>
<iframe src="https://drive.google.com/file/d/1cqQmFB6Yer_RhwqspCMIZ5aArJ7cjUQT/preview" width="719" height="428" allow="autoplay"></iframe>

Irá retornar um sumário de pré-instalação, clique em *Próximo passo*:<br>
<iframe src="https://drive.google.com/file/d/1asgx0pU7SLHa3Ia3u1ulvhsex5Lvlzp7/preview" width="719" height="427" allow="autoplay"></iframe>

Instalação e configuração concluída com sucesso, clique em *Fim*:<br>
<iframe src="https://drive.google.com/file/d/15FilNr406cVdIQ2CaORMkHknY2v4yqLV/preview" width="719" height="427" allow="autoplay"></iframe>

Será solicitado o login e senha padrão do Zabbix (User: *Admin* | Password: *zabbix*), clique em *Conectar-se*:<br>
<iframe src="https://drive.google.com/file/d/1iV9BabfZ538ESyY4isYh7pYhBC_Tf7la/preview" width="367" height="381" allow="autoplay"></iframe>

<iframe src="https://drive.google.com/file/d/1DfY0LLUcbTTLTcqhRUu6UkbDH5smtvPc/preview" width="719" height="357" allow="autoplay"></iframe>

**Bônus**

Se você colocar um equipamento para monitorar por ICMP Ping, pode encarar esse erro:<br>
<iframe src="https://drive.google.com/file/d/1Js4nf2z2OBciKmy2Tx58YtLr1GmNkhUd/preview" width="719" height="91" allow="autoplay"></iframe>
*At least one of '/usr/sbin/fping', '/usr/sbin/fping6' must exist. Both are missing in the system.*<br>

Para corrigir este erro, acesse novamente seu servidor Zabbix via CLI e aplique os seguintes comandos: (Um de cada vez)<br>
```
ln -s /usr/bin/fping /usr/sbin/fping
ln -s /usr/bin/fping /usr/sbin/fping6
```
*Esses comandos criam um link simbólico que apontam para o executável real.*

*Exemplo:*<br>
<iframe src="https://drive.google.com/file/d/1kR3tSDrXmW0xDyeHHxjcicMbwLZ8-6wJ/preview" width="480" height="63" allow="autoplay"></iframe>

Quando ele realizar outra captura dos dados, o erro será corrigido:<br>
<iframe src="https://drive.google.com/file/d/1f_E6bBica65XH0gPtYEzDIj3kLGtP8RF/preview" width="719" height="89" allow="autoplay"></iframe>

Por hoje é só, hasta!
