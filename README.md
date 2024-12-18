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
