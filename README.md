Configuração do UFW (Uncomplicated Firewall):

Listei os aplicativos disponíveis para configurar o firewall com o comando sudo ufw app list.
Permiti o tráfego HTTP do Nginx com o comando sudo ufw allow 'Nginx HTTP'.
Verifiquei o status do UFW com sudo ufw status, e confirmei que as portas 80 (HTTP) e 443 (HTTPS) estavam abertas.
Verificação do Nginx:

Verifiquei o status do serviço Nginx com o comando systemctl status nginx para garantir que ele estava ativo e em execução.
Usei o comando ip addr show para verificar o endereço IP da máquina.
Gerenciamento do Nginx:

Naveguei até o diretório /var/www/html para verificar os arquivos de configuração do Nginx.
Reiniciei o serviço Nginx algumas vezes com os comandos sudo systemctl stop nginx, sudo systemctl start nginx, sudo systemctl restart nginx e sudo systemctl reload nginx.
Criação do script de monitoramento:

Criei o script monitor_nginx.sh para monitorar o status do serviço Nginx.
Tornei o script executável com chmod +x monitor_nginx.sh e executei o script com ./monitor_nginx.sh.
Configuração do Cron:

Usei o comando crontab -e para editar o cron e configurei o script monitor_nginx.sh para ser executado a cada 5 minutos.
Verifiquei as entradas no cron com crontab -l.
Monitoramento e Log:

O script foi configurado para gerar logs do status do Nginx, com entradas nos arquivos nginx_online.log e nginx_offline.log dependendo do status do serviço.
A cada execução, o script verifica se o Nginx está online ou offline e registra o status nos arquivos de log.
Com isso, garanti que o serviço Nginx fosse monitorado de forma automatizada, com registros a cada 5 minutos sobre o seu status, e o cron cuidou da execução do script sem precisar de intervenção manual.

root@DESKTOP-1HHMDKR:~# sudo ufw app list
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  Postfix
  Postfix SMTPS
  Postfix Submission
root@DESKTOP-1HHMDKR:~# sudo ufw allow 'Nginx HTTP'
Skipping adding existing rule
Skipping adding existing rule (v6)
root@DESKTOP-1HHMDKR:~# sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
Nginx Full                 ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
Nginx Full (v6)            ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)

root@DESKTOP-1HHMDKR:~# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset>
     Active: active (running) since Tue 2024-12-17 21:34:58 -03; 10min ago
       Docs: man:nginx(8)
    Process: 2155 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_p>
    Process: 2157 ExecStart=/usr/sbin/nginx -g daemon on; master_process on>
    Process: 2176 ExecReload=/usr/sbin/nginx -g daemon on; master_process o>
   Main PID: 2158 (nginx)
      Tasks: 9 (limit: 4616)
     Memory: 6.4M ()
     CGroup: /system.slice/nginx.service
             ├─2158 "nginx: master process /usr/sbin/nginx -g daemon on; ma>
             ├─2177 "nginx: worker process"
             ├─2179 "nginx: worker process"
             ├─2180 "nginx: worker process"
             ├─2181 "nginx: worker process"
             ├─2182 "nginx: worker process"
             ├─2183 "nginx: worker process"
             ├─2184 "nginx: worker process"
             └─2185 "nginx: worker process"

Dec 17 21:34:58 DESKTOP-1HHMDKR systemd[1]: Starting nginx.service - A high>
Dec 17 21:34:58 DESKTOP-1HHMDKR systemd[1]: Started nginx.service - A high >
Dec 17 21:35:40 DESKTOP-1HHMDKR systemd[1]: Reloading nginx.service - A hig>
Dec 17 21:35:40 DESKTOP-1HHMDKR nginx[2176]: 2024/12/17 21:35:40 [notice] 2>
Dec 17 21:35:40 DESKTOP-1HHMDKR systemd[1]: Reloaded nginx.service - A high>
root@DESKTOP-1HHMDKR:~# ip ddr show
Object "ddr" is unknown, try "ip help".
root@DESKTOP-1HHMDKR:~# ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet 10.255.255.254/32 brd 10.255.255.254 scope global lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:b6:84:dc brd ff:ff:ff:ff:ff:ff
    inet 172.28.253.37/20 brd 172.28.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:feb6:84dc/64 scope link
       valid_lft forever preferred_lft forever
root@DESKTOP-1HHMDKR:~# cd /var/www
root@DESKTOP-1HHMDKR:/var/www# ls
html
root@DESKTOP-1HHMDKR:/var/www# cd html
root@DESKTOP-1HHMDKR:/var/www/html# ls
index.html  index.nginx-debian.html
root@DESKTOP-1HHMDKR:/var/www/html# sudo systemctl stop nginx
root@DESKTOP-1HHMDKR:/var/www/html# sudo systemctl start nginx
root@DESKTOP-1HHMDKR:/var/www/html# sudo systemctl restart nginx
root@DESKTOP-1HHMDKR:/var/www/html# sudo systemctl reload nginx
root@DESKTOP-1HHMDKR:/var/www/html# cd ~
root@DESKTOP-1HHMDKR:~# nano monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# chmod +x monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# ./monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# crontab -e
crontab: installing new crontab
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# crontab -e
crontab: installing new crontab
root@DESKTOP-1HHMDKR:~# cat /var/log/nginx_status/nginx_status.log
Tue Dec 17 20:49:44 -03 2024 - Nginx Status: ONLINE
2024-12-17 21:05:27 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
Tue Dec 17 21:37:09 -03 2024 - Nginx Status: ONLINE
Tue Dec 17 21:48:24 -03 2024 - Nginx Status: ONLINE
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# crontab -e
crontab: installing new crontab
root@DESKTOP-1HHMDKR:~# cat /var/log/nginx_status/nginx_status.log
Tue Dec 17 20:49:44 -03 2024 - Nginx Status: ONLINE
2024-12-17 21:05:27 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
Tue Dec 17 21:37:09 -03 2024 - Nginx Status: ONLINE
Tue Dec 17 21:48:24 -03 2024 - Nginx Status: ONLINE
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# cat /var/log/nginx_status/nginx_status.log
Tue Dec 17 20:49:44 -03 2024 - Nginx Status: ONLINE
2024-12-17 21:05:27 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
Tue Dec 17 21:37:09 -03 2024 - Nginx Status: ONLINE
Tue Dec 17 21:48:24 -03 2024 - Nginx Status: ONLINE
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# nano monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# chmod +x monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# ./monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# cat /var/log/nginx_status/nginx_status.log
Tue Dec 17 20:49:44 -03 2024 - Nginx Status: ONLINE
2024-12-17 21:05:27 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
Tue Dec 17 21:37:09 -03 2024 - Nginx Status: ONLINE
Tue Dec 17 21:48:24 -03 2024 - Nginx Status: ONLINE
2024-12-17 21:54:58 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# nano monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# chmod +x monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# ./monitor_nginx.sh
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# cat /var/log/nginx_status/nginx_online.log
2024-12-17 21:08:48 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
2024-12-17 21:56:56 - Serviço: nginx - Status: ONLINE - O serviço está funcionando corretamente.
root@DESKTOP-1HHMDKR:~# cat /var/log/nginx_status/nginx_offline.log
2024-12-17 21:09:43 - Serviço: nginx - Status: OFFLINE - O serviço não está funcionando.
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# crontab -e
crontab: installing new crontab
root@DESKTOP-1HHMDKR:~# cd ~
root@DESKTOP-1HHMDKR:~# crontab -l
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
*/5 * * * * /root/scripts/monitor_nginx.sh

root@DESKTOP-1HHMDKR:~#
